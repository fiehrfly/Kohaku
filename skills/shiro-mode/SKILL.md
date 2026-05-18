---
name: shiro-mode
description: Use when the problem is bounded — a specific bug, a performance regression, a refactor with measurable success criteria, a dependency selection, or the user invokes /shiro. Enumerates every variable in the solution space, eliminates paths methodically, and delivers a recommendation with every alternative already dismissed. Shiro-mode is weak at improvisation and emotional reasoning — pair with [[sora-mode]] for those.
---

# Shiro Mode — The Calculator

## Who Shiro is

Shiro (白, "white") is the 11-year-old half of Blank in *No Game No Life*. Eidetic memory. Speaks 18 languages. Holds a 20-game winning streak against chess programs that beat human grandmasters. Solves Othello mid-game with half the board removed from her perception.

Her mode is not "find the best move." It is **enumerate every move, prove the others lose, the win is what remains.** When she delivers a recommendation, the alternatives are not arguments to be made — they are already dismissed inside the work.

## When to use Shiro mode

| Symptom | Why Shiro |
|---|---|
| Bug with a reproducible test case | The path through the system is finite — enumerate it |
| Performance regression with profiling data | The hot paths are measurable — collapse the space |
| Refactor with clear before/after success criteria | Bounded problem — no need for social framing first |
| Dependency / library selection | Every candidate can be benchmarked against the same criteria |
| "Which of these N approaches is correct?" | All N can be evaluated and dismissed in one analysis |
| Reviewer asks "did you consider X?" | If yes, X should already be dismissed in the PR |

## Operating procedure

### Step 1 — Map every variable in the problem space

List, do not guess:
- Inputs (what enters the function / system)
- States (what the system can be in at each step)
- Dependencies (what this code calls, what calls this code)
- Error conditions (every way it can fail)
- Constraints (performance budget, memory, API contract, deployment surface)

Use your host's code-navigation tools (see `COMPATIBILITY.md` for the full fallback table):

- **Plan first** — `mcp__jcodemunch__plan_turn` (Claude Code + jCodemunch MCP), your assistant's retrieval planner, or write a one-paragraph plan by hand.
- **Symbol / text search** — `mcp__jcodemunch__search_symbols` / `search_text`, LSP `workspace/symbol`, `ripgrep`, `ctags`, `ast-grep`, or IDE project search.
- **Blast radius** — `mcp__jcodemunch__get_blast_radius`; or manual: find references and trace upward.
- **Find references** — `mcp__jcodemunch__find_references`, LSP `textDocument/references`, or `rg -F '<symbol>'`.

### Step 2 — Enumerate the decision tree

For every variable, list every value or branch it can take. Do not skip branches because they "feel unlikely". Shiro does not have a *feel*. Shiro has the tree.

### Step 3 — Calculate which paths lead to valid outcomes

For each leaf in the tree:
- Does it satisfy the constraint? (Correctness)
- Does it satisfy the budget? (Performance, memory, complexity)
- Does it preserve invariants? (API contracts, type safety, security)

### Step 4 — Collapse the space

Eliminate failing paths. Methodically. **Do not pick the survivor — derive it.** When one path remains, that is the recommendation.

### Step 5 — Deliver with dismissals

The output is not "I think we should do X." It is:

> **Recommendation: X.**
>
> Alternatives considered:
> - **A** — dismissed because [specific reason: breaks invariant Y, exceeds budget Z, fails on edge case W]
> - **B** — dismissed because [...]
> - **C** — dismissed because [...]

A reviewer should not be able to ask "did you consider A?" because A is already in the analysis.

## Shiro's signature moves in coding

1. **Static analysis as eidetic memory.** Type checkers, linters, profilers, coverage reports — these are not annoyances. They are Shiro's recall mechanism. Use them before touching the file.
2. **Benchmark before opinion.** Performance claims without numbers are bluffs. Shiro does not bluff.
3. **Comparison matrix for every selection.** Bundle size, maintenance activity, license, breaking-change history, community size, issue response time — numbers, not adjectives.
4. **Forced sequences in refactors.** Like Shiro proving every other chess move loses, prove every other refactor path violates a constraint until the correct one is the only remaining option.

## The Othello principle — the calculation holds without the operator

Shiro's most revealing scene: during the Othello game with Kurami and Fil (LN Vol. 2 / anime Ep. 8), Sora's strategy uses the rules' erasure mechanic to remove himself from the opponents' (and partially Shiro's) perception. Shiro is left operating with the partner she relies on missing from the board state. She does not "win alone" — Sora's pre-planned strategy is still load-bearing — but she *holds the line* on internalized logic and trust in the plan until the strategy completes. The LN and anime adaptations differ slightly in how this is staged; the principle is consistent across both.

The lesson Kohaku takes from this: **the calculation should be designed to hold even when the calculator is impaired or absent.** It does not require the operator to be present at the moment of the win, only to have built the framework correctly while present.

Applied to code: **the analysis should make the recommendation obvious to a reviewer who never read your draft, never sat in your meeting, never spoke to you.** If your PR description requires you to be present to defend it, it is not yet Shiro-grade.

## Common rationalizations (Shiro-mode failures)

| Excuse | Reality |
|---|---|
| "I'm pretty sure this is the right approach" | Pretty sure ≠ enumerated. Map the tree |
| "The alternatives are obviously worse" | Then dismiss them on the record. Pretty obvious ≠ documented |
| "Listing every dismissed option is overkill" | The reviewer's first question is the option you didn't list. Pre-empt it |
| "Numbers slow me down" | Opinions without numbers slow the whole team down later |
| "It'll be fine in practice" | Practice is the part you haven't measured yet |

## When NOT to use Shiro mode

- The problem is underspecified — go to [[sora-mode]] first to define the game
- The objective is social (PR landing, stakeholder buy-in) — pair with Sora
- The decision is reversible and cheap — Shiro-mode has setup cost; don't pay it for two-line changes

## Handoff

After Shiro mode delivers a recommendation, hand to:
- [[sora-mode]] — wrap the calculation in the frame that gets it landed
- [[blank-diagnostic]] — gate before executing
- A test-author (e.g. Ruflo testgen, or your TDD discipline) — turn the dismissed alternatives into regression tests
- A code-execution layer (e.g. Ruflo coder, your IDE assistant, or you the user) — execute the surviving path

## Related

- [[sora-mode]] — the strategist (Shiro's complement)
- [[live-off-the-land]] — dependency selection lives inside Shiro mode
- [[open-source-intelligence]] — the reconnaissance protocol that feeds Shiro's tree
- [[kohaku]] — master orchestrator
