---
type: source
source_type: official-documentation
title: "Claude Code Plugins Official Docs"
author: "Anthropic"
date_published: 2026
date_accessed: 2026-06-10
url: "https://code.claude.com/docs/en/plugins"
confidence: high
status: evergreen
tags:
  - source
  - claude-code
  - plugin
  - anthropic
  - official-spec
related:
  - "[[sources/Claude Code Skills Official Docs]]"
  - "[[sources/Claude Code Plugin Reference]]"
  - "[[concepts/Selective Install Pattern]]"
  - "[[questions/Research ECC Plugin Selective-Install Patterns]]"
key_claims:
  - "A plugin is a self-contained directory of components (skills, agents, hooks, MCP servers, LSP servers, monitors)"
  - ".claude-plugin/plugin.json is the manifest; name + description + version + author are the fields"
  - "Plugin skills are namespaced as /plugin-name:skill-name to prevent collisions"
  - "Standalone (.claude/) vs Plugin tradeoff: short names + project-only vs versioned + cross-project shareable"
  - "Two official marketplaces: claude-plugins-official (curated) + claude-plugins-community (open submission)"
---

# Claude Code Plugins Official Docs

## Summary

Anthropic's canonical specification for creating Claude Code plugins. Defines the plugin directory layout, `.claude-plugin/plugin.json` manifest schema, component types (skills, agents, hooks, MCP servers, LSP servers, monitors, bin executables, settings overlays), and the standalone-vs-plugin tradeoff matrix.

## What It Contributes

This is the **foundational spec** that every third-party selective-install system (ECC, wshobson/agents, Compound Engineering Plugin) builds on. The doc establishes the whole-plugin install model — you install a plugin, you get all its components — which selective-install layers refine into component-subset selection.

Key contributions to the research:

- **Plugin manifest fields**: `name` (unique identifier + skill namespace), `description` (shown in plugin manager), `version` (optional; if omitted, git commit SHA is used per commit), `author` (optional attribution). Additional fields documented in `plugins-reference`: `homepage`, `repository`, `license`.
- **Directory layout** (everything at plugin root, NOT inside `.claude-plugin/`):
  - `.claude-plugin/plugin.json` — manifest
  - `skills/` — `<name>/SKILL.md` directories
  - `commands/` — flat markdown files (legacy; prefer `skills/`)
  - `agents/` — custom agent definitions
  - `hooks/hooks.json` — event handlers
  - `.mcp.json` — MCP server config
  - `.lsp.json` — LSP server config
  - `monitors/monitors.json` — background watchers
  - `bin/` — executables added to Bash PATH when plugin enabled
  - `settings.json` — default settings applied when plugin enabled (currently only `agent` + `subagentStatusLine` keys supported)
- **Single-skill shortcut** — plugins shipping one skill can place `SKILL.md` at plugin root, skip `skills/` directory.
- **Two scopes**: User (`~/.claude/skills/`) installs for all projects; Project (`.claude/skills/`) installs for current repo only.
- **Skill namespacing**: standalone `.claude/skills/hello/SKILL.md` → `/hello`. Plugin-bundled → `/plugin-name:hello`. Namespace prefix = `name` field in `plugin.json`.
- **Development workflow**: `claude --plugin-dir ./my-plugin` loads local copy (Claude Code 2.1.128+ also accepts `.zip`). `--plugin-url <url>` fetches archive per session. `/reload-plugins` picks up edits without restart.
- **Marketplaces**: `claude-plugins-official` (Anthropic-curated, auto-available) + `claude-plugins-community` (open submissions, reviewed). User can add custom marketplaces via GitHub repos, git URLs, local paths, remote URLs.
- **Version management** — explicit `version` field bumps trigger user updates. Without it, every commit SHA becomes a new version (noisy for git-installed plugins).
- **Settings override priority** — `settings.json` at plugin root > `settings` in `plugin.json`. Unknown keys silently ignored.

Notably absent: any built-in component-subset install grammar (no `--with`/`--without` at the plugin install command). This is the gap that ECC selective-install fills.

## Notes

- Doc states: "**Plugins are the distribution format, skills are the content. You install plugins; you use skills.**"
- Migration path documented: `.claude/commands/` → `<plugin>/commands/`; hooks from `settings.json` → `hooks/hooks.json`; otherwise same format.
- Submission for `claude-plugins-community`: `claude.ai/settings/plugins/submit` or `platform.claude.com/plugins/submit`. Approved plugins pinned to commit SHA in catalog; nightly sync from review pipeline.
- `claude plugin validate` runs locally before submission; review pipeline runs the same check + automated safety screening.
