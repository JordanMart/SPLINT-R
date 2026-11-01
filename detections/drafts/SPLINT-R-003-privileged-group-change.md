# SPLINT-R-002: Privileged Group Membership Change — DRAFT / NOT TESTED

**Hypothesis:** Adding an account to a privileged group outside an approved change may indicate unauthorized persistence or privilege escalation.

- **Source:** Windows Security events `4728`, `4732`, `4756` (member added to different security group scopes).
- **ATT&CK:** [T1098.007](https://attack.mitre.org/techniques/T1098/007/).
- **Query:** [Draft SPL](SPLINT-R-002-privileged-group-change.spl). This is a broad discovery query, **not yet a high-fidelity privileged-group rule**. Group-name fields and event structure must be verified and restricted to privileged groups before alerting.
- **Validation plan:** Create a disposable lab account and a disposable privileged *test* group; make and observe an authorized membership change; inspect event records. If testing a privileged built-in group, use a temporary lab-only account and promptly revert; never use company systems.
- **False positives:** Approved administration, provisioning automation, group-management tasks.
- **Next improvements:** Normalize actor/member/group fields; constrain to a configured watchlist of privileged groups; record approval context; add an investigation checklist.
