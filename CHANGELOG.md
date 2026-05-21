# Changelog

All notable changes to Kohaku are recorded here. Versioning follows [Semantic Versioning](https://semver.org/).

## [0.2.2] — 2026-05-21

### Manifest fix — `/kohaku` actually loads now

v0.2.1 shipped broken. The manifest declared a top-level `agents` field, which is **not** in Claude Code's plugin schema. Plugin load failed with `Validation errors: agents: Invalid input`, so `/kohaku`, `/blank`, `/sora`, `/shiro`, and `/goal-kohaku` were silently absent from the skill list even when `claude plugin list` reported the plugin as enabled. This was the regression that v0.2.1's own changelog described as a "forward-compatible" hardening — it was the opposite.

### Fixed
- **`.claude-plugin/plugin.json`** — removed the unsupported top-level `agents` field. Claude Code auto-discovers `agents/` by directory convention (verified against `superpowers` and `ruflo-core`, both of which omit it). Collapsed `commands` and `skills` from single-element arrays to plain strings to match the canonical shape used by upstream plugins. Verified clean load: `claude plugin list` shows `enabled`; `claude plugin details kohaku@kohaku` reports 15 skills + 1 agent (`kohaku-mastermind`) + 0 hooks discovered.
- **`.claude-plugin/marketplace.json`** — bumped plugin version to 0.2.2.

### Lesson recorded
- The Claude Code plugin schema accepts `commands` and `skills` as path declarations (string or single-element array), but **does not** accept `agents`. Agents live under `agents/` and are auto-discovered. Future manifest changes should be diffed against a known-good plugin (`superpowers`, `ruflo-core`) before tagging a release.

### Not changed
- The Blank Protocol bodies, the Hacker Helix steps, all 10 skill files, the `kohaku-mastermind` subagent, and the COMPATIBILITY.md portability layer are byte-for-byte unchanged from v0.2.1. v0.2.2 is purely a manifest-schema fix that makes the v0.2.1 release content actually loadable.

---

## [0.2.1] — 2026-05-18

### Install pipeline fix — make `/kohaku` actually invocable

v0.2.0 documented a manual install path (`git clone https://github.com/fiehrfly/Kohaku ~/.claude/plugins/cache/kohaku/kohaku/0.2.0`) that silently failed: cloning into the cache directory does **not** register the plugin in `~/.claude/plugins/known_marketplaces.json` or `~/.claude/plugins/installed_plugins.json`, so Claude Code never sees `/kohaku`, `/blank`, `/sora`, `/shiro`, or `/goal-kohaku`. v0.2.1 fixes the docs and hardens the plugin manifest.

### Fixed
- **README.md** — replaced the misleading "manual clone into cache" path with a correct local-dev install flow: `git clone` to a working directory, then `/plugin marketplace add <path>` + `/plugin install kohaku@kohaku`. Both writes (marketplace + install) are required for Claude Code to recognize the commands.
- **COMPATIBILITY.md** — same correction in the per-host install section.

### Changed
- **`.claude-plugin/plugin.json`** — added explicit `commands`, `skills`, and `agents` path declarations. v0.2.0 relied on Claude Code's directory auto-discovery, which works on current versions but is fragile across plugin runtimes. Explicit paths are forward-compatible.
- **`.claude-plugin/marketplace.json`** — bumped plugin version to 0.2.1.

### Not changed
- The Blank Protocol bodies, the Hacker Helix steps, all 10 skill files, the `kohaku-mastermind` subagent, and the COMPATIBILITY.md portability layer (Copilot CLI, Cursor, Cline / Roo Code, Gemini CLI, Codex, Continue.dev, plain LLM chat) are byte-for-byte unchanged. v0.2.1 is purely an install-pipeline fix.

---

## [0.2.0] — 2026-05-17

### Portability pass — make Kohaku system-agnostic

Kohaku v0.1.0 was authored as a Claude Code plugin, and several skill bodies hardcoded Claude-Code-only tool names and sibling-plugin references. v0.2.0 decouples the protocols from any single host while keeping Claude Code as the primary native install target.

### Added
- **`COMPATIBILITY.md`** — full portability layer documenting how Kohaku skills map onto Copilot CLI, Cursor, Cline / Roo Code, Gemini CLI, Codex, Continue.dev, and plain LLM chat. Includes:
  - The four-capability portability contract (skill loading, code navigation, web reconnaissance, file I/O)
  - Per-host installation walkthroughs
  - A tool-name fallback table (jCodemunch MCP → LSP / ripgrep / ctags / ast-grep / IDE search)
  - A sibling-plugin reference table (superpowers / compound-engineering / Ruflo / OpenDesign → portable substitutes)
  - A capability-degradation matrix for hosts missing one of the four capabilities
- New `claude-code`, `copilot-cli`, `cursor`, `cline`, `gemini-cli`, `codex`, `ai-coding-assistant`, and `portable` keywords in `plugin.json` and `marketplace.json`

### Changed
- **Skills** decoupled from hardcoded `mcp__jcodemunch__*` calls; each tool reference now reads "use jCodemunch `X` if available; otherwise LSP / ripgrep / your IDE's project search — see `COMPATIBILITY.md`":
  - `skills/shiro-mode/SKILL.md` — Step 1 enumeration tools
  - `skills/hacker-helix/SKILL.md` — Step 1 reconnaissance tools, Step 2 leverage-point mapping
  - `skills/live-off-the-land/SKILL.md` — Tier 2 codebase-utility check
  - `skills/open-source-intelligence/SKILL.md` — Step 5 source-reading
  - `commands/shiro.md` — enumeration step
  - `agents/kohaku-mastermind.md` — Tools-you-should-use list reframed with host-equivalent fallbacks
- **Sibling-plugin references** in `skills/kohaku/SKILL.md` ("What Kohaku is NOT" section) now read "e.g. Claude Code's `superpowers:brainstorming`" with portable substitutes inline, instead of hardcoded plugin paths
- **Ruflo / OpenDesign references** reframed as "your execution layer (e.g. Ruflo swarm) / design layer (e.g. OpenDesign)" so non-author users with a different stack can substitute freely. Affected: `skills/kohaku/SKILL.md`, `skills/sora-mode/SKILL.md`, `skills/shiro-mode/SKILL.md`
- **`README.md`** — title section reframed from "Claude Code plugin" to "skill pack for AI coding assistants" with Claude Code as primary native installer; added a per-host installation section pointing at `COMPATIBILITY.md`
- **`ARCHITECTURE.md`** — Layer 3 description reframed to allow any execution layer (Ruflo/OpenDesign as concrete example, not the only option); added portability note at the top; superpowers reference made into a fallback chain in the Versioning section
- **`.claude-plugin/plugin.json` and `marketplace.json`** — descriptions reframed from "for Claude Code" to "native Claude Code; portable to other hosts"; bumped version to 0.2.0

### Fixed
- Added `runs/` to `.gitignore` to keep local Kohaku run artifacts out of git
- `skills/kohaku/SKILL.md` self-reference to `[[ruflo-swarm-bridge]]` and `[[opendesign-router]]` (skills that exist only in the author's personal Claude Code stack) replaced with portable phrasing

### Not changed
- The Blank Protocol bodies (Sora, Shiro, Blank synthesis state) — substance unchanged
- The Hacker Helix 5 steps and 6 principles — substance unchanged
- The 10-question Blank Diagnostic — substance unchanged
- The `commands/*.md` files keep Claude Code's `$ARGUMENTS` substitution (that file format *is* the Claude Code installer; other hosts use prose invocation as documented in `COMPATIBILITY.md`)
- `[[wiki-link]]` cross-references between skills (retained as a portable convention — render as links in wiki-aware hosts, as plain text everywhere else)

## [0.1.0] — 2026-05-17

### Initial release
- 10 skills: `kohaku`, `sora-mode`, `shiro-mode`, `blank-mode`, `identify-the-real-game`, `hacker-helix`, `live-off-the-land`, `deliberate-sacrifice`, `blank-diagnostic`, `open-source-intelligence`
- 1 subagent: `kohaku-mastermind` (Opus tier, strategy-only)
- 5 commands: `/kohaku`, `/blank`, `/sora`, `/shiro`, `/goal-kohaku`
- `README.md`, `ARCHITECTURE.md`, MIT `LICENSE`
- `.claude-plugin/plugin.json` and `marketplace.json` for native Claude Code install
