---
type: source
source_type: open-source-repo
title: "Compound Engineering Plugin — Universal Adapter"
author: "EveryInc / Kieran Klaassen"
date_published: 2026-05-31
date_accessed: 2026-06-10
url: "https://github.com/EveryInc/compound-engineering-plugin"
confidence: medium-high
status: developing
tags:
  - source
  - claude-code
  - cross-harness
  - universal-adapter
  - bidirectional-sync
related:
  - "[[concepts/Harness Adapter Pattern]]"
  - "[[concepts/Cross-Harness Compatibility]]"
  - "[[sources/wshobson Agents Marketplace]]"
  - "[[sources/ECC Repository README]]"
  - "[[questions/Research ECC Plugin Selective-Install Patterns]]"
key_claims:
  - "Official Compound Engineering plugin marketplace by EveryInc"
  - "Universal translator: converts Claude Code plugins → native formats for 11+ harnesses"
  - "Bidirectional sync: outward conversion + inward sync of personal config"
  - "Per-target install at install-time, not build-time: bunx @every-env/compound-plugin install <plugin> --to <harness>"
  - "Supported targets: Cursor, OpenCode, Codex, Factory Droid, Pi, Gemini CLI, GitHub Copilot, Kiro CLI, Windsurf, OpenClaw, Qwen Code"
---

# Compound Engineering Plugin — Universal Adapter

## Summary

EveryInc's universal adapter for Claude Code plugins. Converts plugins **at install time** (not build time) into native formats for 11+ target harnesses. Bidirectional: also syncs personal config from non-Claude harnesses back to Claude Code. Bun/TypeScript installer; works around the absence of native cross-harness install in Anthropic's spec.

## What It Contributes

The **third architectural pattern** in the research — runtime conversion vs ECC's build-time + install-time selective install vs wshobson's build-time-only generation.

- **Install-time conversion**: `bunx @every-env/compound-plugin install <plugin> --to <harness>`. Worked examples:
  - `bunx @every-env/compound-plugin install compound-engineering --to opencode`
  - `bunx @every-env/compound-plugin install compound-engineering --to codex`
  - `bunx @every-env/compound-plugin install compound-engineering --to copilot`
- **Conversion mechanics** documented at high level: "deep semantic translation including Command Mapping, MCP Server Adaptation, and Namespace Intelligence." (Quote from the marketing page — not the same depth of mechanical detail as ECC's `install-plan.js`.)
- **Supported harnesses** (longest list of any cross-harness adapter surveyed):
  - Cursor
  - OpenCode
  - Codex
  - Factory Droid
  - Pi
  - Gemini CLI
  - GitHub Copilot
  - Kiro CLI
  - Windsurf
  - OpenClaw
  - Qwen Code
  - (Qwen Code installs Claude-Code-compatible plugins directly from GitHub and converts on install — no Bun step needed.)
- **Bidirectional sync**: outward conversion (Claude Code plugin → other harness's format) AND inward sync (personal config from another harness → Claude Code). Other surveyed adapters (ECC, wshobson, HarnessForge) are one-way (Claude Code as canonical, others as generated targets).
- **Marketing positioning**: "universal translator, enabling true write-once, run-anywhere capability for your AI extensions" — same goal as ECC + wshobson but with the broadest harness coverage.

## Notes

- Built and maintained by EveryInc (Kieran Klaassen) — also produces editorial content on the compound-engineering pattern (the practice of stacking small reusable AI workflows).
- Companion blog post characterizes it as "AI agent universal adapter" — comparing the role to a hardware adapter for AV equipment.
- The runtime-conversion approach has a tradeoff: every install is a fresh translation (latency cost, potential conversion bugs in CI) vs build-time approaches that precompute (faster install, divergence risk if generation isn't run).
- Less mechanical depth in available docs vs ECC or wshobson. Higher-level marketing emphasis; would need source-code inspection to verify the "deep semantic translation" claim.
- The 11-harness target list is the broadest surveyed. ECC supports 7-9 depending on adapter status. wshobson supports 5-6. HarnessForge supports a smaller named set.
