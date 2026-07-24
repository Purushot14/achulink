# AchuLink

Open-source integration & workflow-automation platform — connect your apps and automate work by wiring
**triggers** to **actions** through visual **flows**. Self-host it, or use the managed cloud.

> **Status: early / pre-MVP.** Building the walking skeleton (one trigger → one action, end to end).

## Why
Most automation tools are cloud-only, priced per task, and can't be self-hosted or extended. AchuLink is
open source and self-hostable — own your data, add your own connectors, and run it at scale.

## Features (target MVP)
- Connect apps (OAuth / API keys) → **connections**
- **Triggers:** webhook, polling, schedule / manual
- **Actions** with field **mapping** between steps
- **Flows:** trigger → actions
- Execution **engine** with **runs**, logs, and retries
- Self-host via `docker-compose`

## Stack
- **Backend:** Python 3.14 + [uv](https://docs.astral.sh/uv/) — API-first, runs **headless**.
- **Frontend:** React (SPA) — an independent client; the backend keeps working even if the UI isn't running.

## Docs
- Architecture → [`docs/architecture.md`](docs/architecture.md)
- Self-hosting → [`docs/self-hosting.md`](docs/self-hosting.md)
- Decisions (ADRs) → [`docs/adr/`](docs/adr/)
- Contributing → [`CONTRIBUTING.md`](CONTRIBUTING.md)

## License
[Apache License 2.0](LICENSE) — © 2026 Purushothaman Kumaravel and the AchuLink contributors.

## Quickstart
_Coming soon (`docker-compose up`)._
