# Security and disclosure policy

This is a **home laboratory** portfolio repository; it is not a production defensive product and its detection content has no warranty of coverage.

- Do not commit credentials, Gmail App Passwords, auth tokens, Splunk internal passwords, SSH private keys, certificates with private keys, browser cookies, `.env`, machine snapshots, production event logs, or personal/company data.
- Sanitize screenshots: personal email addresses, real user names, sensitive directory entries, tokens, public IPs that could identify you, and any exposed Splunk authentication configuration.
- Local private RFC1918 example IPs in this repository describe an isolated lab only.
- Use synthetic testing and authorized systems only. Do not expose the lab's Splunk interface or DC to the public Internet.
- GitHub push protection helps but **is not a replacement** for manual review. If a secret is accidentally published, revoke/rotate it; simply deleting the latest copy is not sufficient because git history retains it.
- For potential safety or security issues in this example repository, open an issue without posting exploit details, sensitive data, or live secrets.
