# README structure and conventions

First decide the **profile**, then assemble only the applicable sections. Verify every path and identifier against the actual repository.

## Choose the profile

- **Core** — applies to *every* Modelica library / research-code repo. Use this on its own for a pure library (e.g. a component, interface, or utilities library).
- **eFMI / embedded add-on** — add the extra sections in the second half *only* when the project does eFMI code generation and/or hardware deployment.

Decide from the paper and repo: if you see eFMU generation configurations (`DymolaEmbedded.EmbeddedConfiguration`), an `STM32/` (or similar firmware) folder, or hardware boards in the paper, use Core **+** the eFMI add-on. Otherwise use Core only and skip everything eFMI/hardware-specific. The OpenIWPI library, for example, is Core-only.

---

# Core sections (every project)

## 1. Title and tagline
- `# <repo-name>`
- One bold tagline sentence. Match the paper title's wording and number (singular/plural).

## 2. Badges and archive line
```
[![License: <SPDX>](https://img.shields.io/badge/License-<SPDX>-blue.svg)](./LICENSE) [![DOI](https://img.shields.io/badge/DOI-<encoded-doi>-blue)](https://doi.org/<doi>)

*Repository archived on Zenodo — DOI: [<doi>](https://doi.org/<doi>)*
```
Use shields.io for the DOI image (Zenodo's own badge often returns an empty SVG and renders broken on GitHub). URL-encode the slash in the DOI as `%2F`. Omit the DOI badge/line until a Zenodo DOI exists.

## 3. Intro paragraph
"This repository is the open-source companion to the paper *\"<title>\"* ...". State the venue. Handle status honestly:
- Published: cite normally with year and, if known, venue/DOI.
- Submitted / under review: say "submitted for review to <venue> <year>" and link a pre-print if one exists. Never imply it is published.

## 4. Overview
Motivation (the problem), what the project does, and the named library/tool it introduces. A few short paragraphs.

## 5. Workflow / concept diagram
A Mermaid `flowchart` of what the library/tool does. For a library, draw the conceptual data/role flow (e.g. for OpenIWPI: phasor grid <-> wave-phasor interface bus <-> EMT subsystem). For eFMI projects, see the add-on for the synthesis-to-deployment pipeline. Keep node labels consistent with the prose.

## 6. Repository structure
A two-column table (`Path | Contents`) of top-level folders. Then, for a Modelica library, a bullet list of its sub-packages read from `package.order` with one-line descriptions.

## 7. Requirements
- **A Modelica tool** and the exact version tested (e.g. Dymola 2025x / 2026x). Note if other tools are untested.
- **Dependency libraries** with versions — read the library's `uses(...)` annotation in `package.mo`, and the paper, for the exact versions (e.g. OpenIPSL 3.1.0, MSL 4.0.0, Complex 4.0.0). Mention required load order if the library depends on others.
- **MATLAB** etc. only if the repo actually uses them.
- Optional analysis tools (e.g. `Modelica_LinearSystems2` for linearization) as a sub-bullet.

## 8. Getting started
- Clone (add `--recurse-submodules` only if `.gitmodules` exists).
- Load order: open the dependencies first, then the library's `package.mo`.
- Initialize / run: for power-system libraries, a `[!NOTE]` about power-flow initialization is often warranted (check the library's own UsersGuide). Then point at the runnable examples — `Library.Examples.*` — and describe what each shows. Flag any example that is a stub / pending integration.

## 9. How to cite
Status-aware: lead with publication status (and pre-print link if under review). Give a prose citation and a BibTeX entry. For under-review work use `note = {Under review. Pre-print: <doi>}` and a `% TODO` for pages/DOI. Add a line offering the Zenodo DOI to cite the repository itself once archived.

## 10. License
One line: SPDX, copyright holders and years (from `LICENSE`), link to `LICENSE`.

## 11. Authors and acknowledgments
Authors with affiliations (from the paper). Then acknowledgments: funding programs, platform/tool access grants, and gifts — name the people/programs the user specifies. Acknowledgments usually omit dollar amounts. Omit this section if the paper lists none.

---

# eFMI / embedded add-on (only for eFMI + hardware projects)

Layer these on top of the Core sections.

## Workflow diagram (replaces the generic one)
The eFMI pipeline, e.g.: Modelica models -> eFMI synthesis (MISRA C:2023 / SEI CERT C code) -> MiL/SiL tests -> STM32 integration -> deploy to boards -> CHiL test. Keep label wording identical to the prose (don't write "MISRA-C" in the diagram and "MISRA C:2023" in the text).

## Repository-size warning
eFMI repos often deliberately commit large generated reproducibility artifacts, so `.git`/checkout can be hundreds of MB. When it is, add a `> [!WARNING]` callout: state the size and why; give a shallow clone that skips history into a short target dir; and warn Windows users about the 260-char `MAX_PATH` limit.
```
> [!WARNING]
> **This is a large repository (~<X> MB history, ~<Y> MB checked out).** By design it ships generated reproducibility artifacts ...
>
> ```bash
> git clone --depth 1 --recurse-submodules --shallow-submodules \
>   https://github.com/<org>/<repo>.git C:/dev/<repo>
> ```
>
> **Windows users:** clone into a short root such as `C:\dev\`; the eFMI build creates deeply nested paths that otherwise hit the Windows 260-char MAX_PATH limit.
```
(For a small pure library this warning does not apply — skip it.)

## Requirements additions
- The **eFMI / Embedded** toolchain and which license enables local generation (Dymola **Source Code Generation** license runs it locally; 3DEXPERIENCE / SOP is the optional online alternative).
- **Java / JDK** — read `DymolaEmbedded.UsersGuide.Requirements` for the minimum (it powers Software Production Engineering, used in both local and online modes); state the version tested; note it is needed only when generating eFMUs, not to inspect shipped artifacts.
- **STM32CubeMX / STM32CubeIDE** with tested versions.
- **Hardware** group: boards with their roles (which is plant, which is controller) and a suggested data-recording instrument (e.g. Digilent Analog Discovery 3, "any comparable scope/DAQ works"). Insert the setup figure from the repo `docs/`.
- **Optional — reproducing the code-quality checks**: Cppcheck (Premium for MISRA C:2023), Python 3 + the eFMPy wheel, .NET 4.8 — needed only to reproduce analyses.

## Getting started additions
- Generate an eFMU (point at the eFMU configuration packages), with a `[!NOTE]` that without the generation license users can just inspect the shipped artifacts.
- Set the in-repo `work` folder as the Dymola working directory (keeps paths short); `[!NOTE]` that regenerating overwrites shipped artifacts and they can `git restore` them.
- Run the tests: MiL / SiL / CHiL.

## Troubleshooting
Prefer pointing to authoritative in-tool docs (`DymolaEmbedded.UsersGuide.Requirements`, openable in Dymola). If you link vendor help pages, check they are public — many 3DS help URLs redirect to a login; flag those as login-gated.
