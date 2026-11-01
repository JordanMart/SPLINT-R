# Setup and Health Checks

> This is a **reconstruction guide**, not a full turnkey installer. Use snapshots and verify each step against your own VM state.

## Architecture checklist

1. VMware: a host-only `192.168.50.0/24` lab network; if NAT is also enabled for updates/SMTP, confirm you are **not bridging** the lab DC directly into a production or untrusted LAN.
2. `SPLUNK01` Ubuntu / Splunk: `192.168.50.10`, web UI reachable from authorized host, receiving port 9997 listening.
3. `DC01` Windows Server (verify actual version) / `corp.local`: `192.168.50.30`, Windows Security logs available.
4. Splunk Universal Forwarder: Security log collection enabled; forward to `192.168.50.10:9997`.
5. **Verified:** Sysmon intake and raw XML event extraction. **Pending:** second Windows endpoint and network sensor.

## Verification order: explain first, then test

**Check 1: Splunk is available.** On the Ubuntu VM, inspect Splunk service status and open the UI using the local lab address from your trusted host. If the UI is unavailable, troubleshoot Splunk *before* the Windows forwarder.

**Check 2: Check intake.** In Splunk Search & Reporting:

```spl
index=main earliest=-24h
| stats count as event_count latest(_time) as latest_event by host sourcetype
| convert ctime(latest_event)
| sort -event_count
```

**Interpretation:** This tells you which hosts are forwarding, whether events are recent, and what sourcetypes arrived. Empty results might mean an unpowered source, wrong index, forwarding fault, or old data; if the lab was off, widen to 30 days to check history.

**Check 3: Confirm failed login signal.**

```spl
index=main EventCode=4625 earliest=-30d
| stats count as failures by host sourcetype
```

**Check 4: Compare saved alert to version-controlled rule.** Your original working Splunk saved search may differ from this repository's `SPLINT-R-001-failed-logons.spl`. Export the *actual* query and its schedule/threshold settings into sanitized docs after checking field names; do not overwrite a functioning alert blindly.

## Sysmon collection (verified in this lab)

1. Sysmon is already generating events in the lab and its Operational log is ingested by Splunk. Check existing service/configuration before changing it.
2. On a fresh replica, if missing, obtain Sysmon from [Microsoft Sysinternals](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon). Plan a minimal test configuration that captures process creation (Event 1); expand DNS (22) / network (3) selectively because of volume.
3. In Event Viewer look for `Applications and Services Logs → Microsoft → Windows → Sysmon → Operational`.
4. Have the Windows Universal Forwarder monitor that channel (example under `config/examples/inputs.conf.example`). Restart the forwarder after adjusting the config, using an elevated shell and your actual installed path.
5. Query Splunk for the source/Event ID. Use a harmless process launch such as opening Notepad to test Event 1.
6. Record exact app versions, screenshots, and source field names in the validation log.

**Why Event 1 first?** It exposes command line, parent process and image context, giving the lab more behavioral detection opportunities than Security 4625 alone.

## Gotchas

- A host-only VM may not reach SMTP without separate managed NAT internet access.
- Check timezones and clock sync between VMs before interpreting correlation.
- Never put SMTP App Passwords or Splunk authentication tokens in this repository. Keep only example forwarder configs with synthetic addresses.
- Use the exact Event Log channel name reported by Event Viewer for the Sysmon input stanza.

**Docs:** [Splunk Event Log collection](https://help.splunk.com/en/splunk-enterprise/get-started/get-data-in/9.4/get-windows-data/monitor-windows-event-log-data-with-splunk-enterprise); [Microsoft Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon).
