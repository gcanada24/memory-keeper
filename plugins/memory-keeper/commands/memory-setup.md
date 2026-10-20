---
description: Set up or restore the portable memory store on this machine (clone/scaffold ~/.claude-memory, recreate links from links.map, remap paths).
argument-hint: "[optional: git remote URL of the memory store]"
---

Set up or restore the Claude Code memory store on the current machine.

**Path encoding (all platforms):** Claude Code names a project's folder under `~/.claude/projects/` by taking the project root path and replacing **every character that is not a letter or digit with `-`**. macOS `/Users/me/repo` → `-Users-me-repo`; Windows `C:\Users\gcana\Documents\Claude Files` → `C--Users-gcana-Documents-Claude-Files`. When unsure, list `~/.claude/projects/` and match.

1. Ensure the store exists at `~/.claude-memory`:
   - If missing and a remote is provided ($ARGUMENTS) or known: `git clone <remote> ~/.claude-memory`.
   - If starting fresh: create `~/.claude-memory/{global,projects}`, seed `global/CLAUDE.md` from this plugin's `templates/global-CLAUDE.md` (fill in the user's identity + project constellation), create an empty `links.map`, then `git init`. If the user's global git signs commits, set `git -C ~/.claude-memory config commit.gpgsign false`.
2. Recreate the links listed in `~/.claude-memory/links.map` (format `<store_path> -> <claude_path>`, where `<claude_path>` starts with `~/.claude/`):
   - For each line: if `<claude_path>` already exists and is not the correct link, back it up as `<claude_path>.bak` (if it is an empty directory, just remove it); create the parent directory, then create the link:
     - **macOS / Linux:** `ln -s ~/.claude-memory/<store_path> <claude_path>`
     - **Windows:** use a **directory junction** (no admin or Developer Mode needed). From PowerShell:
       `New-Item -ItemType Junction -Path "<claude_path>" -Target "$HOME\.claude-memory\<store_path>"`
       Do NOT use Git Bash `ln -s` on Windows — without Developer Mode it silently makes a copy, not a link.
   - **PATH REMAP:** `<claude_path>` embeds this machine's home + project location in encoded form (see above). If the username, OS or project paths differ on this machine, rewrite that encoded segment in `links.map` before linking.
3. Link the global switchboard `~/.claude/CLAUDE.md` → `~/.claude-memory/global/CLAUDE.md`:
   - **macOS / Linux:** `ln -s ~/.claude-memory/global/CLAUDE.md ~/.claude/CLAUDE.md`
   - **Windows:** file symlinks need admin/Developer Mode, so instead write `~/.claude/CLAUDE.md` containing the single import line `@~/.claude-memory/global/CLAUDE.md`. Claude Code expands the import at load time.
   - Back up any existing `~/.claude/CLAUDE.md` to `.bak` first.
4. Verify: `~/.claude/CLAUDE.md` resolves (via link or import) and each project memory dir resolves through its link — e.g. write a temp file through the link and confirm it appears in the store, then delete it.
5. Report what was linked and anything that still needs manual path remapping.

Backend reminder (changeable anytime, non-destructive): the store can be local-only, git-backed on a PRIVATE remote (`git remote add origin <url>` + push), or synced via iCloud/Dropbox/OneDrive. Keep any remote private — memories may hold work context.
