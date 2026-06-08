---
name: paper-companion-repo
description: >-
  Build or refresh the open-source companion repository for a research paper — tuned for ALSETLab Modelica/Dymola/OpenIPSL/eFMI projects. Use when the user wants to write or update a repository README from a paper, create a CITATION.cff, link the repo to Zenodo and add a DOI badge, or produce a propose-first repo cleanup before a release. Trigger phrases include: "paper companion repo", "write a README for my paper's code", "README for this Modelica repo", "make my repo citable", "set up a Zenodo DOI", "add a DOI badge", "clean up the repo for release", "prep this repo for the paper".
---

# Paper-companion repository

Turn a research paper plus its code repository into a polished, citable, archived open-source companion. Four capabilities, run individually or end to end:

1. Write the repository README from the paper and the repo.
2. Create a `CITATION.cff`.
3. Guide the Zenodo release and add a DOI badge.
4. Produce a propose-first repo cleanup.

This skill is tuned for ALSETLab power-systems projects (Modelica, Dymola, eFMI, OpenIPSL), but the structure generalizes. Keep the user's own conventions; do not genericize tool names.

## Before you start

Establish two inputs, asking only for what you cannot find:

- **The paper** — a LaTeX source (`root.tex` + `sections/`) or a PDF. Read it directly. Extract: exact title, full author list with affiliations, the venue and whether it is published / submitted / under review, the abstract and core contribution, the named library or tool the paper introduces, the hardware, and the methods/validation steps.
- **The repository** — the local clone path. If you do not have access, request it (the user usually has it cloned via a desktop Git client). Map the tree with `Glob`/bash `find`; read `.gitmodules` for submodules; check `.git` size for the size warning.

Then confirm which of the four steps to run. Default to the README.

**Source of truth:** the paper and the *current* repository. Treat meeting notes, drafts, and old READMEs as background only — never override the paper or repo with them.

## Step 1 — Write the README

Read `references/readme-structure.md`. It is organized as a **Core** template (every Modelica library / research repo) plus an **eFMI / embedded add-on**. First decide the project profile, then assemble only the applicable sections — do not force eFMI, hardware, or code-generation sections onto a pure Modelica library.

Process:

1. Read the paper; pull the facts listed above.
2. Map the repo: top-level folders, the Modelica library sub-packages (read `package.order`/`package.mo`), submodules, and `.git` size.
3. Cross-reference: describe only what the paper and current repo actually contain. Verify every path, package name, and identifier against the repo before writing it — do not carry stale names from an old README. Open example/model files to confirm they are real, not empty stubs.
4. Choose the profile — Core only vs Core + eFMI add-on — from the paper and repo: eFMU configurations, an `STM32/` folder, or hardware boards signal the eFMI add-on; otherwise Core only. Then write the README section by section per the applicable profile, using GitHub callouts (`[!WARNING]`, `[!NOTE]`) and a Mermaid `flowchart` for the workflow/concept diagram.
5. If a requirement depends on a library you can read locally (e.g. `DymolaEmbedded.UsersGuide.Requirements`), read it rather than guessing — it is the authoritative source.
6. Save the README into the repo, then show it with the file-presentation tool.

## Step 2 — CITATION.cff

Read `references/citation-cff.md`. Fill authors and affiliations from the paper, license from the repo `LICENSE`, the repository URL, a `preferred-citation` pointing at the paper, and the Zenodo concept `doi` once available. Creating it before the Zenodo release lets Zenodo read correct metadata instead of guessing from commits.

## Step 3 — Zenodo release and DOI badge

Read `references/zenodo-doi.md`. Most actions happen in the browser, so guide the user step by step. The one critical rule: connect Zenodo and flip the repo's switch **before** publishing the GitHub release. Use the **concept** DOI, and render the badge from **shields.io** (Zenodo's own badge image is frequently broken). After the user supplies the DOI, write the badge into the README and the `doi:` into `CITATION.cff`.

## Step 4 — Repo cleanup proposal

Read `references/repo-cleanup.md`. Respect the curated `.gitignore` (maintainers often deliberately keep large generated artifacts for reproducibility). Reference-check candidate-cruft packages before flagging them. Sort findings into "clear-cut" vs "verify with the team". Note that deleting files does not shrink `.git` history (a rewrite is a separate, sign-off-required operation). Write everything into `CLEANUP_PROPOSAL.md` and change nothing until the user approves.

## Working style

- Edit files directly in the user's repo; present results with the file-presentation tool; keep prose concise.
- The user may not be comfortable with Git. Let them commit in their own client. If a commit fails with `index.lock` or "index file corrupt", explain plainly that a desktop Git client and the sandbox Git use different index formats, and offer to clear the stale `index.lock` / regenerable graph caches — never anything that touches file contents.
- Never delete repository files without explicit approval. When deletion is approved and the sandbox refuses with "Operation not permitted", request delete permission rather than declaring it impossible.
- Always finish a step by verifying identifiers and links against the repo and the live web where relevant.
