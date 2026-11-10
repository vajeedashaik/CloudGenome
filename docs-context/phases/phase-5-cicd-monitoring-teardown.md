# Phase 5 — CI/CD, Monitoring Evidence and Teardown

**Goal:** An independent pipeline for Module A that tests, builds, scans and deploys; monitoring evidence for the running workload; and a verified, complete teardown. **Cost: small. Ends with zero AWS resources.**

## CI/CD (PROPOSED)
- Workflow file in repo-root `.github/workflows/module-a.yml` (agree the location and naming with Vajeeda; use `paths: module-a-infrastructure/**` so it only runs for Module A and never collides with Module B).
- Jobs: **lint/test** (pytest, ruff) → **build** (Docker image) → **scan** (Trivy image/fs/config, `pip-audit`; fail on CRITICAL with documented exceptions) → **publish** (ECR or GHCR — decide) → **deploy** → **smoke test**.
- Auth to AWS: prefer GitHub OIDC with a least-privilege IAM role over long-lived keys. Needs Aryan (IAM role) and repo secrets/variables (Aryan or Vajeeda adds them — manual).
- Deployment mechanism options (decide with Aryan): SSH + `kubectl apply` via Ansible, or SSM. Keep it simple.
- Rollback: `kubectl rollout undo`, demonstrated by deploying a deliberately bad version.
- The workflow must be **actually executed** on GitHub; screenshots and run links count as evidence. A local-only dry run (`act`) is not a substitute.

## Monitoring (PROPOSED)
- App `/metrics` is already available (Phase 1).
- Install Prometheus + Grafana on the cluster (Helm or manifests; mind RAM on a small instance — if it will not fit, say so and propose a fallback such as Prometheus only, or running them locally scraping the instance).
- Dashboard: request rate, latency, error rate, pod count and restarts, CPU/memory, items by type.
- Alert rules: pod down, high error rate, high latency, crash looping.
- Generate real traffic; trigger one real alert (kill a pod or scale to zero); capture the evidence.
- CloudWatch: optional basic EC2 CPU alarm — only if cheap and quick (Praveen owns CloudWatch integration; avoid overlap).

## Security (carry-over from earlier phases)
- Scans embedded in the pipeline with reports uploaded as artifacts.
- IAM review, security group review, secrets scan (`gitleaks`/Trivy secret scan).

## Teardown (mandatory)
1. Remove cluster resources and any ECR images.
2. `terraform destroy`; confirm.
3. Verify with the CLI and the Console (Aryan): no EC2, EBS volumes, Elastic IPs, ECR repos, security groups, key pairs or log groups left from the project. Check the Billing page for the following day.
4. Record "AWS resources live: none" in `PROGRESS.md`.

## Acceptance tests
- [ ] Pipeline run on GitHub is green end to end; link and screenshot saved.
- [ ] A deliberately failing test blocks the pipeline (shown once).
- [ ] A critical vulnerability gate works (or the exception policy is documented).
- [ ] Image is in the registry with a unique tag (commit SHA).
- [ ] Deploy updates the running workload; the smoke test passes.
- [ ] Rollback demonstrated.
- [ ] Prometheus targets are UP; the dashboard shows real data; one alert fired and cleared.
- [ ] Teardown verified by CLI and Console; no leftover resources.
- [ ] No secrets in the repository history (`gitleaks` result).

## Common problems
- OIDC trust policy `sub` mismatch (branch/repo name).
- Runner cannot reach the instance (security group) — prefer SSM, or restrict and document.
- Prometheus OOM on a small node.
- GitHub Actions minutes/limits for private repos.
- Pipeline needs the repo to exist on GitHub with the right permissions (Vajeeda).

## Document
Pipeline diagram, workflow explanation by job, monitoring screenshots, alert evidence, teardown proof.
