---
name: pr
description: Use when raising or updating a GitHub pull request for this repo. Generates the PR from the branch's commits (which follow the commit skill's format) — emoji + module title, detailed What/Why/How description with examples when needed, and a DYNAMIC verification checklist derived from what the PR actually touches, pre-ticking items already verified. Also updates an existing PR in place when new commits land. Never merges — merging is the repo owner's call.
disable-model-invocation: true
allowed-tools: Bash, Read, Grep, Glob
---

# pr — raise & update pull requests (from clean commits)

The commits are the source of truth: this repo's `commit` skill already produced clean
`<emoji> <category>(<module>): <summary>` commits with What/Why/How bodies — the PR is **generated from them**,
never written from memory.

## Title
```
<emoji> <module>: <summary>                     e.g.  ✨ engine: webhook trigger end-to-end
<emoji> <module>: <summary> (#123)              when it follows a task/issue — ALWAYS record it
```
- **module** = the high-level area the PR is about (`engine`, `connector`, `api`, `ui`, `docs`, `agents`,
  `build`…). Multi-module → the dominant one, or `<m1>+<m2>` for a true pair.
- **emoji** = the branch's dominant commit emoji (most-significant change wins: feat ✨ > fix 🐛 > docs 📝 > chore 🧹).
- Follows a task/issue → `(#N)` in the title **and** `Closes #N` / `Refs #N` in the body.

## Description (generated from the commits)
```markdown
## What
<one paragraph — what this PR delivers, synthesized from commit Whats>

## Why
<the intent — from commit Whys; link the issue/task if any>

## How
<key approach points — from commit Hows; call out anything reviewers must know>

## Example            <!-- include when it clarifies: new API → request/response; new skill → invocation; UI → before/after -->
<concrete usage example>

## Commits
<the `git log --oneline main..HEAD` list — the PR's table of contents>

## Verification checklist
<the DYNAMIC checklist — see below>
```
Detail beats brevity here: a reviewer must understand the PR **without opening the diff**. Add an example
whenever the change is usable (API, CLI, skill, component).

## The dynamic checklist — derived, then pre-ticked
**Derive** items from what the diff actually touches (not a fixed list):

| PR touches | Checklist items it generates |
|---|---|
| any code | build passes · lint clean · tests added/updated · all tests green |
| public API | API docs updated · backward-compat considered · example in description |
| engine / runtime | idempotency/retry behavior stated · run logging intact |
| connector | framework contract tests pass · vendor quirks recorded |
| UI | degrades gracefully w/o backend · design spec matched · states (empty/loading/error) covered |
| docs only | links valid · matches current behavior · ADR added if a decision changed |
| security-adjacent (auth, secrets, tenancy) | security review done · no secrets in diff (state scan result) |
| CI / build | pipeline green on this branch |
| always (last items) | commits follow the commit-skill format · docs updated with the change |

**Pre-tick (✅) what is already verified — with the evidence source.** Primary source: the commits'
**`Verified:` trailers** (the commit skill records tests/scan/docs evidence at commit time — read it verbatim);
plus CI runs and review sign-offs. Leave genuinely-unverified items unticked (⬜) — an unticked box is
information, not shame. Never tick without evidence: a ticked-but-false box is worse than no checklist.

```markdown
- [x] Tests green — `pytest` 42 passed (CI run #18)
- [x] No secrets in diff — scanned at commit time
- [ ] API docs updated — pending
```

## Raise / update
```bash
# raise (from the current feature branch; never from main)
gh pr create --title "<title>" --body-file <generated-body.md> --base main
# update — when new commits land or checklist state changes: regenerate body from commits,
# KEEP existing ticks whose evidence still holds, add items the new commits introduce
gh pr edit <number> --title "<title>" --body-file <regenerated-body.md>
```
- **Update, don't recreate:** one branch = one PR, edited in place as it evolves.
- **Never merge, never touch `main`** — merging is the repo owner's decision, always.
- A PR whose commits don't follow the commit-skill format: **fix the commits first**
  (interactive rebase on the feature branch), then raise — the conventions check will block it anyway.

## Enforcement (why non-conforming PRs get blocked)
CI (`.github/workflows/pr-conventions.yml`) validates the PR title and every commit subject against the
format above and fails otherwise; with the check marked **required** in the repo ruleset, GitHub physically
blocks merging non-conforming PRs. Contributors: use the `commit` skill and this skill and you'll never see it fail.
