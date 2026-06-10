---
type: meta
title: "Wiki Index"
created: 2026-04-07
updated: 2026-06-10
tags:
  - meta
  - index
status: evergreen
related:
  - "[[overview]]"
  - "[[log]]"
  - "[[hot]]"
  - "[[Wiki Map]]"
  - "[[concepts/_index]]"
  - "[[entities/_index]]"
  - "[[sources/_index]]"
---

# Wiki Index

Last updated: 2026-06-10 | Total pages: 57 | AgroNutikas multi-repo platform

Navigation: [[overview]] | [[log]] | [[hot]] | [[Wiki Map]] | [[getting-started]]

---

## MODE A: Concepts & Entities

### Foundational Concepts

- [[LLM Wiki Pattern]] — Persistent knowledge bases using LLMs (status: mature)
- [[Hot Cache]] — ~500-word session context file (status: mature)
- [[Compounding Knowledge]] — Why wiki knowledge grows more valuable over time (status: mature)
- [[cherry-picks]] — Feature backlog from ecosystem research; 13 features to add (status: current)

### Entities (Resources & People)

- [[Andrej Karpathy]] — Originator of LLM Wiki pattern
- [[LEISSON]] — AgroNutikas founder, owner
- [[Affaan Mustafa]] — Author/maintainer of ECC (Everything Claude Code) — selective install + harness adapter pioneer (NEW 2026-06-10)
- [[Ar9av-obsidian-wiki]], [[Nexus-claudesidian-mcp]], [[ballred-obsidian-claude-pkm]] — Multi-agent wiki plugins
- [[kepano-obsidian-skills]], [[Claudian-YishenTu]] — Obsidian ecosystem tools
- [[rvk7895-llm-knowledge-bases]] — Reference impl: persistent LLM wiki pattern

---

## MODE B: Codebase Architecture

### System Design & Modules

- **[[Architecture]]** — AgroNutikas system overview, deployment, layers, integrations (HUB PAGE)
- **[[modules/_index]]** — 20 backend NestJS pilot modules (auth, farm, data, compliance, rules, fertility, analytics, etc.)
- **[[components/_index]]** — 20+ views + 40+ components (Demo SPA, marketing Web)
- **[[decisions/_index]]** — 27 Architecture Decision Records (ADRs)
- **[[dependencies/_index]]** — External services, APIs, libraries (Railway, Vercel, PostgreSQL, PRIA, X-tee, weather, etc.)
- **[[flows/_index]]** — 15 data flows (auth, parcel loading, compliance checking, subsidy estimation, etc.)

---

## MODE C: Business & Market

### Strategy & Stakeholders

- **[[Business]]** — Market opportunity, competitive positioning, go-to-market roadmap, business model (HUB PAGE)
- **[[stakeholders/_index]]** — Founder, farmers, government, data sources, cloud providers
- **[[deliverables/_index]]** — Shipped features, module status matrix, feature completeness
- **[[intel/_index]]** — Market TAM (3500+ farms), competitive landscape, risks

### Regulatory Domain

- **[[Compliance]]** — Estonian agricultural regulations, restriction zones (NTA, water, reserves), nitrogen limits, subsidy programs, 36 rules (HUB PAGE)

---

## MODE D: Knowledge Synthesis

### Concepts (Foundational Ideas)

- [[LLM Wiki Pattern]] — Core architecture
- [[Hot Cache]] — Session context mechanism
- [[Compounding Knowledge]] — Knowledge growth dynamics
- [[Documentation Operating System]] — System with ownership, automation, review cycles (NEW 2026-04-23)
- [[Architecture Drift]] — Critical risk: conflicting system narratives (NEW 2026-04-23)
- [[Four-Wave Documentation]] — Delivery model: narrative → product → legal → scale (NEW 2026-04-23)
- [[Selective Install Pattern]] — Component-subset selection on top of plugin specs (NEW 2026-06-10)
- [[Harness Adapter Pattern]] — Per-target translators for one-source-many-harnesses (NEW 2026-06-10)
- [[Cross-Harness Compatibility]] — Comparison of selective-install + adapter approaches (NEW 2026-06-10)

### Questions (Open Inquiry)

- [[How does the LLM Wiki pattern work]]
- [[Research ECC Plugin Selective-Install Patterns]] — 2026-06-10 | 2 research rounds | 6 sources | synthesis of selective-install + harness-adapter patterns across ECC / wshobson / Compound Engineering / HarnessForge (NEW)

### Comparisons (Trade-offs)

- [[Wiki vs RAG]] — When to use wiki vs retrieval-augmented generation
- [[claude-obsidian-ecosystem]] — Feature matrix of 16+ Claude+Obsidian projects

---

## Domain Hubs (Cross-cutting)

### [[domains/_index]]

- **[[Architecture]]** — System design, layers, modules, flows
- **[[Compliance]]** — Regulatory rules, zones, programs
- **[[Business]]** — Market, strategy, roadmap

---

## Sources & Research

- [[Post-Business-Plan Documentation Program]] — 2026-04-23 | 35-document inventory | 4-wave delivery model
- [[claude-obsidian-ecosystem-research]] — 2026-04-08 | 16+ repos analyzed | 8 wiki pages created
- [[Claude Code Plugins Official Docs]] — Anthropic spec (foundational) (NEW 2026-06-10)
- [[Claude Code Skills Official Docs]] — Anthropic spec (foundational) (NEW 2026-06-10)
- [[Claude Code Plugin Reference]] — Anthropic full reference (NEW 2026-06-10)
- [[ECC Repository README]] — Cross-harness agent OS, 261 skills, 7+ harnesses (NEW 2026-06-10)
- [[ECC v1.9.0 Release Notes]] — Selective install introduction, March 2026 (NEW 2026-06-10)
- [[wshobson Agents Marketplace]] — 84 plugins to 5 harnesses, build-time generation (NEW 2026-06-10)
- [[Compound Engineering Plugin]] — Bidirectional universal adapter, 11+ harnesses (NEW 2026-06-10)

---

## Meta / Sessions

- [[meta/claude-obsidian-v1.2.0-release-session]] — Release session log (v1.2.0)
- [[meta/claude-obsidian-v1.4-release-session]] — Release session log (v1.4)
- [[meta/dashboard]] — Wiki dashboard (Bases-driven)
- [[meta/full-audit-and-system-setup-session]] — Audit + system setup session
- [[meta/agronutikas-multirepo-phase-a-bc-shipping]] — AgroNutikas multi-repo enhancement plan + Phase A B/C shipping (2026-06-10)

---

## Quick Stats

| Category                   | Count                                                      |
| -------------------------- | ---------------------------------------------------------- |
| **Wiki pages**             | 44                                                         |
| **Backend modules**        | 20 pilot + 5 shared                                        |
| **Frontend views**         | 20+                                                        |
| **Compliance rules**       | 36 (20 conventional + 16 organic)                          |
| **Data flows**             | 15                                                         |
| **Architecture decisions** | 27 ADRs                                                    |
| **External integrations**  | 5 (PRIA, e-Äriregister, X-tee, weather, business registry) |

---

## Getting Started

1. **First time?** → Read [[overview]] (5-min executive summary)
2. **Need context fast?** → Check [[hot]] (latest session cache)
3. **Exploring architecture?** → Start at [[Architecture]] (system design hub)
4. **Understanding compliance?** → Read [[Compliance]] (regulatory domain)
5. **Researching business?** → See [[Business]] (market strategy)

---

## Directory Structure

```
wiki/
├── concepts/          Mode A: Foundational ideas
├── entities/          Mode A: Resources, people, tools
├── sources/           Research sources
├── meta/              Wiki metadata, governance
├── questions/         Open inquiry
├── comparisons/       Trade-offs, feature matrices
├── modules/           Mode B: Backend modules (20)
├── components/        Mode B: Frontend components (20+ views, 40+ components)
├── decisions/         Mode B: Architecture Decision Records (27)
├── dependencies/      Mode B: External services, APIs
├── flows/             Mode B: Data flows (15 key flows)
├── stakeholders/      Mode C: Team, users, partners
├── deliverables/      Mode C: Shipped features, status
├── intel/             Mode C: Market, competitive intelligence
├── domains/           Domain hubs (Architecture, Compliance, Business)
├── index.md           Main index (this file)
├── overview.md        Executive summary
├── hot.md             Session context cache
└── [other files]      Log, Wiki Map, getting-started
```

---

## Navigation

**Hub Pages:** [[Architecture]] | [[Compliance]] | [[Business]] | [[domains/_index]]

**Indexes:** [[modules/_index]] | [[components/_index]] | [[decisions/_index]] | [[dependencies/_index]] | [[flows/_index]] | [[stakeholders/_index]] | [[deliverables/_index]] | [[intel/_index]]

**Concepts:** [[concepts/_index]] | [[entities/_index]] | [[sources/_index]] | [[questions/_index]] | [[comparisons/_index]]

**Meta:** [[overview]] | [[log]] | [[hot]]
