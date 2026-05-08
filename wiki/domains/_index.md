---
type: index
title: "Domain Hub Index"
created: 2026-04-23
updated: 2026-04-23
tags:
  - domains
  - hub
  - index
status: seed
related:
  - "[[index]]"
  - "[[Architecture]]"
  - "[[Compliance]]"
  - "[[Business]]"
---

# Domain Hub Index

Cross-cutting domain knowledge: system architecture, regulatory landscape, business strategy.

---

## System Architecture

**[[Architecture]]** — AgroNutikas system design, deployment topology, 20 backend pilot modules, frontend layers, compliance rule engines, integration points, versioning.

Key subsections:

- High-level overview (6 workspaces, 3 deployment targets)
- Backend architecture (Controller-Service-Repository, 24+ tables, 036 migrations)
- Frontend architecture (20+ views, 4 engines, module-scoped state)
- Compliance rules (20 conventional + 16 organic, 6-component fertility)
- CI/CD pipelines (auto-versioning, smoke tests, security scanning)

---

## Estonian Agricultural Compliance

**[[Compliance]]** — Regulatory domain, restriction zones (7 types: NTA, water, reserves, etc.), nitrogen limits, subsidy programs, compliance rules.

Key subsections:

- Restriction zones (NTA, water protection, protected areas, Natura2000)
- Nitrogen limits (170 kg/ha normal, 85 kg/ha NTA, crop-specific)
- Conventional rules (20: activity-based, crop-based, geospatial, subsidy, input, record-keeping)
- Organic rules (16 EU 2018/848: crop restrictions, transitions, inputs, rotation)
- Subsidy programs (BPS, greening, organic cert, young farmer)
- Data sources (PRIA WFS, Keskkonnaamet, e-Äriregister, X-tee)

---

## Business Strategy

**[[Business]]** — Market opportunity, competitive positioning, go-to-market roadmap, business model, funding strategy.

Key subsections:

- Mission & vision (compliance OS, expand to 500+ farms)
- Market TAM (3500+ farms, EUR 200M+ CAP spend)
- Competitive analysis (vs. PRIA tools, EU SaaS, spreadsheets)
- Go-to-market phases (MVP 2026 Q2, expansion 2026-2027, scale 2027+)
- Business model (freemium SaaS, EUR 30-500/month pricing)
- Strategic bets (PRIA partnership, organic focus, WhatsApp gateway)
- Funding roadmap (bootstrap → pre-seed → seed)

---

## Navigation Patterns

### Mode B (Technical) Pages

- [[Architecture]] (system design)
- [[modules/_index]] (19 backend modules)
- [[components/_index]] (20+ views, 40+ components)
- [[decisions/_index]] (27 architecture decision records)
- [[dependencies/_index]] (external services, APIs, libraries)
- [[flows/_index]] (data movement, 15 key flows)

### Mode C (Business) Pages

- [[Business]] (strategy, market, competition)
- [[stakeholders/_index]] (team, users, government, partners)
- [[deliverables/_index]] (shipped features, module status)
- [[intel/_index]] (market TAM, competitive landscape, risks)

### Hub (This Section)

- [[Architecture]] (technical overview, all layers)
- [[Compliance]] (regulatory domain, rules engine)
- [[Business]] (strategic vision, market positioning)
- [[domains/_index]] (this page)

---

## Cross-Reference Examples

### From Architecture to Business

- Architecture → Business: Read [Business] to understand market constraints on system scope (single-farm MVP → multi-farm future)
- Architecture → Compliance: Read [Compliance] to understand rule semantics (20 rules, NTA zones, organic checks)

### From Business to Technical

- Business → Architecture: Feature requests → [[modules/_index]] for implementation scope
- Business → Compliance: Subsidy program changes → [[Compliance]] for rule updates

### From Compliance to Code

- Compliance → Architecture: NTA zone rules → [[pilot-compliance]] module
- Compliance → Components: Restriction layer visualization → [[components/_index#Maps]] for Leaflet layers

---

## Stats Rollup

| Category                  | Count                                                      |
| ------------------------- | ---------------------------------------------------------- |
| **Domains**               | 3 (Architecture, Compliance, Business)                     |
| **Backend modules**       | 19 (with 5 shared)                                         |
| **Frontend views**        | 20+                                                        |
| **Frontend components**   | 40+                                                        |
| **Rules**                 | 20 conventional + 16 organic + fertility                   |
| **Data flows**            | 15 key flows                                               |
| **External integrations** | 5 (PRIA, e-Äriregister, X-tee, weather, business registry) |
| **Deployment targets**    | 3 (Railway, Vercel, GitHub Actions)                        |
| **Database tables**       | 24 (pilot\_ prefix)                                        |
| **Migrations**            | 35+                                                        |
| **ADRs**                  | 27                                                         |

---

## Navigation

[[index]] | [[Architecture]] | [[Compliance]] | [[Business]]
