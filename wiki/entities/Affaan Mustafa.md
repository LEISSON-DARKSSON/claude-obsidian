---
type: entity
entity_type: person
title: "Affaan Mustafa"
created: 2026-06-10
updated: 2026-06-10
tags:
  - entity
  - person
  - ecc
  - open-source
  - claude-code
status: developing
related:
  - "[[sources/ECC Repository README]]"
  - "[[sources/ECC v1.9.0 Release Notes]]"
  - "[[concepts/Selective Install Pattern]]"
  - "[[questions/Research ECC Plugin Selective-Install Patterns]]"
---

# Affaan Mustafa

## Role

Author and maintainer of **Everything Claude Code (ECC)** — `affaan-m/ECC` (formerly `affaan-m/everything-claude-code`), the cross-harness agent operating system that introduced the [[concepts/Selective Install Pattern]] to the Claude Code plugin ecosystem in March 2026 (v1.9.0).

Also runs **ECC Tools** — the commercial GitHub App layer (`github.com/marketplace/ecc-tools`) with free + Pro ($19/seat/mo) + Enterprise tiers. The commercial business funds the open-source cadence.

## What They've Built

- **`affaan-m/ECC`** — 261 skills, 64 agents, 84 commands, hooks, MCP configs, selective-install tooling. 211.9K+ stars, 32.5K+ forks, 230+ contributors (June 2026).
- **`ecc-universal`** npm package — the install + lifecycle CLI.
- **`ecc-agentshield`** — security scanning layer with CVE database for MCPs and supply-chain verification.
- **ECC Tools GitHub App** — hosted PR review + security scanning service.
- **AgentShield** — standalone security tool, 1,609 tests at v1.4.0.

## Distinctive Contributions

- **`--with` / `--without` CLI grammar** for component-subset selection at install (v1.9.0, March 2026). The introduction of this grammar to the Claude Code ecosystem is the inflection point that defines the [[concepts/Selective Install Pattern]] research.
- **Two-stage installer architecture** — `install-plan.js` (plan) + `install-apply.js` (apply) + `install-state.json` (track). Mirrors classical package-manager design adapted for AI-agent plugin ecosystems.
- **Component family taxonomy** — `lang:`, `agent:`, `skill:`, `framework:`, `capability:` prefixed component IDs as the install grammar.
- **Hook runtime gating** — `ECC_HOOK_PROFILE=minimal|standard|strict` + `ECC_DISABLED_HOOKS=<csv>` for runtime control without editing hook files (v1.8.0).
- **Cross-harness adapter system** — per-target install adapters for 7-9 harnesses (`claude`, `cursor`, `codex`, `gemini`, `opencode`, `zed`, `joycode`, `qwen`, `antigravity`) from one source.
- **Operator control plane** (v2.0.0) — session adapters + MCP inventory + worktree-lifecycle service + `orch-*` orchestrator family.

## Operating Pattern

Single-maintainer cadence sustained by sponsors + Pro revenue. Ships **weekly across 7 harnesses** per the repo README. Public-only OSS code; private commercial infrastructure for the GitHub App + billing portal. MIT licensed.

## Online Presence

- GitHub: `affaan-m`
- X / Twitter: `@affaan` (used to share Shorthand + Longform + Security guide releases)
- Website: `ecc.tools` (commercial App + Pro pricing)
- ECC Discord community: `discord.gg/36yGMHGFbR` (launched alongside v2.0.0, June 2026)

## Why They Matter to This Vault

ECC is the upstream that the AgroNutikas workspace's `performance/` repo originates from. Phase B of the AgroNutikas multi-repo enhancement plan ([[meta/agronutikas-multirepo-phase-a-bc-shipping]] + the long-run plan at `~/.claude/plans/a-composed-stallman.md`) explicitly adopts ECC's selective-install model per-repo. Affaan Mustafa's design decisions therefore shape AgroNutikas operator tooling going forward.
