---
name: kohaku
description: Wrap /goal with the Kohaku Mastermind — set $ARGUMENTS as the session goal, then run the Blank Protocol (Sora + Shiro) on it
---

$ARGUMENTS

## Goal binding (mandatory first action)

Treat the text above (the `$ARGUMENTS` block) as the **session goal** for this conversation. Mirror the behavior of the built-in `/goal` command:

1. Acknowledge the goal verbatim back to the user in one line, in the form:
   > **Kohaku goal set:** _<the args, verbatim>_
2. Hold this goal as a binding success criterion until it is satisfied. Do **not** declare the work done, stop, or hand control back to the user until the condition the goal describes is actually met — exactly the discipline `/goal` enforces via its session-scoped Stop hook.
3. If the user typed `/kohaku` with no arguments, ask once for the objective, then continue.
4. If a `/goal` Stop hook is **already** active in this session (you will see a `<system-reminder>` telling you so), do not duplicate the acknowledgment — log `Kohaku goal inherited from /goal: <condition>` instead and proceed.

The literal session-scoped Stop hook is owned by the built-in `/goal` and cannot be programmatically (re)installed from inside a slash-command expansion. If the user explicitly wants that hook *in addition to* Kohaku, they can type `/goal <args>` first and then `/kohaku <args>` — but the goal-binding discipline above is sufficient for almost all cases.

## Mastermind procedure (run on the bound goal)

Engage the Kohaku Mastermind layer on the bound goal above.

1. Load the `kohaku` skill to orient.
2. Run [[identify-the-real-game]] on the stated objective.
3. Select [[sora-mode]], [[shiro-mode]], or both ([[blank-diagnostic]] for the gate).
4. Apply [[hacker-helix]] Step 1 (Reconnaissance) before any execution decision.
5. If FOSS / tool selection is involved, apply [[live-off-the-land]] and [[open-source-intelligence]].
6. Design failure modes per [[deliberate-sacrifice]].
7. Hand execution to:
   - **Ruflo swarm** if 3+ files or "big-project coding work"
   - **OpenDesign router** if any design intent
   - **Direct skill stack** if single-file or specialist
   - **Subagent** (compound-engineering reviewer, superpowers TDD, etc.) if review-bound or discipline-bound
8. Drive the chosen executor through to the goal-bound success criterion. Kohaku is the framing layer, but because `/kohaku` is now goal-bound, you stay on the hook for completion — do not declare success until the criterion holds. If the executor stalls, fall back to Kohaku for re-framing (the Mastermind is on call).

**Kohaku decides what should be executed and how to recognize success — and with `/kohaku`, also enforces that success is reached.**
