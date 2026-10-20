---
description: Checkpoint recent work into the right worklog memory (what→result→state, dated, never "solved"), update the index, and commit the store.
argument-hint: "[optional: work-thread name or note]"
---

Checkpoint the current session's meaningful progress into the persistent memory store, following the conventions in `~/.claude/CLAUDE.md`.

Steps:
1. Identify the active work-thread(s). Resolve the target project's memory dir: `~/.claude/projects/<encoded-project-root>/memory/` where `<encoded-project-root>` is the project root (git repo root, or the session's working directory if it isn't a repo) with **every character that is not a letter or digit replaced by `-`**. Examples: macOS `/Users/me/repo` → `-Users-me-repo`; Windows `C:\Users\gcana\Documents\Claude Files` → `C--Users-gcana-Documents-Claude-Files`. If unsure, list `~/.claude/projects/` and pick the match. These dirs are links (symlinks on macOS/Linux, directory junctions on Windows) into `~/.claude-memory`; check `~/.claude-memory/links.map` to find the store folder (`<PROJ>`) for this project.
2. For each thread, update its **per-work-thread worklog** memory file (create if new) with a dated entry: **what was done → result → current state**, plus decisions and OPEN questions. Rules: falsifiable; **never "solved!"**; collapse resolved back-and-forth to its conclusion; remove superseded claims; keep open items. Capture only the important curated bits — the Claude Code `.jsonl` transcript is the lossless backstop.
3. Add or refresh the one-line pointer in that repo's `MEMORY.md` index.
4. Link related memories with `[[name]]`.
5. Commit the store (do NOT push — the user pushes). **Scope the commit to this project's own memory subtree** — never `add -A`:
   ```
   PROJ=<project folder under ~/.claude-memory/projects, e.g. capture>
   git -C ~/.claude-memory add -- "projects/$PROJ/memory"
   git -C ~/.claude-memory -c commit.gpgsign=false commit \
       -m "checkpoint($PROJ): <short summary>" -- "projects/$PROJ/memory"
   ```
   Also stage `global/CLAUDE.md` in the same command **only** if you deliberately changed it.

   **Why scoped, not `add -A`:** all projects share ONE git repo (`~/.claude-memory/.git`),
   so `add -A` stages the whole store. Verified 2026-08-03: a capture checkpoint using
   `add -A` swept a *concurrent SpatialVue session's half-written draft* into capture's
   commit. No data is lost, but the history stops being trustworthy — and that is the one
   thing a memory store cannot afford. Scoping also makes `git log` readable per project.

   **If git reports `Unable to create '.git/index.lock': File exists`,** another session is
   committing at that instant. Wait ~2s and retry (up to ~3 times); the memory files are
   already safely on disk, so a retry is all that is needed. Low likelihood (~1 in 2400 per
   checkpoint at a few-minute cadence) but silent if unhandled — an unretried failure leaves
   the memory unversioned until the next checkpoint sweeps it up.

Optional focus / note from the user: $ARGUMENTS
