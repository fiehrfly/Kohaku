---
name: live-off-the-land
description: Use before adding any dependency, importing any library, writing any new utility, or selecting any external tool. Enforces a hard ordering — standard library → existing codebase utilities → free and open-source → paid — and requires written justification before crossing any tier. Gee's third principle from "The Hacker Mindset"; Kohaku's default tool-acquisition policy.
---

# Living off the Land

## The principle

Before importing a dependency, **exhaust the capability of what already exists.**

This is the third of Garrett Gee's six principles in *The Hacker Mindset* (2024). In offensive security, "living off the land" means using tools already present on the target system instead of dropping new binaries — because new binaries leave forensic traces and trigger defenders. In software engineering, the same instinct applies: every new dependency leaves a trace (version pin, transitive deps, supply-chain surface, license obligation, future-upgrade tax). The cheapest dependency is the one you didn't add.

**Kohaku's default policy: free and open-source first. Always. Paid solutions are an explicit, justified, recorded escalation.**

## The hard ordering

Cross each tier only after the previous one is exhausted *with evidence*.

### Tier 1 — Standard library

Before anything: **does the language's standard library already do this?**

The most common failure mode in modern projects is adding a 200KB dependency for what is a one-line stdlib call. The Node/Python/JS ecosystems are especially prone to this.

Check:
- Official language documentation, latest version
- "How do I do X in [language] without dependencies?" search
- The language's own batteries-included modules (Python `itertools`, `functools`, `pathlib`, `dataclasses`; JavaScript `Intl`, `URL`, `URLSearchParams`, structuredClone; Rust `std::collections`; Go's `net/http` and stdlib generally)

### Tier 2 — Already in the codebase

**Does the existing codebase already have this pattern?**

Common failure: a developer adds a utility that already exists 50 lines deep in a sibling module under a slightly different name.

Check:
- `mcp__jcodemunch__search_symbols` with the operation name and synonyms
- `mcp__jcodemunch__search_text` for the relevant strings
- `mcp__jcodemunch__find_similar_symbols` for near-duplicates
- The project's own utility modules (`utils/`, `lib/`, `common/`)

If the utility exists but is named differently — rename towards convergence, don't duplicate.

### Tier 3 — Free and open-source (FOSS)

**Has someone else solved this in a well-maintained, permissively-licensed package?**

This is where most "I need a library" cases land. Default to FOSS — never reach for paid first.

Discovery protocol → [[open-source-intelligence]].

Selection criteria:
- **License** — MIT / Apache 2.0 / BSD preferred; GPL/AGPL requires legal review for commercial/SaaS projects
- **Maintenance** — commits in the last 6 months; issue response time under 30 days
- **Tests** — coverage badge, CI passing
- **Community** — non-trivial contributor count; recent merged PRs from non-author contributors
- **Bus factor** — single-maintainer is a warning, not a blocker; record it
- **Bundle size / runtime cost** — measured, not assumed

### Tier 4 — Paid solutions

**Only when Tiers 1–3 are genuinely insufficient — and the why is documented.**

Document, before adopting any paid tool:
- Which FOSS alternatives were evaluated
- What specific feature/SLA/scale they lacked
- What the affordable-loss budget is
- Who owns the contract and the cancellation trigger

If any of those are not documented, the answer is not yet "paid solution." Go back to Tier 3.

## Living off the land *inside* a codebase

The principle generalizes beyond dependency selection:

- **The function you need probably already exists**, named slightly differently. Search before writing.
- **The configuration you need is already in the framework**, just not surfaced in the tutorial. Read the source.
- **The pattern you are reinventing is already the project's established pattern.** Read more files before writing new ones.
- **The framework's quirks are often capabilities in disguise.** Mapping them gains you things the docs never advertised.

## Common rationalizations

| Excuse | Reality |
|---|---|
| "It's just one dependency" | One per developer per week × N developers × 52 weeks = supply chain risk |
| "Everyone uses this library" | Adoption velocity at point X in history. Check the commit graph now |
| "Writing it myself takes longer" | Maintaining a dependency forever takes longer than writing once |
| "The free version has limitations" | List them. Specifically. Often the limitation is "I would have to read the README more carefully" |
| "Paid has support" | Paid support is the optimistic case. The realistic case is a support ticket that takes 72 hours to get a useful reply |
| "We have the budget" | Budget is not a justification. It is a constraint. The justification is feature gap |

## Red flags — you've skipped this skill

- A `package.json` / `Gemfile` / `requirements.txt` diff with a new entry but no PR description note
- A "this is easier with library X" comment without naming what was already evaluated
- A paid SaaS adoption without a written comparison against at least two FOSS alternatives
- An import of `lodash` when ES2020+ already does the same thing
- An import of `moment.js` in 2026

## When NOT to live off the land

- Cryptography. Never roll your own. Reach directly for well-audited libraries (libsodium, ring, OpenSSL bindings).
- Compliance-mandated tooling (SOC2, HIPAA-scoped systems may require specific vendor certifications).
- True scale-only problems where the FOSS equivalent fails benchmarks by 10x+ — but verify the benchmark.

## Open-source intelligence handoff

Once you've decided "Tier 3 — FOSS", you need a reconnaissance protocol to find the right package. Hand off to [[open-source-intelligence]].

## Related

- [[open-source-intelligence]] — the discovery stack for Tier 3
- [[shiro-mode]] — the comparison matrix lives here
- [[hacker-helix]] — Principle 3 in context of the full methodology
- [[blank-diagnostic]] — Q8 ("what resources already exist that I have not fully used?")
- [[kohaku]] — master orchestrator
