---
description: Sweep stored memory for claims that have gone stale — test them against the current state of the world, then fix whichever is actually wrong.
argument-hint: "[optional: a specific claim or area to focus on]"
---

Run the memory consistency sweep, then act on what it finds.

    python3 ~/.claude-memory/bin/memory-consistency.py

This is the "measure against something external" rung of the self-arbitration ladder
(`~/.claude-memory/global/self-arbitration.md`), applied to memory itself. 🔴 **Stale memory is
worse than absent memory, because it is trusted.**

## Then, for each failure

1. 🔴 **Decide whether the MEMORY is stale or the CHECK is wrong.** Both happen. On the sweep's
   first run it flagged `week-of-code-2026` as missing from `links.map` when that absence was
   deliberate, and flagged `dcam-patent-*.md` for containing "patent holder" when that file is the
   record of the correction.
2. **Fix the one that is actually wrong**, at the source — the memory file, or the check.
3. **If the check was wrong, record why in it**, so the same false positive is not rediscovered.
4. **If a claim was wrong, do not just delete it** — move it to a "do not claim this" note. The
   correction is usually worth more than the original claim was, because it records a way of being
   wrong that will recur.
5. **Extend the sweep** whenever a new falsifiable claim gets recorded. A claim nobody can test is
   a claim nobody will notice going stale.

## Worth checking by hand too, since the script cannot

- Do any memories contradict each other? Two files asserting different values for the same fact.
- Does a memory reference a file, function or flag that no longer exists?
- Is anything recorded as "open" or "pending" that has since been resolved?
- Is anything recorded as a plan that was superseded and never marked so?

Optional focus from the user: $ARGUMENTS
