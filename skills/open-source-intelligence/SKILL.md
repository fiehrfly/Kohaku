---
name: open-source-intelligence
description: Use as the discovery and evaluation protocol whenever [[live-off-the-land]] reaches Tier 3 (FOSS selection). Concrete reconnaissance stack — GitHub search, package registries, awesome-lists, community signal, source-reading, license check. Produces a comparison matrix that [[shiro-mode]] can dismiss alternatives from. Default for any "what library should we use" decision.
---

# Open-Source Intelligence Protocol

## What this is

The concrete reconnaissance stack Kohaku runs when [[live-off-the-land]] determines a FOSS dependency is needed (Tier 3). This skill encodes the discovery order, the signal weights, and the source-reading discipline.

**The output of this skill is a comparison matrix.** That matrix becomes [[shiro-mode]]'s input — every alternative listed and dismissed with a specific reason.

## When to run

- Selecting between libraries for the same job
- Replacing a deprecated dependency
- Evaluating "we've always used X" against current alternatives
- Pre-empting a "did you consider Y?" PR comment

## The discovery stack (run in order)

### 1. GitHub search

- Query patterns:
  - `topic:<domain> language:<lang> stars:>100 pushed:>2025-01-01`
  - `topic:<domain> sort:updated`
  - Replace year to filter for recent activity
- Signals to capture per candidate:
  - Contributor count
  - Issue response time (open issue age, recent closures)
  - PR merge frequency
  - CHANGELOG / Releases activity
  - License (visible on repo home)
  - Test coverage badge

### 2. Package registries

- **npm** — npmjs.com — filter by weekly downloads, last publish date
- **PyPI** — pypi.org — filter by recent upload
- **crates.io** — filter by recent version, download count
- **pub.dev** (Dart/Flutter) — filter by likes, pub points, popularity
- **RubyGems** — rubygems.org — total downloads, recent versions

Cross-check the registry's metadata against the GitHub repo's. Divergence (registry says latest is v2.0 but repo's last release is v1.4 from 2022) is a red flag.

### 3. Awesome lists

- [github.com/sindresorhus/awesome](https://github.com/sindresorhus/awesome) — root list
- Search GitHub for `awesome <domain>` for framework/language-specific curated lists
- Awesome lists are curated subjective signal — treat as a starting point, not a ranking

### 4. Community signal

- Hacker News "Ask HN" threads (search: site:news.ycombinator.com "<library>")
- Reddit communities: r/programming, r/webdev, r/<framework>
- Stack Overflow most-voted answers — **check the date**; favour recent over historic
- Discord / Slack community activity (if public)

### 5. Source reading

**Always read the source of the top candidate before committing.** The README is marketing; the source is truth.

- Read at least: `package.json` / `Cargo.toml` / `pyproject.toml`, the main entry file, one test file, the CONTRIBUTING.md
- Check for: dependency count and quality, test density, code style (does it match what you'd write?), commented-out code (often signals abandonment)

For the top 2 candidates, compare *side by side* — `git clone` both into a local sandbox and (if your assistant indexes repos, e.g. `mcp__jcodemunch__index_folder`) ingest them for navigation.

### 6. License check

- **MIT / Apache 2.0 / BSD** — permissive, commercial use cleared
- **ISC / 0BSD** — even more permissive
- **GPL / LGPL** — copyleft; may have implications for closed-source projects depending on linking
- **AGPL** — strong copyleft; affects web service deployment
- **SSPL / BSL / Commons Clause** — source-available but commercially restricted; verify your use case before adopting
- **No license** — legally not open-source; do not use

Verify the license matches your project's compatibility constraints. If your project has a license file at root, confirm dependency licenses are compatible with it.

## The selection criteria matrix

Build this for every non-trivial selection. Fill in real values.

| Signal | Weight | Candidate A | Candidate B | Candidate C |
|---|---|---|---|---|
| Last commit | HIGH | < 1 month | 3 months | 14 months |
| License | HIGH | MIT | Apache 2.0 | GPL-3.0 |
| Test coverage | HIGH | 87% | none | 62% |
| Open issue age (median) | MEDIUM | 12 days | 45 days | 240 days |
| Stars | MEDIUM (vanity) | 4,200 | 800 | 12,000 |
| Forks | MEDIUM | 320 | 65 | 980 |
| Contributors | MEDIUM | 47 | 4 | 110 |
| Bundle size / runtime cost | CONTEXT | 12 KB | 4 KB | 38 KB |
| Bus factor (top maintainer % of commits) | LOW | 38% | 95% | 22% |
| Documentation quality | LOW | Good | Sparse | Excellent |

**Highly-weighted failures eliminate a candidate; medium-weighted failures require explicit acknowledgement; low-weighted failures are notes.**

## The output: a decision record

The matrix becomes the body of the decision. Hand it to [[shiro-mode]] to produce:

> **Recommendation: Candidate B.**
>
> - **A dismissed:** GPL-3.0 license incompatible with project's MIT license (HIGH weight failure)
> - **C dismissed:** Last commit > 12 months, open issue age 240 days median — abandonment signal (HIGH weight failure)
> - **B selected:** Apache 2.0 ✓, active maintenance ✓, smaller bundle ✓, lower contributor count and bus factor noted but tolerable for the use case

The reviewer cannot ask "did you consider A or C?" because A and C are already dismissed *with specific reasons*.

## Common rationalizations

| Excuse | Reality |
|---|---|
| "I trust this library — it's popular" | Popularity at some point in history ≠ current health |
| "Reading the source is overkill for a small dependency" | A small dependency that breaks is the same incident as a large dependency that breaks |
| "I'll just pick the first result" | Then you are letting GitHub's ranking algorithm make your architecture decisions |
| "The matrix is bureaucracy" | The matrix is a pre-written defense against the next "did you consider X?" review comment |

## Red flags in candidate evaluation

- Last commit > 12 months (especially if framework dependencies have moved)
- Single maintainer with >90% of commits, no recent contributors
- Issues open > 6 months with no maintainer response
- "Star history" graph shows decline (use star-history.com)
- License is "see LICENSE.md" but the file is missing or contradictory
- README says "production-ready" but no consumer is publicly listed
- Test directory exists but coverage badge is broken or absent
- npm/PyPI shows version newer than the repo's latest tagged release

## When NOT to run the full protocol

- The dependency is single-purpose, deeply audited, and used by your entire ecosystem (e.g., React in a React project, Tokio in a Rust async project)
- The selection is constrained externally (compliance, customer requirement, framework convention)
- You're doing a one-off script and the affordable loss is "delete and rewrite"

For those, the abbreviated check is: license + last commit + a single source-read of the entry file.

## Related

- [[live-off-the-land]] — this skill is Tier 3's protocol
- [[shiro-mode]] — consumes the matrix output
- [[hacker-helix]] — Step 1 (Reconnaissance) at the dependency level
- [[blank-diagnostic]] — Q8 routes here when the answer involves external tools
- [[kohaku]] — master orchestrator
