# 0003 — Frontend and backend are independent (API-first, backend runs headless)

Status: accepted
Date: 2026-07-21

## Context
The product has a backend (API + automation engine) and a React frontend. We want them **decoupled**. Most
importantly: **if the frontend is not running, the backend must keep working** — flows must execute, webhooks
must be received, schedules must fire, and the API must respond.

## Decision
- The **backend is API-first / headless**: it exposes a complete HTTP API and runs *all* core functionality
  (flow execution, triggers, webhooks, scheduling) with **no dependency on the frontend**.
- The **frontend is a separate React SPA** that is purely a **client** of the backend API. It can be deployed,
  updated, or taken offline independently.
- The **only contract** between them is a documented, versioned HTTP API. No backend-required logic lives in the
  frontend; the backend never requires the frontend to run.

## Consequences
- ✅ **UI outage ≠ automation outage** — the engine keeps running if the frontend is down.
- ✅ Backend can be self-hosted **headless** (API-only) — good for programmatic users.
- ✅ Independent deploy, scaling and release cadence for each side.
- ✅ A clean API seam encourages good API design and enables third-party / CLI clients later.
- ⚠️ Must maintain a **versioned, documented API** as the contract; handle cross-origin (CORS) + auth between the
  two sides.
- ⚠️ The full product runs **two deployables** — but self-host can run backend-only.
- Implies repo layout: independent `backend/` and `frontend/` apps under `achulink/`, communicating only over
  HTTP.

## Alternatives considered
- **Monolith with server-rendered UI** (e.g. Django templates) — rejected: couples the UI to the backend and
  makes a headless / API-first mode awkward.
- **Full-stack framework where the frontend owns a backend-for-frontend** (e.g. Next.js API routes) — rejected:
  risks core logic leaking into the frontend tier; we want the engine fully independent.
