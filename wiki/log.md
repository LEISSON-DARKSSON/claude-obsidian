---
type: meta
title: "Operation Log"
created: 2026-04-08
updated: 2026-04-08
tags:
  - meta
  - log
status: evergreen
related:
  - "[[index]]"
  - "[[hot]]"
  - "[[overview]]"
  - "[[sources/_index]]"
---

# Operation Log

Navigation: [[index]] | [[hot]] | [[overview]]

Append-only. New entries go at the TOP. Never edit past entries.

Entry format: `## [YYYY-MM-DD] operation | Title`

Parse recent entries: `grep "^## \[" wiki/log.md | head -10`

---

## [2026-05-08] ship | Design System V6.0 brand-pack rollout

- Type: shipped
- PRs: agronutikas-platform#94 (`663ca1b`), agronutikas-web#19 (`df911c71`)
- Scope: trimmed mid-execution — V6 zip's `tokens.css` is v2.0 monolith but production main is v3.1 adapter, so token diff dropped to avoid regression. Phase 2 (modular CSS rewrite) also dropped — V6 ships monolith but production is modular.
- Shipped: V6 brand-pack assets (5 imagery + 65 logos) at `packages/design-system/assets/`, 6 hero artifact HTMLs (pitch-deck, strategic-plan, brand-cartography, brand-logos, plus -print variants) at `agronutikas.ee/reports/`
- Spec: `docs/superpowers/specs/2026-05-08-ds-v6-rollout-design.md`. Plan: `docs/superpowers/plans/2026-05-08-ds-v6-rollout-plan.md` (10 active tasks, 6 dropped post-trim)
- Superseded: DS V4 spec, DS v0.3.0 spec (v0.3.0 work shipped earlier via #67/#73/#84)

## [2026-05-08] ship | Track 3 pilot customer onboarding automation

- Type: shipped
- PRs: agronutikas-web#18, agronutikas-platform#90 (Phase B), agronutikas-platform#91 (Phase C)
- Loop: signup → admin notify → approval → welcome → setup → day-1 insight → weekly digest
- Spec: `docs/superpowers/specs/2026-05-08-pilot-customer-onboarding-automation-design.md`

## [2026-05-08] ship | Track 2 developer onboarding automation

- Type: shipped
- PRs: agronutikas-platform#85 (spec+plan), #86 (Phase A bootstrap), #87 (Phase B discoverability), #88 (Phase C glossary+tutorial)
- Day 1: `npm run onboard` + `docs/ONBOARDING.md`
- Discoverability: `docs/HUMAN_INDEX.md` (CI drift gate)
- Week 1-2: `docs/GLOSSARY.md`, `docs/FIRST_PR_TUTORIAL.md`

## [2026-05-08] ship | Claude tooling + agent hardening

- Type: shipped
- PR: agronutikas-platform#93 (`49eef76`)
- Scope: adopted Claude CLI docs, hardened subagent `tools:` arrays on 8 subagents, restored `name:` field on 5 subagents, closed 2 specs
- Follow-up: PR #96 (`8edea8c`) fixed post-deploy-smoke `warnOnly` to cover JSON.parse failures + npm audit fix for fast-uri/fast-xml-builder HIGH advisories

---

## [2026-04-23] ingest | Post-Business-Plan Documentation Program

- Type: strategic ingest
- Source: `C:\Users\gert\Desktop\agronutikas-platform\Post-Business-Plan Documentation Program for a Startup Project.md`
- Raw save: `.raw/post-business-plan-documentation-program.md`
- Pages created: [[Post-Business-Plan Documentation Program]] (source summary), [[Documentation Operating System]], [[Architecture Drift]], [[Four-Wave Documentation]], [[Documentation Program]]
- Key finding: Architecture drift is AgroNutikas' #1 blocking risk — E-PID vision conflicts with NestJS monorepo reality
- Deliverable: 35-document inventory (P0-P3), four-wave delivery model (12-16 weeks small team), budget scenarios (€8k-€250k+), legal/compliance baseline (GDPR, AI Act, SSDF)
- Updated: wiki/domains/Business.md, wiki/intel/\_index.md, wiki/index.md, wiki/hot.md

## [2026-04-08] save | claude-obsidian v1.4 Release Session

- Type: session
- Location: wiki/meta/claude-obsidian-v1.4-release-session.md
- From: full release cycle covering v1.1 (URL/vision/delta tracking, 3 new skills), v1.4.0 (audit response, multi-agent compat, Bases dashboard, em dash scrub, security history rewrite), and v1.4.1 (plugin install command hotfix)
- Key lessons: plugin install is 2-step (marketplace add then install), allowed-tools is not valid frontmatter, Bases uses filters/views/formulas not Dataview syntax, hook context does not survive compaction, git filter-repo needs 2 passes for full scrub

## [2026-04-08] ingest | Claude + Obsidian Ecosystem Research

- Type: research ingest
- Source: `.raw/claude-obsidian-ecosystem-research.md`
- Queries: 6 parallel web searches + 12 repo deep-reads
- Pages created: [[claude-obsidian-ecosystem]], [[cherry-picks]], [[claude-obsidian-ecosystem-research]], [[Ar9av-obsidian-wiki]], [[Nexus-claudesidian-mcp]], [[ballred-obsidian-claude-pkm]], [[rvk7895-llm-knowledge-bases]], [[kepano-obsidian-skills]], [[Claudian-YishenTu]]
- Key finding: 16+ active Claude+Obsidian projects; 13 cherry-pick features identified for v1.3.0+
- Top gap confirmed: no delta tracking, no URL ingestion, no auto-commit

## [2026-04-07] session | Full Audit, System Setup & Plugin Installation

- Type: session
- Location: wiki/meta/full-audit-and-system-setup-session.md
- From: 12-area repo audit, 3 fixes, plugin installed to local system, folder renamed

## [2026-04-07] session | claude-obsidian v1.2.0 Release Session

- Type: session
- Location: wiki/meta/claude-obsidian-v1.2.0-release-session.md
- From: full build session — v1.2.0 plan execution, cosmic-brain→claude-obsidian rename, legal/security audit, branded GIFs, PDF install guide, dual GitHub repos

- Source: `.raw/` (first ingest)
- Pages updated: [[index]], [[log]], [[hot]], [[overview]]
- Key insight: The wiki pattern turns ephemeral AI chat into compounding knowledge — one user dropped token usage by 95%.

## [2026-04-07] setup | Vault initialized

- Plugin: claude-obsidian v1.1.0
- Structure: seed files + first ingest complete
- Skills: wiki, wiki-ingest, wiki-query, wiki-lint, save, autoresearch
