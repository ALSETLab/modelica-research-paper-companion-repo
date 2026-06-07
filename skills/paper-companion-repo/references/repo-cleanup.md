# Repo cleanup proposal

Produce a reviewed keep/remove plan before a release. Change nothing until the user (and, where relevant, co-authors) approve. Deliver it as `CLEANUP_PROPOSAL.md` at the repo root.

## Method
1. **Map the repo** with bash `find` (exclude `.git` and submodules). Note sizes of the big folders and the total `.git` size.
2. **Read `.gitignore` carefully.** It is often deliberately curated — maintainers may keep large generated artifacts (eFMUs, SiL FMUs, batch result `.mat`/`.csv`) on purpose for reproducibility, via `!`-negation patterns. Those are NOT cruft. Call this out explicitly and leave them alone.
3. **Cross-reference the paper.** Only the models/configs/boards the paper actually uses are "in scope". Abandoned board variants, superseded namespaces, and empty stub packages are candidates.
4. **Reference-check candidates before flagging.** Grep the source tree for each candidate package name. If every reference is internal to the candidate itself (nothing on the paper's model path points in), it is safe to remove. If it is woven into live packages, it is NOT a quick delete.

## Tiers in the proposal
- **Clear-cut (safe to remove):** isolated scratch/`UnderDevelopment` packages, empty stub packages, leftover build directories that are not curated keeps.
- **Verify with the team:** duplicates of canonical packages, deeply-referenced extra variants, abandoned-board firmware projects, large vendor PDFs.

## Size note (always include)
Deleting files now does **not** shrink `.git` — the blobs remain in history. Reclaiming space needs a history rewrite (e.g. `git filter-repo`) plus a force-push that rewrites every collaborator's clone. Flag it as a separate, sign-off-required operation; do not do it casually.

## Execution
- Propose first; get approval per item.
- Do removals on a branch with ordinary `git rm`, fix any affected `package.order` files, and show the diff before pushing.
- If the sandbox refuses to delete ("Operation not permitted"), request delete permission rather than giving up. Mind stale `index.lock` from desktop Git clients.
