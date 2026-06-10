---
type: concept
title: "Selective Install Pattern"
created: 2026-06-10
updated: 2026-06-10
tags:
  - concept
  - install
  - plugin
  - manifest
  - architecture
status: developing
related:
  - "[[concepts/Harness Adapter Pattern]]"
  - "[[concepts/Cross-Harness Compatibility]]"
  - "[[sources/ECC v1.9.0 Release Notes]]"
  - "[[sources/Claude Code Plugins Official Docs]]"
  - "[[questions/Research ECC Plugin Selective-Install Patterns]]"
---

# Selective Install Pattern

## Definition

A plugin-distribution pattern where the consumer picks **which components inside a plugin** (or across multiple plugins) get installed for a given target, rather than installing the whole plugin atomically. Implemented as a manifest-driven CLI on top of a base plugin spec.

The pattern decomposes into three primitives:

1. **Profile** — coarse-grained named subset (e.g., `minimal`, `core`, `developer`, `security`, `research`, `full`).
2. **Component family** — taxonomy with prefixed IDs (e.g., `lang:typescript`, `agent:security-reviewer`, `skill:tdd-workflow`, `framework:nextjs`, `capability:security`).
3. **Inclusion / exclusion flags** — `--with <component>` adds; `--without <component>` removes.

## Why It Exists

Whole-plugin install becomes wasteful when:

- The plugin ships hundreds of components (ECC has 261 skills, 64 agents, 84 commands at v2.0.0).
- The consumer only needs a small per-repo subset (e.g., `agronutikas-business-materials` doesn't need `postgres-patterns`).
- Context-window cost is a constraint (every loaded skill consumes tokens during description compression at session start, even if the body lazy-loads).
- One source-of-truth must ship to multiple harnesses with different capabilities (Codex has 8 KB skill cap; some harnesses lack hook runtimes).

## Reference Implementation

ECC (`affaan-m/ECC`, v1.9.0+):

```bash
ecc install \
  --profile developer \
  --target claude \
  --with lang:typescript \
  --with agent:security-reviewer \
  --without skill:continuous-learning
```

Two-stage execution:

1. **`install-plan.js`** — resolves dependencies (e.g., `--with agent:security-reviewer` pulls in `agents-core` module), filters by target compatibility, produces a deployment plan.
2. **`install-apply.js`** — executes filesystem operations through per-target adapter (`.claude/`, `.cursor/`, `.codex/`, etc.), writes `install-state.json` for incremental updates / repair / uninstall.

See [[sources/ECC v1.9.0 Release Notes]] for the introduction and [[sources/ECC Repository README]] for current state.

## Where It Differs from Whole-Plugin Install

Anthropic's official Claude Code plugin spec ([[sources/Claude Code Plugins Official Docs]]) treats the plugin as the atomic install unit — `claude plugin install <name>` brings in **all** of the plugin's skills, agents, hooks, MCPs. There is no built-in `--with skill:X --without agent:Y` grammar at the plugin install command.

Selective install is therefore a **layer added on top** of the base spec, not a replacement. ECC implements it; future Claude Code releases may or may not absorb the pattern.

## Cousins / Variations

- **wshobson/agents** uses **per-plugin atomic units** — install grain is the whole plugin (84 plugins), but each plugin is small and single-purpose (e.g., `python-development` ships only Python content). Achieves similar right-sizing without component-subset flags.
- **Compound Engineering Plugin** uses **install-time per-target conversion** — `bunx @every-env/compound-plugin install <plugin> --to <harness>` — without sub-plugin selection.
- **HarnessForge** uses **single-command repo bootstrap** — `uvx harnessforge init` generates everything for all harnesses at once; no subset selection.

## Why It Matters

Context tokens are the binding cost of plugin installation. A `full` install of a 261-skill plugin can consume ~10-15k tokens per session in description compression alone. The same source under `--profile core --without capability:orchestration --without lang:perl` may be ~3-5k tokens. For a single-developer using multiple plugins across multiple repos, this difference compounds.

The pattern also enables **per-repo right-sizing** in multi-repo workspaces — backend repo gets `developer` + `postgres-patterns` + `api-design`; marketing repo gets `core` + `brand-voice` + `content-engine`; designer-only repo gets `minimal` + design-tokens-only skills.

## Open Questions

- No surveyed source quantifies actual token savings empirically. Plausible mechanism; magnitude unmeasured.
- Whether component-family prefix conventions (`lang:`, `agent:`, etc.) become a de facto standard or stay ECC-specific.
- How the pattern interacts with the official `claude plugin install` flow — does ECC's layer require workarounds when shipping into the official Anthropic marketplace?
