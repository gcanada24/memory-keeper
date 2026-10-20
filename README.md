# memory-keeper — portable cross-project memory for Claude Code

A small **Claude Code plugin** that packages a portable, git-backed, cross-project memory
*system* (the engine). Your actual memories are **not** in this plugin — they live in your
own private `~/.claude-memory` store. Share this plugin freely; your memories stay private.

> Works with **Claude Code** (Anthropic's official agent). Not related to bespoke API clients.

## What you get
Slash commands (instructions Claude follows) + the conventions:
- **`/memory-checkpoint [note]`** — distill the session's meaningful progress into the right
  per-work-thread worklog memory (`what → result → current state`, dated, never "solved"),
  refresh the `MEMORY.md` index, and commit the store.
- **`/memory-migrate [path]`** — mine legacy `.md`/`.rtf` "crutch" docs in the current repo into
  curated memory, then clean them up (archive untracked / `git rm` tracked, with confirmation).
  Never touches real reference docs (README/ARCHITECTURE/CONTRIBUTE/CHANGELOG/LICENSE).
- **`/memory-setup [remote-url]`** — set up or restore the store on a machine: clone/scaffold
  `~/.claude-memory`, recreate the symlinks in `links.map`, remap encoded paths for a new machine.

## Architecture (engine vs data)
- **Engine (this plugin):** conventions + commands. Portable, no personal data. Path-agnostic.
- **Data (`~/.claude-memory`):** your memories — a git repo of markdown. `global/CLAUDE.md` is the
  always-loaded switchboard; `projects/<repo>/memory/` holds per-repo depth. Claude Code's expected
  paths (`~/.claude/CLAUDE.md`, `~/.claude/projects/<encoded>/memory`) are **symlinks** into this store
  (see `links.map`). Back it up to a **private** remote (or iCloud/Dropbox); the `.jsonl` transcripts
  Claude Code already writes are the lossless backstop.

Two more commands, `/arbitrate` and `/memory-verify`, are parked in
`plugins/memory-keeper/commands-disabled/` because they depend on store files not shipped here
(`global/self-arbitration.md`, `bin/memory-consistency.py`). Move them back into `commands/` once
those exist.

## Install
```
/plugin marketplace add https://github.com/gcanada24/memory-keeper.git   # or a local clone path
/plugin install memory-keeper@claude-memory-marketplace
```
The marketplace manifest is `.claude-plugin/marketplace.json` at the repo root.

For instant use without the marketplace, the command files also work if copied to
`~/.claude/commands/` (user-global) or a repo's `.claude/commands/`.

## Windows notes
- Project folders under `~/.claude/projects/` are encoded by replacing every non-alphanumeric
  character with `-` (e.g. `C:\Users\me\Proj X` → `C--Users-me-Proj-X`).
- Project memory dirs are linked with **directory junctions** (`New-Item -ItemType Junction`),
  which need no admin rights. Git Bash `ln -s` silently copies instead of linking.
- `~/.claude/CLAUDE.md` is a one-line import (`@~/.claude-memory/global/CLAUDE.md`) instead of a
  file symlink.

## New machine / new user
1. `git clone <your-memory-store-remote> ~/.claude-memory` (or start fresh).
2. Run `/memory-setup` — it recreates the symlinks and remaps paths.
3. A fresh user: `/memory-setup` scaffolds `~/.claude-memory` + seeds `global/CLAUDE.md` from
   `plugins/memory-keeper/templates/global-CLAUDE.md`.

## Notes
- Commands are auto-discovered from `commands/*.md`. If your Claude Code version needs them
  declared in `plugin.json`, add a `commands` array; otherwise auto-discovery is enough.
- The store uses `commit.gpgsign=false` locally if your global git config signs commits (avoids
  headless-signing failures).
