# Phase 6 — Viva Pack and Integration Contract

**Goal:** Everything needed to explain and demonstrate all seven rubric areas without help, plus a documented interface for the other modules. **Cost: free** (a demo re-run on AWS is optional and must be re-destroyed).

## Deliverables
1. **Module README:** prerequisites, setup, run, verify, teardown — a teammate should be able to follow it cold. Test it by following it literally, on a clean checkout.
2. **Architecture diagram** for Module A and its place in CloudGenome.
3. **Viva notes** (`docs/viva/`), one section per rubric item with: what it is, what I built, why this choice, how to demo it, likely questions with answers, and honest limitations:
   - Git · Terraform & Ansible · CI/CD · Docker & Kubernetes · Monitoring · Security · the Second Brain feature.
4. **Demo script:** a timed sequence (aim ~10 min) with exact commands, expected output, and a backup plan if AWS or the network fails (e.g. recorded run or the local cluster).
5. **Evidence index:** screenshots and outputs mapped to the rubric.
6. **Integration contract** (PROPOSED; shared interface — **agree with Vajeeda and Praveen before treating it as final**):
   - Service name, port, health/ready/metrics paths, namespace/labels, image naming/tagging, env var names.
   - Second Brain API schema (OpenAPI export) and example calls.
   - How Praveen's intelligence service could look up related runbooks/incidents (`/search`, `/items/{id}`) and how deployment events from Vajeeda's pipeline could create deployment notes. Mark these as future integration, not implemented, unless built.
7. **Known limitations** list (honest): e.g. single-node K3s, SQLite constraints, no HA, basic search, no authentication unless added.
8. **Open Pull Requests** from `aryan-infrastructure` with clear descriptions (Aryan reviews/merges; teammates review).

## Final checks
- [ ] Fresh clone + README only → app runs locally.
- [ ] Demo script rehearsed end to end at least twice, timings recorded.
- [ ] Every claim in the docs matches `PROGRESS.md` verified items; remove or flag anything unverified.
- [ ] Secrets scan clean; `.gitignore` correct.
- [ ] AWS resources: none live (or documented if a demo environment is intentionally kept, with its teardown date).
- [ ] Aryan can explain each file in `terraform/`, `ansible/`, `k8s/`, the Dockerfile and the pipeline without notes.
- [ ] Commit history is meaningful and attributed to Aryan.

## Mock viva topics
Why K3s over EKS? Why Terraform state is isolated? What makes the Ansible roles idempotent? Probe types and differences? How does HPA decide to scale? What did Trivy find and what did you do? Why store notes as Markdown instead of a graph DB? What happens if the pod restarts — is data lost? What would you change for production?

## Document
Final README, viva pack, demo script, evidence index, contract proposal.
