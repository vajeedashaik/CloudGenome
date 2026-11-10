# Phase 2 — Docker, Security Scanning and Vulnerability Policy

**Goal:** Small secure image; all scanners run for real; written policy for handling findings. **Cost: free** (no registry push yet).

## Steps
1. **Dockerfile** (`docker/Dockerfile`): multi-stage, pinned slim base (`python:3.12-slim`), non-root user, no dev deps, `HEALTHCHECK` using Python (no curl), `EXPOSE 8000`, build args `APP_VERSION` and `GIT_SHA` → env, `PYTHONDONTWRITEBYTECODE`.
2. **`.dockerignore`:** `.git`, `.venv`, tests, caches, `.env`, `*.tfstate`, `*.pem`.
3. Build and run: `docker build -t cg-b-sample:dev --build-arg GIT_SHA=$(git rev-parse --short HEAD) .`; run mapped to host.
4. **Scans** (outputs saved under `security/reports/`, not edited by hand):
   - Source/dependency: `pip-audit -r requirements.txt`
   - Filesystem: `trivy fs .` (vuln + misconfig)
   - Secrets: `gitleaks detect` (or `trivy fs --scanners secret`)
   - Dockerfile: `trivy config docker/`
   - Image: `trivy image cg-b-sample:dev`
5. **Vulnerability policy** → `security/POLICY.md` (PROPOSED, confirm with team):
   - CRITICAL or HIGH with a fix available → **pipeline fails**.
   - HIGH/CRITICAL with no fix → allowed only via `.trivyignore` entry containing CVE, reason, owner, **expiry date**.
   - MEDIUM/LOW → reported, not blocking.
   - Secrets found → always fail.
   - Exceptions reviewed in PR.
6. **Triage:** for each HIGH/CRITICAL record package, version, CVE, fix available, action (upgrade / accept with reason / not exploitable). Fix what can be fixed (bump base image or dependency) and **re-scan for before/after evidence**.
7. Write `security/FINDINGS.md` using only real scan output. If a scan finds nothing, say so; do not invent findings.
8. (Optional demo) Intentionally add a known-vulnerable pinned dependency on a throwaway branch to show the gate failing; never merge it.

## Acceptance tests
- [ ] `docker build` succeeds from clean cache (`--no-cache`).
- [ ] Container runs; `/health`, `/ready`, `/version`, `/metrics` work via host port.
- [ ] `docker exec <c> id` shows non-root.
- [ ] `docker image ls` size recorded (real number).
- [ ] `/version` shows the injected commit SHA.
- [ ] All five scans ran; raw reports saved.
- [ ] `FINDINGS.md` matches raw reports (spot-check three entries).
- [ ] Policy exit codes verified: `trivy image --severity HIGH,CRITICAL --ignore-unfixed --exit-code 1` returns 0 when clean and 1 on the demo-vulnerable image.
- [ ] Before/after shows applied fixes.

## Common problems
- Trivy DB download slow/blocked — retry, note if offline.
- `--ignore-unfixed` hides unfixable CVEs — document that this is the policy, not an accident.
- Alpine/musl wheel issues — prefer slim.
- Docker Desktop not running.

## Document
Dockerfile rationale, scan commands, findings table, policy, fixed vs accepted and why.
