# Phase 3 — Local Kubernetes, Rollout and Rollback

**Goal:** Prove the deploy/verify/rollback logic locally before touching AWS. **Cost: free.**

## Steps
1. Create local cluster (kind or k3d); `kubectl get nodes`.
2. Load the image into the cluster (`kind load docker-image` / `k3d image import`).
3. Manifests in `k8s/` (plain YAML, optionally `kustomize` for image tag):
   - `namespace.yaml` (`cg-devsecops`)
   - `deployment.yaml`: 2 replicas, labels per contract, resource requests/limits, liveness `/health`, readiness `/ready`, rolling update (`maxUnavailable: 0`), `securityContext` (runAsNonRoot, readOnlyRootFilesystem, drop ALL caps, no privilege escalation), env `APP_VERSION`/`GIT_SHA`, `revisionHistoryLimit: 5`.
   - `service.yaml`: ClusterIP (NodePort only if needed for AWS; do not expose more than necessary).
   - Optional `networkpolicy.yaml`.
4. Scripts in `scripts/`:
   - `deploy.sh <image:tag>` → set image, `kubectl rollout status --timeout`.
   - `verify.sh` → port-forward or in-cluster curl `/health` and `/version`, assert expected version, exit non-zero on failure.
   - `rollback.sh` → `kubectl rollout undo` + verify previous version returns.
5. Deploy v1, then v2 (rebuild with different `APP_VERSION`), then a **broken v3** (bad readiness or crashing) to see rollout stall and be rolled back.
6. Kill a pod; observe self-healing.

## Acceptance tests
- [ ] Both pods Ready; `kubectl get pods -n cg-devsecops` output saved.
- [ ] `/version` returns v1, then v2 after rollout.
- [ ] Broken v3: rollout does not complete, old pods keep serving (zero downtime shown by looped curl).
- [ ] `rollback.sh` restores v2 (or v1); `/version` confirms.
- [ ] Deleted pod is recreated.
- [ ] `kubectl rollout history` shows revisions.
- [ ] `kubectl auth can-i` / pod runs non-root (`kubectl exec … id`).
- [ ] Manifests pass `trivy config k8s/` — findings triaged in FINDINGS.md.

## Common problems
- `ImagePullBackOff` — image not loaded into cluster or `imagePullPolicy` wrong.
- Port-forward drops during rollout — use a retry loop in `verify.sh`.
- `readOnlyRootFilesystem` breaks writes — mount an `emptyDir` for `/tmp`.

## Document
Manifest explanation (probes, strategy, securityContext), rollout and rollback walkthrough with real outputs.
