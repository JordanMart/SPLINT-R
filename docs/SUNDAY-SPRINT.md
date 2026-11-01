# Sunday (Nov 1, 2026) — Accelerated build plan

Goal: publish a technically honest GitHub SOC portfolio and validate as much of the original charter as resources permit. These are strict time boxes, **not guarantees**. Record results as *working*, *tested partially*, or *planned*.

| Workstream | Budget | Concrete acceptance test |
| --- | --- | --- |
| Publish GitHub baseline | 30 min | README renders and evidence links open |
| Verify/finish SPLINT-R-002 alert | 30 min | A new triggered alert's View Results contains a recent `-e` test event |
| CALDERA | 60–90 min | Single safe operation on an isolated disposable VM, with recorded results |
| Linux telemetry/osquery/Velociraptor | 60–90 min | At least one documented query/artifact plus a recent forwarded event |
| Zeek/Suricata | 90 min | Capture traffic and prove sensor log/alert generation |
| Cribl | 60–90 min | Test event routed and observed at destination |
| TheHive/Cortex | 90–150 min | One case and observable-enrichment action confirmed |
| Final documentation/review | 60 min | Sanitized screenshots and truthfully updated README |

Stop any unproductive installation after its allotted time, record the blocker, and move to the next workstream. On a single host, stagger heavyweight applications. Networked test activity should stay within your owned lab, preferably on disposable VMs. Never disable security controls merely to make a test pass.

## Completed before Sunday

- Splunk and DC01 telemetry verified; Sysmon Event ID extraction working.
- SPLINT-R-002 scheduled-alert history confirmed; manual encoded PowerShell tests found.
- Official Atomic Red Team T1059.001 Test #17 **manually** executed; its `-e` process event found in Splunk.
- Detection modified to include `-e`; the newest automatic trigger-to-event association remains unconfirmed.
