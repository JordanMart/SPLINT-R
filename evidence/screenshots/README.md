# Lab evidence screenshots (Nov 1, 2026)

| File | What it proves |
| --- | --- |
| `encoded-powershell-positive.png` | Manual encoded-command search returned a process event |
| `encoded-powershell-trigger-history.png` | Earlier saved alert enabled with a triggered-alert record (some identifying text redacted) |
| `alert-trigger-history.png` | Subsequent records in the scheduled alert's history; **not** proof of event association on its own |
| `atomic-test17-sysmon-event.png` | The `powershell.exe -e` Atomic test appeared in Sysmon Splunk data |
| `detection-v2-three-matches.png` | After changing SPL, both manual tests and Atomic `-e` execution matched |
| `detection-v2-latest-test.png` | New `-e` execution matched the revised search in the five-minute window |

Images are evidence of the described observations only. They do **not** prove a full production-grade SIEM, other tool deployments, or a complete end-to-end multi-stage response. Review any images for lab usernames, network details and secrets before publishing publicly.
