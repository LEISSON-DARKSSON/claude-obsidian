---
type: source
source_type: open-source-repo
title: "ECC (Everything Claude Code) Repository README"
author: "Affaan Mustafa"
date_published: 2026
date_accessed: 2026-06-10
url: "https://github.com/affaan-m/ECC"
confidence: high
status: developing
tags:
  - source
  - ecc
  - claude-code
  - cross-harness
  - selective-install
  - open-source
related:
  - "[[entities/Affaan Mustafa]]"
  - "[[sources/ECC v1.9.0 Release Notes]]"
  - "[[concepts/Selective Install Pattern]]"
  - "[[concepts/Harness Adapter Pattern]]"
  - "[[questions/Research ECC Plugin Selective-Install Patterns]]"
key_claims:
  - "ECC ships 261 skills, 64 agents, 84 commands, automated hook workflows"
  - "Cross-harness: Codex, Claude Code, Cursor, OpenCode, Gemini, Zed, GitHub Copilot, others"
  - "v2.0.0 ships control-pane substrate (session adapters + MCP inventory), worktree-lifecycle service, orch-* orchestrator family"
  - "v1.9.0 introduced selective install with --with/--without flags + manifest-driven pipeline"
  - "Hook runtime controls: ECC_HOOK_PROFILE=minimal|standard|strict + ECC_DISABLED_HOOKS=..."
  - "211.9K+ GitHub stars, 32.5K+ forks, 230+ contributors as of June 2026"
---

# ECC (Everything Claude Code) Repository README

## Summary

The flagship cross-harness AI-agent operating system. Affaan Mustafa's `affaan-m/ECC` (formerly `affaan-m/everything-claude-code`) packages 261 skills + 64 agents + 84 commands + hooks + MCP configs + selective-install tooling that ships to 7+ harnesses (Claude Code, Codex, Cursor, OpenCode, Gemini, Zed, GitHub Copilot). The pattern: **one source-of-truth, per-target adapters at install time, manifest-driven selective install on top**.

## What It Contributes

This is the **primary source** for the research topic. ECC is where the selective-install pattern was introduced and refined:

- **Scale**: 261 skills, 64 agents, 84 commands, 12+ language ecosystems, 7+ harnesses, 230+ contributors, 211.9K+ stars (June 2026).
- **NPM package**: `ecc-universal` (currently `2.0.0-rc.1` per `npm i -g ecc-universal`). Single binary entry: `ecc`.
- **CLI grammar**:
  - `ecc install --profile <p> --target <t>` — coarse-grained
  - `--with <component>` / `--without <component>` — fine-grained
  - `--skills <id,id>` / `--pack <name>` — explicit lists
  - `--dry-run` — plan without applying
- **Profiles**: `minimal`, `core`, `developer` (default), `security`, `research`, `full`.
- **Targets**: `claude`, `claude-project`, `cursor`, `codex`, `gemini`, `opencode`, `zed`, `joycode`, `qwen`, `antigravity`.
- **Component families** (the `--with`/`--without` namespace):
  - `lang:<language>` (typescript, python, go, rust, java, kotlin, c, csharp, fsharp, perl, php, ruby)
  - `agent:<agent-id>` (typescript-reviewer, security-reviewer, database-reviewer, etc.)
  - `skill:<skill-id>` (tdd-workflow, security-review, postgres-patterns, etc.)
  - `framework:<framework>` (nextjs, django, laravel, springboot, etc.)
  - `capability:<capability>` (security, orchestration, machine-learning, etc.)
- **Hook runtime controls** — `ECC_HOOK_PROFILE=minimal|standard|strict` selects the active hook bundle; `ECC_DISABLED_HOOKS=<csv>` disables specific hooks. Avoids editing hook files.
- **Lifecycle commands**: `install`, `plan`, `catalog`, `consult`, `control-pane`, `list-installed`, `doctor`, `repair`, `auto-update`, `status`, `platform-audit`, `security-ioc-scan`, `sessions`, `work-items`, `session-inspect`, `loop-status`, `uninstall`.
- **State store** — SQLite at `~/.ecc/state.db` records sessions, skill runs, decisions, install state. `ECC_STATE_DB` env overrides path. Used by `status`, `sessions`, `control-pane`, cost-tracker hook.
- **Two binaries that both ship as `ecc`** — the Node `ecc-universal` CLI (`scripts/ecc.js`) handles install/manage; the separate Rust `ecc-tui` crate (`ecc2/`) adds `migrate` subcommands and the TUI control plane. Doc disambiguation matters (lesson from session [[meta/agronutikas-multirepo-phase-a-bc-shipping]]).

## Notes

- Repo identity history: originally `affaan-m/everything-claude-code`; renamed to `affaan-m/ECC` while keeping the long form alive for backward compat. The npm package stayed `ecc-universal`.
- Commercial layer: ECC Tools is a GitHub App (`github.com/marketplace/ecc-tools`) with free + Pro ($19/seat/mo) + Enterprise tiers. Funds the OSS work.
- MIT licensed. Maintainer ships weekly across 7 harnesses (single-developer cadence sustained by sponsors + Pro revenue).
- Companion repo: `affaan-m/everything-claude-code-tools` — the commercial App service code.
