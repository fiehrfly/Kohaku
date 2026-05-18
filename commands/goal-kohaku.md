---
name: goal-kohaku
description: Wrap /goal with the Kohaku Mastermind layer — strategic framing first, then Ruflo/OpenDesign/direct executor
---

$ARGUMENTS

Run `/goal` with the Kohaku Mastermind layer wrapped around it. Strategic framing happens *before* the goal decomposes into tasks.

## Procedure

1. **Mastermind first** — Engage the `kohaku-mastermind` subagent (or load the `kohaku` skill in-conversation) on the stated objective.
2. **Strategy document** — Mastermind returns:
   - Surface request (verbatim)
   - Real game (meta-game reframe)
   - Mode selection (Sora / Shiro / Blank)
   - Blank Diagnostic (all 10 questions answered)
   - Reconnaissance plan
   - Execution handoff (Ruflo / OpenDesign / direct)
   - Success criteria
   - Failure-mode design (Deliberate Sacrifice)
   - Reporting plan
3. **Then /goal** — Use the strategy document as `/goal`'s input. The goal's tasks now inherit the framing.
4. **During execution** — If Ruflo or another executor signals uncertainty, fall back to Kohaku for re-framing. The Mastermind is on call.
5. **At completion** — Run Hacker Helix Step 5 (Reporting / Exfiltration). PR description as decision record. Documentation as persistent Shiro analysis.

## When to use this instead of `/goal` directly

- Multi-file, multi-stakeholder, multi-week work
- Architecture decisions with long blast radius
- Novel domains where reconnaissance is non-trivial
- Tool / dependency selection embedded inside a larger objective
- Any time the user says "godmode" or "engage Kohaku"

## When to skip this and use `/goal` directly

- Single-file changes
- Bug fixes with clear repro and obvious cause
- Doc updates, lint fixes, formatting
- Code the user wrote this week

Strategic framing has setup cost. Pay it when the work warrants it.
