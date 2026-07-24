# Contributing

Thanks for your interest! AchuLink is early — the most useful contributions right now are connectors, bug
reports, and feedback on the architecture.

## Getting started
1. _(Coming soon)_ `docker-compose up` for a local stack — see [`docs/self-hosting.md`](docs/self-hosting.md).
2. Read [`docs/architecture.md`](docs/architecture.md) to understand the parts.

## Ground rules
- Discuss big changes in an issue first.
- Keep modules small and well-bounded; add / adjust an ADR (`docs/adr/`) for architectural changes.
- **No secrets in commits.**

## AI-assisted contributing (optional)
If you use [Claude Code](https://claude.com/claude-code), this repo ships helpers:
- **`/commit`** — commit-message conventions (emoji + `category(module)` + What/Why/How) → `.claude/skills/commit/`
- **`/docs`** — how docs/ADRs are kept in step with code → `.claude/skills/docs/`
- **`contribution-reviewer` agent** — ask it to self-review your branch/diff against these rules before you
  open a PR → `.claude/agents/contribution-reviewer.md`

## Adding a connector
_Connector authoring guide coming as the engine stabilizes._
