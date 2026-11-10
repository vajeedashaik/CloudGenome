# Phase 1 — Second Brain Service + Sample Workload

**Goal:** A working local FastAPI service that is both the Kubernetes workload and the CloudGenome Second Brain, with an Obsidian-compatible vault. **Cost: free.** No Docker or cloud yet.

## User problem and journey (PROPOSED)
Engineers repeatedly rediscover how a service works, how an incident was fixed, and why a decision was made. Journey: an incident occurs → the engineer searches "high CPU after deploy" → finds a past incident note → follows links to the related service, deployment and runbook → applies the runbook → records the outcome.

## Design (PROPOSED — confirm with Aryan before coding)
**Source of truth:** an Obsidian vault (folder of `.md` files), openable directly in the Obsidian app.

Note format:
```markdown
---
id: inc-2026-001
type: incident        # service | deployment | incident | runbook | decision
title: High CPU after v1.2 deploy
tags: [cpu, deployment]
created: 2026-11-01T10:00:00Z
updated: 2026-11-01T12:00:00Z
source: https://example/ticket/123   # optional reference
---
Root cause ... links: [[svc-orders]] [[dep-orders-v1-2]] [[rb-high-cpu]]
```

**Service behaviour:** parse the vault → build a **rebuildable SQLite index**: `items` table, `links` table (source, target), and an FTS5 table for search. Backlinks = reverse lookup on `links`. The index is disposable (re-created from the vault).

**Editing:** API writes Markdown files back to the vault (frontmatter + body), then re-indexes that item. Files stay the source of truth, so Obsidian edits and API edits coexist.

**Why not a graph DB:** a links table answers backlinks, neighbours and 2-hop queries at this scale. Trade-off: complex graph algorithms would need more work; not needed for the MVP.

## API (PROPOSED)
| Method | Path | Purpose |
|---|---|---|
| GET | `/health` | liveness (`{"status":"ok"}`) |
| GET | `/ready` | readiness (vault readable, index loaded) |
| GET | `/metrics` | Prometheus metrics |
| GET | `/api/v1/items` | list, filter by `type`/`tag` |
| POST | `/api/v1/items` | create (writes `.md`) |
| GET | `/api/v1/items/{id}` | view with links, backlinks |
| PUT | `/api/v1/items/{id}` | edit |
| DELETE | `/api/v1/items/{id}` | remove |
| GET | `/api/v1/search?q=` | full-text search with snippets |
| GET | `/api/v1/graph` | nodes+edges JSON (graph view) |
| POST | `/api/v1/reindex` | rebuild index from vault |

Minimal UI (PROPOSED, only if it stays small and reliable): a single static page for search, view, backlinks and a basic graph. API first; UI is optional for this phase.

**Config via environment variables:** `VAULT_PATH`, `DB_PATH`, `PORT` (default 8000), `LOG_LEVEL`.

**Metrics (examples):** request count/latency by route, items by type, index size, last reindex timestamp.

## Seed data
Four linked notes at minimum: a service note, a deployment note, an incident report, a runbook — realistic and consistent, plus one decision note. Add one dangling link to test handling.

## Steps
1. Python venv, pinned `requirements.txt` (FastAPI, uvicorn, python-frontmatter/PyYAML, prometheus-client, pytest, httpx).
2. Vault parser (frontmatter, `[[links]]`, tags).
3. SQLite index with FTS5 (verify FTS5 is available in the Python build).
4. CRUD + search + backlinks + graph endpoints.
5. Health, ready, metrics.
6. Validation, safe file paths (prevent path traversal via `id`), error handling.
7. Seed vault.
8. Tests and README section with run instructions.

## Acceptance tests
- [ ] `pytest` passes; coverage covers parser, index, each endpoint, path-traversal rejection, dangling links, unicode, empty vault.
- [ ] `uvicorn` starts; `curl /health` → 200; `/ready` → 200.
- [ ] `/metrics` returns Prometheus text including custom metrics.
- [ ] Search for a known phrase returns the right seeded incident.
- [ ] `GET /items/{service}` lists the incident as a **backlink**.
- [ ] Creating a note via API creates a valid `.md` file; deleting the DB and calling `/reindex` restores everything.
- [ ] Opening the vault in the Obsidian app shows the notes, links and graph (Aryan checks, screenshot).
- [ ] Runs with **no** dependency on Module B or C.
- [ ] No secrets in the repo.

## Common problems
- FTS5 missing in some Python builds → report it and choose a fallback explicitly.
- Windows path/line-ending differences in the vault → normalise on read.
- Concurrent writes to SQLite → keep it single-process for the MVP; document the limit.

## Document
Data model, API examples (curl with real output), design trade-offs, the later link to Praveen's module (his service could call `/search` and `/items` to attach runbooks to incidents — contract finalised in Phase 6).
