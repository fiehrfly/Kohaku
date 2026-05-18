---
name: hacker-helix
description: Use as the reconnaissance layer before any non-trivial code move, tool adoption, or architecture decision. Five-step methodology adapted from Garrett Gee's "The Hacker Mindset" (2024). Maps to standard penetration-testing phases applied to software work — recon, scan, access, persist, report. Run before [[blank-diagnostic]] when the codebase or domain is unfamiliar.
---

# The Hacker Helix — Reconnaissance Methodology

## Source

Adapted from **Garrett Gee, *The Hacker Mindset: A 5-Step Methodology for Cracking the System and Achieving Your Dreams* (BenBella Books, 2024)** — USA TODAY Bestseller.

Gee is a white-hat cybersecurity practitioner: started at 15 at Sandia National Laboratories and the Federal Reserve Bank of San Francisco, founded Hacker Warehouse (penetration testing hardware) in 2013, consulted on Mr. Robot, Jack Ryan, Jason Bourne, and Sense8.

**Transparency note:** The framework is canonically called the "Hacker Helix" in the book, and its 5-step structure is committed in the book's subtitle. The individual step labels used below — **Reconnaissance → Scanning → Gaining Access → Maintaining Access → Reporting/Exfiltration** — are not publicly indexed against the book's exact wording. They are this skill's faithful adaptation of the standard penetration-testing lifecycle (EC-Council CEH / PTES framework), which is the methodology Gee's book draws from. If you have direct access to the book and find different step labels, update this skill.

## When to run

Before any of:
- Adopting a new dependency, library, or tool
- Touching an unfamiliar codebase or subsystem
- Proposing an architecture change
- Selecting between competing technical approaches
- Bidding on an estimated body of work

Skip for: known territory, trivial fixes, conversational tasks.

## Step 1 — Reconnaissance

**Map the terrain. Do not touch the keyboard yet.**

For a codebase:
- Language, framework, architecture pattern
- Who wrote it, who reviews changes, who owns the architectural decisions
- Rules: linting standards, test coverage requirements, deployment pipeline constraints
- Gaps: what is undocumented, undertested, inconsistently implemented

For an open-source tool / dependency:
- Last commit date (dead projects are debt)
- Open issues age (issues > 6 months old = warning)
- PR merge frequency
- Contributor count and bus-factor
- Test coverage badge
- CHANGELOG maintenance
- License (MIT / Apache 2.0 / BSD permissive; GPL/AGPL copyleft — verify before adopting)

**Tools for this step:**
- `mcp__jcodemunch__plan_turn` — opening move; get confidence + recommended files
- `mcp__jcodemunch__get_repo_outline` / `get_file_tree` — structural map
- `mcp__jcodemunch__get_repo_health` — calibrated quality signal
- `mcp__jcodemunch__suggest_queries` — when repo is unfamiliar
- `gh repo view` / GitHub API for external repos

**Stars are vanity. Look at activity.**

## Step 2 — Scanning

**Identify the specific leverage point.** What is undervalued, overlooked, or misunderstood?

In a codebase: the function everyone is afraid to touch is usually the most important one to understand. Map its `find_references` and `get_blast_radius` before going further.

In tool selection: the library with 800 stars and active maintenance frequently beats the one with 8000 stars and a last commit from 2021.

**The scan question:** Where is the asymmetry? Where does small effort produce large effect?

## Step 3 — Gaining Access

**Choose the entry point with the highest leverage-to-effort ratio.** Not the most impressive entry — the most effective one.

In code:
- The first contribution should be a small, well-scoped PR that fixes one real thing, adds one test, and demonstrates understanding of the system
- **Never a 2000-line refactor as a first contribution.**
- For external dependencies: ship a working prototype using the dependency before proposing it in an architecture meeting. **The demo is the entry point.**

## Step 4 — Maintaining Access

**Once inside, stay. Deliver proof points. Build dependency through value.**

In code: the developer who maintains the thing others avoid, documents what others do not, and answers questions in PR review threads is the one who gets influence over the next architectural decision.

This is the principle behind "ownership accrues to the persistent." It is not glamorous. It compounds.

## Step 5 — Reporting / Exfiltration

**Translate what was learned into a form others can act on.**

A well-written PR description is not a summary of changes — it is a **decision record**:
- What problem was being solved?
- What alternatives were considered (and dismissed)?
- What was tried and abandoned, and why?
- What does the reviewer specifically need to evaluate?

**The hack is incomplete until the insight is communicated.** Code that works but no one can maintain is a meta-level loss.

This step is also where [[shiro-mode]]'s analysis becomes durable. The PR description, the ADR, the post-mortem — these are the form in which Shiro's calculation survives the author's absence.

## The Six Principles (Gee's underlying axioms)

These ride on top of the Helix and apply at every step. Names taken from Gee's publisher and marketing materials:

1. **Being on the Offense** — Hackers do not wait for the bug report. They find the systemic pattern producing bugs and fix it before the next one appears. Write the test before the bug recurs. Document the decision before someone re-debates it. Open the issue before it becomes an incident.

2. **Reverse Engineering** — Work backward from the desired output. From the user story, trace backward through every layer until you find where it breaks. For architecture: deconstruct a system that already solved the problem you're facing. Read the source, not the README. *The README tells you what the author wanted to build. The source tells you what they actually built.*

3. **Living off the Land** — Before adding a dependency, exhaust what already exists. Standard library → existing codebase utilities → FOSS → paid solution. In that order. See [[live-off-the-land]] for the full protocol.

4. **Risk-Based Decisioning** — Hackers do not avoid risk; they frame it asymmetrically. The question is never "is this risky?" It is: *is the downside bounded? Is the upside compounding?* Gee: **"The biggest risk you can take is not taking any risks at all."** Apply the Effectuation frame: what is the affordable loss? What is the minimum viable experiment?

5. **Social Engineering** — For any recommendation that requires buy-in, build the social architecture and the logical architecture in parallel. Sora's frame + Shiro's calculation. Neither layer alone closes the room. See [[sora-mode]] and [[shiro-mode]].

6. **Pivoting** — Pivot is not quitting. Pivot is recognising that the game has changed and updating accordingly. Signals: same bug recurring in different forms → frame is wrong; every PR in this area requires rework → architecture is fighting the requirement; dependency used outside its intent → wrong tool. **The hacker does not fall in love with the approach. They fall in love with the outcome.**

## Common rationalizations

| Excuse | Reality |
|---|---|
| "Reconnaissance is bikeshedding" | Bikeshedding is debating colour. Recon is mapping the terrain. Different things |
| "I'll figure it out as I go" | Then you will figure it out as the system breaks |
| "The README is enough" | The README is marketing. The source is truth |
| "Stars indicate quality" | Stars indicate adoption velocity at some point in history. Check the commit graph |
| "I don't have time for a 5-step process" | The process is what makes the 5 steps fast. Skipping it slows the work |

## When NOT to run the Helix

- Single-line fixes
- Documentation typos
- Code you wrote this week
- Conversational tasks

For those, the recon overhead exceeds the work itself. Use judgment.

## Related

- [[live-off-the-land]] — Principle 3, fully elaborated
- [[open-source-intelligence]] — Reconnaissance protocol for external tools
- [[shiro-mode]] — analytical layer the Helix feeds into
- [[blank-diagnostic]] — Helix Q1 ("have I completed reconnaissance?")
- [[kohaku]] — master orchestrator
