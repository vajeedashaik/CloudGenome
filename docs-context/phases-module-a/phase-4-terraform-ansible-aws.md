# Phase 4 — Terraform and Ansible on AWS

**Goal:** Provision an isolated environment with Terraform, configure it with Ansible, and deploy the Phase 3 workload on K3s. **Cost: small but real. Never leave it running.**

## Prerequisites
- Phases 0–3 verified in `PROGRESS.md`.
- Aryan has: AWS credentials configured, billing alert active, key pair created, region and instance type chosen.
- Approval from Aryan before any `apply`.

## Terraform (`terraform/`) — PROPOSED
- Provider pinned; Terraform version constraint.
- Resources: VPC (or default VPC use, decide for cost/simplicity), subnet, internet gateway/route, security group (SSH only from Aryan's IP `/32`, app port restricted), EC2 instance (small, e.g. `t3.small`/`t3.medium`), IAM role/instance profile (minimal), encrypted root volume, optional ECR repo.
- Variables with sensible defaults; `terraform.tfvars.example` committed, real `.tfvars` ignored.
- Outputs: public IP, SSH command, instance ID.
- Tags on everything: `Project=cloudgenome`, `Module=a`, `Owner=aryan`.
- **Isolated state:** start with local state (ignored by Git) for simplicity; if an S3 backend is wanted, it needs a separate bucket (cost and bootstrap complexity) — ask Aryan first. Never share state with other modules.
- Cost note: EC2 hourly charge, EBS storage, public IPv4 address charge, data transfer. Quote current prices only after checking AWS pricing; do not guess.

## Ansible (`ansible/`) — PROPOSED
- Inventory generated from Terraform output (or templated).
- Roles: `common` (updates, basic hardening, time sync), `docker`, `k3s` (single-node install), `app-deploy` (kubectl apply manifests / image import), optional `monitoring-agent`.
- Idempotent: a second run reports zero changes.
- Runs from WSL on Windows.

## Steps
1. `terraform fmt -check`, `init`, `validate`, `plan` — offline-safe checks first.
2. Scan Terraform (`trivy config terraform/`); fix or document.
3. Show the plan to Aryan; get explicit approval.
4. `terraform apply`; record outputs.
5. Verify SSH, then run `ansible-lint` and `ansible-playbook --check`, then the real run.
6. Re-run the playbook to prove idempotency.
7. Deploy the workload on K3s; get the image there (options: build on the instance, load via `docker save`/`k3s ctr images import`, or ECR later in Phase 5).
8. Test from outside: the API, `/health`, `/metrics`.
9. Capture evidence (screenshots, command outputs).
10. If Aryan wants to pause: `terraform destroy` now; re-apply later is cheaper than leaving it up. Teardown is **mandatory** at the end of Phase 5 at the latest.

## Acceptance tests
- [ ] `terraform validate` and `plan` clean; scan results documented.
- [ ] `apply` created exactly the planned resources (compare to plan).
- [ ] SSH works only from the allowed IP; the open-port check from outside matches the intent.
- [ ] Ansible first run succeeds; second run has `changed=0`.
- [ ] `kubectl get nodes` on the instance → Ready.
- [ ] Workload `Running`; API reachable at the public IP/port; `/metrics` works.
- [ ] Instance reboot test: K3s and the workload come back (record the result).
- [ ] Resources are tagged; AWS Console matches Terraform state.
- [ ] **Teardown rehearsed** (or scheduled in Phase 5) and `terraform destroy` leaves no resources.

## Common problems
- SSH timeout: wrong security group IP (home IP changed), wrong key, or the public IP not assigned.
- Windows → WSL key permission errors (`chmod 600` on a file inside the WSL filesystem, not `/mnt/c`).
- K3s install download needing internet from the instance — check egress.
- Low-memory instance OOM — resize.
- State drift if resources are edited in the Console.

## Document
Architecture diagram, resource list with purpose, cost estimate with its source, Terraform and Ansible command transcripts, and idempotency evidence.
