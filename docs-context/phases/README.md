# Module A — Phase Plan (index)

All content below is a **PROPOSAL** until implemented and tested. Status lives in `../../PROGRESS.md`.

| # | File | Goal | AWS cost |
|---|---|---|---|
| 0 | [phase-0-setup.md](phase-0-setup.md) | Tools, Git identity, branch, skeleton | Free |
| 1 | [phase-1-second-brain-and-workload.md](phase-1-second-brain-and-workload.md) | Working app: Second Brain API + vault + metrics | Free |
| 2 | [phase-2-docker-and-security.md](phase-2-docker-and-security.md) | Image, local run, Trivy findings | Free |
| 3 | [phase-3-local-kubernetes.md](phase-3-local-kubernetes.md) | Manifests, probes, scaling, recovery locally | Free |
| 4 | [phase-4-terraform-ansible-aws.md](phase-4-terraform-ansible-aws.md) | Provision, configure, deploy on EC2 | Small |
| 5 | [phase-5-cicd-monitoring-teardown.md](phase-5-cicd-monitoring-teardown.md) | Pipeline, monitoring evidence, teardown | Small |
| 6 | [phase-6-viva-and-integration.md](phase-6-viva-and-integration.md) | Viva notes, integration contract | Free |

## Rubric coverage

| Rubric item | Where it is earned |
|---|---|
| Git | All phases: branch, meaningful commits, PRs |
| Terraform & Ansible | Phase 4 |
| CI/CD | Phase 5 (workflow drafted earlier is fine, executed in 5) |
| Docker & Kubernetes | Phases 2, 3, 4 |
| Monitoring | Phases 1 (endpoints), 5 (evidence) |
| Security | Phase 2 (Trivy), 4 (IAM/SG), 5 (pipeline scan) |
| Viva/Demo | Phase 6 |

## Proposed technical choices (to confirm in Phase 0)
- Language/framework: Python 3.12 + FastAPI (matches Praveen's stack, easy to explain).
- Second Brain storage: **Obsidian-style Markdown vault is the source of truth** (files with YAML frontmatter, `[[wiki-links]]`, `#tags`). The service parses the vault into a **rebuildable SQLite index** (FTS5 search, links table for backlinks). No graph database.
- Kubernetes: K3s on one EC2 instance for AWS; kind/k3d (or K3s in WSL) locally.
- Monitoring: `/metrics` (Prometheus format) + Prometheus/Grafana on the cluster (placement to confirm).
- Security: Trivy (image + filesystem), `pip-audit`, Terraform scan (e.g. Trivy config / tfsec) — all documented.

## Phase rules
1. Finish and verify a phase fully before starting the next.
2. Every phase has acceptance tests; all must pass from a clean state.
3. Update `PROGRESS.md` at the end of every task.
4. Stop after each phase and wait for "next".
