# Scope, Roadmap, and Done Criteria

## MVP: publishable baseline

- [ ] Recheck existing Splunk and DC status
- [ ] Export/check exact original saved search and compare with repo rule
- [ ] Attach at least 3 genuine sanitized screenshots
- [ ] Write a short case-study conclusion with what worked and what did not
- [ ] Publish clean repository, no credentials or personally identifying records

## Interview-ready build

- [ ] Sysmon Event 1 ingestion; verify with a benign process launch
- [ ] Validate the privileged-group rule and encoded-PowerShell rule (or substitute two other fully tested use cases)
- [ ] Add a detection coverage table with dates and evidence
- [ ] Build a useful Splunk dashboard with screenshot
- [ ] Walk through one investigation timeline with evidence and false positives

## Future platform expansion (optional, not part of MVP)

- Additional Windows client endpoint and Linux telemetry
- Network sensor: Zeek / Suricata
- osquery / Velociraptor collection
- TheHive / Cortex case workflow
- CALDERA or Atomic Red Team simulations in a tightly isolated lab
- Cribl pipelines, additional analytics, cloud log ingestion (AWS)

## Definition of done

A reviewer can: (1) understand architecture and scope in 60 seconds; (2) inspect at least three **validated** detections with a clear query, detection hypothesis, ATT&CK mapping, a test, and limitations; (3) see *real sanitized evidence*; (4) follow a reproducible setup and triage flow; (5) tell completed work from future ambitions.
