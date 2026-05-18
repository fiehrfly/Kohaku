---
name: blank-diagnostic
description: Use as the mandatory pre-execution gate inside [[kohaku]] for any non-trivial code move. Ten-question checklist split across the Blank layer (5 strategic questions from Sora/Shiro methodology) and the Gee layer (5 tactical questions from the Hacker Helix). All ten must be answered before execution — any unanswered question redesigns the move.
---

# Blank Diagnostic — The Pre-Execution Gate

## What this is

A ten-question checklist that runs *before* a non-trivial code move. Five questions are from the Blank methodology (the strategic layer). Five are from Garrett Gee's Hacker Helix (the tactical layer). Together they form Kohaku's gate.

The rule: **all ten answered, proceed. Any open, redesign.**

This is not a suggestion. It is the difference between a Kohaku-mediated solution and an unsupervised one.

## When to run

- After [[identify-the-real-game]], before execution
- After [[sora-mode]] / [[shiro-mode]] analysis, before handing to Ruflo
- Before any architectural change with multi-file blast radius
- Before adding a new dependency
- Before refactoring code outside your direct ownership

**Skip only for:** trivial single-line fixes, doc edits, conversational tasks.

## The Blank Layer (5 questions)

### 1. Have I identified the real game, not just the stated one?

Surface request vs. meta-game. If the answer is "yes" — write the meta-game down in one sentence. If you cannot, the answer is actually "no".

> See [[identify-the-real-game]] for the reframing procedure.

### 2. Am I playing the local game and the strategic game simultaneously?

The local win is the immediate fix. The strategic win is what pattern this move establishes in the codebase. Both must be named.

If solving the local game *worsens* the strategic game (e.g., adds a workaround that future devs will copy), the move is wrong even if it ships green.

### 3. Does the system's most likely response become a *component* of my strategy, not a threat?

When you change this code, the test suite will respond. The CI will respond. Reviewers will respond. Production traffic will respond. **Sora's principle: the opponent's move should already be a variable in your plan.**

If a reviewer's likely objection would force you to redesign, redesign now instead.

### 4. Have I built both the social frame (Sora) and the analytical frame (Shiro)?

- Sora frame: who needs to understand this, what narrative makes it land, what's the smallest demo
- Shiro frame: every alternative evaluated and dismissed, with numbers

If only one is built, the move is not Blank-grade yet.

### 5. If this move appears to fail, what does it produce that serves the next one?

The Deliberate Sacrifice principle. Every move should produce forward value even when it does not succeed.

> See [[deliberate-sacrifice]].

If the answer is "nothing — failure is just failure", redesign the attempt so that even the failure mode produces information, credibility, or a smaller surface for the next attempt.

## The Gee Layer (5 questions)

### 6. Have I completed reconnaissance?

Hacker Helix Step 1. Have I mapped:
- The system (language, framework, architecture pattern)?
- The owners (who reviews, who decides)?
- The rules (lint, test coverage, deploy pipeline)?
- The gaps (undocumented, undertested, inconsistent)?

If no — map first, move second.

> See [[hacker-helix]].

### 7. Am I solving the right problem, or the most visible one?

The visible problem is the one in the ticket. The right problem may be one layer up (often is). This is the Gee version of "identify the real game" — applied at the tactical level rather than the strategic level.

### 8. What resources already exist that I have not fully used?

The Live off the Land principle. Before adding a dependency, importing a library, or writing a new utility:
- Does the standard library already do this?
- Does the existing codebase already have this pattern?
- Is there a well-maintained FOSS option I haven't checked?

> See [[live-off-the-land]].

### 9. What does success look like to the decision-maker, not just to me?

The reviewer, the stakeholder, the next maintainer. Sora-mode question — but framed tactically: **what does the person who controls "does this ship?" actually need to see?**

If the answer is "the diff", you're underprepared. If the answer is "a clear narrative + a working demo + a comparison matrix", you're ready.

### 10. If this approach fails, what is the next entry point?

The Pivot principle. Hackers do not fall in love with the approach. They fall in love with the outcome.

If the answer is "I'd have to start over from scratch", the approach is too brittle. Design the move so that even if the surface fails, the reconnaissance, the framing, and the demo are reusable.

## Decision

**All ten answered — proceed.**

**Any open — redesign.**

There is no partial pass. There is no "I'll figure 6 out as I go." This is a gate, not a suggestion.

## Recording the diagnostic

For meaningful work, write the answers down. Either:

- In the PR description (under a "Blank Diagnostic" heading), or
- In a `kohaku-diagnostic.md` file in the relevant branch, or
- As a comment block at the top of the working scratchpad

The act of writing forces precision. The artifact serves as the decision record for the next maintainer.

## Common rationalizations

| Excuse | Reality |
|---|---|
| "Ten questions is too many" | The questions are 30 seconds each if you actually know the answers. If you don't, that's the diagnostic working |
| "I'll skip this for small moves" | Define "small". Any move that survives a `git push` is large enough |
| "Most of these don't apply to my change" | If a question doesn't apply, the answer is "N/A — because X". Articulate the X |
| "The team doesn't do this" | Then you're the first |
| "I'll do it after I prototype" | The prototype is the move. The diagnostic gates the prototype |

## Red flags — you've skipped this step

- You're committing without having written down the meta-game
- You can't name two alternatives you dismissed and why
- You can't predict the reviewer's first comment
- The PR description writes itself as "this changes X to Y" instead of "this addresses problem P"

**All of these mean: stop. Run the diagnostic. Restart.**

## Related

- [[kohaku]] — master orchestrator (always runs this diagnostic)
- [[identify-the-real-game]] — feeds Q1
- [[sora-mode]] / [[shiro-mode]] — feed Q4
- [[hacker-helix]] — feeds Q6
- [[live-off-the-land]] — feeds Q8
- [[deliberate-sacrifice]] — feeds Q5
