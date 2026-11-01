# SPLINT-R Validation Log

| Date (2026) | Area | Test / observation | Result | Evidence / limit |
| --- | --- | --- | --- | --- |
| Oct 11 | SPLINT-R-001 | Repeated failed logons | Previously observed Splunk alert + email notification | Historical session; saved rule needs parity check |
| Nov 1 | Sysmon intake | Enumerate Sysmon XML events | 1,886 events / 24h incl. 593 Event ID 1 at one checkpoint | Live screenshot, counts change with time |
| Nov 1, 00:37 | SPLINT-R-002 v1 | Harmless `-EncodedCommand` | Positive hit in Splunk | [Screenshot](../evidence/screenshots/encoded-powershell-positive.png) |
| Nov 1 | Splunk alert | Saved scheduled alert; trigger history | Automated alert records created | [Trigger history](../evidence/screenshots/alert-trigger-history.png), post-update association unverified |
| Nov 1, 01:16 | Atomic T1059.001 #17 | Manual execution of official `-e` test | Sysmon Event ID 1 found | [Screenshot](../evidence/screenshots/atomic-test17-sysmon-event.png) |
| Nov 1, 01:32 | SPLINT-R-002 v2 | Broaden search to include `-e` | 3 historical matching events | [Screenshot](../evidence/screenshots/detection-v2-three-matches.png) |
| Nov 1, 01:47 | SPLINT-R-002 v2 | Repeat `-e` test and query | Matching Sysmon Event ID 1 | [Screenshot](../evidence/screenshots/detection-v2-latest-test.png) |
| Pending | SPLINT-R-002 alert v2 | Confirm View Results of newest scheduled trigger | Not verified yet | Avoid claiming alert-to-test linkage |
| Pending | SPLINT-R-003 | Group-change detection | Draft only | No test evidence |

**Note:** Timestamps are as displayed in the Splunk UI and may differ from host or UTC time zone labels. Test executions are controlled lab simulations, not evidence of a real intrusion.
