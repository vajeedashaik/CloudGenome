# PROGRESS — Module A (Aryan)

Update at the end of **every** task, phase and session. Status labels: `NOT STARTED` · `IN PROGRESS` · `DONE (verified)` · `BLOCKED`.

**Current phase:** 0 — Setup
**Last updated:** 2026-11-10 (Phase 0 session)
**AWS resources live:** none

## Phase tracker

| # | Phase | Status | Verified on | Notes |
|---|---|---|---|---|
| 0 | Setup | IN PROGRESS | — | Local work done; waiting on Aryan (trivy, ansible, Python PATH, AWS, decisions) |
| 1 | Second Brain service + sample workload | NOT STARTED | — | |
| 2 | Docker + Trivy | NOT STARTED | — | |
| 3 | Local Kubernetes | NOT STARTED | — | |
| 4 | Terraform + Ansible on AWS | NOT STARTED | — | Costs money |
| 5 | CI/CD, monitoring, teardown | NOT STARTED | — | Costs money |
| 6 | Viva pack + integration contract | NOT STARTED | — | |

## Decisions log

| Date | Decision | By |
|---|---|---|
| 2026-11-10 | Build in 7 phases (0–6); free local phases before AWS | Aryan/Claude |
| 2026-11-10 | Obsidian is used as the Second Brain product feature | Aryan |

## Phase 0 tool report (checked 2026-11-10, read-only)
| Tool | Result |
|---|---|
| git | 2.52.0.windows.1 |
| docker | 29.3.1; daemon responds (`docker info`) |
| kubectl | client v1.34.1 (Docker Desktop); no kube contexts configured |
| terraform | v1.17.0 |
| aws | aws-cli/2.34.30 |
| gh | 2.98.0 |
| node | v24.12.0 |
| python | `python` on PATH is a Microsoft Store stub (does NOT run). `py -3.13` works = Python 3.13.12 (also uv 3.12.13 listed, not usable via `py -3.12`) |
| WSL | v2; distros: `docker-desktop` (default, no bash), `Ubuntu` 26.04.1 LTS. Python 3.14.4 inside Ubuntu |
| trivy | 0.76.2 — installed 2026-11-10 via `winget install AquaSecurity.Trivy` (open a new terminal for PATH) |
| ansible | core 2.21.6 in Ubuntu WSL — installed via `uv` (user-space, no sudo: `~/.local/bin/uv tool install --with-executables-from ansible-core ansible`); verified `wsl -d Ubuntu -e bash -lc "ansible --version"` |

## Phase 0 status
- DONE (verified): git identity `AryanVihan` / `aryanvihan888@gmail.com`; branch `aryan-infrastructure` (`git branch --show-current`); skeleton under `module-a-infrastructure/` (git status); `module-a-infrastructure/.gitignore` tested with `git check-ignore -v` (tfstate, .terraform/, .pem, .env, *.tfvars, __pycache__, .venv, *.db ignored; `*.tfvars.example` not ignored).
- Repo state: repo is cloned with history (merged PR #1 from Vajeeda's repo), so the "no commits / init locally" concern no longer applies.
- NOT DONE (Aryan): make `python` work (disable Store alias or use `py -3.13`); AWS account/IAM/MFA/budget alert/key pair; `aws sts get-caller-identity` (not run by Claude); decisions below; team confirmations.
- Not committed yet (phase not closed).

## Open questions
- AWS region, budget limit (suggest $10 alert), instance type (suggest t3.small/t3.medium), Python version (3.13 available), local K8s tool (kind/k3d/WSL K3s; Docker Desktop K8s not enabled) — Aryan decides
- Confirm with Vajeeda: workflow file name (e.g. `module-a.yml`), app port 8000, `/health` + `/metrics`, namespace `cloudgenome-a`, image naming
- Ports/API contracts with Vajeeda and Praveen (shared; needs agreement)

## Session log
<!-- newest first. Format: date — what was done — how verified — what failed — next step -->
- 2026-11-10 — Installed trivy (winget) and ansible (uv in WSL) at Aryan's request (CLAUDE.md lists installs as manual-only; explicit override). Remaining: python alias, AWS, decisions.
- 2026-11-10 — Phase 0: tool check, skeleton, .gitignore, PROGRESS update. Verified via tool `--version` calls and `git check-ignore -v`. Failed/missing: trivy, ansible, `python` alias. Next: Aryan resolves the NOT DONE list, then I re-run acceptance tests and commit.
- 2026-11-10 — Moved context docs to `docs-context/`; wrote phase plans, `CLAUDE.md`, this file. Nothing implemented or tested yet. Next: Phase 0.

## Known issues / risks
- Repo lives in OneDrive; no `.git` sync problems seen so far, but watch for lock issues.
