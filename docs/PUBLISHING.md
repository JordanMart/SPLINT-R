# Publish to GitHub (Windows-friendly)

## Before publishing

- Ensure all code/screenshots/log samples are your own and permitted for public sharing.
- Review `.gitignore` **and every staged file** for credentials, hostnames, personal data, company data, auth tokens, and email addresses. `.gitignore` does not protect secrets already committed.
- Verify the README status table and remove claims that have not actually been demonstrated.

## Option A — GitHub web + Git command line

1. Sign in to [GitHub](https://github.com/) and create a new **public** repository named `SPLINT-R`. Suggested description: `Self-hosted VMware + Splunk SOC lab focused on Windows telemetry, detection engineering, MITRE ATT&CK, and alert validation.` Do not create a second README or `.gitignore` on GitHub if you're pushing this folder.
2. Extract the ZIP, open Windows Terminal / PowerShell in the `SPLINT-R` directory, then execute:

```powershell
git init
git branch -M main
git add .
git status   # Read this output; inspect staged files before committing.
git commit -m "Initial SPLINT-R SOC lab documentation and detection baseline"
git remote add origin https://github.com/YOUR-USERNAME/SPLINT-R.git
git push -u origin main
```

Replace `YOUR-USERNAME` with your real GitHub username. GitHub may ask you to authenticate through your normal Git credential workflow. Do not paste authentication tokens into your source files.

3. View the GitHub README and Mermaid architecture diagram; confirm all relative links work.
4. Turn on available **Secret scanning** and **Push protection** in the repository security settings. See [GitHub's guidance](https://docs.github.com/en/code-security/getting-started/quickstart-for-securing-your-repository).
5. Pin the project on your GitHub profile and link it from your resume / LinkedIn **after** you have evidence/screenshots.

## Option B — GitHub Desktop

Open the extracted project in GitHub Desktop (File → Add local repository, creating a repository from the folder if prompted); review changes, commit, and choose **Publish repository**. Ensure the selected visibility is Public when ready. Verify everything on the website.

## Future update cycle

After each lab test, update the SPL, status table, validation log and redacted screenshots; `git add .`, inspect `git diff --cached`, commit with a descriptive message, and push. Keep draft rules clearly separated from validated ones.
