# Phase 7 — Viva Pack, Evidence and Integration Contract

**Goal:** Everything needed to demonstrate and explain all seven rubric items alone, plus a clean integration contract. **Cost: free** (demo can reuse Phase 6 or run local-only fallback).

## Steps
1. **Module README** (`module-b-devsecops/README.md`): purpose, architecture diagram, prerequisites, setup, run, verify, rollback, teardown, troubleshooting. A teammate must be able to follow it cold.
2. **Evidence folder** `docs/evidence/`: screenshots and saved outputs indexed per rubric item (Git PRs/commits, Terraform plan/apply, Ansible run, workflow runs, ECR images, kubectl outputs, Prometheus, Trivy reports).
3. **Viva notes** `docs/VIVA.md` — per rubric item: what I built, why, commands to show, 5+ likely questions with answers:
   - Git: branching, PR flow, why protected main, merge vs squash.
   - Terraform/Ansible: state, idempotency, plan vs apply, why OIDC instead of keys.
   - CI/CD: job flow, gates, policy, rollback logic.
   - Docker/K8s: multi-stage, non-root, probes, rolling update, `rollout undo`.
   - Monitoring: what is scraped, alert logic, health gate.
   - Security: what each scanner finds, policy, exceptions with expiry, least-privilege IAM.
   - Demo: scripted run order and timing.
4. **Demo script** (`docs/DEMO.md`): code change → PR → CI → merge → ECR → deploy → verify → broken change → rollback; 8–10 minute version and a local-only fallback (kind) if AWS is down/torn down.
5. **Dry runs:** run the full demo twice from a clean state; time it; note failures and fixes.
6. **Integration contract** `integration/` proposal (shared dir — **ask team first**): image naming, ECR path, ports, probe paths, labels, env vars, deployment inputs, what Module B can publish for A/C and what it expects. Confirm values against the draft in the phase README.
7. Honest limitations section: what is proposed/untested/skipped, costs, known gaps.
8. Final cleanup: PROGRESS.md final entry, repo hygiene (no secrets, no state files), PR to `main` reviewed by teammate.

## Acceptance tests
- [ ] Teammate follows README cold; issues found are fixed.
- [ ] Every rubric item has at least one saved evidence artifact and one live command.
- [ ] Full demo run twice, within time limit, from clean state.
- [ ] Local fallback demo works without AWS.
- [ ] `git log --author=vajeeda` shows meaningful commit history; PRs linked.
- [ ] Secrets sweep: `gitleaks detect` clean on full history; `git ls-files` has no `.tfstate`/`.pem`/`.env`.
- [ ] Integration contract agreed in writing by Aryan and Praveen (or listed as open).
- [ ] PROGRESS.md shows no AWS resources live (or lists them).

## Common problems
- Demo depends on live network — pre-pull images, keep recorded fallback.
- Stale screenshots not matching current code — regenerate after the last change.

## Document
Final README, VIVA.md, DEMO.md, evidence index, limitations.
