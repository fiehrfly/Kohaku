# Kohaku — Architecture

## Position in the stack

Kohaku is the **strategic / mastermind layer**. It sits one level above:

- **Ruflo** — the multi-agent swarm methodology for big-project coding work
- **OpenDesign** — the design-domain router
- The **direct skill stack** — single-file work, specialist skills

Kohaku does not compete with any of these. It decides which of them runs, with what framing, and how the result will be made durable.

```
LAYER 1 — User / /goal command
LAYER 2 — Kohaku (this plugin)        ← strategic framing, decides what runs
LAYER 3 — Ruflo / OpenDesign / direct ← execution
LAYER 4 — Subagents (CE reviewers, superpowers TDD, jCodemunch nav)
LAYER 5 — Tools (Edit, Write, Bash, MCP servers)
```

When a `/goal` arrives at Layer 1, Kohaku (Layer 2) gets the first read. It:
1. Reframes the surface request into the real game
2. Runs the Blank Diagnostic (10-question gate)
3. Decides which Layer-3 executor will run the work
4. Compiles the reporting artifact at the end

## Skill graph

```
                              kohaku (master entry)
                                │
                ┌───────────────┼───────────────┐
                │               │               │
        identify-the-real-game  │       blank-diagnostic
                │               │               │
                ▼               │               │
        ┌──────────────┐        │               │
        │  reframe     │        │               │
        │  surface →   │        │               │
        │  meta game   │        │               │
        └──────┬───────┘        │               │
               │                ▼               │
               │       ┌────────────────┐       │
               │       │  Mode select   │       │
               │       │ (Sora/Shiro)   │       │
               │       └────────┬───────┘       │
               │                │               │
               │     ┌──────────┴─────────┐     │
               │     │                    │     │
               │     ▼                    ▼     │
               │  sora-mode           shiro-mode│
               │  (cold-read +         (enumerate│
               │  social frame)        + collapse│
               │     │                    │     │
               │     └──────────┬─────────┘     │
               │                │               │
               │                ▼               │
               │       ┌────────────────┐       │
               │       │  hacker-helix  │◄──────┘
               │       │  Step 1: Recon │
               │       └────────┬───────┘
               │                │
               │      ┌─────────┴──────────┐
               │      │                    │
               │      ▼                    ▼
               │ live-off-the-land   open-source-intelligence
               │ (FOSS tier-1→4)     (tool discovery protocol)
               │      │                    │
               │      └─────────┬──────────┘
               │                │
               │                ▼
               │       deliberate-sacrifice
               │       (design failure modes)
               │
               └─────► Hand to Layer-3 executor
```

## Routing rules

| User signal | Kohaku response |
|---|---|
| `/kohaku <obj>` | Full Blank Protocol |
| `/blank <obj>` | Alias for `/kohaku` |
| `/sora <obj>` | Force Sora-mode only |
| `/shiro <obj>` | Force Shiro-mode only |
| `/goal-kohaku <obj>` | Mastermind wraps `/goal` |
| `/goal <obj>` + "godmode" / "engage Kohaku" | Equivalent to `/goal-kohaku` |
| "I'm stuck" / "conventional approach isn't working" | Auto-engage [[identify-the-real-game]] |
| "find me open source tools" / "what repos exist for this" | Auto-engage [[live-off-the-land]] + [[open-source-intelligence]] |
| Single-file fix / typo / lint | Skip Kohaku; direct execution |

## Layer-3 handoff matrix

After Kohaku produces the strategy document, it hands off:

| Work type | Handoff target |
|---|---|
| 3+ files, structured coding | Ruflo swarm (`/swarm` or `/ruflo-status`) |
| UI / brand / visual design | OpenDesign router (`opendesign-router` skill) |
| Single-file tweak | Direct skill stack |
| Code review at PR boundary | `compound-engineering:ce-code-review` |
| TDD-bound discipline | `superpowers:test-driven-development` |
| Systematic debugging | `superpowers:systematic-debugging` |
| Documentation | `document-skills:doc-coauthoring` |
| Browser/UI testing | `document-skills:webapp-testing` |
| Worktree isolation | `superpowers:using-git-worktrees` |
| Plan-document writing | `superpowers:writing-plans` |

## Transparency on source material

### Gee — *The Hacker Mindset* (2024)

**Confirmed against public sources** (publisher, Goodreads reader data, author marketing, USA TODAY Bestseller announcement, podcast appearances):

- Book title, subtitle, "Hacker Helix" framework name
- Six principle names, in their canonical gerund forms: Being on the Offense, Reverse Engineering, Living off the Land, Risk-Based Decisioning, Social Engineering, Pivoting
- Author background: Sandia Labs at 15, Federal Reserve Bank of San Francisco, Hacker Warehouse (founded 2013), consulting on Mr. Robot / Jack Ryan / Jason Bourne / Sense8

**Inferred, not directly verified:**

- The five Helix step labels used in `skills/hacker-helix/SKILL.md` — **Reconnaissance → Scanning → Gaining Access → Maintaining Access → Reporting/Exfiltration** — map onto the standard penetration-testing lifecycle (EC-Council CEH / PTES framework), which is the methodology Gee's book draws from. No public index lists the book's exact step labels word-for-word. If the book uses different labels for the lay-audience adaptation, this skill should be updated to match.

### Kamiya — *No Game No Life* (2012–present)

**Confirmed against canonical sources** (NGNL Fandom wiki, Wikipedia, TV Tropes, Yen Press LN, Madhouse anime):

- Etymology: Sora (空) + Shiro (白) = Kuuhaku (空白 / 『 』 / "blank")
- Ages: Sora 18, Shiro 11 at series start
- 280+ games as Blank with zero recorded losses (anime narration; LN figure consistent)
- Sora's canonical quote (anime Ep. 1 / LN Vol. 1) about invisible variables / pre-game decision
- Shiro: 18 languages, learned Immanity in 15 min, 20-game streak against grandmaster-tier chess
- Sora's weakness: catastrophic codependence on Shiro (paralyzed alone)
- Shiro's weakness: difficulty reading emotional / irrational behavior
- The Ten Pledges (anime sub) / Ten Covenants (Yen Press LN) — same content, two renderings; this plugin uses "Pledges" with a Covenants note
- NGNL Zero (2017 film): 6,000 years prior; Riku + Schwi as structural parallel; Schwi transmits the plan in her dying moments (not posthumously)

**Disclaimed:**

- **Kohaku (琥珀, "amber") ≠ Kuuhaku (空白, "blank").** Different kanji, different meaning, no canonical NGNL connection. The plugin name is a phonetic / aesthetic echo only. This is stated explicitly in the master `kohaku` skill and in the README.

## Versioning

`v0.1.0` — initial scaffold. The plugin treats the published skill descriptions and command surface as stable; internal phrasing of each SKILL.md body may evolve in patch releases as the Blank Protocol is exercised against real workloads.

When changing skill content significantly:
1. Run pressure scenarios via subagents (see `superpowers:writing-skills` for the TDD-for-skills methodology)
2. Update `version` in `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`
3. Note the change in `CHANGELOG.md` (to be added at first content-affecting release)

## Out of scope (for v0.1.0)

- A `/kohaku-audit` command that retrospectively rates a completed PR against the Blank Diagnostic — planned for v0.2
- Hooks that auto-engage Kohaku on `/goal` invocations matching certain patterns — planned for v0.2
- A Ruflo agent file that mirrors `kohaku-mastermind` directly inside the Ruflo swarm — planned for v0.3 (waits on a Ruflo extension point)

## Closing

Kohaku is the layer that asks the question before the question gets answered. It is not a productivity boost — it is a discipline.

Use it when the work warrants framing. Skip it when it doesn't. The strategic layer has setup cost; pay it deliberately.

Aschente.
