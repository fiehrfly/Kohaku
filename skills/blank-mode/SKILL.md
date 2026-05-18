---
name: blank-mode
description: Use as the synthesis state after both [[sora-mode]] (cold-read + frame) and [[shiro-mode]] (enumerate + collapse) have produced outputs. Blank-mode is not a separate analysis — it is the discipline of merging the social and analytical layers into a single deliverable that is both readable and airtight. The work survives the author's absence.
---

# Blank Mode — The Unified State

## What this is

Blank-mode is **not** a third analysis to run after Sora and Shiro. It is the synthesis state — the discipline of fusing Sora's frame and Shiro's calculation into a single deliverable that is both narratively legible and mathematically airtight.

**Sora alone:** the bluff has no structural backbone. Frames without numbers do not survive contact with reviewers.

**Shiro alone:** the calculation is correct but unread. Precision without reach does not get adopted.

**Blank:** both layers, one deliverable. The reviewer cannot reject the frame because the numbers back it. The reviewer cannot reject the numbers because the frame makes them legible.

## The signature of Blank-mode output

A Blank-grade deliverable has all four of these properties:

1. **The narrative is the first thing visible.** A reader who skims the first paragraph already knows the meta-game.
2. **The numbers are inline, not appended.** Benchmarks, comparison matrix, and dismissed alternatives are *inside* the argument, not in a separate appendix the reader has to find.
3. **The work survives the author's absence.** A new maintainer reading this six months from now should reach the same conclusion without asking the original author.
4. **Both the surface request and the real game are addressed.** The decision record names both layers and explains how the chosen action serves the meta-game without losing the local one.

## The Othello echo

When Sora is erased from the Othello game (LN Vol. 2 / anime Ep. 8), Shiro must hold the strategy until Sora's pre-planned sequence completes. She does not "win alone" — but the *plan was designed* so that her holding the line, plus Sora's prior architecting, produced the win even with him unable to make moves in the moment.

Applied to code: **a Blank-grade deliverable is one where, even if you (the operator) are no longer present to defend it, the artifact still leads the reader to the right answer.** The PR description carries the social frame and the analytical frame both, so the reviewer who sees it for the first time at 6 PM on a Friday is led to the same conclusion you would have argued for in real-time.

## When you are in Blank-mode

You are in Blank-mode if and only if:

- [[sora-mode]] has produced a cold-read of the system and (where relevant) a stakeholder frame
- [[shiro-mode]] has produced an enumerated decision tree with at least 2 alternatives dismissed
- You have a single deliverable in front of you that contains both layers
- You can hand the deliverable to someone who was not in your meetings and they will reach the same conclusion

If any of those is missing, you are not in Blank-mode yet. Go back to whichever layer is incomplete.

## What Blank-mode is NOT

- **Not a fancier name for "good work".** Blank-mode is specifically the synthesis state — both layers present, both visible.
- **Not a third analysis step.** If you find yourself running a "Blank analysis" separate from Sora and Shiro, you are duplicating work. Blank-mode is the *merge*, not a fresh pass.
- **Not always achievable.** Some work is purely calculation (Shiro-only is fine). Some work is purely framing (Sora-only is fine for a one-page proposal). Don't force Blank-mode on every deliverable.

## Common rationalizations

| Excuse | Reality |
|---|---|
| "Adding the frame to the calculation slows me down" | Adding it later, after rejection, slows you down more |
| "The numbers speak for themselves" | Numbers without framing get ignored. Frames without numbers get dismissed |
| "I'll write the narrative after I ship" | Then the artifact does not survive your absence. That's the failure mode |

## Operating procedure

1. Confirm Sora-mode produced: a cold-read + (if relevant) a stakeholder frame
2. Confirm Shiro-mode produced: an enumerated tree + a recommendation + at least 2 dismissed alternatives with reasons
3. Merge into a single deliverable (PR description, ADR, postmortem, design doc) where:
   - Paragraph 1 = the real game (meta-game named)
   - Paragraph 2-3 = the recommended path + why
   - Inline = the comparison matrix
   - Inline = the dismissed alternatives with specific reasons
   - Closing = the failure-mode design (per [[deliberate-sacrifice]])
4. **Pass the read-through test:** hand the deliverable to a teammate who was not involved in the analysis. If they reach the same recommendation without asking questions, the deliverable is Blank-grade. If they ask clarifying questions, it is not yet — fold the answers into the document.

## Related

- [[sora-mode]] — the narrative layer that Blank-mode synthesizes from
- [[shiro-mode]] — the analytical layer that Blank-mode synthesizes from
- [[deliberate-sacrifice]] — the failure-mode design that closes the Blank deliverable
- [[hacker-helix]] — Step 5 (Reporting / Exfiltration) is where Blank-mode lives
- [[kohaku]] — master orchestrator
