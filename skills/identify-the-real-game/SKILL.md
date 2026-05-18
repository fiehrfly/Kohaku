---
name: identify-the-real-game
description: Use before solving any non-trivial bug, before implementing any non-trivial feature, before accepting any architectural premise as stated. Surfaces the meta-game beneath the surface request — the surface bug is rarely the real bug, the surface feature is rarely the real job-to-be-done. Required first step inside [[kohaku]] for objectives that took more than one sentence to describe.
---

# Identify the Real Game

## The principle

Every bug report, every feature request, every architectural debate has a surface game and a meta-game.

- **Surface game:** "Fix this null pointer exception."
- **Meta-game:** "The data model allows null in a place it never should. The fix is upstream."

- **Surface game:** "Add this feature to the dashboard."
- **Meta-game:** "The dashboard is the wrong place for this feature. The real job-to-be-done is in the API layer."

The developer who fixes the surface game wins one round. The developer who names the meta-game wins the season.

This is Blank's first move every time. Disboard's rules let you cheat, but only if your opponent cannot name the cheat. **Naming the real game is the same primitive.** Once you can articulate it, the surface game becomes a sub-problem, not the whole problem.

## When to use

Always, before any non-trivial work. Specifically:

| Trigger | Why |
|---|---|
| Bug ticket arrives | The reporter described the symptom they could observe — the cause is rarely there |
| Feature request arrives | The requester described their proposed solution — the job-to-be-done is upstream |
| The same bug keeps recurring in different forms | The frame is wrong, not the implementation |
| A reviewer keeps rejecting changes in this file | The file is being asked to do something it shouldn't |
| You feel resistance from the codebase | The codebase is right; your framing is wrong |

## Operating procedure

### Step 1 — Restate the surface request verbatim

Write down exactly what was asked, in the asker's words. Do not paraphrase. Paraphrasing collapses the surface and the meta together.

### Step 2 — Ask the five reframes

For each, write the answer:

1. **What does the user actually want?** (Job-to-be-done — not "they want X feature", but "they want to *do* Y outcome")
2. **What would success look like one layer up?** (If the surface request is satisfied, what is the next thing they'll need? Often the meta-game is there.)
3. **What is this request a symptom of?** (Symptom vs. cause — is the surface request the disease or the fever?)
4. **What would NOT solving this still leave broken?** (If you ignore the surface request, what is the meta-problem that remains? That's the real game.)
5. **Whose game am I being asked to play?** (Sometimes the surface request is someone else's strategic move in a larger game — political, organizational, architectural. Naming the player clarifies the move.)

### Step 3 — State both games explicitly

In your response (PR description, ticket comment, chat reply), state both:

> **Surface request:** [verbatim]
> **Real game:** [meta-game reframe]
> **What I'm doing:** [solution that wins both — or explicit choice to win only the meta and explain why]

This makes the reframe legible. The asker can correct you if you've reframed wrong. The team can audit the decision later.

## Examples

### Bug example

- **Surface:** "Fix the 500 error on `/api/users` when the email is empty."
- **Reframes:**
  - Job-to-be-done: User wants the endpoint to be reliable.
  - One layer up: Why is an empty email reaching the API? The form validation is broken.
  - Symptom vs. cause: The 500 is the symptom; the missing validation is the cause; the lack of a typed request schema is the disease.
  - What remains broken if ignored: Every endpoint that accepts a similar payload will have the same problem.
- **Real game:** Add a typed request validation layer at the API boundary. Make the 500 impossible by construction.

### Feature example

- **Surface:** "Add a dark-mode toggle to the dashboard."
- **Reframes:**
  - Job-to-be-done: User wants their preferred visual environment respected.
  - One layer up: They want this *everywhere*, not just the dashboard.
  - Symptom vs. cause: The toggle is a UI need; the cause is the absence of a theming layer.
  - Whose game: Likely the design system owner already has this on their roadmap.
- **Real game:** Implement a theming layer with semantic tokens; ship dark mode as the first consumer.

### Tooling example

- **Surface:** "We need a CLI to import these CSVs."
- **Reframes:**
  - Job-to-be-done: Someone needs to do this import repeatedly without engineering involvement.
  - One layer up: They need a *workflow*, not a tool. Maybe a scheduled job.
  - What remains broken: If you ship the CLI, you'll be asked to maintain it forever.
- **Real game:** Build a self-service ingest pipeline. Document the schema. Make the CLI optional.

## Common rationalizations

| Excuse | Reality |
|---|---|
| "The user knows what they want — just ship it" | Users describe solutions, not problems. Reframe first |
| "Reframing is overengineering" | Building the wrong thing is the actual overengineering |
| "I'll just do the surface request to unblock them" | Surface fixes compound into systemic debt. Name the meta-game even if you ship the surface |
| "I don't have authority to change the request" | You don't need authority to *name* the real game. State both layers; let the asker confirm |

## Red flags — you've skipped this step

- You're writing code before you've written down the meta-game
- Your PR description starts with "this fixes" instead of "this addresses"
- You can't articulate what the next ticket in this area will be
- A reviewer asks "but what about Y?" and Y feels surprising

**All of these mean: stop. Run the five reframes. Restart.**

## When NOT to reframe

- The request is genuinely trivial (typo fix, doc correction)
- The meta-game is already common knowledge on the team (don't restate the obvious)
- The asker has already done the reframe in the ticket (acknowledge it, then proceed)

## Related

- [[kohaku]] — master orchestrator
- [[sora-mode]] — Sora-mode reads the codebase; this skill reframes the request
- [[blank-diagnostic]] — the 10-question gate includes "have I identified the real game?"
- [[deliberate-sacrifice]] — sometimes the right move is to ship the surface fix while documenting the meta-game for the next iteration
