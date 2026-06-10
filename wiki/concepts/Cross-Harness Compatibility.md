---
type: concept
title: "Cross-Harness Compatibility"
created: 2026-06-10
updated: 2026-06-10
tags:
  - concept
  - cross-harness
  - compatibility
  - comparison
  - architecture
status: developing
related:
  - "[[concepts/Selective Install Pattern]]"
  - "[[concepts/Harness Adapter Pattern]]"
  - "[[sources/ECC Repository README]]"
  - "[[sources/wshobson Agents Marketplace]]"
  - "[[sources/Compound Engineering Plugin]]"
  - "[[questions/Research ECC Plugin Selective-Install Patterns]]"
---

# Cross-Harness Compatibility

## Definition

The property of a plugin / skill / agent ecosystem that lets the same logical content (a skill instruction, an agent system prompt, a hook command) run across multiple AI coding agent harnesses (Claude Code, Cursor, Codex CLI, Gemini CLI, OpenCode, Copilot, Zed, etc.) without re-authoring.

Achieved through some combination of:

1. A common base format (the Agent Skills open standard at agentskills.io, which Claude Code skills follow).
2. Per-harness [[concepts/Harness Adapter Pattern]] for the gaps.
3. Optional [[concepts/Selective Install Pattern]] for per-target right-sizing.

## Approaches Compared

| Project                                    | Source-of-truth                                           | Distribution                                                      | Selection grain                           | Harnesses                                                                                        |
| ------------------------------------------ | --------------------------------------------------------- | ----------------------------------------------------------------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **ECC** (`affaan-m/ECC`)                   | `skills/`, `agents/`, `commands/`, `hooks/`, `manifests/` | `npm i -g ecc-universal`; `ecc install`                           | Component-subset (`--with` / `--without`) | 7-9: claude, cursor, codex, opencode, gemini, zed, qwen, joycode, antigravity                    |
| **wshobson/agents**                        | `plugins/` (Markdown)                                     | `/plugin marketplace add wshobson/agents` + per-harness install   | Whole-plugin (84 plugins)                 | 5-6: claude, codex, cursor, opencode, gemini, copilot                                            |
| **Compound Engineering Plugin** (EveryInc) | `compound-engineering-plugin/` Claude-Code-native         | `bunx @every-env/compound-plugin install <plugin> --to <harness>` | Whole-plugin, install-time convert        | 11+: cursor, opencode, codex, factory-droid, pi, gemini, copilot, kiro, windsurf, openclaw, qwen |
| **HarnessForge**                           | `harnessforge` template                                   | `uvx harnessforge init`                                           | Whole-repo bootstrap                      | Claude, Cursor, Codex, Gemini CLI, Aider + others                                                |
| **Anthropic Claude Code official**         | `.claude-plugin/plugin.json` per plugin                   | `/plugin install` from marketplace                                | Whole-plugin                              | 1 (Claude Code) — base layer everyone else builds on                                             |

## What Each Sacrifices

- **ECC**: complexity. Adopters must understand profile vs component-family vs adapter layering, and the install-state lifecycle (`doctor`, `repair`, `uninstall`). Highest flexibility, steepest curve.
- **wshobson/agents**: per-plugin granularity. If you want some skills from `python-development` but not others, your only option is to install the whole plugin (then disable specific skills manually).
- **Compound Engineering Plugin**: build-time validation. Runtime conversion means conversion bugs surface at install, not at CI. More-supported harness list but less mechanical visibility.
- **HarnessForge**: per-component customization. The init generates a deterministic template; subsequent per-skill changes need re-running or manual edits.
- **Anthropic official**: cross-harness coverage entirely. Official spec is Claude-Code-only; third-party adapters bridge the gap.

## Why the Field Fragmented

- **Multiple vendor launches in 2025-2026** — Anthropic Claude Code, OpenAI Codex CLI, Cursor, Gemini CLI, Copilot CLI, OpenCode, Zed AI all shipped agent harnesses within ~18 months.
- **Different design philosophies** — some target IDE integration (Cursor, Zed, Copilot), some target terminal (Claude Code, Codex CLI, Gemini CLI), some target both (OpenCode).
- **Different runtime models** — some allow hook execution, some only allow instruction text; some have MCPs natively, some need adapters; some have skill size caps, some don't.
- **No coordinating body** until `agentskills.io` open standard emerged. Even with the standard, each harness extends it in incompatible ways (Claude Code's `disable-model-invocation`, Codex's 8 KB cap, etc.).

The cross-harness adapter pattern is the ecosystem's response: rather than waiting for one harness to win or one standard to emerge, plugin authors ship adapters that translate.

## When Cross-Harness Compatibility Matters

For an end user:

- **Single-harness developer** (only uses Claude Code, never plans to switch): cross-harness adapters are mostly noise. Use native `claude plugin install` or selective install with `--target claude` only.
- **Multi-harness team** (some on Cursor, some on Claude Code, some on Codex): adapters become essential. Without them, the same engineering knowledge gets re-encoded per harness.
- **Migrating off a harness** (e.g., Claude Code → Cursor): bidirectional adapters (Compound Engineering Plugin) ease the move.
- **Multi-machine, multi-environment operator**: per-target install lets you ship the right shape per environment without manual copy-paste.

## Open Questions

- **Will Anthropic absorb the pattern?** Official spec is whole-plugin Claude-Code-only. If Anthropic adds `--with`/`--without` natively, third-party selective-install layers become redundant. No public signal as of June 2026.
- **Will an open standard absorb the adapter layer?** `agentskills.io` is the closest. Whether it grows beyond skills to cover plugins + hooks + MCPs cross-harness is open.
- **Drift management at scale**: with 100+ plugins ships to 5-10 harnesses, drift between source and generated artifacts is a real failure mode. `make garden` (wshobson) and `ecc doctor` (ECC) are the surveyed solutions; their effectiveness in production isn't benchmarked publicly.
- **Bidirectional round-trip fidelity**: Compound Engineering Plugin syncs from non-Claude harnesses back to Claude Code. Is the round-trip lossless? Open.
