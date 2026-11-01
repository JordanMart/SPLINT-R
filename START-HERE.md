# Publish checklist — SPLINT-R, Nov 1, 2026

1. Review `README.md` and `docs/VALIDATION-LOG.md` for accurate status.
2. Review all six screenshots in `evidence/screenshots/` and remove any sensitive data. Images may display usernames, internal paths and IP addresses. No passwords or authentication material should be uploaded.
3. Create a new repository named `SPLINT-R` (public only if comfortable with the sanitized content). Do not initialize with an extra README if uploading this complete folder via Git.
4. From the extracted project root, run: `git init`, `git add .`, `git commit -m "Initial SPLINT-R lab documentation"`, `git branch -M main`, `git remote add origin https://github.com/YOUR_HANDLE/SPLINT-R.git`, `git push -u origin main`. Replace YOUR_HANDLE. Git must be installed and authenticated. See `docs/PUBLISHING.md` for alternatives.
5. In repository settings, add a short description and topics: `splunk`, `sysmon`, `detection-engineering`, `atomic-red-team`, `powershell`, `soc-lab`.
6. Ensure the README shows in the repo landing page and every relative screenshot and detection link works.

The repository is intentionally honest about missing integrations. Future components remain in `docs/ROADMAP.md`. Only the official Atomic test **manual execution** was validated; `Invoke-AtomicTest` did not successfully execute it.

## Browser-only first publication (fastest)

1. Create `SPLINT-R` at https://github.com/new; choose public only after reviewing contents, and leave GitHub README initialization unchecked.
2. Extract this ZIP on your PC. Inside the extracted `SPLINT-R` folder, select the contents (`README.md`, `docs/`, `detections/`, `evidence/`, `config/`, `.gitignore`, etc.), not the enclosing folder or ZIP file.
3. Use **uploading an existing file** on the empty repo (or **Add file → Upload files**), drag the selected files/folders to GitHub, and commit. Verify `README.md` appears at the repository root.
4. Inspect the architecture diagram and the evidence gallery on the published repository. Do not publish unredacted screenshots or any secrets.
5. Only use the verified completion status in this repository; do not claim framework-executed Atomic tests or unbuilt integrations.
