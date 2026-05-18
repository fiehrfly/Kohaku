---
name: deliberate-sacrifice
description: Use when designing any move where the outcome is uncertain, the stakes are high, or the approach is novel. Before committing to a plan, require that even apparent failure produces forward value — credibility, information, a smaller surface for the next attempt, or a reusable artifact. If the failure mode produces nothing, redesign the move. Counter-intuitive principle: the best moves are the ones that win even when they "lose".
---

# Deliberate Sacrifice

## The principle

The most counter-intuitive Blank principle: **design every move so that even the apparent failure mode produces forward value.**

If a PR is rejected, the work should still have demonstrated rigorous thinking. If a prototype fails, it should still have proven an assumption false (which is the fastest path to the right solution). If a recommendation is overruled, the analysis should still serve the next debate.

**A move that produces nothing when it fails is poorly designed, regardless of whether it succeeds.**

## The NGNL Zero precedent

In *No Game No Life: Zero* (2017 film, set 6,000 years before the main series), Riku (the human strategist) and Schwi (the Ex-Machina computational core) are the structural parallels to Sora and Shiro. They face an existential war and design a plan that requires Schwi to operate beyond survival limits.

Schwi is mortally damaged. In her dying moments — not posthumously — she reconnects to the Ex-Machina hive-mind and transmits the plan's final step, recruiting the hive-mind into Riku's hand. She uses her last energy to protect his wedding ring. Riku then completes the plan and dies reaching for the Suniaster; Tet claims it and becomes the One True God of Disboard.

The pattern: **Schwi's contribution did not require her continued presence to land. The plan was designed so that her sacrifice produced the win even when she could no longer see it.**

This is Shiro-mode at its limit: building something that holds after the operator is gone. The calculation does not need the calculator to be present.

Applied to coding: **the best PRs, ADRs, and architecture decisions are the ones that continue to work after the author has left the project.** This is the Deliberate Sacrifice writ small.

## When to apply

Before committing to any move where:
- The outcome is genuinely uncertain (new tech, novel approach, high political surface)
- The cost is non-trivial (large PR, long meeting, vendor commitment)
- Failure modes are multiple (not just "it works or it doesn't")
- The stakes affect the team or the strategic direction

## Operating procedure

### Before the move — design the failure mode

Ask, *before committing*:

1. **If this PR is rejected, what does it still produce?**
   - Did it demonstrate the alternative options exist?
   - Did it surface a hidden assumption?
   - Did it document the current behavior in a way the team needed?

2. **If this prototype proves the approach is wrong, what does it still produce?**
   - Did it falsify a load-bearing assumption?
   - Did it bound the problem ("we now know X is impossible under constraint Y")?
   - Did it produce reusable components for the alternative approach?

3. **If this recommendation is overruled, what does it still produce?**
   - A documented comparison matrix the next debater can use
   - A record of the trade-off so the team doesn't re-litigate it
   - Credibility for being thorough — bankable in the next decision

4. **If this prediction is wrong, what does the attempt still teach?**
   - The variable I didn't account for
   - The signal I should monitor going forward
   - The model update I now need

### The redesign rule

**If the answer to any of the above is "nothing — failure is just failure", redesign the move.**

Specifically:
- Make the PR's *analysis* land even if the *code* doesn't (write the comparison matrix into the PR description)
- Make the prototype's *learnings* durable even if the *prototype* dies (a scratch repo with a README explaining what was tried and what it disproved)
- Make the recommendation's *framing* useful even if the *conclusion* is rejected (document the alternatives, not just the recommended path)

## Examples from coding work

### Example 1 — The rejected PR

A developer proposes refactoring an auth module. Reviewer rejects: "scope is too large, defer."

**Poor sacrifice design:** The branch is abandoned. Three weeks of investigation evaporate.

**Good sacrifice design:** The PR description already documented:
- Why the current module is brittle (with specific incident references)
- What three alternative refactor paths were considered
- Why the chosen path was selected
- The exact line ranges that change and the test surface

Now even with rejection, the team has a durable artifact. Next quarter, when the refactor *is* prioritized, the work resumes from a position of knowledge, not from zero.

### Example 2 — The failed prototype

A team prototypes a feature using framework X. Two weeks in, it's clear framework X has a fundamental incompatibility.

**Poor sacrifice design:** Delete the branch. Email "framework X didn't work."

**Good sacrifice design:** Write a `prototype-postmortem.md`:
- The specific incompatibility (with code samples)
- The constraint of framework X that we missed in initial recon (Helix Step 1)
- What we would do differently next time
- Which assumptions are now proven false

The "failure" is now a permanent contribution to the team's collective knowledge. The next person to consider framework X for a similar use case finds the doc and saves two weeks.

### Example 3 — The overruled architecture proposal

You propose adopting a new database. The team selects a different option.

**Poor sacrifice design:** Move on quietly.

**Good sacrifice design:** Even after the decision, publish the analysis:
- The specific use cases where the rejected option would have won
- The signal monitors that would suggest revisiting the choice
- The migration path estimate, if a future revisit becomes necessary

Now in 18 months, when scale or use case shifts, the team has the analysis already in hand. The "loss" produced an option for the future.

## Common rationalizations

| Excuse | Reality |
|---|---|
| "I'll just see what happens" | Hope is not a sacrifice plan. Design the failure mode |
| "It's fine if it fails — I learned something" | Internal learning is not durable. Make it external |
| "Writing up failure is admitting failure" | Writing up failure is converting it to forward value |
| "No time to design the failure mode" | The failure mode is going to happen on its timeline. The only question is whether you captured it |

## Red flags — you've skipped this skill

- A branch deleted with no postmortem
- A proposal abandoned with no documented alternatives
- A failed experiment that left no artifact behind
- A repeating discussion the team keeps having (because the prior round produced no durable artifact)

## The closing principle

**Blank does not play games to win games. Blank plays games to answer the question that was running before the game existed.**

The best work in code is the same. The PR that ships, the PR that's rejected, the prototype that succeeds, the prototype that fails — all of them should answer a question that survives the move. That is how a team compounds.

Aschente.

## Related

- [[kohaku]] — master orchestrator
- [[shiro-mode]] — the calculation should hold without the calculator
- [[blank-diagnostic]] — Q5 explicitly tests for this principle
- [[hacker-helix]] — Step 5 (Reporting / Exfiltration) is where the durable artifact lives
