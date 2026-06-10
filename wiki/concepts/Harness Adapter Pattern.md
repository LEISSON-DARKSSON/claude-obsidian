---
type: concept
title: "Harness Adapter Pattern"
created: 2026-06-10
updated: 2026-06-10
tags:
  - concept
  - harness
  - adapter
  - cross-harness
  - architecture
status: developing
related:
  - "[[concepts/Selective Install Pattern]]"
  - "[[concepts/Cross-Harness Compatibility]]"
  - "[[sources/ECC Repository README]]"
  - "[[sources/wshobson Agents Marketplace]]"
  - "[[sources/Compound Engineering Plugin]]"
  - "[[questions/Research ECC Plugin Selective-Install Patterns]]"
---

# Harness Adapter Pattern

## Definition

A code organization pattern where **one source-of-truth plugin or skill** is translated into **multiple harness-native artifact shapes** by per-target adapters. Each adapter understands the destination harness's directory layout, file format, capability constraints, and idioms — and emits artifacts that look native, not lowest-common-denominator translations.

## Why It Exists

The AI-agent harness ecosystem fragmented quickly through 2025-2026:

- **Anthropic Claude Code** — `.claude-plugin/plugin.json`, `skills/<name>/SKILL.md`, `hooks/hooks.json`
- **OpenAI Codex CLI** — `AGENTS.md` + `.codex/skills/` (8 KB skill cap)
- **Cursor** — `.cursor-plugin/`, `.cursor/rules/`
- **OpenCode** — `.opencode/{agents,commands,skills}/` (permission blocks from `tools:` allowlists)
- **Gemini CLI** — TOML-format commands + native subagents (April 2026 spec)
- **GitHub Copilot** — `.copilot/{agents,skills,commands}/` (model maps to GPT-5 family)
- **Zed**, **Qwen Code**, **Kiro CLI**, **Windsurf**, **Pi**, **Factory Droid**, **OpenClaw**, **OpenHarness** — each with their own variant

Without an adapter pattern, plugin authors have to maintain N forks of the same content. With it, one source ships to N targets.

## Pattern Variants

### Build-time generation (wshobson/agents)

```bash
make generate HARNESS=gemini
make generate-all
make validate    # structural checks
make garden      # drift / dead-link / cap detection
```

Generated artifacts are gitignored (Gemini, OpenCode) or committed (Cursor, Codex) depending on the adapter's storage strategy. Build runs in CI or manually.

### Install-time selective install (ECC)

```bash
ecc install --profile developer --target cursor
ecc install --profile minimal --target codex
```

Two-stage: `install-plan.js` produces a per-target deployment plan; `install-apply.js` writes harness-native artifacts directly into the target directories. State tracked per target in `install-state.json` for incremental updates and clean uninstall.

### Install-time per-target conversion (Compound Engineering Plugin)

```bash
bunx @every-env/compound-plugin install <plugin> --to <harness>
```

Runtime conversion at install. Bidirectional: also syncs other-harness personal config back to Claude Code.

### Single-command repo bootstrap (HarnessForge)

```bash
uvx harnessforge init
```

Deterministic local generation — no LLM, no network — produces AGENTS.md, SOUL.md, TOOLS.md, MEMORY.md, SKILLS/, per-IDE adapter files, blueprint validators, MCP recommendations, forbidden-path rules. Whole-repo scope, not per-component.

## Per-Target Constraints to Absorb

A working adapter handles harness-specific quirks invisibly to the source author:

- **Codex 8 KB skill cap** — adapter splits oversize skills or warns at validate time.
- **Commands → skills mapping** — Codex doesn't have commands as a separate concept; adapter folds command files into the skill namespace.
- **OpenCode permission blocks** — `permission:` derived from skill's `tools:` allowlist field.
- **Gemini TOML format** — markdown-frontmatter skill → TOML-format command file.
- **Cursor rules-vs-skills** — `.cursor/rules/` for always-on rules; `.cursor/skills/` for invoked workflows; adapter chooses based on `disable-model-invocation` in source.
- **Hook event-name differences** — Claude Code uses `PreToolUse`/`PostToolUse`; Cursor uses different event names; adapter remaps.

## Compliance Surface

ECC ships `docs/architecture/harness-adapter-compliance.md` + `scripts/harness-adapter-compliance.js` — a matrix tracking which harnesses cover which capabilities (skills, agents, hooks, MCPs, LSPs, monitors, marketplaces). wshobson ships `docs/harnesses.md` for the same purpose. The matrix is the canonical artifact that says "this adapter is at parity for X capability."

## Differs From

- **Selective Install Pattern** ([[concepts/Selective Install Pattern]]) is about **which components install**; Harness Adapter Pattern is about **how those components are translated per target**. The two are orthogonal — ECC layers both; wshobson uses adapter pattern without component selection; HarnessForge uses neither (whole-repo bootstrap).
- **Universal-translator marketing claim** vs the technical reality — most adapters explicitly avoid lowest-common-denominator. Each target gets its native idioms; the "translation" is semantic, not syntactic word-for-word mapping.

## Open Questions

- No surveyed source benchmarks adapter coverage parity quantitatively (e.g., "Cursor adapter covers 85% of Claude Code surface, Codex adapter covers 60%").
- Adapter lifecycle: when a harness adds a new feature (e.g., Gemini April 2026 subagents), how quickly do adapters incorporate it? No public timeline data.
- Bidirectional sync (Compound Engineering's pattern) — is the round-trip lossless or are there shape mismatches that lose info?
