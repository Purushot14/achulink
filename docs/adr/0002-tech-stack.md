# 0002 — Tech stack: Python 3.14 + uv (backend), React (frontend)

Status: accepted
Date: 2026-07-21

## Context
We need to pick the core technologies for the backend (API + execution engine) and the frontend (flow-builder UI).

## Decision
- **Backend:** **Python 3.14**, with **uv** (Astral) for dependency, virtualenv and Python-version management
  (single tool; `uv.lock` for reproducible installs).
- **Frontend:** **React** (single-page app).
- Web framework, queue / worker and datastore are **separate decisions** — see "Still open" below.

## Consequences
- ✅ Python has a strong ecosystem for integrations, APIs, async I/O and data work; large contributor pool.
- ✅ uv gives fast, reproducible installs and manages the Python version too — standardize on it for dev and CI
  (`uv sync`, `uv run`, `uv add`); avoid bare `pip`.
- ✅ React has the richest ecosystem for a node / graph-style flow builder and for hiring.
- ⚠️ Two toolchains (Python + Node) to maintain — standard for an API + SPA product.
- ⚠️ Python 3.14 is very new — **pin it** and watch third-party library compatibility.

## Still open (future ADRs)
- Backend **web framework** — recommendation: **FastAPI** (async, Pydantic, API-first).
- **Queue / worker** — e.g. Arq / Celery / Dramatiq / RQ.
- **Datastore** — likely **PostgreSQL**.

## Alternatives considered
- **Node / TypeScript backend** (one language across the stack) — deferred; Python preferred for the
  integration / data ecosystem and familiarity.
- **Go backend** — great for performance, but slower to author many connectors early on.
- **Package managers:** pip / Poetry / PDM — **uv** chosen for speed + integrated Python / venv management.
- **Frontend:** Vue / Svelte — **React** chosen for ecosystem and hiring.
