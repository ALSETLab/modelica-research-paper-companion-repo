# Zenodo release and DOI badge

Goal: archive a GitHub release on Zenodo, mint a DOI, and show a DOI badge in the README. Most steps are browser actions by the user — guide them clearly.

## The one rule people get wrong
Zenodo only archives releases created **after** its switch is turned on for the repo. So: connect Zenodo and flip the switch **before** publishing the GitHub release.

## Steps
0. **Commit `CITATION.cff` first** so Zenodo reads correct authors/title instead of guessing from commits.
1. **Connect Zenodo to GitHub** — log in at zenodo.org with GitHub and authorize (first time only).
2. **Flip the switch** — at zenodo.org/account/settings/github, toggle the repo ON. If it is missing, "Sync now"; for org repos, the org may need to approve the Zenodo OAuth app.
3. **Create the GitHub release** — Releases → Draft a new release → new tag (e.g. `v1.0.0`), title, notes → Publish.
4. **Wait** a minute or two for Zenodo to ingest.
5. **Grab the concept DOI** — Zenodo shows a version DOI and a "Cite all versions" **concept** DOI. Use the **concept** one so the badge always tracks the latest release. Copy its Markdown snippet.

## Badge: use shields.io, not Zenodo's image
Zenodo's `zenodo.org/badge/<id>.svg` endpoint frequently returns an empty/broken image, and GitHub's image proxy then caches the broken version. Render the badge from shields.io instead, keeping the DOI link:
```
[![DOI](https://img.shields.io/badge/DOI-<encoded-doi>-blue)](https://doi.org/<doi>)
```
Encode the slash in the DOI as `%2F` (e.g. `10.5281%2Fzenodo.20583549`).

## After the DOI exists
- Add the badge line near the title and a "Repository archived on Zenodo — DOI: ..." line.
- Fill `doi: <concept-doi>` in `CITATION.cff`.
- Because the badge points at the concept DOI, you never redo this for later releases — the same badge auto-resolves to the newest version.
