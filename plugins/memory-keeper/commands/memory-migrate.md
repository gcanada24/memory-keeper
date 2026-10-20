---
description: Migrate legacy .md/.rtf context ("crutch") docs in this repo into curated memory, then clean them up (archive untracked / git rm tracked, with confirmation).
argument-hint: "[optional: path or glob to scan]"
---

Migrate hand-written context docs in the current repo into the memory system, following the conventions in `~/.claude/CLAUDE.md`.

Pipeline:
1. **DISCOVER** candidate docs ($ARGUMENTS if given, else scan the repo for `*.md`/`*.rtf`, excluding vendor dirs).
2. **CLASSIFY** each as KEEP (durable reference — README, ARCHITECTURE, CONTRIBUTE, CHANGELOG, LICENSE, API/user guides) vs CRUTCH (session artifacts — `*_SUMMARY`, `*_STATUS`, `*_ANALYSIS`, `*_TRANSCRIPT`, `TODO`, `DEBUG_SESSION`, `*_FIX_NOTES`, `IMPLEMENTATION`, `FUTURE_*`). **Never modify or delete real reference docs.**
3. **EXTRACT** net-new durable facts from crutch docs (dedup against existing memory), rewriting "SOLVED!" → "what → result → current state," dated, falsifiable. Discrete facts → fact files; narrative → per-work-thread worklog. Link into `MEMORY.md`.
4. **CLEANUP** (confirm with the user before deleting): archive UNTRACKED crutch docs into `~/.claude-memory/_migrated_docs/<repo>/...` then remove; for TRACKED crutch docs use `git rm` + commit (recoverable from history). For branches that aren't checked out, use a temporary `git worktree` so the user's active working tree is never disturbed. Never `git rm` on the user's active checkout without saying so.
5. Commit the store.

For large doc sets (dozens), fan this out as an **ultracode workflow**: one agent per doc to classify+extract (dedup-aware), then a synthesis stage that proposes the memory files + keep/delete lists.
