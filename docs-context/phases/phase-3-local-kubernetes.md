# Phase 3 — Local Kubernetes

**Goal:** Prove the Kubernetes design works on a free local cluster before spending anything on AWS. **Cost: free.**

## Cluster choice (PROPOSED)
Use **kind** or **k3d** with Docker Desktop (fast, free). K3s itself is used on AWS in Phase 4. State this difference openly; where behaviour could differ (Traefik ingress, storage class), note it.

## Manifests (`k8s/`)
- `namespace.yaml` — `cloudgenome-a` with labels (`app.kubernetes.io/part-of: cloudgenome`, `module: a`).
- `configmap.yaml` — non-secret settings.
- `pvc.yaml` — vault/DB storage (or an init step to seed the vault).
- `deployment.yaml` — 2 replicas, resource requests/limits, liveness `/health`, readiness `/ready`, startup probe, non-root `securityContext`, rolling update strategy.
- `service.yaml` — ClusterIP; NodePort or ingress for access.
- `hpa.yaml` — CPU-based autoscaling (needs metrics-server; verify it works locally).
- `pdb.yaml` (optional).
- Prometheus scrape annotations or a ServiceMonitor (decide in Phase 5).

Concern to resolve: **state with multiple replicas.** SQLite on a shared PVC with several writers is unsafe. Options (decide with Aryan): (a) run replicas read-only with one writer, (b) the vault is a seeded read-mostly volume and the index is per-pod, rebuilt at start, (c) keep 1 replica for the stateful part. Do not hide this limitation.

## Steps
1. Create the cluster; `kubectl get nodes`.
2. Load the Phase 2 image into the cluster (`kind load` / `k3d image import`).
3. Apply manifests; check rollout.
4. Access the service (port-forward or NodePort); exercise the API.
5. **Scaling:** `kubectl scale`, then HPA under load (a simple load generator).
6. **Recovery:** delete a pod and watch it recreate; break the readiness check and watch traffic stop going to it; roll out a bad image and roll back (`kubectl rollout undo`).
7. Validate manifests (`kubectl apply --dry-run=server`, optionally `kubeconform`) and scan them (`trivy config k8s/`).
8. Write the K8s deployment runbook.

## Acceptance tests
- [ ] All pods `Running` and `Ready`; `kubectl rollout status` succeeds.
- [ ] API works through the Service.
- [ ] Killing a pod → a new one is created; service stays available (record observed downtime honestly).
- [ ] Failing readiness probe removes the pod from endpoints (`kubectl get endpoints`).
- [ ] Scale 2 → 4 → 2 works.
- [ ] HPA scales out under load and back in (record actual numbers and timings; or document why not, if metrics-server cannot be made to work).
- [ ] Bad rollout is rolled back successfully.
- [ ] Data persistence behaviour documented and tested.
- [ ] Manifest scan results saved.

## Common problems
- `ImagePullBackOff` — image not loaded in the cluster, or `imagePullPolicy` wrong.
- metrics-server needs `--kubelet-insecure-tls` on kind.
- Probe failures from slow start — tune `startupProbe`.
- Permission errors on PVC with non-root user — fix `fsGroup`.

## Document
Manifest explanations (viva-ready), scaling and recovery transcripts, the state/replica decision.
