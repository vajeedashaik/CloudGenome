# Phase 6 — Continuous Delivery, Monitoring, Rollback and Teardown

**Goal:** Merge to deploy target: approved image pushed to ECR, deployed to K3s, verified, rollback demonstrated, monitoring evidence captured, everything torn down cleanly. **Cost: small — approval for AWS actions.**

## Steps
1. **Pipeline extension** (`module-b-cd.yml` or extend CI): after all gates pass on `main`:
   - `aws-actions/configure-aws-credentials` using OIDC role (no stored keys; `permissions: id-token: write` only on this job).
   - Login to ECR, push image tagged with git SHA (immutable).
   - Optional: re-scan pushed image digest.
   - GitHub **Environment** `aws-demo` with required reviewer (manual approval gate) for deploy job.
2. **Deploy job:** SSM `send-command` runs `scripts/deploy.sh <ecr-uri>:<sha>` on the host; host pulls from ECR using instance role (ECR read only). Wait for command result.
3. **Health verification job:** poll `/health` and `/version` until the new SHA is reported or timeout.
4. **Automatic rollback:** if verification fails, run `scripts/rollback.sh` (`kubectl rollout undo`), re-verify, mark pipeline failed with clear message.
5. **Rollback demo:** deploy good v2, deliberately deploy broken v3, capture pipeline auto-rollback, show `/version` back to v2.
6. **Monitoring (`monitoring/`):**
   - Prometheus on K3s scraping app `/metrics` (annotations or ServiceMonitor-free static config).
   - Alert rules: app down, high 5xx rate, pod restarts.
   - Evidence: Prometheus targets page UP, a query graph (request rate / build_info), alert firing when pods scaled to 0, then resolved.
   - Grafana only if memory allows; otherwise Prometheus UI plus health gate is sufficient evidence (say which).
   - Optional: CloudWatch alarm on EC2 status; billing alarm.
7. **Teardown:** `scripts/teardown.sh` — delete K8s namespace, `terraform destroy` (after plan shown and approved), delete ECR images if repo force-delete not set, confirm no leftovers.
8. **Leftover check commands** (record output): EC2 instances, EBS volumes, Elastic IPs, ECR repos, IAM roles/OIDC provider, security groups, CloudWatch log groups, budget still present as intended.

## Acceptance tests
- [ ] Merge to `main` pushes image to ECR; `aws ecr describe-images` shows SHA tag.
- [ ] Deploy job waits for approval, then succeeds; `/version` matches the commit.
- [ ] Failing health check triggers automatic rollback; pipeline red, service still healthy on previous version.
- [ ] Manual rollback via `rollback.sh` verified.
- [ ] Prometheus target UP; alert fires and resolves (screenshots).
- [ ] No AWS access keys in repo/secrets (OIDC only); role session visible in CloudTrail (optional).
- [ ] Teardown run; leftover check shows nothing billable remains. Record in PROGRESS.md.
- [ ] Total AWS spend recorded from billing console (real number, by Vajeeda).

## Common problems
- SSM command timeout — raise timeout; check instance role.
- ECR login expires (12h) — login per run.
- `terraform destroy` blocked by non-empty ECR — `force_delete` or empty first.
- Rollback to a tag already deleted by lifecycle policy — keep ≥ 5 images.

## Document
Pipeline diagram, deploy/rollback sequence, monitoring evidence, teardown log.
