# Global Memory & Working Agreement — <YOUR NAME> (loads in every Claude Code session)

<!-- TEMPLATE: /memory-setup seeds this. Fill in the bracketed parts. Keep it LEAN —
     it loads in every session. Depth lives in per-repo memory dirs. -->

This file is the always-on **switchboard**. It holds only cross-cutting essentials + pointers.
Per-repo depth lives in that repo's memory dir; read across repos on demand when a past solve is relevant.

## Who
<YOUR NAME> — <role / focus>. Works across several projects that dovetail (see the constellation below).

## How to work with me (standing preference)
<!-- Edit to taste. Example defaults: -->
- Default to thorough, proactive execution; surface genuinely destructive / irreversible / outward-facing actions before doing them.
- (Optional) For the heaviest multi-agent work, the lever is `ultracode`.
- I switch models freely (`/model`) — never assume a fixed model.

## Project constellation (cross-repo — recount past solves across these)
<!-- One bullet per repo/project: path, one-line what-it-is, and pointer to its memory dir. -->
- **<repo>** `</path/to/repo>` (memory keys to git-repo-root): <what it is>. Memory: `~/.claude/projects/<encoded-repo-root>/memory/`.
- **<other repo>** `</path>` (SEPARATE repo → own memory scope): <what it is>. Memory: `~/.claude/projects/<encoded>/memory/`.
- **Dovetail:** <how these projects relate / feed each other>.

## Memory conventions (apply everywhere)
- Record facts as **"what was done → result → current state,"** dated, falsifiable. **Never "solved!"** — a fact about what happened stays true; a claim about the present rots.
- Capture the **important curated bits only**; the Claude Code `.jsonl` transcript is the lossless backstop (never store whole conversations).
- Keep a dated living **worklog per work-thread** (resolved→conclusion, keep open items, remove superseded).
- **Checkpoint frequently** — after commits, validated results, and key decisions (`/memory-checkpoint`).
- This global file stays lean; promote only `user` / `feedback` / the constellation map here. Project/reference depth stays per-repo.

## Memory types (frontmatter `metadata.type`)
- `user` — who the user is (role, expertise, preferences).
- `feedback` — how I should work (corrections + confirmed approaches; include the why).
- `project` — ongoing work/goals/constraints not derivable from the code or git history.
- `reference` — pointers to external resources (URLs, dashboards, tickets).
