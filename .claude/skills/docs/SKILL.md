---
name: docs
description: Use when writing or organizing documentation in this repository — how to place, name, structure, cross-link, and index docs so both humans and AI agents can navigate them. Follows the Diátaxis model, keeps CLAUDE.md a lean pointer layer, and an index.md "map of content" at every folder.
---

# docs — how we document AchuLink

This repo is **public / open-source**. These are the doc conventions for the public codebase — follow them so
contributors (and AI agents) can find and trust the docs. Forked AchuLink? This skill is yours too.

## The 4 doc kinds (Diátaxis)
Every doc is **exactly one** of these — decide which before you write:

| Kind | Purpose | Answers | Example |
|------|---------|---------|---------|
| **Tutorial** | learning | "teach me, step by step" | first-connector walkthrough |
| **How-to** | a task | "how do I X" | "how to add a connector" |
| **Reference** | facts | "what exactly is X" | API / config / schema |
| **Explanation** | understanding | "why is it built this way" | architecture overview |

Don't mix kinds in one doc — split and link them.

## Where docs live
- `README.md` — front door (what / why / quickstart / links).
- `CONTRIBUTING.md` — how to contribute.
- `docs/` — everything else, grouped by Diátaxis kind or by area.
- `CLAUDE.md` — the AI pointer layer (see below).
- Docs are **edited in place** to stay current — this repo holds "how it is now", not history.

## Structure & navigation
- **`index.md` at every folder** — a "map of content": one line for the folder's purpose + a link to each child.
  It's the "start here" for that folder.
- **Up-link:** every non-index doc links back to its parent `index.md` (a breadcrumb near the top).
- **One concept per file** — small, single-purpose docs beat big rolling ones (cleaner diffs + retrieval).
- **Stable paths** — don't rename/move a doc others link to; if you must, leave a stub that redirects.
- Link by **relative path** in prose: `[text](../area/thing.md)`.

## Frontmatter (public subset)
Put small YAML frontmatter on every doc so it's machine-navigable:
```yaml
---
title: Add a connector
type: how-to        # tutorial | how-to | reference | explanation
area: connectors    # the module/area it covers
status: active      # draft | active | deprecated
updated: 2026-07-22
---
```
`deprecated` docs stay in git history but drop out of indexes.

## CLAUDE.md — the pointer layer
- **Root `CLAUDE.md` stays lean (< ~200 lines)** — build/test/style + a "where things live" map + links. It's
  read every turn, so it points **outward** (links / `@imports`), it is not a container for content.
- **Nest a `CLAUDE.md` in a subfolder only when its rules diverge** from root. Closest file to the edited file
  wins; nested files load on demand.

## Writing bar
- Specific & testable over aspirational ("Server components by default; add a client boundary only when needed"
  beats "write clean code").
- Show, don't just tell — examples/snippets for how-tos and references.
- Change behaviour → update its doc in the **same PR**.

## Before you finish a doc — checklist
- [ ] It's exactly one Diátaxis kind.
- [ ] Has frontmatter (`title` / `type` / `area` / `status` / `updated`).
- [ ] Up-links to its folder `index.md`; that `index.md` links to it.
- [ ] One concept; small; examples where useful.
- [ ] No secrets; public-safe.
