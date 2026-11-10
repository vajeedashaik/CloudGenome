# Phase 0 — Setup

**Goal:** A clean, verified starting point: tools installed, Git identity correct, branch and folder skeleton in place, key decisions recorded. **Cost: free.**

## Who does what
| Task | Claude | Aryan |
|---|---|---|
| Check installed tools and versions | ✅ | |
| Install missing tools | gives commands | ✅ |
| Fix git identity | tells what to run | ✅ (or approves) |
| Create branch, skeleton, `.gitignore` | ✅ | |
| AWS account, billing alert, IAM | guides | ✅ |
| Choose region and budget | recommends | ✅ decides |

## Steps
1. **Tool check** (read-only): `git`, `python`, `docker`, `kubectl`, `terraform`, `aws`, `ansible` (needs WSL on Windows), `trivy`, `gh`, `node` (optional), WSL status. Report found/missing with versions.
2. **Install missing tools** (Aryan). Suggested: Docker Desktop (with WSL2), Terraform, AWS CLI v2, Trivy, kubectl, `gh`, Ansible inside WSL (Ubuntu).
3. **Git identity:** confirm `git config user.name` and `user.email` are Aryan's own. Currently `AryanVihan` with a session email of `vajeeda.mastan@gmail.com` — resolve before the first commit.
4. **Repo state:** the repo has no commits. Either clone Vajeeda's `cloudgenome` repo (preferred once it exists) or init locally and rebase later. Record which in `PROGRESS.md`.
5. **Branch:** create `aryan-infrastructure` (not on `main`).
6. **Skeleton (PROPOSED):**
   ```text
   module-a-infrastructure/
   ├── README.md
   ├── app/                  # FastAPI Second Brain + sample workload
   │   ├── src/  tests/  requirements.txt
   ├── vault/                # Obsidian-compatible seed notes
   ├── docker/               # Dockerfile (or at app root)
   ├── k8s/                  # manifests
   ├── terraform/
   ├── ansible/
   ├── monitoring/           # prometheus/grafana config, alert rules
   ├── security/             # scan reports, findings
   ├── docs/                 # architecture, runbooks, viva, evidence
   └── .github/workflows/ -> note: workflows must live in repo-root .github/; see Phase 5
   ```
   Note: GitHub only runs workflows from the repo-root `.github/workflows/`. Use a uniquely named file (e.g. `module-a.yml`) with `paths:` filters so it does not clash with Vajeeda's workflows. Confirm with Vajeeda.
7. **`.gitignore`:** `*.tfstate*`, `.terraform/`, `*.tfvars` (keep `*.tfvars.example`), `*.pem`, `.env`, `__pycache__/`, `.venv/`, `*.db`.
8. **AWS prep (Aryan):** account/IAM user with least privilege, MFA, a **budget alert** (e.g. $10), pick a region near you, create the EC2 key pair. Never paste secrets into chat.
9. **Record decisions** in `PROGRESS.md`: region, budget, instance type proposal (e.g. `t3.small`/`t3.medium`; K3s needs ~2 GB RAM), Python version, local K8s tool (kind / k3d / WSL K3s).
10. **Confirm with team:** workflow file naming, ports (proposal: app `8000`), `/health` and `/metrics` conventions, namespace `cloudgenome-a`, image naming.

## Acceptance tests
- [ ] Tool report shows every needed tool with a working version (`--version` output recorded).
- [ ] `git config user.name` / `user.email` are correct.
- [ ] `git branch --show-current` → `aryan-infrastructure`.
- [ ] Skeleton exists; `git status` shows no secrets or ignored files tracked.
- [ ] `.gitignore` blocks `.tfstate`, `.pem`, `.env` (test with `git check-ignore -v`).
- [ ] Decisions are recorded in `PROGRESS.md`.
- [ ] AWS: `aws sts get-caller-identity` works (Aryan runs it) and the budget alert exists (screenshot).

## Common problems
- Docker Desktop needs virtualization/WSL2 enabled in BIOS/Windows features.
- Ansible does not run natively on Windows — use WSL.
- OneDrive syncing a repo can cause lock/sync problems with `.git`; if it does, say so and consider moving the repo out of OneDrive.

## Document
Tool versions, decisions, any install issues.
