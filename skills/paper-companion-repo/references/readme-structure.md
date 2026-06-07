# README structure and conventions

Write the README in this order. Omit sections that do not apply. Verify every path and identifier against the actual repository.

## 1. Title and tagline
- `# <repo-name>`
- One bold tagline sentence. Match the paper title's wording and number (singular/plural).

## 2. Badges and archive line
```
[![License: <SPDX>](https://img.shields.io/badge/License-<SPDX>-blue.svg)](./LICENSE) [![DOI](https://img.shields.io/badge/DOI-<encoded-doi>-blue)](https://doi.org/<doi>)

*Repository archived on Zenodo — DOI: [<doi>](https://doi.org/<doi>)*
```
Use shields.io for the DOI image (Zenodo's own badge often returns an empty SVG and renders broken on GitHub). URL-encode the slash in the DOI as `%2F`.

## 3. Intro paragraph
"This repository is the open-source companion to the paper *\"<title>\"* ...". State the venue. Handle status honestly:
- Published: cite normally with year/DOI.
- Submitted / under review: say "submitted for review to <venue> <year>" and link a pre-print if one exists. Never imply it is published.

## 4. Overview
Motivation (the problem), what the project does, and the named library/tool it introduces. Keep to a few short paragraphs.

## 5. Workflow at a glance
A Mermaid `flowchart LR` of the pipeline. For an eFMI use case: Modelica models → eFMI synthesis (MISRA C:2023 / SEI CERT C code) → MiL/SiL tests → STM32 integration → deploy to boards → CHiL test. Keep node labels consistent with the prose (don't say "MISRA-C" in the diagram and "MISRA C:2023" in the text).

## 6. Repository size warning (only if `.git` is large)
If history or checkout is hundreds of MB (often because reproducibility artifacts are intentionally committed), add a `> [!WARNING]` callout that: states the size and why; gives a shallow clone that skips history and names a short target dir; and warns Windows users about the 260-char `MAX_PATH` limit.
```
> [!WARNING]
> **This is a large repository (~<X> MB history, ~<Y> MB checked out).** By design it ships generated reproducibility artifacts ... If you only want to read and run the models, clone a shallow snapshot:
>
> ```bash
> git clone --depth 1 --recurse-submodules --shallow-submodules \
>   https://github.com/<org>/<repo>.git C:/dev/<repo>
> ```
>
> **Windows users:** clone into a short root such as `C:\dev\`; the eFMI build creates deeply nested paths that otherwise hit the Windows 260-char MAX_PATH limit.
```

## 7. Repository structure
A two-column table (`Path | Contents`) of top-level folders. Then, for a Modelica library, a short bullet list of its sub-packages read from `package.order`.

## 8. Requirements
Three groups:
- **Software** — the Modelica tool and exact version tested (e.g. Dymola 2026x Refresh 1), the eFMI/Embedded toolchain and which license enables local generation, dependency libraries with versions (from the library's `uses(...)` annotation), Java/JDK (state the library's minimum and the version you tested with; note it is only needed when generating), STM32CubeMX/CubeIDE versions, MATLAB.
- **Hardware** — boards with their roles (which board is plant, which is controller) and a suggested data-recording instrument (e.g. Digilent Analog Discovery 3) with a note that any comparable scope/DAQ works.
- **Optional — reproducing the code-quality checks** — Cppcheck (Premium for MISRA C:2023), Python 3 + the eFMPy wheel, .NET 4.8. Mark as needed only to reproduce analyses, not to build/run.

Insert the hardware/setup figure (an SVG/PNG that exists in the repo `docs/`) under Hardware.

## 9. Getting started
- Clone with submodules (and point to the shallow option).
- Load the models: Option A (the repo's one-click launcher, if present) and Option B (manual open of the dependency + library `package.mo`; set the in-repo `work` folder as the Dymola working directory to keep paths short). Add a `[!NOTE]` that regenerating overwrites shipped artifacts and they can `git restore` them.
- Generate an eFMU (point at the eFMU configuration packages), with a `[!NOTE]` that without the generation license they can just inspect the shipped artifacts.
- Run the tests: MiL / SiL / CHiL.

## 10. Troubleshooting
Prefer pointing to authoritative in-tool docs (e.g. `DymolaEmbedded.UsersGuide.Requirements`, openable in Dymola). If you link vendor help pages, check they are public — many 3DS help URLs redirect to a login; flag those as login-gated.

## 11. How to cite
Lead with the status (under review + pre-print link if applicable). Give a prose citation and a BibTeX entry. For under-review work use `note = {Under review. Pre-print: <doi>}` and a `% TODO` for pages/DOI. Add a line offering the Zenodo DOI to cite the repository itself.

## 12. License
One line: SPDX, copyright holders and years, link to `LICENSE`.

## 13. Authors and acknowledgments
Authors with affiliations (from the paper). Then acknowledgments: funding programs, platform/tool access grants, and gifts — name the people/programs the user specifies (e.g. a champions program contact, a research-lab gift that funded hardware). Acknowledgments usually omit dollar amounts.
