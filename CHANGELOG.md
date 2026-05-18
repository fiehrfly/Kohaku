# Changelog

All notable changes to Kohaku are recorded here. Versioning follows [Semantic Versioning](https://semver.org/).

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
