---
name: commit
description: Use when creating git commit(s) in this repository. Splits changes into logical groups and commits them one-by-one — each commit gets an `<emoji> <category>(<module>)` subject and a What/Why/How body. Auto-detects the module from the changed file paths, self-extends its taxonomy when a genuinely new category/module appears, and never lumps unrelated changes into one commit.
---

# commit — logical, well-described git commits

Turn a messy set of working changes into a series of small, logically-grouped commits, each with a clear
**`<emoji> <category>(<module>)`** subject and a **What / Why / How** body.

> This repo is **public / open-source** — write commits a stranger can understand, and **never commit secrets**.

## Golden rules
1. **Never** `git add -A && git commit` everything into one commit. Group first, then commit one by one.
2. **One commit = one logical concern**, ideally **one module**. If one file serves two concerns, split it with
   `git add -p`.
3. Every commit subject is **`<emoji> <category>(<module>): <summary>`**, with a **What / Why / How** body.
4. Show the **grouping plan to the user before committing**. Adjust on feedback. (Skip the wait only if the user
   said "just commit".)
5. Public repo → scan the diff for **secrets/keys/tokens** before committing.
6. Keep this skill **current**: add a new category/module when one genuinely appears (see "Extending this skill").
7. **Docs before commit** — first update the docs these changes affect (via the `docs` skill); commit code and its
   docs **together**. Nothing lands with stale docs.

## Docs first — always
**Before** grouping or committing, update the docs these changes affect — follow the **`docs` skill**
(`.claude/skills/docs/SKILL.md`). Those doc edits then join the commit grouping below. Proportionate: a trivial fix
may touch no docs; a feature updates its reference / how-to / architecture. **No commit lands with stale docs.**

## Workflow
0. **Update docs first** (see above) — do this before anything else, so doc changes are part of the grouping.
1. **See everything** — `git status`, `git diff`, `git diff --staged`. Review *all* changes, tracked + untracked.
2. **Detect the module(s)** — from the changed file *paths* (see "Module / scope"). No fixed list; the code tree
   is the source of truth.
3. **Group into logical units** — cluster by concern **and** module. Typical groups: feature, bug fix, refactor,
   lint/format fix, new constants/config, dependency change, tests, docs.
4. **Order the commits** — usually: config/deps → constants/types → core change → tests → docs (or dependency
   order, so each commit builds on its own).
5. **Present the plan** — list each group with: its files/hunks, chosen `emoji category(module)`, and a one-line
   summary. Wait for the user's OK.
6. **Commit each group, one by one**
   - Stage only that group: `git add <paths>` (or `git add -p <file>` for partial hunks).
   - Confirm nothing extra is staged: `git diff --staged --stat`.
   - Commit using the template below.
7. **Verify** — `git log --oneline -n <k>` to confirm the series reads well.

## Commit message template
```
<emoji> <category>(<module>): <imperative, ≤ ~50 char summary>

Why:  <the motivation — problem solved, goal, or trigger>
What: <the concrete change>
How:  <the approach — key decisions / notable details; use bullets for multi-part changes>

- <optional headline-outcome bullets; inline emoji ok, e.g. 🔢 15 shapes scrubbed (was 3)>

Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>
```
- Subject in the **imperative** ("add", "fix", "rename") — not past tense.
- Body: 1–3 lines each for a small change; **expand freely** (paragraphs + bullets + outcome bullets) for a
  substantial one, like the rich example below. Don't pad trivial changes.
- Drop the `Co-Authored-By` line if you don't want AI attribution in this repo.

## Module / scope — dynamic, from the code
The **module** names the part of the codebase a commit touches. There is **no fixed list** — derive it from the
**paths of the changed files**, so it always matches the real code.

- **Detect it:** take the top-level component under the source root of the changed files.
  - `backend/<module>/…` → `<module>` (e.g. `backend/engine/runner.py` → `engine`)
  - `frontend/<area>/…` → `<area>` (or `ui` for cross-cutting UI)
  - repo-root tooling (pyproject, docker, CI) → `config` / `build` / `ci`, or omit the scope
- **No code yet?** Then there are no modules yet — **omit `(<module>)`** until the codebase has structure.
  Modules appear automatically as folders/packages appear.
- **One module per commit.** If a group spans modules, that's usually a sign to split. For a genuinely
  cross-cutting change, use the most-affected module or omit the scope.
- **Record new ones:** the first time you use a module, append it to **Known modules** at the bottom (bookkeeping
  only — the code tree is always authoritative).

## Emoji + category reference
Pick the row that matches the commit's **primary intent**.

### Features & fixes
| Emoji | Category | Use when |
|-------|----------|----------|
| ✨ | feat | New feature / capability |
| 🐛 | fix | Fix a bug |
| 🩹 | fix-minor | Small, non-critical fix |
| 🚑 | hotfix | Critical / urgent fix |
| 🔒 | security | Fix a security issue |
| 💥 | breaking | Introduce a breaking change |
| ⏪ | revert | Revert a previous change |

### Code quality
| Emoji | Category | Use when |
|-------|----------|----------|
| ♻️ | refactor | Restructure code, no behaviour change |
| ⚡ | perf | Improve performance |
| 🎨 | style | Formatting / structure (ruff format, black, prettier) — no logic change |
| 🚨 | lint | Fix linter / type-checker warnings (**ruff**, eslint, mypy) |
| 🔠 | constants | Extract/define constants; remove magic numbers/strings |
| 🏷️ | types | Add / adjust type hints or type definitions |
| 🦺 | validation | Add input validation |
| 🥅 | errors | Add / improve error handling |
| 🔊 | logs-add | Add logging |
| 🔇 | logs-remove | Remove logging |
| 💡 | comments | Add / adjust code comments |
| 🔥 | remove | Remove code or files |

### Tests & docs
| Emoji | Category | Use when |
|-------|----------|----------|
| ✅ | test | Add / fix passing tests |
| 🧪 | test-failing | Add a (currently failing) test |
| 📝 | docs | Documentation (README, docs/, ADRs) |
| ✏️ | typo | Fix typos / wording |

### Deps, config & tooling
| Emoji | Category | Use when |
|-------|----------|----------|
| ➕ | dep-add | Add a dependency |
| ➖ | dep-remove | Remove a dependency |
| ⬆️ | dep-up | Upgrade dependencies |
| ⬇️ | dep-down | Downgrade dependencies |
| 🔧 | config | Config files (pyproject, tool config, .env.example) |
| 🔨 | scripts | Dev scripts / build tooling |
| 📦 | build | Packaging / build |
| 🧹 | chore | Housekeeping (incl. updating this commit skill) |

### Infra, CI & release
| Emoji | Category | Use when |
|-------|----------|----------|
| 👷 | ci | CI pipeline changes |
| 💚 | ci-fix | Fix failing CI |
| 🧱 | infra | Infrastructure (docker, deploy manifests) |
| 🚀 | deploy | Deployment |
| 🔖 | release | Tag a release / bump version |

### Data
| Emoji | Category | Use when |
|-------|----------|----------|
| 🗃️ | db | DB schema / migrations |
| 🌱 | seed | Seed / fixture data |

### Meta
| Emoji | Category | Use when |
|-------|----------|----------|
| 🎉 | init | Initial commit / start of a module |
| 🚧 | wip | Work in progress (avoid on shared branches) |
| 🔀 | merge | Merge branches |
| 📄 | license | Add / change license |

### Product-specific (AchuLink) — extend freely
| Emoji | Category | Use when |
|-------|----------|----------|
| 🔌 | connector | New/updated connector or connection logic |
| ⚙️ | engine | Execution engine / run handling |
| 🪝 | webhook | Webhook trigger / ingest |
| ⏱️ | scheduler | Polling / scheduled triggers |
| 🗺️ | mapping | Field mapping / transforms between steps |

## Extending this skill (self-update)
This skill is **self-extending** — keep it current as you commit:
- **New category:** if a change fits no row and you're confident it's a genuinely new, useful case (not a
  near-duplicate), **add a row** to the right table in this file (Edit it) — a clear emoji + short lowercase slug —
  then use it.
- **New module:** add it under **Known modules**.
- Commit these skill edits **in the same run** as their own commit, e.g.
  `🧹 chore(commit-skill): add <slug> category`.
- Be conservative — only add when it clearly earns a place. **Reuse before inventing.**

## Known modules
_(Grows as the codebase does. Empty for now — no code yet, so omit `(<module>)`.)_
- _(none yet)_

## Worked example (with modules + a rich body)
Working tree: a new webhook trigger, a secret-scrub hardening in connections, magic numbers → constants, and a
README update → **four concerns → four commits**, in build order:

1. `🔠 constants(engine): extract retry/backoff limits to named constants`
2. `✨ feat(webhook): add webhook trigger endpoint for flows`
3. `🔒 security(connections): scrub connection secrets from run logs`
4. `📝 docs(readme): document the webhook trigger`

A **rich body** (commit 3) — expand when the change warrants it:
```
🔒 security(connections): scrub connection secrets from run logs

Why:  run logs echoed connection payloads verbatim — tokens/passwords could
      leak into stored run history and any log sink.
What: redact secret-bearing fields before a run record or log line is written;
      also stop flattening a structured `msg` payload into a string.
How:
  - redact keys: token / apikey / clientsecret / authorization / password / sas
  - value shapes: bare, quoted, and `Authorization: Bearer <jwt>` (matched first)
  - URI userinfo redacted whole (`://:pass@`, raw `@` inside the password)

- 🔢 all known secret shapes now scrubbed at the log boundary
- ✅ run history no longer stores raw credentials

Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>
```

Each committed separately with its own body — **not** one big "update stuff" commit.
