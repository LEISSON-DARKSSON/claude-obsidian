---
type: source
source_type: open-source-repo
title: "wshobson/agents Multi-Harness Marketplace"
author: "wshobson"
date_published: 2026
date_accessed: 2026-06-10
url: "https://github.com/wshobson/agents"
confidence: high
status: developing
tags:
  - source
  - claude-code
  - cross-harness
  - marketplace
  - multi-harness
related:
  - "[[concepts/Harness Adapter Pattern]]"
  - "[[concepts/Cross-Harness Compatibility]]"
  - "[[sources/ECC Repository README]]"
  - "[[sources/Compound Engineering Plugin]]"
  - "[[questions/Research ECC Plugin Selective-Install Patterns]]"
key_claims:
  - "84 plugins, 192 agents, 156 skills, 102 commands, 16 orchestrators"
  - "Multi-harness: Claude Code (native), Codex CLI, Cursor, OpenCode, Gemini CLI, GitHub Copilot"
  - "One source-of-truth (plugins/), five harnesses — each adapter emits native artifacts"
  - "Make-based generation: make generate-all, make validate, make garden (drift detection)"
  - "Codex 8 KB skill cap respected by the adapter; commands → skills mapping"
  - "Three-tier model strategy: Opus 4.7 (architecture/security) → inherit → Sonnet → Haiku"
---

# wshobson/agents Multi-Harness Marketplace

## Summary

The closest architectural comparable to ECC. wshobson's `agents` repo ships 84 plugins (each an isolated, composable unit with its own `.claude-plugin/plugin.json`) to 5 harnesses (Claude Code native + Codex CLI + Cursor + OpenCode + Gemini CLI + GitHub Copilot) from one Markdown source. Adapters generate harness-native artifacts at build time (`make generate-all`) rather than runtime.

## What It Contributes

Provides the **alternative-pattern reference** to ECC's selective install:

- **Per-plugin atomic units, not component-subset selection**. Install grain is the whole plugin (e.g., `python-development` ships 3 agents + 1 command + 16 skills as one bundle). User selects which plugins to install; not which components within a plugin.
- **Build-time generation**: `make generate-all` produces all five harness directories from the source `plugins/`. Outputs:
  - Claude Code: source-of-truth, `marketplace.json` + `plugins/`
  - Codex CLI: `.agents/plugins/marketplace.json` + `plugins/*/.codex-plugin/plugin.json` (committed); `.codex/skills/`, `.codex/agents/` (gitignored)
  - Cursor: `.cursor-plugin/` + `.cursor/rules/` (thin marketplace, curated rules, reuses `.claude/`)
  - OpenCode: `.opencode/agents/` + `.opencode/commands/` + `.opencode/skills/` (`permission:` block from `tools:` allowlist; OpenCode-safe skill names)
  - Gemini CLI: `skills/` + `agents/` + `commands/` (TOML; native skills + subagents per April 2026 Gemini spec)
  - Copilot: `.copilot/agents/` + `.copilot/skills/` + `.copilot/commands/` (Markdown agent profiles; model maps to GPT-5 family)
- **Per-harness install commands**:
  - Claude Code: `/plugin marketplace add wshobson/agents` then `/plugin install <plugin-name>`
  - Codex CLI: `npx codex-marketplace add wshobson/agents`
  - Cursor: clone + adapter use
  - Gemini CLI: `gh repo clone wshobson/agents ~/agents && cd ~/agents && make generate HARNESS=gemini && gemini extensions install .`
  - OpenCode: `make install-opencode` (runs generate + symlinks)
- **Per-harness constraints absorbed by the adapter**: Codex 8 KB skill cap (split or trim); commands → skills (Codex doesn't have commands as a distinct concept).
- **Drift detection**: `make garden` checks drift / dead links / cap violations between source and generated outputs.
- **Validation**: `make validate` runs structural checks.
- **Three-tier model strategy** for plugin authors:
  - Tier 1 Opus 4.7 — architecture, security, code review, production-critical
  - Tier 2 inherit — user-chosen for backend / frontend / AI-ML / specialized
  - Tier 3 Sonnet — docs, testing, debugging, API references
  - Tier 4 Haiku — fast operational tasks, SEO, deployment, content

## Notes

- Phrasing from the README that distinguishes the approach: **"Each adapter emits harness-native artifacts (not lowest-common-denominator translations)."** Same goal as ECC, different scope (per-plugin vs per-component).
- Quality evaluation harness shipped: `plugin-eval` runs static + LLM judge + Monte Carlo scoring on plugins. Three-layer eval (deterministic, semantic, statistical reliability).
- Comparable scale to ECC mid-tier: 84 plugins (vs ECC's 261 skills shipped as install components), 156 skills, 102 commands. ECC has more total content but ships it under fewer profiles; wshobson has fewer items but each is an installable atomic unit.
- Repository is actively maintained as of June 2026. License + contribution model match ECC's MIT + community PRs.
