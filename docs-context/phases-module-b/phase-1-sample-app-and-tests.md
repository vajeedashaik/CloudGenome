# Phase 1 — Sample App and Automated Tests

**Goal:** A small, honest service that the pipeline can build, test, ship and deploy. Independent of Modules A and C. **Cost: free.**

## Why a sample app
The pipeline is the product here. The app only needs to make deploy, health and rollback **visible**: `/version` shows which build is running.

## Steps
1. `app/` FastAPI service (Python 3.12), pinned `requirements.txt` (runtime) and `requirements-dev.txt` (pytest, httpx, coverage).
2. Endpoints:
   - `GET /health` → liveness, `{"status":"ok"}`
   - `GET /ready` → readiness (200 when app initialised)
   - `GET /version` → `{"version": APP_VERSION, "commit": GIT_SHA}` from env
   - `GET /` → small JSON hello
   - `GET /metrics` → Prometheus format (request count, latency histogram, build-info gauge)
3. Config only from env: `PORT` (default 8000), `APP_VERSION`, `GIT_SHA`, `LOG_LEVEL`. No secrets in code.
4. Structured JSON logs to stdout.
5. Tests (pytest): each endpoint, version env injection, metrics contains expected series, readiness behaviour, one negative test (404).
6. Coverage report; target a sensible threshold (e.g. ≥80%), report the real number.
7. Add a `Makefile` or `scripts/` helper for `run`, `test`.
8. Optional intentionally-failing branch/test (kept out of main) to demo the pipeline blocking a bad change in the viva.

## Acceptance tests
- [ ] `pip install -r requirements-dev.txt` in a fresh venv succeeds.
- [ ] `pytest -q` passes; paste real result.
- [ ] Coverage number recorded.
- [ ] `uvicorn` run: `curl localhost:8000/health`, `/ready`, `/version`, `/metrics` return expected output.
- [ ] Changing `APP_VERSION=1.2.3` changes `/version` output.
- [ ] A deliberately broken test fails the run (proves tests are real), then reverted.
- [ ] No secrets or `.env` in `git status`.

## Common problems
- Python version mismatch on Windows — use venv with 3.12.
- Metrics duplicated registration when reloading — keep one registry.

## Document
Endpoint table, how to run and test, why these endpoints (probes + monitoring + rollback visibility).
