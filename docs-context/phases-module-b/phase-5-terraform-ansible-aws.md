# Phase 5 — Terraform and Ansible on AWS

**Goal:** Provision and configure an isolated, cheap AWS environment for Module B, with documented apply and teardown. **Cost: small — approval needed before every apply.**

## What gets provisioned (PROPOSED)
- ECR repo `cloudgenome/module-b-sample` (immutable tags, scan-on-push, lifecycle policy keep last N).
- GitHub OIDC provider + IAM role for Actions (trust limited to this repo and branch/environment).
  - Permissions: ECR push to that one repo; SSM `SendCommand` to instances tagged `cloudgenome:module=b`; nothing else.
- 1 small EC2 (instance size to decide; cheapest that runs K3s + Prometheus) with SSM instance profile.
- Security group: **no inbound SSH/6443**; only the app port if public demo is needed, ideally restricted to your IP.
- Tags on everything: `project=cloudgenome`, `module=b`, `owner=vajeeda`.
- AWS Budget alert (also manual-verified).

## Steps
1. `terraform/`: `versions.tf`, `providers.tf`, `variables.tf`, `ecr.tf`, `iam_oidc.tf`, `ec2.tf`, `network.tf`, `outputs.tf`, `terraform.tfvars.example` (no real values committed).
2. **State strategy (decide, document):** default local state excluded from Git for the academic demo, or S3 bucket + lock with a Module B-only key. Never share state with Aryan/Praveen.
3. `terraform fmt -check`, `terraform validate`, `trivy config terraform/` (free, local). Fix/triage findings.
4. **Show `terraform plan`; get explicit approval; only then `apply`.** Vajeeda runs credentials; Claude never asks for them.
5. `ansible/`: inventory (SSM or SSH per decision), `site.yml` roles: base packages, K3s install (pinned version), kubeconfig permissions, Prometheus manifests apply, deploy user, hardening basics (unattended upgrades, no root login).
6. Ansible check: `ansible-playbook --syntax-check`, `ansible-lint`, run with `--check` then real run; run **twice** to show idempotency (second run: `changed=0`).
7. Verify: `kubectl get nodes` on the host (via SSM session), K3s Ready.
8. Capture outputs: `terraform output`, `terraform state list`.

## Acceptance tests
- [ ] `terraform validate` and `fmt -check` pass; `trivy config` run, findings triaged.
- [ ] Plan reviewed and approved before apply (record resource count).
- [ ] After apply: ECR repo, role, EC2 exist (CLI listing saved).
- [ ] No ingress rule on 22 or 6443 (`aws ec2 describe-security-groups`).
- [ ] Role trust policy restricts repo/branch (read it back).
- [ ] Ansible second run reports `changed=0`.
- [ ] K3s node `Ready`.
- [ ] No `.tfstate`, `.tfvars`, keys in `git status`.
- [ ] Idle monthly cost estimate is stated as an estimate, labelled so, with the pricing source.

## Common problems
- OIDC thumbprint/audience mismatch — check `sts.amazonaws.com` audience and `sub` claim format.
- SSM agent not registering — instance profile or no outbound internet.
- K3s memory pressure on tiny instance — raise size, note cost.
- Ansible on Windows — run inside WSL.

## Document
Resource table, state strategy rationale, IAM policy JSON explained, apply/teardown commands, cost notes.
