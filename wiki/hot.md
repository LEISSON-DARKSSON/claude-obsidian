---
type: meta
title: "Hot Cache"
created: 2026-04-07
updated: 2026-06-10
tags:
  - meta
  - hot-cache
  - agronutikas
  - multi-repo
  - ecc
  - phase-a
status: evergreen
related:
  - "[[index]]"
  - "[[log]]"
  - "[[overview]]"
  - "[[Architecture]]"
  - "[[Compliance]]"
  - "[[Business]]"
  - "[[meta/agronutikas-multirepo-phase-a-bc-shipping]]"
  - "[[questions/Research ECC Plugin Selective-Install Patterns]]"
  - "[[concepts/Selective Install Pattern]]"
  - "[[concepts/Harness Adapter Pattern]]"
  - "[[concepts/Cross-Harness Compatibility]]"
---

# Hot Cache — Latest Context

Last updated: 2026-06-10 | AgroNutikas multi-repo platform

Navigation: [[index]] | [[overview]] | [[log]] | [[Architecture]]

---

## 2026-06-10 — Autoresearch: ECC Plugin Selective-Install Patterns

12 new pages filed via `/autoresearch`. Master synthesis: [[Research ECC Plugin Selective-Install Patterns]]. Direct input for Phase B of [[meta/agronutikas-multirepo-phase-a-bc-shipping]] long-run plan.

**Headline findings**:

- Selective install (`--profile / --with / --without` over a manifest pipeline) is a **third-party layer on top of Anthropic's whole-plugin spec** — Claude Code's official `plugin.json` model treats plugins as atomic install units.
- [[entities/Affaan Mustafa]]'s ECC v1.9.0 (March 2026) **introduced the pattern**; component families use `lang:`, `agent:`, `skill:`, `framework:`, `capability:` prefixes.
- Four parallel architectures surveyed:
  1. **ECC** — component-subset selective install, two-stage `install-plan.js` + `install-apply.js`, per-target adapters, 7-9 harnesses
  2. **wshobson/agents** — 84 atomic per-plugin units, build-time `make generate-all`, 5-6 harnesses
  3. **Compound Engineering Plugin** — install-time conversion, bidirectional sync, 11+ harnesses (broadest coverage)
  4. **HarnessForge** — single-command `uvx harnessforge init` repo bootstrap, whole-repo not per-component
- Per-target adapter constraints absorbed invisibly: Codex 8 KB skill cap, commands→skills mapping, OpenCode permission blocks, Gemini TOML format.
- All approaches respect `agentskills.io` open standard at the skill-content level; differ only in distribution + selection grammar.

**Concepts**: [[concepts/Selective Install Pattern]] · [[concepts/Harness Adapter Pattern]] · [[concepts/Cross-Harness Compatibility]]

---

## 2026-06-10 — Multi-repo enhancement plan + Phase A B/C shipped

18-month, 3-horizon multi-repo enhancement plan approved (`C:\Users\gert\.claude\plans\a-composed-stallman.md`): Phase A (foundation consolidation + safety automation, months 0–3), Phase B (ECC selective adoption per repo, months 3–9), Phase C (operator control plane + cross-repo orchestration, months 6–18). Drafted via 3 parallel Explore + 3 parallel Plan agents.

**3 PRs shipped same day:**

- [PR #1 `AgroNutikas/performance`](https://github.com/AgroNutikas/performance/pull/1) `9533c520` — ECC Hermes docs clarify `ecc-tui` Rust crate ownership of `migrate *` subcommands + 2 Windows test fixes.
- [PR #3 `AgroNutikas/agronutikas-workspace`](https://github.com/AgroNutikas/agronutikas-workspace/pull/3) `ff0140d7` — TECH-DEBT P0.1 reality sync. Discovery: feared credential leak never had remote exposure (files always gitignored, never committed). Local copies quarantined to `_handoff/p0-1-quarantine-2026-06-10/`.
- [PR #4 `AgroNutikas/agronutikas-workspace`](https://github.com/AgroNutikas/agronutikas-workspace/pull/4) `a01c3573` — `scripts/branch-status.sh` workspace operator script (first of 4 Phase A.4 scripts).

**Phase A status**: A.2 done, A.4 first of 4 scripts done. Pending: A.1 anchors sprint (~3 weeks), A.3, remaining A.4 scripts, A.5–A.8.

**Session note**: [[meta/agronutikas-multirepo-phase-a-bc-shipping]] — full session arc, decisions, file list, next entry point.

---

## 2026-05-08 — Track 3 shipped

PRs #18 (agronutikas-web), #90 + #91 (platform). Full farmer onboarding flow live:

landing page (`agronutikas.ee/talunikule`) → signup → admin email notify → approval → welcome email → setup wizard → day-1 insight card → weekly digest cron Mondays 09:00 UTC.

- **New table:** `pilot_weekly_digest_log` (migration 037)
- **New service:** `TransactionalEmailService` (Resend HTTP)
- **New worker subsystem:** `WeeklyDigestScheduler`
- **Flow page:** [[farmer-onboarding-flow]]

Today merged: Phase B (`73fe5a2`) + Phase C (`6858d1a`). Branch context up to today: `feat/demo-audit-v2-bundle-b`.

---

## AgroNutikas Platform Overview

Single-farm Estonian agricultural compliance platform (pilot scope, Viraito OÜ on 942 ha, Põltsamaa).

### 3 Repos

| Repo                 | Workspace                   | Deploy                  |
| -------------------- | --------------------------- | ----------------------- |
| agronutikas-platform | API, Worker, Bot, Contracts | Railway, GitHub Actions |
| AgroNutikas-Demo-v1  | Demo (separate deploy)      | Vercel (auto-synced)    |
| agronutikas-web      | Marketing site              | Vercel (static)         |

### Stack

API: NestJS 11 + Fastify 5 + PostgreSQL 16 + PostGIS 3.4 (Railway). Demo: Vanilla JS + Vite + Leaflet + Chart.js (Vercel). Web: static HTML (Vercel). Worker: TypeScript + pg outbox (Railway). Bot: TypeScript + Anthropic SDK (Railway).

### Key Numbers (current)

- **20 pilot modules** (auth, farm, data, crop-history, activities, compliance, rules, fertility, org, automation, weather, xtee, subsidies, business-registry, import, alerts, history, feedback, setup, **analytics**)
- **36 migrations** (chain at 036, next: 037; 008 + 035 unused)
- **20+ views** (demo SPA), **40+ components**, **458+ Vitest tests**
- **36 compliance rules** (20 conventional + 16 organic EU 2018/848)
- **27 ADRs**, **5 external integrations** (PRIA, e-Äriregister, X-tee, Open-Meteo, Ilmateenistus)

---

## Domain Quick Reference

**7 restriction zones:** NTA (85 kg N/ha), veekaitsevöönd, kaitseala, hoiuala, piiranguvöönd, Natura2000, muu.

**Nitrogen limits:** 170 kg N/ha normal, 85 kg N/ha NTA.

**Auth:** PBKDF2-SHA256, HttpOnly `sessionid` cookie, `pilot_sessions` table, `PilotAuthGuard`.

**Layer pattern:** Controller → Service → Repository (parameterized SQL, `pilot_` prefix, GiST spatial indexes, SRID 4326).

---

## Repo Locations

- platform: `C:\Users\gert\Desktop\agronutikas-platform\`
- demo deploy: `C:\Users\gert\Desktop\AgroNutikas-Demo-v1\`
- web: `C:\Users\gert\Desktop\agronutikas-web\`
- wiki: `C:\GitHub\claude-obsidian\`

---

## Style Reminders

- Terse, no trailing summaries. Estonian domain terms.
- Wikilinks `[[Page Name]]`, frontmatter required (type, title, created, updated, tags, status).
- Status values: seed, developing, current, mature, deprecated, live.
- No `console.log` in production. Use structured logger / `lib/logger.js`.

---

Navigation: [[index]] | [[Architecture]] | [[Compliance]] | [[Business]] | [[domains/_index]]
