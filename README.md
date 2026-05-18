# Kohaku 琥珀

> A strategic mastermind layer for Claude Code. Combines Garrett Gee's Hacker Mindset (2024) with Blank's (Sora + Shiro from *No Game No Life*) unified problem-solving methodology — encoded as composable skills that sit *above* the Ruflo swarm without replacing it.

---

## What it is

Kohaku is a Claude Code plugin. It is **not** a code executor. It is a router and a framer — a strategic layer that runs **before** `/goal`, **alongside** Ruflo, and **after** execution to compile the decision record.

When you invoke `/kohaku`, `/blank`, `/sora`, or `/shiro`, Claude:

1. Reframes your stated request into the *real game* (meta-game beneath the surface)
2. Selects the right mode — Sora (cold-read + frame), Shiro (enumerate + collapse), or both (Blank)
3. Runs the **Blank Diagnostic** (10-question pre-execution checklist)
4. Hands execution to Ruflo, OpenDesign, or the direct skill stack
5. Returns to compile **Reporting / Exfiltration** — PR description, ADR, postmortem — as a durable artifact

The name **Kohaku** (琥珀, "amber") is a phonetic echo of **Kuuhaku** (空白 / 『 』 / "blank"), the joint identity of Sora and Shiro. The kanji are different. The naming is editorial homage, not a canonical NGNL term.

---

## Why it exists

`/goal` is a powerful Claude Code primitive. But many `/goal` invocations skip the most important step: deciding **what game is actually being played** before decomposing it into tasks.

Ruflo orchestrates execution well, but Ruflo doesn't decide *which* objective is worth executing.

Kohaku fills that strategic gap. It runs the **Blank Protocol** — a discipline that:

- Names the real problem beneath the stated one
- Reads the codebase (Sora-mode) before touching it
- Enumerates the solution space (Shiro-mode) before recommending
- Builds the social frame and the analytical frame in parallel
- Lives off the land (FOSS first) before paying for tools
- Designs every move so even apparent failure produces forward value

---

## Installation

### Via marketplace.json (recommended)

```bash
# Add this marketplace to your Claude Code config
/plugin marketplace add fiehrfly/Kohaku

# Install the plugin
/plugin install kohaku@kohaku
```

### Manual

```bash
git clone https://github.com/fiehrfly/Kohaku ~/.claude/plugins/cache/kohaku/kohaku/0.1.0
```

After install, the skills are available via the `Skill` tool and the commands via `/kohaku`, `/blank`, `/sora`, `/shiro`, `/goal-kohaku`.

---

## Skills

| Skill | Purpose |
|---|---|
| `kohaku` | Master orchestrator. The entry point that selects the next skill. |
| `sora-mode` | Cold-read the codebase / system. Build social and narrative frame. |
| `shiro-mode` | Enumerate solution space. Collapse to the correct path. Deliver with dismissed alternatives. |
| `blank-mode` | The synthesis state — merge Sora's frame and Shiro's calculation into one deliverable that survives the author's absence. |
| `identify-the-real-game` | Reframe the surface request into the meta-game. |
| `hacker-helix` | Garrett Gee's 5-step reconnaissance methodology. |
| `live-off-the-land` | FOSS-first tool acquisition. Standard library → existing codebase → FOSS → paid. |
| `deliberate-sacrifice` | Design moves so even failure produces forward value. |
| `blank-diagnostic` | 10-question pre-execution gate. |
| `open-source-intelligence` | Reconnaissance protocol for external libraries / tools. |

## Commands

| Command | Effect |
|---|---|
| `/kohaku <objective>` | Full Blank Protocol on the objective. |
| `/blank <objective>` | Alias for `/kohaku`. |
| `/sora <objective>` | Force Sora-mode (cold-read + frame). |
| `/shiro <objective>` | Force Shiro-mode (enumerate + collapse). |
| `/goal-kohaku <objective>` | Wrap `/goal` with the Kohaku Mastermind layer. |

## Subagent

`kohaku-mastermind` — Opus-tier strategic subagent. Receives an objective, returns a structured strategy document with reconnaissance plan, mode selection, diagnostic answers, and execution handoff. **Does not edit code.**

---

## How it sits in the stack

```
                ┌────────────────────────────────┐
                │       USER / /goal             │
                └────────────────┬───────────────┘
                                 │
                ┌────────────────▼───────────────┐
                │     KOHAKU (this plugin)       │
                │  • Real-game reframing         │
                │  • Mode selection              │
                │  • Blank Diagnostic            │
                │  • Recon plan                  │
                └────────────────┬───────────────┘
                                 │
       ┌─────────────────┬───────┴────────┬──────────────────┐
       ▼                 ▼                ▼                  ▼
  ┌─────────┐      ┌──────────┐     ┌──────────┐      ┌────────────┐
  │  RUFLO  │      │OPENDESIGN│     │  DIRECT  │      │ SUBAGENTS  │
  │  swarm  │      │  router  │     │  skills  │      │ (CE, SPRS) │
  │ (3+ fs) │      │ (design) │     │ (1-file) │      │  (review)  │
  └─────────┘      └──────────┘     └──────────┘      └────────────┘
                                 │
                ┌────────────────▼───────────────┐
                │  KOHAKU returns: Reporting /   │
                │  Exfiltration (PR description, │
                │  ADR, postmortem)              │
                └────────────────────────────────┘
```

Kohaku is the **strategic** layer. Ruflo is the **execution** layer. OpenDesign is the **design** layer. They cooperate. None replaces the others.

---

## The two source frameworks

### Blank Protocol — from *No Game No Life*

**Blank (『 』 / Kuuhaku)** is the joint gaming identity of two siblings:

- **Sora (空)** — 18, strategist, reads people, builds the situation in which the opponent's available moves all lead to his outcome. Canonical quote: *"The victor of a game is decided before it even begins."*
- **Shiro (白)** — 11, calculator, eidetic memory, 18 languages, 20-game streak against grandmaster-level chess simulators. Does not find the best move — proves every other move loses until the win is what remains.

Together, **Blank** holds top ranking in over 280 games with zero recorded losses. The principle: every move tracks two boards simultaneously (local + strategic), every recommendation has both narrative (Sora) and calculation (Shiro), and the work is designed to hold even after the operator is gone.

### Hacker Mindset — from Garrett Gee

**Garrett Gee** is a white-hat cybersecurity practitioner — Sandia Labs at 15, Manager of Information Security at the Federal Reserve Bank of San Francisco, founder of Hacker Warehouse, consulted on Mr. Robot and Jack Ryan. His 2024 book *The Hacker Mindset* (BenBella, USA TODAY Bestseller) encodes a 5-step methodology ("Hacker Helix") and six underlying principles:

1. Being on the Offense
2. Reverse Engineering
3. Living off the Land
4. Risk-Based Decisioning
5. Social Engineering
6. Pivoting

Kohaku translates these from offensive security into software-engineering practice.

---

## Attribution and lore disclaimer

- *No Game No Life* © Yuu Kamiya / MF Bunko J / Madhouse. The character names Sora, Shiro, Blank, Riku, and Schwi are used for editorial homage. No affiliation or endorsement is claimed.
- *The Hacker Mindset* © Garrett Gee (BenBella Books, 2024). Methodology references are commentary and application. No affiliation or endorsement is claimed.
- The 5 Hacker Helix step labels in this plugin are inferred from the standard penetration-testing lifecycle that Gee's book draws from; they are not directly extracted from the book's exact wording. See [`ARCHITECTURE.md`](./ARCHITECTURE.md) for the full transparency note.

---

## License

MIT. See [`LICENSE`](./LICENSE).

---

*Built by [@fiehrfly](https://github.com/fiehrfly). Aschente.*
