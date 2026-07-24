---
name: pr-merge
description: Use when merging a pull request in this repo — but only after full diligence. Verifies the PR is truly ready (no unresolved comments, checklist fully ticked, all CI checks green, no conflicts, no outstanding change-requests) AND that the PR + its commits follow our conventions; violations get a detailed comment + change-request (rejection), readiness gaps get a pending-list report. Merges ONLY when everything is green. Never squashes — the clean commit series is the point.
disable-model-invocation: true
allowed-tools: Bash, Read, Grep, Glob
---

# pr-merge — diligence-gated merging

Merging is the **repo owner's decision** — this skill is invoked BY the owner and executes their diligence +
merge. It is never model-invoked and never called by another automation.

**Target resolution — never merge a randomly-picked PR:** an explicit PR number given by the owner → use it;
no number → the **current branch's** PR (`gh pr list --head <branch>`); neither exists → **list the open PRs
(number · title · branch · draft?) and ask the owner which** — ambiguity is a question, never a guess.

## The diligence gate — ALL must pass before merge
Run every check; collect results; decide once at the end.

| # | Check | How |
|---|---|---|
| 1 | **All CI checks green** | `gh pr checks <n>` — every required + non-required check passing (pending = not green) |
| 2 | **No unresolved review threads** | GraphQL: `reviewThreads(first:100){nodes{isResolved}}` — any `false` = open comment |
| 3 | **No outstanding change-requests** | `gh pr view <n> --json reviewDecision` — `CHANGES_REQUESTED` blocks |
| 4 | **Checklist fully ticked** | PR body: any `- [ ]` remaining = unverified item. (Ticks were evidence-backed by the `pr` skill — an open box means an unverified fact, not paperwork.) |
| 5 | **No conflicts / mergeable** | `gh pr view <n> --json mergeable,mergeStateStatus` |
| 6 | **Conventions — the PR as-per-expectation** | title matches `<emoji> <module>: <summary>[ (#N)]`; body has What/Why/How + Commits + checklist sections; description detailed enough to review without the diff |
| 7 | **Conventions — every commit** | each subject matches `<emoji> <category>(<module>): <summary>`; bodies carry What/Why/How (+ `Verified:` where applicable) — `git log --format=... base..head` |

## Three outcomes
- **✅ All green → merge:**
  ```bash
  gh pr merge <n> --merge --delete-branch     # merge commit — NEVER --squash (preserve the clean series)
  git checkout main && git pull                # sync local
  ```
  Report: merged SHA, branch deleted, anything post-merge (e.g. a check to promote to required).
- **⚠️ Ready-gaps (checks 1–5 fail) → DON'T merge, report pending:** a compact list — each failing check,
  what exactly is pending (which thread, which box, which CI job), and who/what unblocks it. No comment spam
  for ordinary pendings; the report is the output.
- **❌ Convention violations (checks 6–7 fail) → comment + reject:**
  1. Post ONE structured comment on the PR: every violation (bad title / bad commit subjects listed verbatim /
     missing body sections), with the expected format and a pointer to `.claude/skills/{commit,pr}/SKILL.md`.
  2. `gh pr review <n> --request-changes --body "<summary>"` — the formal rejection.
     **Own-PR fallback:** GitHub forbids change-requests on your own PR → post the comment, skip the review,
     **refuse to merge**, and say so in the report. (Fixing the commits = rebase on the feature branch by the
     PR author, then the `pr` skill updates the PR in place.)
- Mixed failures → do both reports; never merge.

## Rules
- **Never merge on partial green.** No overrides inside this skill — an override is the owner doing it by hand,
  deliberately.
- **Never `--squash` / `--rebase`:** commits are individually crafted (What/Why/How + Verified) — a merge
  commit preserves them; squashing destroys the evidence chain.
- **Never merge a draft.**
- Delete the remote branch on merge (`--delete-branch`); local cleanup in the report.

## Report format (always, whatever the outcome)
```
PR #<n> <title>
Outcome: MERGED <sha> | PENDING | REJECTED
Checks:  ✅ CI (5/5) · ✅ threads (0 open) · ❌ checklist (2 open) · …
Pending: - [ ] <item> — <what unblocks it>
```
