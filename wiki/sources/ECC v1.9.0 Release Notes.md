---
type: source
source_type: release-notes
title: "ECC v1.9.0 Release Notes — Selective Install & Language Expansion"
author: "Affaan Mustafa"
date_published: 2026-03-21
date_accessed: 2026-06-10
url: "https://github.com/affaan-m/everything-claude-code/releases/tag/v1.9.0"
confidence: high
status: evergreen
tags:
  - source
  - ecc
  - release-notes
  - selective-install
  - history
related:
  - "[[entities/Affaan Mustafa]]"
  - "[[sources/ECC Repository README]]"
  - "[[concepts/Selective Install Pattern]]"
  - "[[questions/Research ECC Plugin Selective-Install Patterns]]"
key_claims:
  - "v1.9.0 introduced the selective install architecture (March 2026)"
  - "Manifest-driven install pipeline: install-plan.js + install-apply.js"
  - "State store tracks what's installed → incremental updates possible"
  - "--with and --without flags added for fine-grained component selection"
  - "12 language ecosystems shipped: C#, Rust, Java, Kotlin, C++, Go, Python, TypeScript, Perl, PyTorch, Nuxt 4, Flutter"
  - "Released alongside AgentShield v1.4.0 + ECC Tools Pro (Stripe billing $19/seat)"
---

# ECC v1.9.0 Release Notes — Selective Install & Language Expansion

## Summary

The release that introduced selective install to the ECC ecosystem (March 21, 2026). Two-stage installer architecture (`install-plan.js` plans deployment; `install-apply.js` executes filesystem ops), `--with`/`--without` CLI flags for component-level subset selection, state store for incremental updates. Shipped alongside 6 new agents, 15 new skills, 6 new language rule packs, the ECC Tools Pro commercial tier, and AgentShield v1.4.0 (CVE database + supply-chain verification).

## What It Contributes

The **canonical reference for when, why, and how selective install entered the ecosystem**. Direct quotes from the release notes:

- **"Selective install is here. Install only what you need with `--with` and `--without` flags."**
- **Worked example from release notes**: `ecc install --profile developer --with lang:typescript --with agent:security-reviewer --without skill:continuous-learning`
- **Architecture**: "Manifest-driven install pipeline with `install-plan.js` and `install-apply.js` for targeted component installation. State store tracks what's installed and enables incremental updates."
- **Component family expansion**: agent + skill families added to the install manifest as first-class component types (alongside lang + framework + capability).

Other notable v1.9.0 additions documented in the release notes:

- **6 new agents**: `typescript-reviewer`, `pytorch-build-resolver`, `java-build-resolver`, `java-reviewer`, `kotlin-reviewer`, `kotlin-build-resolver`
- **15 new skills**: `pytorch-patterns`, `nuxt4-patterns`, `codebase-onboarding`, `architecture-decision-records`, `agent-eval`, `documentation-lookup`, `bun-runtime`, `nextjs-turbopack`, plus 7 more
- **6 language rule packs**: C#, Rust, Java, C++, Perl, Flutter/Dart
- **Infrastructure**: agent description compression with lazy loading, skill inspection for recurring failure pattern detection, governance event capture hooks, MCP health-check hook with auto-reconnect, SQLite state store for session/skill/decision tracking, session adapters for canonical snapshots, Codex sync merges AGENTS.md instead of replacing.
- **Stats at release**: 219 commits since v1.8.0, 30+ contributors, 88K+ GitHub stars, 1,540+ tests passing across all platforms.

## Notes

- v1.9.0 was the inflection point — before it, ECC was an all-or-nothing install. After it, the same source ships subset installs per target.
- The release predates Claude Code's plugin marketplace stabilization. ECC's selective install evolved independently of (and arguably ahead of) Anthropic's official plugin grammar.
- Versioning context: v1.10.0 followed (140K stars, 156 skills), then v2.0.0-rc.1 (April 2026) added the dashboard GUI + control-pane substrate, then v2.0.0 stable (June 2026) added the orch-\* orchestrator family.
- The release pairs technical capability (selective install) with commercial monetization (ECC Tools Pro at $19/seat/mo). This is what funded the cross-harness ship cadence going forward.
