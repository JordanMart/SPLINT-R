# Three-Minute SOC Interview Demo

Only say and show what is backed by evidence. No slides are required if you can explain it naturally.

**0:00–0:30 — Problem.** “I wanted hands-on experience collecting Windows telemetry, writing detection logic, and verifying alerts end-to-end rather than only reading about SIEMs.”

**0:30–1:10 — Architecture.** Point to `README.md` Mermaid. Explain the isolated VMware network, `DC01`, Splunk server, and Universal Forwarder over TCP 9997. Explicitly label planned endpoints as future work.

**1:10–2:00 — Detection.** Show Event 4625 and your rule for five failures over three minutes. Explain grouping, signal versus noise, and the difference between counting events and confirming a brute-force attack.

**2:00–2:30 — Validation.** Show a sanitized event, detection result, triggered alert and email. State the date of validation; if new screenshots are not available, say the old lab test succeeded but the current repo implementation needs a fresh check.

**2:30–3:00 — Investigation and next steps.** Describe triage pivots (user, source, host, subsequent successes), potential false positives, and planned Sysmon + group membership / PowerShell detections.

## Questions you should answer without notes

- Why 5 failures? Why 3 minutes? What would produce false positives?
- Does a 4625 prove an attack? What additional events help distinguish benign and malicious activity?
- What's the difference between a Universal Forwarder and a Splunk indexer?
- What if network communication on TCP 9997 fails?
- How does a rolling three-minute window compare to fixed 3-minute bins?
- Why does the same burst sometimes generate multiple scheduled alerts?
- What did you actually build, and what is still planned?
