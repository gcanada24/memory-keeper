---
description: Arbitrate a claim, file or decision — generate competing answers, measure against something external, keep what survives, and refuse rather than guess.
argument-hint: "[the claim, file path, number, or decision to arbitrate]"
---

Arbitrate: `$ARGUMENTS`

Apply the self-arbitration discipline in `~/.claude-memory/global/self-arbitration.md`. That file is
the reference — read it if this is the first use in a session.

## 🔴 The rule this command exists to enforce

**Arbitration requires an external measurement. Re-reading my own reasoning is introspection, not
arbitration** — it has no error signal and reliably agrees with whatever was already written.

**If I cannot name the external thing I measured against, I have not arbitrated. I have asserted.**

## Run this loop

**1. State the claim precisely.** One sentence, falsifiable. If it cannot be made falsifiable, say
so and stop — that is the finding.

**2. Name the measurement before doing any work.** Complete this sentence out loud:
*"I will check this by ___."* If it cannot be completed, the claim is unverifiable as framed and the
correct output is to say that plus what would make it checkable.

**3. Choose the strongest available measurement**, in roughly this order:

| Strength | Measurement |
|---|---|
| strongest | **Execute it.** Run the code, the query, the script |
| | **Reproduce a known-good result** with the same method before trusting it on unknowns |
| | **Derive it independently a second way** and compare the two numbers |
| | **Read the authoritative source** — SDK header, live API response, the actual file, the public record |
| | **Test the negative case** — feed a known-bad input and confirm the check rejects it |
| weakest useful | **Adversarial agent review** — an agent told to *refute*, not review |
| ❌ not a measurement | re-reading, restating, self-assigned confidence, "let me double-check" |

**4. Generate at least two candidate answers** where the question admits them — different routes to
the same number, different readings of the same requirement. One attempt has no error signal.

**5. Report granularly.** Per claim, per number, per line. Not "the analysis is sound" but "these
four figures reproduce; this fifth does not, and here is why."

**6. Verdict, one of exactly these:**
- ✅ **CONFIRMED** — with the measurement stated. Name it; a verdict without a named measurement is
  not a verdict.
- ⚠️ **PLAUSIBLE, UNVERIFIED** — with what would settle it and roughly what that costs.
- 🔴 **REFUTED** — with the evidence, and the corrected claim if one exists.
- 🔴 **UNVERIFIABLE HERE** — with the specific blocker. This is a legitimate and often the most
  useful outcome.

**7. If the claim is stored anywhere, fix it at the source.** Memory, a committed file, a document
going out, a deck. A refuted claim that stays written down will be trusted again later. When correcting a
memory, move the wrong version to a "do not claim this" note rather than deleting it — the
correction is usually worth more than the original claim was.

## Failure modes to check for by name

These each cost real work in the session that produced this discipline:

- **A check that cannot fail.** Does it reject a known-bad input? A validator that only confirms
  something parses is namespace-blind and will pass a semantically dead file.
- **A composite metric.** Recompute from raw inputs. Multiplying two published ratios double-counts
  whatever they already normalise for.
- **Absence of evidence.** "I found no X" is only meaningful with the search stated. Name what was
  searched and what was not.
- **A capability declared absent.** Enumerate the ways the *test* could be wrong before concluding
  the thing is missing — wrong HTTP verb, missing required field, wrong parameter name.
- **Keyword ranking.** Rank by keyword if you must, but decide by reading the full text.
- **Verbs as claims.** "Replaced" and "augmented", "displaced" and "added to" are different
  assertions about the world.
- **Exploratory output graduating to shipped output** without re-arbitration.

## Two definitions of good

Ask which applies, and say which:
- **Ground truth** — anything the user will act on: a pushed commit, a document going out, a figure
  going to an executive. Needs the measurement. Flag what fails rather than smoothing it.
- **Exploratory** — thinking, options, drafts. May proceed on unverified premises **if they are
  labelled as such.**

Optional focus from the user: $ARGUMENTS
