---
type: source
source_type: official-documentation
title: "Claude Code Skills Official Docs"
author: "Anthropic"
date_published: 2026
date_accessed: 2026-06-10
url: "https://code.claude.com/docs/en/skills"
confidence: high
status: evergreen
tags:
  - source
  - claude-code
  - skill
  - anthropic
  - official-spec
  - agent-skills
related:
  - "[[sources/Claude Code Plugins Official Docs]]"
  - "[[concepts/Selective Install Pattern]]"
  - "[[questions/Research ECC Plugin Selective-Install Patterns]]"
key_claims:
  - "Skills extend Claude's capabilities; SKILL.md frontmatter + markdown body is the format"
  - "Commands and skills have merged: .claude/commands/<name>.md and .claude/skills/<name>/SKILL.md both create /<name>"
  - "Claude Code skills follow the Agent Skills open standard (agentskills.io)"
  - "Skill body loads only when used — long reference material costs almost nothing until needed"
  - "Skills extend the standard with invocation control, subagent execution, dynamic context injection"
---

# Claude Code Skills Official Docs

## Summary

Anthropic's canonical specification for Claude Code skills. Defines the SKILL.md file format (YAML frontmatter + markdown body), skill discovery (`~/.claude/skills/` user-scope, `.claude/skills/` project-scope), invocation control (auto by Claude vs explicit `/name`), and the merger of commands and skills into one model.

## What It Contributes

Skills are the **content layer** under the plugin distribution layer. This doc establishes:

- **File format**: SKILL.md with YAML frontmatter (`name`, `description`, optional `disable-model-invocation`, optional `allowed-tools`) + markdown body of instructions.
- **Path convention**: a skill is a folder containing `SKILL.md`. Optional supporting files in subfolders (`reference.md`, `scripts/`, etc.). Single-file `.md` in `.claude/commands/` also creates a skill (legacy command path; equivalent behavior).
- **Two install scopes**:
  - `~/.claude/skills/<name>/SKILL.md` — user scope, all projects
  - `.claude/skills/<name>/SKILL.md` — project scope, current repo
- **Lazy loading** — "a skill's body loads only when it's used, so long reference material costs almost nothing until you need it." This is the cost mechanism that makes selective install meaningful — skills cost on use, not on install.
- **Open standard alignment** — "Claude Code skills follow the Agent Skills open standard, which works across multiple AI tools." This is what enables cross-harness adapter patterns (the same SKILL.md can be re-emitted into Cursor/Codex/Gemini directories).
- **Claude Code extensions** to the open standard:
  - **Invocation control** — `disable-model-invocation: true` makes a skill explicit-only (`/skill-name` invocation, no automatic Claude use).
  - **Subagent execution** — skills can run in a subagent context for isolated tool calls.
  - **Dynamic context injection** — skill body can be templated against the live session.
- **`$ARGUMENTS` placeholder** — user input after the skill name is interpolated into the body.
- **Bundled skills** — `/help`, `/compact`, `/debug`, `/code-review` ship in the box.
- **Plugin-shipped skills** — namespaced as `/plugin-name:skill-name`; standalone skills get short names like `/hello`.

## Notes

- The doc explicitly recommends creating a skill "when you keep pasting the same instructions, checklist, or multi-step procedure into chat, or when a section of CLAUDE.md has grown into a procedure rather than a fact." This is the migration trigger.
- "Custom commands have been merged into skills" — this is the unification event that ECC's component-family taxonomy (`agent:`, `skill:`, `command:`) had to absorb.
- Cross-tool portability claim (`agentskills.io`) is what makes selective-install systems' cross-harness adapter approach viable in principle.
