# CITATION.cff template

Place at the repo root. GitHub then shows a "Cite this repository" button and Zenodo reads it for archive metadata. Fill from the paper and repo. Keep UTF-8 (umlauts etc. are fine).

```yaml
cff-version: 1.2.0
message: "If you use this repository, please cite both the software and the paper below."
title: "<repo-name>: <short descriptive title>"
type: software
authors:
  - family-names: <Family>
    given-names: <Given>
    affiliation: "<Institution>"
  # ... one block per author, in paper order
license: <SPDX, e.g. BSD-3-Clause>
repository-code: "https://github.com/<org>/<repo>"
# After the Zenodo release, set the concept DOI:
# doi: 10.5281/zenodo.XXXXXXXX
preferred-citation:
  type: conference-paper
  title: "<paper title>"
  authors:
    - family-names: <Family>
      given-names: <Given>
  collection-title: "Proceedings of <venue>"
  year: <year>
```

Notes:
- Create it **before** the Zenodo release.
- If the paper is under review, still list it as `preferred-citation`; add the DOI/pages later.
- Authors here are the paper authors; affiliations come from the paper.
