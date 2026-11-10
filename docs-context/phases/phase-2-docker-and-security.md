# Phase 2 — Docker and Security Scanning

**Goal:** A small, secure container image that runs the Phase 1 service, plus real scan results with documented findings. **Cost: free** (local only; no registry push yet).

## Steps
1. **Dockerfile:** multi-stage, slim pinned base (e.g. `python:3.12-slim`), non-root user, no dev dependencies, `HEALTHCHECK`, vault and DB on volumes (`/data`), `EXPOSE 8000`, env-driven config.
2. **`.dockerignore`:** `.git`, `.venv`, tests caches, `*.db`, secrets.
3. **Build and run locally:** `docker build -t cloudgenome-a:dev .` then run with a mounted vault volume.
4. **docker compose (optional):** one service + volume for easy demo.
5. **Trivy scans** (all real, outputs saved to `security/`):
   - image scan (`trivy image`)
   - filesystem/dependency scan (`trivy fs`)
   - config scan of the Dockerfile (`trivy config`)
   - `pip-audit` on requirements
6. **Triage findings:** for each HIGH/CRITICAL, record: package, version, CVE, fix available?, action (upgrade / accept with reason / not exploitable). Fix what can be fixed (bump the base image or dependency) and **re-scan to show the before/after**.
7. Write `security/FINDINGS.md` using only real scan output.

## Acceptance tests
- [ ] `docker build` succeeds from a clean cache.
- [ ] Container runs; `/health`, `/ready`, `/metrics` and search work from the host via the mapped port.
- [ ] `docker exec ... id` shows a non-root user.
- [ ] Data survives container restart (volume test).
- [ ] Image size recorded (aim for under ~250 MB; report the real number).
- [ ] Trivy image, fs and config scans ran; raw reports saved.
- [ ] `FINDINGS.md` matches the raw reports (spot-check three entries).
- [ ] Before/after scan shows any fixes actually applied.

## Common problems
- Docker Desktop not running / WSL2 backend off.
- Trivy DB download blocked or slow — retry; mention if offline.
- Alpine/musl vs slim/glibc wheel issues — prefer slim.
- Healthcheck needs `curl` or Python-based check; do not add heavy tools just for this.

## Document
Dockerfile rationale (multi-stage, non-root), scan commands, findings table, what was fixed vs accepted and why.
