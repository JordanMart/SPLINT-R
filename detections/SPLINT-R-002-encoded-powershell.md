# SPLINT-R-002 — Encoded PowerShell Execution

**Status (Nov 1, 2026):** Positive search tests succeeded for manual encoded commands and a manually executed official Atomic Red Team test; a scheduled Splunk alert is configured and trigger history has been observed. The *newest alert-to-event association* is not yet established from a Triggered Alert → View Results screenshot. The query was revised to cover `-e` and the revised search returned the test event.

**ATT&CK:** [T1059.001 — PowerShell](https://attack.mitre.org/techniques/T1059/001/). **Source:** `DC01`, Sysmon Event ID 1, `index=main`.

## Detection hypothesis

PowerShell launched with an encoded-command argument can merit investigation because encoding can obscure the actual command. The parameter **does not establish compromise** by itself: legitimate tools can also use encoded PowerShell. Follow-up analysis should examine the *decoded* command, executing user, source process, time, and related log events.

## Search logic

See [the SPL file](SPLINT-R-002-encoded-powershell.spl). The XML Sysmon sourcetype arrived without field extractions for the fields used in this lab. The rule uses `rex` to extract `EventID`, `Image`, `ParentImage`, and `CommandLine` from raw XML, then tests for these command-line arguments:

- `-EncodedCommand`
- `-enc`
- `-e`

These string checks are **lab heuristics, not comprehensive PowerShell parameter parsing**. Substring matches, uncommon formatting, or variants could create false positives and false negatives. `splunk-powershell.exe` is excluded to reduce the Forwarder's normal self-generated noise; this exclusion is specific to the environment and should be re-evaluated for deployment.

## Initial manual positive tests

On the isolated, owned lab DC01, a harmless command was encoded as UTF-16LE Base64 and run in a child PowerShell process:

```powershell
$command = "Write-Output 'SPLINTR_TEST_002'"
$encoded = [Convert]::ToBase64String([Text.Encoding]::Unicode.GetBytes($command))
powershell.exe -NoProfile -EncodedCommand $encoded
```

**Observed:** Splunk found a process-creation record with `-EncodedCommand` and PowerShell as both image and parent image. A scheduled alert was configured (cron every 5 minutes, results >0, trigger once, throttle 10 minutes, Medium severity), and alert history recorded triggers. See [positive test](../evidence/screenshots/encoded-powershell-positive.png) and [alert trigger history](../evidence/screenshots/alert-trigger-history.png).

## Atomic Red Team uncovered a gap

**Technique:** T1059.001, [official Atomic Red Team Test #17](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1059.001/T1059.001.md).

- `Invoke-AtomicRedTeam` 2.3.0 and `powershell-yaml` 0.4.12 were installed and verified on DC01.
- The YAML test definition was not available in the expected directory, so **`Invoke-AtomicTest` did not run the test**.
- The published Test #17 command was reviewed and **executed manually**; it starts `powershell.exe -e` with an encoded, harmless print statement.
- A Sysmon Event ID 1 from this execution appeared in Splunk at **2026-11-01 01:16:18.606** (time displayed in lab UI), including PowerShell parent/image and the abbreviated `-e` parameter. See [Atomic event screenshot](../evidence/screenshots/atomic-test17-sysmon-event.png).
- The existing v1 search did not match `-e`; the revised v2 search added the third argument check.
- The revised search returned **three** historical matches (two manual and one Atomic) and, later, a new `-e` event at **01:47:09.122**. See [three matches](../evidence/screenshots/detection-v2-three-matches.png) and [latest match](../evidence/screenshots/detection-v2-latest-test.png).

**Result:** Successfully demonstrated an adversary-emulation **search** validation and gap remediation. A post-change scheduled alert trigger linked to the latest `-e` event remains a separate verification task.

## Alert scheduling caveat

The first saved alert inadvertently contained an inline `earliest=-24h` despite a five-minute UI time range. That can cause repeated triggers on older events. The revised query above removes the inline time constraint and uses the scheduled time picker. A five-minute search every five minutes can miss late arrivals, so we plan to evaluate overlap/deduplication or index-time evaluation.

## Further validation

1. Confirm updated scheduled alert **View Results** contains an actual newly generated `-e` event.
2. Test the nonmatch case with a benign PowerShell command (no encoded argument), and document false positives.
3. Replace substring matching with more robust process-basename and parameter parsing; consider field extraction at ingestion/search configuration.
4. Apply noise tuning and a strategy for handling delayed ingestion without duplicate alerts.
