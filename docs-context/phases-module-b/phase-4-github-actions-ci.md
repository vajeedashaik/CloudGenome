# Phase 4 — GitHub Actions CI (no AWS yet)

**Goal:** Every push and PR runs test → scan → build → image scan, with policy enforced. **Cost: free** (GitHub-hosted runner minutes; check your plan's allowance).

## Steps
1. `.github/workflows/` is a repo-root path. **Ask the team before adding it** (shared root area). If using `module-b-devsecops/` paths only, put workflow at `.github/workflows/module-b-ci.yml` with `paths:` filter `module-b-devsecops/**` so it does not run for others' changes.
2. Triggers: `push` (branches `vajeeda-devsecops`, `main`) and `pull_request` to `main`, filtered by path. Set `permissions: contents: read` at top; widen per-job only where needed.
3. Jobs:
   - `test`: setup Python, cache pip, install, `pytest --cov`, upload coverage artifact.
   - `source-scan`: `pip-audit`, `gitleaks`, `trivy fs` (fail per policy).
   - `build`: Docker Buildx build (no push), tag `<sha>`, load image.
   - `image-scan`: Trivy on the built image, fail on policy, upload SARIF/JSON report as artifact (and to Security tab if available).
   - `lint-iac` (placeholder until Phase 5): `trivy config` on Dockerfile/k8s.
4. Pin third-party actions to a version (ideally full commit SHA) and note why.
5. Branch protection: require the CI checks to pass before merge.
6. Demonstrate three runs and keep links/screenshots:
   - green run
   - failing test blocks merge
   - vulnerable dependency/image blocks merge
7. Optionally Dependabot config for pip/docker/actions (propose first).

## Acceptance tests
- [ ] Workflow visible in Actions tab, triggered by push *and* PR.
- [ ] Green run on clean branch (link recorded).
- [ ] Red run from intentional test failure; no image published.
- [ ] Red run from intentional vulnerable dependency; job message shows the CVE.
- [ ] Scan reports downloadable as artifacts.
- [ ] No secrets in workflow files; `permissions` minimal.
- [ ] Path filter works: a change under another module does not trigger Module B CI.
- [ ] Run time recorded (real).

## Common problems
- Windows line endings in shell scripts — set `.gitattributes` `eol=lf` for `*.sh` (propose; root file).
- Trivy rate-limited DB pulls — cache DB or retry.
- Workflow YAML not picked up — must be under repo-root `.github/workflows/`.
- Forked-PR secrets unavailable — irrelevant here (same-repo branches).

## Document
Workflow diagram, job purposes, policy mapping to exit codes, run links.
