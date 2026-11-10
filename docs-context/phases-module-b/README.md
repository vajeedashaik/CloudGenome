# Module B (Vajeeda) — Phase Plan (index)

All content below is a **PROPOSAL** until implemented and tested. Status lives in `module-b-devsecops/PROGRESS.md` (create in Phase 0).
Scope: `module-b-devsecops/` on branch `vajeeda-devsecops`. Source docs: `../CloudGenome - Common_Project_Context.md`, `../CloudGenome_Vajeeda_Master_Prompt.md`.

| # | File | Goal | AWS cost |
|---|---|---|---|
| 0 | [phase-0-repo-and-setup.md](phase-0-repo-and-setup.md) | Repo admin, conventions, tools, module skeleton | Free |
| 1 | [phase-1-sample-app-and-tests.md](phase-1-sample-app-and-tests.md) | Small service with health/metrics + automated tests | Free |
| 2 | [phase-2-docker-and-security.md](phase-2-docker-and-security.md) | Image, source/dependency/image scans, vulnerability policy | Free |
| 3 | [phase-3-local-kubernetes.md](phase-3-local-kubernetes.md) | Manifests, probes, rollout and rollback locally | Free |
| 4 | [phase-4-github-actions-ci.md](phase-4-github-actions-ci.md) | CI workflow: test, scan, build, Trivy gate (no AWS) | Free |
| 5 | [phase-5-terraform-ansible-aws.md](phase-5-terraform-ansible-aws.md) | ECR, OIDC role, EC2+K3s via Terraform/Ansible, budget | Small |
| 6 | [phase-6-cd-monitoring-teardown.md](phase-6-cd-monitoring-teardown.md) | Push to ECR, deploy, verify, rollback, monitoring, teardown | Small |
| 7 | [phase-7-viva-and-integration.md](phase-7-viva-and-integration.md) | Evidence pack, viva notes, integration contract | Free |

## Rubric coverage (own evidence for each)

| Rubric item | Where earned |
|---|---|
| Git | All phases; Phase 0 conventions; every phase ends in a PR |
| Terraform & Ansible | Phase 5 (run + teardown in 6) |
| CI/CD | Phase 4 (CI), Phase 6 (CD) |
| Docker & Kubernetes | Phase 2 (image), 3 (local K8s), 6 (K3s on EC2) |
| Monitoring | Phase 1 (endpoints), 6 (Prometheus + alert/health evidence) |
| Security | Phase 2 (policy + scans), 4 (in pipeline), 5 (IAM/OIDC, SG, IaC scan) |
| Viva/Demo | Phase 7 |

## Proposed technical choices (confirm in Phase 0)
- **App:** Python 3.12 + FastAPI, tiny "release-demo" service. Endpoints `/health`, `/ready`, `/version`, `/metrics`. Version string injected at build (git SHA) so rollback is visible.
- **Tests:** pytest (+ coverage).
- **Source/dependency scan:** `pip-audit`, `trivy fs`, `gitleaks` (secrets), Dockerfile via `trivy config`.
- **Image scan:** Trivy. Policy: fail on CRITICAL/HIGH **with a fix available**; exceptions only in `.trivyignore` with reason and expiry date.
- **Local K8s:** kind or k3d. **AWS K8s:** K3s on one small EC2.
- **AWS auth from GitHub:** OIDC federation to an IAM role (no long-lived keys). Least privilege: ECR push to one repo, SSM send-command to one tagged instance.
- **Deploy mechanism:** SSM Run Command to the EC2 host (no inbound SSH, no exposed 6443). Fallback: SSH restricted to your IP. Decide in Phase 5.
- **Monitoring:** `/metrics` scraped by lightweight Prometheus on K3s; Grafana only if memory allows; post-deploy health gate in pipeline. CloudWatch alarm/budget alert as extra evidence.
- **Terraform state:** isolated to Module B (own S3 key/prefix or local state, never shared with Aryan/Praveen).

## Proposed integration contract (draft, needs team agreement before root files change)
| Item | Module B value |
|---|---|
| ECR repo | `cloudgenome/module-b-sample` |
| Image tag | `<git-sha>` (immutable) plus `latest` only for local use |
| Container port | `8000` |
| Health / ready / metrics | `/health`, `/ready`, `/metrics` |
| Namespace | `cg-devsecops` |
| Labels | `app.kubernetes.io/name`, `app.kubernetes.io/part-of=cloudgenome`, `cloudgenome.io/module=b` |
| Env vars | `APP_VERSION`, `LOG_LEVEL`, `PORT` |

## Phase rules
1. Finish and verify a phase fully before starting the next. Free phases (0–4) must cost nothing.
2. Run every acceptance test from a clean state. No claim without seen output.
3. Update `module-b-devsecops/PROGRESS.md` at end of every task, phase and session.
4. Each phase ends: PROGRESS updated, commits made (only when asked), stop and wait for "next".
5. Label everything `PROPOSED`, `IMPLEMENTED`, `TESTED`, or `VERIFIED-ON-AWS`.
6. Never `terraform apply/destroy` or create/delete AWS resources without showing the plan and getting explicit approval.
