# Kohaku — Compatibility & Portability

Kohaku is authored as a **Claude Code plugin** (that's its native installer), but the skills themselves are plain Markdown protocols. They are designed to be **system-agnostic** — usable from any AI coding assistant, IDE plugin, or even a plain LLM chat.

This file documents the portability layer so that an agent running on Copilot CLI, Cursor, Cline, Codex, Gemini CLI, Continue.dev, or any future system can apply Kohaku without modification.

---

## The portability contract

Every Kohaku skill is written against four **abstract capabilities**. If your host system provides each of them — under whatever name — Kohaku works.

| Capability | What it means | Example hosts |
|---|---|---|
| **Skill loading** | Load a Markdown protocol file by name | Claude Code `Skill`, Copilot CLI `skill`, Gemini CLI `activate_skill`, Cursor's "rules", Cline's `.clinerules`, or simply `Read skills/<name>/SKILL.md` |
| **Code navigation** | Plan, search symbols, find references, read files | jCodemunch MCP, LSP, `ripgrep`, `ctags`, `ast-grep`, IDE-native search, or just `grep + find` |
| **Web reconnaissance** | Search and fetch web pages | `WebSearch`/`WebFetch`, `firecrawl`, `exa`, `curl + a search engine`, or the assistant's built-in browser |
| **File I/O** | Read, write, edit local files | Every assistant has this in some form |

If your host system is missing one of these capabilities, the affected skill section degrades gracefully — see the **Fallback table** below.

---

## Invocation across systems

Kohaku ships five Claude Code slash commands (`/kohaku`, `/blank`, `/sora`, `/shiro`, `/goal-kohaku`). On other systems, replicate the same effect by prefixing your prompt with a directive:

| Claude Code | Equivalent on any other system |
|---|---|
| `/kohaku <objective>` | "Run the full Blank Protocol on: `<objective>`. Treat the objective as the binding session goal. Start by loading `skills/kohaku/SKILL.md`." |
| `/blank <objective>` | Same as `/kohaku`. |
| `/sora <objective>` | "Run Sora-mode on: `<objective>`. Load `skills/sora-mode/SKILL.md` and follow it." |
| `/shiro <objective>` | "Run Shiro-mode on: `<objective>`. Load `skills/shiro-mode/SKILL.md` and follow it." |
| `/goal-kohaku <objective>` | Equivalent to `/kohaku`; legacy form for when the user has already typed Claude Code's `/goal` separately. |

Some systems (Copilot CLI, Cline, Continue.dev) auto-discover skill files in `skills/`. On those, the skill descriptions handle activation — no slash prefix needed.

---

## Tool-name fallback table

Wherever a skill names a concrete tool, you may substitute the closest equivalent your host provides.

| Skill references | Portable alternatives |
|---|---|
| `mcp__jcodemunch__plan_turn` | Your assistant's retrieval planner; or write the plan yourself in one paragraph |
| `mcp__jcodemunch__search_symbols` | LSP `workspace/symbol`, `ctags -R + grep`, `ast-grep`, GitHub code search |
| `mcp__jcodemunch__search_text` | `ripgrep`, IDE project search, `grep -rn` |
| `mcp__jcodemunch__find_references` | LSP `textDocument/references`, `rg -F '<symbol>'` |
| `mcp__jcodemunch__get_blast_radius` | Manual: `find_references` + trace upward |
| `mcp__jcodemunch__get_repo_outline` | `tree -L 3`, `eza --tree --level 3`, `git ls-files \| head` |
| `mcp__jcodemunch__get_file_tree` | `tree`, `find . -type f` |
| `mcp__jcodemunch__get_repo_health` | Optional — skip if unavailable |
| `mcp__jcodemunch__find_similar_symbols` | Fuzzy match: `rg -i`, `fzf` against `ctags` output |
| `mcp__jcodemunch__suggest_queries` | Optional — write the queries manually |
| `mcp__jcodemunch__index_folder` | `git clone` the candidate and explore locally |
| `WebSearch` / `WebFetch` | `firecrawl`, `exa`, browser-use, `curl + a search engine` |
| `Skill` tool | `Read skills/<name>/SKILL.md` and follow the protocol |
| `Edit` / `Write` | Whatever file-edit tool your assistant provides |

**Rule:** if a sibling tool name like `mcp__jcodemunch__plan_turn` appears in a skill body, treat it as "the most precise example I know of; substitute your own equivalent."

---

## Sibling-plugin references

Some skills reference other plugins from the author's personal Claude Code stack:

| Reference | What it means | Portable substitute |
|---|---|---|
| `superpowers:brainstorming` | A brainstorming protocol | Any structured ideation skill, or just "spend 5 minutes listing options before picking" |
| `superpowers:test-driven-development` | A TDD discipline skill | Standard TDD: red → green → refactor |
| `superpowers:systematic-debugging` | A debugging protocol | Standard scientific debugging: hypothesis → reproduce → bisect |
| `superpowers:using-git-worktrees` | Worktree isolation | `git worktree add` manually |
| `superpowers:writing-plans` | Plan-document writing | Any plan template |
| `superpowers:writing-skills` | Skill authoring | Just write Markdown |
| `compound-engineering:ce-code-review` | Multi-persona code review | Your team's review process |
| `document-skills:doc-coauthoring` | Documentation authoring | Standard doc writing |
| `document-skills:webapp-testing` | Browser/UI test authoring | Playwright/Cypress/Selenium |
| Ruflo swarm | Multi-agent code-execution swarm | Any execution layer — even just you, the user, doing the coding |
| OpenDesign router | Design-domain router | Any design tool/workflow |

**These are not required.** They are the author's preferred execution layer. Substitute or omit freely.

---

## Wiki-link convention

Kohaku skills cross-reference each other with `[[skill-name]]` syntax. This is a portable convention:

- **Claude Code, Obsidian, many wiki systems:** treats `[[skill-name]]` as a link.
- **Everywhere else:** renders as plain text. Interpret it as "see `skills/<skill-name>/SKILL.md`".

Wiki-links are intentionally retained because they round-trip safely across systems without breaking rendering.

---

## Installation across hosts

### Claude Code (native)

```bash
# Via marketplace
/plugin marketplace add fiehrfly/Kohaku
/plugin install kohaku@kohaku

# Or clone
git clone https://github.com/fiehrfly/Kohaku ~/.claude/plugins/cache/kohaku/kohaku/0.2.0
```

### Copilot CLI

Skills under `skills/` are auto-discovered by `gh copilot`'s plugin loader if dropped into the assistant's skill path. Or clone and reference manually:

```bash
git clone https://github.com/fiehrfly/Kohaku ~/.copilot/skills/kohaku
```

### Cursor

Cursor's "Rules" feature can ingest any of the skill files. Either:
- Add to `.cursor/rules/` per project, or
- Paste a skill's body into Cursor's "Rules for AI" global setting

### Cline / Roo Code (VS Code extensions)

These read `.clinerules` and `.roomodes` files. Symlink or copy individual skill bodies in:

```bash
git clone https://github.com/fiehrfly/Kohaku ~/Tools/kohaku
ln -s ~/Tools/kohaku/skills/kohaku/SKILL.md .clinerules
```

### Gemini CLI

```bash
git clone https://github.com/fiehrfly/Kohaku ~/.gemini/skills/kohaku
```

Gemini auto-loads skill frontmatter at session start; the `activate_skill` tool then expands them on demand.

### Codex CLI

Codex doesn't have a native skill system, but you can prepend the skill body to your system prompt or paste it into a `.codexrc` file.

### Continue.dev

Continue reads `.continue/config.yaml` for system prompts. Reference the skill file path under `customCommands` or `systemMessage`.

### Plain LLM chat (no agent harness)

Paste the skill body into the chat as a system instruction, then state your objective. Manual, but it works.

---

## Capability degradation

If a host is missing a capability, here is how each skill degrades:

| Missing capability | Affected skills | Degraded behavior |
|---|---|---|
| Code navigation | `shiro-mode`, `hacker-helix`, `live-off-the-land` Tier 2, `open-source-intelligence` | Skip the symbol/blast-radius queries; rely on the user's prose description of the codebase |
| Web reconnaissance | `open-source-intelligence`, `hacker-helix` external recon | The user provides the comparison-matrix data manually |
| Skill loading | All | Inline the skill body into the system prompt once at session start |
| File I/O | All execution | Kohaku itself never executes — only the downstream layer needs file I/O |

---

## What is **not** portable

These pieces are Claude-Code-specific by design and have no equivalent elsewhere:

- The `commands/` directory and `$ARGUMENTS` substitution (Claude Code slash-command runtime)
- `.claude-plugin/marketplace.json` and `plugin.json` (Claude Code marketplace format)
- The `kohaku-mastermind` subagent file (Claude Code subagent format)

These do not block portability — they are the **Claude Code installer**. Other hosts ignore them; the `skills/` directory is the portable payload.

---

## Closing

The skills are the product. The plugin scaffolding is the Claude Code delivery mechanism. Kohaku's strategic protocols (Blank Protocol, Hacker Helix, the 10-question diagnostic) are framework-agnostic by intent — Garrett Gee's mindset and the Blank methodology predate every AI coding assistant.

If your assistant can read files and follow instructions, Kohaku runs.

Aschente.
