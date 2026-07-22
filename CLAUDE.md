# CLAUDE.md — AchuLink (public code repo)

This is the **public, open-source** codebase for AchuLink — an integration / workflow-automation platform
(Zapier / n8n-style), offered as managed SaaS and self-hostable.

## This repo is PUBLIC
- **No secrets, no credentials, no private business / strategy.** Product strategy & vision live in a separate
  **private** workspace, not here.
- Write for external contributors: clear README, architecture docs, and ADRs.

## Where things live
- Technical architecture → `docs/architecture.md`
- Technical decisions → `docs/adr/` (record *why* we chose a stack / DB / queue / etc.)
- Self-hosting → `docs/self-hosting.md`

## Stack
- **Backend:** Python 3.14 + **uv** (deps / venv / lockfile). API-first, runs **headless**.
- **Frontend:** **React** SPA — a client of the backend API, deployed independently.
- The **backend must work fully without the frontend** — flows, webhooks, schedules and the API keep running if
  the UI is down. Never put backend-required logic in the frontend.
- Use **uv** for all backend commands (`uv run`, `uv add`, `uv sync`) — not bare `pip`.

## Core domain (keep naming consistent)
Connector, Connection, Trigger (webhook / polling / schedule), Action, Flow, Step, Mapping, Run (execution),
Engine, Tenant.

## Principles
- **Reliability first:** every run is recorded, retryable, and observable.
- Secrets encrypted at rest; least privilege; rate-limit / back off on external APIs.
- Small, well-bounded modules with clear interfaces (engine, connectors, api, ui are separable).
- Self-host must stay first-class (`docker-compose up`), not crippleware.

## Before adding a dependency or a big architectural change
Record an ADR in `docs/adr/`.
