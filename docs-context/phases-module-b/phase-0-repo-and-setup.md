# Phase 0 — Repo Administration and Setup

**Goal:** Shared repo is safe for three contributors; your tools work; Module B skeleton exists. **Cost: free.**

## Steps
**Manual (Vajeeda):**
1. Confirm GitHub repo `cloudgenome` exists; confirm Aryan **and Praveen** are collaborators (Settings → Collaborators). Aryan's PR #1 already merged, so check only Praveen.
2. Install/verify: Git, Python 3.12, Docker Desktop (WSL2 on), kubectl, kind or k3d, Trivy, Terraform, Ansible (via WSL), AWS CLI, `gh` CLI (optional).
3. Decide AWS region and billing alert; set AWS Budget (manual, console). Configure credentials yourself — never paste them in chat.
4. Branch protection on `main` (Settings → Branches): require PR, 1 approval, no force-push. (May be limited on private free plans — note if unavailable.)

**Claude can do:**
5. Verify `git config user.name/email`; branch `vajeeda-devsecops` exists.
6. Create `module-b-devsecops/` skeleton:
   ```text
   module-b-devsecops/
   ├── app/            # sample service + tests
   ├── docker/         # Dockerfile, .dockerignore
   ├── k8s/            # manifests
   ├── terraform/      # ECR, OIDC, EC2, SG
   ├── ansible/        # K3s + host config
   ├── security/       # scan reports, FINDINGS.md, policy
   ├── monitoring/     # prometheus config, alert rules
   ├── scripts/        # deploy, rollback, verify, teardown
   ├── docs/           # architecture, evidence, viva
   ├── PROGRESS.md
   └── README.md
   ```
7. Draft `module-b-devsecops/docs/GIT_WORKFLOW.md`: branch names (`vajeeda-devsecops`, short-lived `feat/…` off it), Conventional Commits (`feat:`, `fix:`, `docs:`, `ci:`, `chore:`), PR flow, review expectations (reviewer checks module-boundary + no secrets), issue/task label convention.
8. **PROPOSE only (ask first — shared root files):** root `.gitignore` additions (`.env`, `*.pem`, `*.tfstate*`, `*.tfvars`, `.terraform/`, `__pycache__/`, `.venv/`), `.github/PULL_REQUEST_TEMPLATE.md`, `CODEOWNERS` mapping each module dir to its owner. Do not edit root files without agreement.
9. Draft `module-b-devsecops/CLAUDE.md` with the honesty/phase/cost rules (Module A's root CLAUDE.md is Aryan's; avoid conflicting root edits).
10. Create `PROGRESS.md`.

## Acceptance tests
- [ ] `git branch --show-current` prints `vajeeda-devsecops`.
- [ ] `git config user.name` / `user.email` are yours.
- [ ] Each tool prints a version: `git`, `python`, `docker`, `kubectl`, `kind`/`k3d`, `trivy`, `terraform`, `ansible`, `aws`.
- [ ] `docker run hello-world` works.
- [ ] `aws sts get-caller-identity` works (you ran it; output not pasted with secrets).
- [ ] AWS Budget alert exists (screenshot saved).
- [ ] Praveen shown as collaborator (screenshot).
- [ ] Skeleton committed only inside `module-b-devsecops/`; `git status` shows no stray root changes.
- [ ] Draft `.gitignore` entries verified: `git check-ignore -v test.pem .env` matches.

## Common problems
- Docker Desktop needs WSL2 / virtualization enabled in BIOS.
- Ansible does not run on native Windows — use WSL.
- Branch protection unavailable on free private repos — compensate by team rule + PR habit, say so in viva.

## Document
Tool versions table, repo settings screenshots, Git workflow doc.
