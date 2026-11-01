# SPLINT-R-001: Windows Failed-Logon Burst

**State:** A five-failed-logons / three-minute lab alert was demonstrated in October 2026. The SPL in [SPL file](SPLINT-R-001-failed-logons.spl) is a *portable recreation* and **must be tested** before it is called the exact validated implementation.

- **Hypothesis:** Numerous failures for one account from a host / source address over a short interval may indicate password guessing.
- **Data requirement:** Windows Security log Event ID `4625` from a lab Windows host.
- **ATT&CK:** [T1110 — Brute Force](https://attack.mitre.org/techniques/T1110/). It is **not proof** of an attack.
- **Threshold:** `>= 5` failures within a rolling three-minute search window, grouped by host, target account, and source address.
- **Alert cadence suggestion:** Every minute, over the last five minutes; per-result suppression 10–15 minutes by host/account/source as needed. Compare alert records carefully to ensure repeated windows do not create repeated incident notifications.

## Understand the query

The search normalizes common field names, sorts events chronologically, uses `streamstats time_window=3m` to count within each host/account/source grouping, filters for five or more failures, then summarizes the peak count. Field extraction differs by `sourcetype`, UF settings and add-ons. Do **not** assume `TargetUserName` or `IpAddress` exist in your specific indexed events.

First inspect:

```spl
index=main EventCode=4625 earliest=-30d
| head 5
| table _time host sourcetype TargetUserName Account_Name IpAddress Source_Network_Address _raw
```

Check the field sidebar and one raw event; adjust names or extract data if needed. If this produces no results, check the timeframe, index, and Windows audit configuration before changing the rule.

## Safe, controlled validation

1. Start Splunk and `DC01`. Verify a fresh 4625 is visible in Splunk.
2. Use a **dedicated lab test account** with a known password and check the lab account lockout policy.
3. Produce five incorrect logon attempts in under three minutes only on your own lab system. Avoid locking an account needed for recovery.
4. Run the SPL with a wider test time range and verify the grouped event count and timestamps.
5. Review Search & Reporting → Activity → Triggered Alerts after saving a scheduled alert; confirm email action separately.
6. Save sanitized screenshots of the raw event, search results, triggered alert, and email. Record the observed result in [validation log](../docs/VALIDATION-LOG.md).

## SOC triage decision tree

- **Verify:** Which host, account, IP, and logon type appear in the 4625 details? Is the account valid and is the time range accurate?
- **Correlate:** Search for success Event ID 4624 for the same account/source around the same time; inspect other targeted accounts.
- **Investigate benign causes:** Expired/stale credentials, scheduled services, disconnected mapped drives, mistyped passwords, or password changes.
- **Escalation criteria:** High-volume failures followed by a success, attacks across accounts, privileged account attempts, unknown sources, or activity outside expected behavior.
- **Document:** Timeline, hypotheses, supporting events, limits of visibility, and action taken.

## Known limitations

- Host/account/source grouping can miss password spraying spread across many accounts; create a separate spray rule if expanding coverage.
- Source IP may be missing or `-` in local Windows logons.
- The query looks back five minutes and can return the same burst in successive runs; tune alert throttling.
- Email received = notification delivered, not confirmation of a compromise.
