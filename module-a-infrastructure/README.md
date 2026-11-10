# Module A — Cloud Infrastructure, Kubernetes & Second Brain

Owner: Aryan. Branch: `aryan-infrastructure`. Status: skeleton only (Phase 0). See `../PROGRESS.md`.

| Folder | Purpose |
|---|---|
| `app/` | FastAPI Second Brain + sample workload (`src/`, `tests/`) |
| `vault/` | Obsidian-compatible seed notes |
| `docker/` | Dockerfile(s) |
| `k8s/` | Kubernetes manifests |
| `terraform/` | AWS infrastructure |
| `ansible/` | Server configuration |
| `monitoring/` | Prometheus/Grafana config, alert rules |
| `security/` | Scan reports, findings |
| `docs/` | Architecture, runbooks, viva, evidence |

CI workflow lives in repo-root `.github/workflows/` (uniquely named, `paths:`-filtered) — see Phase 5.
