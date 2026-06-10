---
type: synthesis
title: "Research: ECC Plugin Selective-Install Patterns"
created: 2026-06-10
updated: 2026-06-10
tags:
  - research
  - ecc
  - selective-install
  - claude-code
  - plugin
  - harness-adapter
status: developing
related:
  - "[[concepts/Selective Install Pattern]]"
  - "[[concepts/Harness Adapter Pattern]]"
  - "[[concepts/Cross-Harness Compatibility]]"
  - "[[entities/Affaan Mustafa]]"
  - "[[meta/agronutikas-multirepo-phase-a-bc-shipping]]"
sources:
  - "[[sources/Claude Code Plugins Official Docs]]"
  - "[[sources/Claude Code Skills Official Docs]]"
  - "[[sources/ECC Repository README]]"
  - "[[sources/ECC v1.9.0 Release Notes]]"
  - "[[sources/wshobson Agents Marketplace]]"
  - "[[sources/Compound Engineering Plugin]]"
---

# Research: ECC Plugin Selective-Install Patterns

## Overview

Selective install in AI-agent harnesses lets a single source-of-truth plugin package ship right-sized component subsets per consumer repo, per harness, and per use case — instead of an all-or-nothing install. The Everything Claude Code (ECC) project pioneered the pattern in March 2026 (v1.9.0) with a `--profile / --with / --without` CLI grammar on top of a manifest-driven install pipeline. The pattern has since been replicated by `wshobson/agents` (84 plugins ship to 5 harnesses from one Markdown source) and Compound Engineering Plugin (`bunx @every-env/compound-plugin install <plugin> --to <harness>`), while official Claude Code plugins (Anthropic's `claude plugin` CLI + `marketplace.json`) ship whole-plugin units only — no per-component subset.

The pattern matters because the Claude Code plugin ecosystem hit ~101 official + 425 community plugins, ~2,810 skills, and ~200 agents by March 2026 — installing everything wastes context tokens and creates noise. Selective install is the cost-control and signal-to-noise mechanism for large plugin catalogs.

## Key Findings

- **ECC ships a two-stage installer** — `scripts/install-plan.js` resolves dependencies and produces a deployment plan; `scripts/install-apply.js` executes filesystem operations through per-target adapters. State is recorded in `install-state.json` per target so subsequent runs can incrementally update, repair, or uninstall (Source: [[sources/ECC v1.9.0 Release Notes]], [[sources/ECC Repository README]]).
- **CLI grammar** — `ecc install --profile <name> --target <harness> --with <component> --without <component>` plus shortcuts `--skills <a,b,c>`, `--pack <name>`. Components use family prefixes: `lang:typescript`, `agent:security-reviewer`, `skill:tdd-workflow`, `framework:nextjs`, `capability:security`. Per the [[sources/ECC v1.9.0 Release Notes]], example: `ecc install --profile developer --with lang:typescript --with agent:security-reviewer --without skill:continuous-learning`.
- **Six built-in profiles** — `minimal`, `core`, `developer` (default), `security`, `research`, `full`. `minimal` ships only baseline rules + hooks-runtime; `full` ships everything. Profile selection is the coarse-grained switch; `--with/--without` is the fine-grained refinement. (Source: [[sources/ECC Repository README]], [[sources/ECC v1.9.0 Release Notes]]).
- **Cross-harness adapter targets** — `claude`, `claude-project`, `cursor`, `codex`, `gemini`, `opencode`, `zed`, `joycode`, `qwen`. Each adapter writes harness-native artifacts into the appropriate directory (`~/.claude/`, `.cursor/`, `.codex/`, etc.) rather than lowest-common-denominator translations (Source: [[sources/ECC Repository README]], [[sources/wshobson Agents Marketplace]]).
- **Claude Code official plugin spec is whole-plugin** — Anthropic's plugin model treats each plugin as an atomic unit with a `.claude-plugin/plugin.json` manifest (`name`, `description`, `version`, `author`) bundling `skills/`, `commands/`, `agents/`, `hooks/hooks.json`, `.mcp.json`, `.lsp.json`, `monitors/monitors.json`, `bin/`, `settings.json`. There is no built-in component-level subset install — selective install is a layer ECC adds on top (Source: [[sources/Claude Code Plugins Official Docs]]).
- **Per-harness constraints differ** — Codex has an 8 KB skill cap that requires the adapter to split or trim oversize skills. Cursor uses `.cursor-plugin/` + `.cursor/rules/`. Gemini CLI uses TOML for commands (April 2026 spec). Each adapter respects the destination harness's idioms rather than forcing one shape (Source: [[sources/wshobson Agents Marketplace]], [[sources/Compound Engineering Plugin]]).
- **State + lifecycle tooling** — ECC ships `doctor`, `repair`, `auto-update`, `uninstall`, `list-installed` commands that read `install-state.json` to maintain per-target installs. Rollback for any install: `ecc uninstall --target <t> --remove-skills --remove-hooks --keep-mcp` then `git clean -fdX .claude/ .codex/` (Source: [[sources/ECC Repository README]]).
- **Three alternative architectures** for cross-harness ship-once-run-everywhere:
  1. **ECC** — profile + component selective install, per-target adapters generated at install time. Most granular.
  2. **wshobson/agents** — per-plugin atomic units, `make generate-all` produces all harness artifacts at build time, committed registries point at source. Coarsest unit but committed.
  3. **Compound Engineering Plugin** — runtime per-target conversion via `bunx @every-env/compound-plugin install <plugin> --to <harness>`. Bidirectional sync (Claude Code ↔ other harnesses).
  4. **HarnessForge** — single-command repo bootstrap (`uvx harnessforge init`), deterministic local generation of AGENTS.md/SOUL.md/TOOLS.md/MEMORY.md/SKILLS/ + per-IDE adapter files. Whole-repo not per-component (Source: search Round 2 results).
- **The general "agent harness" pattern** — modern agent infrastructure treats the LLM as a swappable adapter, the harness as the "hands/eyes/memory/safety". A `HarnessProfile` declaratively configures runtime behavior per provider/model without per-agent setup code (Source: [[sources/wshobson Agents Marketplace]], search Round 1 LangChain blog).

## Key Entities

- **[[entities/Affaan Mustafa]]** — ECC author/maintainer, runs `affaan-m/ECC` + `affaan-m/everything-claude-code` + ECC Tools commercial GitHub App. Hands-on weekly across 7+ harnesses; the design owner of the `--profile/--with/--without` grammar.
- **Anthropic** — owns the foundational Claude Code plugin + skill specs that all third-party selective-install systems layer on top of. Manages `claude-plugins-official` curated marketplace and `claude-plugins-community` open marketplace.
- **EveryInc / Kieran Klaassen** — ships Compound Engineering Plugin, the bidirectional Claude Code ↔ N-harness converter, with the most-supported harness list (Claude Code, Cursor, OpenCode, Codex, Factory Droid, Pi, Gemini CLI, GitHub Copilot, Kiro CLI, Windsurf, OpenClaw, Qwen Code).
- **wshobson** — author of `wshobson/agents` 84-plugin multi-harness marketplace, the most direct architectural comparable to ECC's selective-install approach.

## Key Concepts

- **[[concepts/Selective Install Pattern]]** — manifest-driven component subset selection layered over a base plugin format.
- **[[concepts/Harness Adapter Pattern]]** — per-target translators that emit harness-native artifacts from one source.
- **[[concepts/Cross-Harness Compatibility]]** — comparison of approaches (build-time generation, runtime conversion, single-shot init, profile-based selective install).
- **HarnessProfile** — declarative configuration bundle that tunes runtime behavior per provider/model selection (LangChain primitive, conceptually parallel to ECC profiles).

## Contradictions

- **Whole-plugin vs component-subset install** — [[sources/Claude Code Plugins Official Docs]] treats a plugin as the atomic install unit (you install a plugin, get all its components). [[sources/ECC v1.9.0 Release Notes]] introduces sub-plugin component selection via `--with/--without`. Both can be true at once — ECC's selective install is an additional layer on top of Anthropic's plugin spec, not a replacement for it. Future Claude Code releases may absorb the pattern.
- **"Universal adapter" wording** — Compound Engineering Plugin claims "write-once, run-anywhere"; wshobson/agents says "each adapter emits harness-native artifacts (not lowest-common-denominator translations)". Both can be true if the conversion preserves source semantics while emitting different on-disk shapes. The difference is generation-time (wshobson, `make generate-all`) vs install-time (Compound Engineering, `bunx ... install --to`).

## Open Questions

- **Concrete cost saving from selective install** — no source quantifies the actual context-token savings (e.g., "developer profile + 3 skills costs X tokens vs full install costs Y tokens"). ECC v1.9.0 release notes describe the mechanism but not the measured impact.
- **Anthropic's roadmap on sub-plugin selection** — official docs (`code.claude.com/docs/en/plugins-reference`) describe whole-plugin install. Is component-subset selection a planned addition or out-of-scope by design? No public signal found.
- **Drift management between source plugin and per-harness generated artifacts** — wshobson uses `make garden` for drift/dead-link/cap detection; ECC uses `doctor` + `repair`. No published comparison of which approach catches more drift in practice.
- **Per-harness capability matrix freshness** — `wshobson/agents` ships `docs/harnesses.md` capability matrix; ECC ships `docs/architecture/harness-adapter-compliance.md`. Both are maintained per-release. No source documents the lifecycle (who notices when a harness adds a feature, how the matrix is refreshed).
- **Component-family naming conventions** — ECC uses `lang:`, `agent:`, `skill:`, `framework:`, `capability:` prefixes. No other system surveyed adopts identical naming. Whether this becomes a de facto standard or stays ECC-specific is unclear.

## Sources

- [[sources/Claude Code Plugins Official Docs]] — Anthropic, accessed 2026-06-10
- [[sources/Claude Code Skills Official Docs]] — Anthropic, accessed 2026-06-10
- [[sources/Claude Code Plugin Reference]] — Anthropic, accessed 2026-06-10
- [[sources/ECC Repository README]] — Affaan Mustafa, accessed 2026-06-10
- [[sources/ECC v1.9.0 Release Notes]] — Affaan Mustafa, 2026-03-21
- [[sources/wshobson Agents Marketplace]] — wshobson, accessed 2026-06-10
- [[sources/Compound Engineering Plugin]] — EveryInc / Kieran Klaassen, 2026-05-31

## Research Metadata

- Rounds: 2 (Round 3 skipped — no major contradictions)
- Searches: 11 (6 Round 1 + 5 Round 2)
- Pages created: 10 (1 synthesis + 6 sources + 3 concepts + 1 entity)
- Confidence: medium-high on factual claims (multiple authoritative sources agree); medium on quantitative claims (cost savings, drift detection rates — no primary measurements found).
