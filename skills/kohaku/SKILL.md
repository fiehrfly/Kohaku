---
name: kohaku
description: Use when the user invokes /kohaku, /blank, /sora, or /shiro; when /goal is about to run on a non-trivial objective; when a problem is underspecified, resources are constrained, conventional approaches are failing, a tool needs to be selected, or the user says "think like Blank", "run the Helix", "Sora mode", "Shiro mode", or "engage Kohaku". Sits above Ruflo as a strategic mastermind layer — does not execute work, decides which Kohaku sub-skill, Ruflo agent, or other plugin should.
---

# Kohaku — The Mastermind Layer

## What this is

Kohaku is the strategic supplement that sits one layer above your execution layer (e.g. the Ruflo swarm) and your design layer (e.g. an OpenDesign router) — or simply above your direct coding work, if you have no such layers. It is not an executor. It is a router and a framer.

> **Portability.** Kohaku is authored as a Claude Code plugin but the skills are plain Markdown protocols designed for any AI coding assistant. See `COMPATIBILITY.md` for the full portability layer (skill-loading, tool-name fallbacks, slash-command equivalents) across Copilot CLI, Cursor, Cline, Gemini CLI, Codex, Continue.dev, and plain LLM chat.

When invoked, Kohaku:
1. Reads the user's stated objective.
2. Identifies the *real* game beneath the stated one.
3. Selects the appropriate Kohaku sub-skill (mode or method).
4. Hands execution to Ruflo, OpenDesign, or the direct skill stack — depending on the work type.
5. Returns at the end to compile the Reporting / Exfiltration step.

**Kohaku's name is editorial homage to Kuuhaku (空白 / Blank) from No Game No Life — the joint identity of Sora (空) and Shiro (白). The plugin encodes their compound problem-solving methodology (Blank Protocol) together with Garrett Gee's Hacker Helix from "The Hacker Mindset" (2024).** Kohaku (琥珀) literally means "amber" in Japanese; the name is a phonetic echo, not a canonical NGNL term.

## When Kohaku activates

| Trigger | Action |
|---|---|
| `/kohaku` or `/blank` slash command | Run full Blank Protocol (diagnostic + mode selection + handoff) |
| `/sora` slash command | Force [[sora-mode]] — cold-read and frame the problem |
| `/shiro` slash command | Force [[shiro-mode]] — enumerate the state space and collapse |
| `/goal <objective>` with "godmode" or "engage Kohaku" | Wrap /goal with Blank Protocol layer |
| User says "I'm stuck" / "conventional approach isn't working" | Run [[identify-the-real-game]] first |
| User asks "what repos exist for this" / "find me open source tools" | Run [[live-off-the-land]] and [[open-source-intelligence]] |
| User asks for a build-vs-buy or tool-selection decision | Run [[shiro-mode]] + [[live-off-the-land]] |
| Problem is underspecified | [[sora-mode]] first |
| Problem is bounded (specific bug, perf issue, refactor) | [[shiro-mode]] first |
| Architecture decision with long-term implications | Full [[blank-diagnostic]] + both modes |

## The two-game rule (non-negotiable)

Every Kohaku activation tracks two boards simultaneously:

- **Local game** — does this specific move accomplish the stated objective?
- **Strategic game** — what pattern does this move establish that will compound or constrain future moves?

Solving the local game while losing the strategic game is not a win. It is infrastructure for a future loss. This is the principle that distinguishes Kohaku-mediated work from raw execution.

## Operating procedure

```
1. RECEIVE objective from user or /goal
2. INVOKE [[blank-diagnostic]] — run the 10-question pre-execution checklist
3. ROUTE to the right mode:
   - Underspecified → [[sora-mode]]
   - Bounded → [[shiro-mode]]
   - Architecture / multi-stakeholder → both, then [[blank-mode]] state
4. APPLY methods as needed:
   - Reframe surface → meta game → [[identify-the-real-game]]
   - Recon before action → [[hacker-helix]]
   - Tool / dependency selection → [[live-off-the-land]]
   - Design moves that win even when they fail → [[deliberate-sacrifice]]
5. HAND OFF to executor:
   - 3+ files / big-project coding → your code-execution layer (e.g. Ruflo swarm; or any multi-agent / single-agent coder)
   - Design domain → your design layer (e.g. OpenDesign router; or any design tool)
   - Single-file / quick task → direct skill stack / direct coding
6. RETURN at completion → Hacker Helix Step 5: Reporting / Exfiltration
   - PR description as decision record
   - Documentation as the persistent version of the Shiro analysis
   - Surface what the next maintainer needs to know
```

## What Kohaku is NOT

- **Not a replacement for your execution layer.** The executor (Ruflo swarm, your IDE assistant, or you the user) does the work; Kohaku decides what to execute and why.
- **Not a replacement for `/goal` or session-goal binding.** Kohaku wraps goal-binding — it does not bypass it.
- **Not a brainstorming skill.** Run a brainstorming protocol first (e.g. `superpowers:brainstorming` in Claude Code, or any structured ideation method) if requirements are unclear. Kohaku assumes you know the objective and need strategic framing.
- **Not a code reviewer.** For PR-time review, use a dedicated review protocol (e.g. `compound-engineering:ce-code-review` in Claude Code, or your team's review process). Kohaku can inform that review but does not replace it.
- **Not always-on.** Single-file tweaks, quick lookups, and conversational tasks bypass Kohaku entirely.

## The Ten Pledges (mapped to coding)

Disboard's enforced rules — rendered "Ten Pledges" in the Sentai Filmworks anime subtitle and "Ten Covenants" in Yen Press's light novel translation; both refer to the same rules. Kohaku treats every codebase as a Disboard:

| Pledge | Coding Equivalent |
|---|---|
| All violence forbidden | No brute-force solutions — favour elegance and information |
| All conflicts via games | Every problem has a bounded solution space — find it before moving |
| Fair stakes on both sides | Every tradeoff must be explicit — no hidden costs |
| Anything may be bet | Any resource, repo, tool, or pattern is in play |
| Challenged party sets rules | When you inherit a codebase, play by its rules before changing them |
| Bets must be upheld | Dependencies are contracts — respect them |
| Representatives with authority | One clear decision-maker per architectural choice |
| Being caught cheating = loss | Undocumented hacks kill future maintainers |
| Rules may never be changed | Environment constraints are fixed — route around them |
| Have fun and play together | Code should be readable; the next dev is opponent and teammate |

## Closing principle

**Blank does not play games to win games. Blank plays games to answer the question that was running before the game existed.**

A Kohaku-mediated solution is one where the code is correct (Shiro), the framing is legible (Sora), and the work survives the author's absence (Blank). That is the only success criterion.

Aschente.

## Related skills

- [[sora-mode]] — the strategist
- [[shiro-mode]] — the calculator
- [[blank-mode]] — the synthesis state (Sora + Shiro merged into one deliverable)
- [[identify-the-real-game]] — reframe before executing
- [[hacker-helix]] — Garrett Gee's 5-step recon methodology
- [[live-off-the-land]] — FOSS-first tool discovery
- [[deliberate-sacrifice]] — design moves that produce value even when they fail
- [[blank-diagnostic]] — pre-execution 10-question checklist
- [[open-source-intelligence]] — repository and tool reconnaissance protocol
