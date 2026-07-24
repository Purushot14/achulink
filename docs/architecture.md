# Architecture

> The technical / architecture home for AchuLink. (Product vision & strategy live elsewhere, privately.)

## Overview
AchuLink is an **event-driven job runner** with connectors:

```
[Apps] --triggers--> [Ingest/API] --> [Engine: queue + workers] --> [Actions -> Apps]
                                            |
                                    [Runs store: status, logs, retries]
                                            |
                                      [UI: flow builder + runs]
```

## Stack
- **Backend:** Python 3.14, managed with **uv**. **API-first / headless** — runs all core functionality (flow
  execution, triggers, webhooks, scheduling) with no dependency on the frontend. → `adr/0002`, `adr/0003`.
- **Frontend:** **React** SPA — a pure client of the backend HTTP API; deployed / updated / taken down independently.
- **Seam:** a documented, versioned HTTP API is the only contract between backend and frontend.
- Still open: web framework (recommend FastAPI), queue / worker, datastore (likely Postgres) → future ADRs.

## Components
- **API / Ingest** — receives webhooks, serves the UI / API, enqueues work.
- **Engine** — queue + workers that execute a **run** step by step; handles retries, backoff, idempotency.
- **Connectors** — per-app definitions (auth, triggers, actions). Pluggable.
- **Datastore** — flows, connections (secrets encrypted), runs / history.
- **UI** — flow builder + runs / observability.

## Key decisions
Recorded as ADRs in [`adr/`](adr/). Decided: stack + frontend/backend independence (`adr/0002`, `adr/0003`).
Open ones: web framework, queue, datastore, connector SDK shape, multi-tenancy model.

## Cross-cutting concerns
Secrets, idempotency, rate-limiting, observability, multi-tenancy (SaaS) vs single-tenant (self-host).

> This is a skeleton — fill it in as the stack is chosen, and record each choice as an ADR.
