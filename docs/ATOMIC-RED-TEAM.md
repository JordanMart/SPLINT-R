# Atomic Red Team — T1059.001 Test #17 (manual run)

**Status:** Manually executed on Nov 1, 2026; Sysmon telemetry and a revised Splunk *search* were validated. Framework invocation and automatic-alert-to-test association remain unverified.

## Setup and execution method

On isolated lab DC01, PowerShell modules `Invoke-AtomicRedTeam` v2.3.0 and `powershell-yaml` v0.4.12 were installed. `Get-Command Invoke-AtomicTest` found the function. The T1059.001 YAML definition download did not persist: `Test-Path C:\AtomicRedTeam\atomics\T1059.001\T1059.001.yaml` returned `False`. A security product had previously blocked a different downloaded installer; the exact cause of the missing YAML file was **not established**.

Instead of disabling endpoint security, the operator reviewed and manually ran **the published command from [T1059.001 Test #17](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1059.001/T1059.001.md)**. It uses PowerShell's abbreviated encoded-command switch `-e` to print a greeting. Only the reviewed, harmless test was executed; no broad test collection was run.

## Observation and learning

1. First search for Sysmon Event ID 1 found the `-e` command at **2026-11-01 01:16:18.606**, on DC01.
2. The existing SPLINT-R-002 query matched `-EncodedCommand` and `-enc` but not `-e`, revealing a coverage gap.
3. After adding a third `like(lower(CommandLine), "% -e %")` clause, a manual search returned three events (two previous benign manual tests, one Atomic execution).
4. A later `-e` event at **01:47:09.122** also matched the revised query.
5. The triggered-alert history contained multiple scheduled alert records, but the screenshots do not conclusively tie the latest automatic trigger to the final `-e` event.

**What this demonstrates:** manually running an official Atomic Red Team test, Sysmon process-creation visibility, triage of command-line/parent process, identification and remediation of a detection logic gap. It is **not** evidence of a complete CALDERA simulation, a framework-driven `Invoke-AtomicTest` execution, or confirmed malicious compromise.

## Safety notes

Use a dedicated disposable endpoint for future simulations where possible; preserve a VM snapshot and keep Defender enabled. Review each test's effects, prerequisites, and cleanup before execution. Do not run an entire technique pack blindly on the domain controller.
