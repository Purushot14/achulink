# 0001 — Record architecture decisions

Status: accepted
Date: 2026-07-21

## Context
Technical choices (language, queue, datastore, connector SDK, multi-tenancy) will shape the whole system and need
to be understandable by contributors.

## Decision
We record significant **technical** decisions as ADRs in `docs/adr/`, numbered `0001-`, `0002-`, … Product and
business decisions live in the separate **private** workspace, not here.

## Consequences
- Contributors can see why the system is the way it is.
- Superseded decisions are kept and marked, never deleted.

## Format
Context → Decision → Consequences → Alternatives. Keep it short.
