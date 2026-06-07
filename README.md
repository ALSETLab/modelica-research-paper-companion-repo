# modelica-research-paper-companion-repo

A Cowork plugin that turns a research paper and its code repository into a polished, citable, archived open-source companion — tuned for ALSETLab Modelica / Dymola / OpenIPSL / eFMI projects.

## What it does

One skill, `paper-companion-repo`, covering four steps (run individually or end to end):

1. **Write the README** from the paper and the repo — overview, workflow diagram, structure table, requirements, getting-started, citation, acknowledgments, with the repo-size + Windows MAX_PATH warnings and a shields.io DOI badge.
2. **Create `CITATION.cff`** with the paper authors, license, and a preferred-citation.
3. **Guide the Zenodo release and DOI badge** — correct ordering, concept DOI, and the shields.io workaround for Zenodo's broken badge image.
4. **Produce a propose-first repo cleanup** — respects curated `.gitignore` keeps, reference-checks candidates, and never deletes without approval.

## How to use

Open a repository and a paper (LaTeX source or PDF), then ask, e.g.:

- "Write a README for this paper's code repository."
- "Make this repo citable — CITATION.cff and a Zenodo DOI."
- "Propose a cleanup of this repo before the release."

The skill reads the paper and repo, then works section by section. It edits files directly in the repository and leaves committing to your own Git client.

## Notes

- Treats the paper and the current repo as the source of truth; meeting notes and old READMEs are background only.
- Reads authoritative in-library docs (e.g. `DymolaEmbedded.UsersGuide.Requirements`) rather than guessing requirements.
