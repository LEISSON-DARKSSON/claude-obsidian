---
type: source
source_type: official-documentation
title: "Claude Code Plugin Reference"
author: "Anthropic"
date_published: 2026
date_accessed: 2026-06-10
url: "https://code.claude.com/docs/en/plugins-reference"
confidence: high
status: evergreen
tags:
  - source
  - claude-code
  - plugin
  - anthropic
  - official-spec
  - reference
related:
  - "[[sources/Claude Code Plugins Official Docs]]"
  - "[[sources/Claude Code Skills Official Docs]]"
  - "[[questions/Research ECC Plugin Selective-Install Patterns]]"
key_claims:
  - "Complete technical specifications for all plugin component schemas + CLI commands"
  - "Skills directory: SKILL.md is the entry, supporting files allowed; commands directory: flat markdown files (legacy)"
  - "Skills + commands are auto-discovered on plugin install; no manual registration"
  - "Plugins shipping a single SKILL.md can place it at plugin root, skipping skills/ dir"
---

# Claude Code Plugin Reference

## Summary

Anthropic's complete technical reference for the Claude Code plugin system. Covers full schemas for `plugin.json`, `marketplace.json`, `hooks.json`, `.mcp.json`, `.lsp.json`, `monitors.json`, `settings.json`, plus the CLI commands (`claude plugin init`, `claude plugin validate`, `claude --plugin-dir`, `claude --plugin-url`), version management strategies, install-state tracking, and debugging/development tools.

## What It Contributes

The companion reference to `Plugins` and `Skills` docs. Defines the full surface area for:

- **All component schemas** — every field name across every component file is documented here.
- **Version management** — two strategies: (a) explicit `version` field in `plugin.json` (semantic versioning, users update on bump); (b) omit `version`, let git commit SHA serve as version identifier (every commit = new version, noisy but always-fresh).
- **Skills-directory plugins** — special class where `~/.claude/skills/<name>/.claude-plugin/plugin.json` makes a skill a fully-fledged plugin loadable as `<name>@skills-dir` without marketplace/install step. Auto-load rules + personal-vs-project-scope + workspace-trust requirements documented here.
- **Settings.json overlays** — currently supports `agent` (activates a plugin's custom agent as main thread) and `subagentStatusLine` keys.
- **Multiple `--plugin-dir`** — repeat flag to load multiple local plugins.
- **`--plugin-url`** — fetch `.zip` archive at startup; if fetch fails, Claude reports plugin load error and starts without it.
- **Plugin-dir override** — local `--plugin-dir` takes precedence over same-name installed plugin for that session (exception: settings-force-enable/disable plugins can't be overridden).
- **Hook schema** — `hooks.json` lives in plugin `hooks/` dir; same format as `.claude/settings.json` hooks block. `stdin` receives JSON event payload; use `jq` to extract fields.
- **Monitor schema** — `monitors/monitors.json` array of `{name, command, description, when?}`. Each stdout line from `command` is delivered to Claude as a notification.
- **LSP schema** — `.lsp.json` per-language: `{command, args, extensionToLanguage: {".ext": "language"}}`. Users must have the LSP binary installed.

## Notes

- Notable warning: "Don't put `commands/`, `agents/`, `skills/`, or `hooks/` inside the `.claude-plugin/` directory. Only `plugin.json` goes inside `.claude-plugin/`."
- Submission and review tooling documented: `claude plugin validate` matches CI check; approved plugins pinned to commit SHA in `anthropics/claude-plugins-community`.
- For the community-marketplace publication path, Anthropic syncs nightly from the review pipeline — delay between approval and `marketplace.json` appearance.

## Output Limit Note

WebFetch returned 76 KB on initial fetch (truncated to preview here). Full reference is comprehensive; preview consulted for the schema fields cited above. Re-fetch for any field not listed here.
