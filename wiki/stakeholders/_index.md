---
type: index
title: "Stakeholders Index"
created: 2026-04-23
updated: 2026-05-08
tags:
  - mode-c
  - business
  - stakeholders
  - index
status: seed
related:
  - "[[Business]]"
  - "[[deliverables/_index]]"
  - "[[intel/_index]]"
---

# Stakeholders Index

Key people, organizations, and roles in AgroNutikas.

---

## Core Team

- **[[LEISSON]]** — Gert Leisson — Founder, product lead, joined 2024. UAT töövari kõrval Riho 2026 hooaeg. LEISSON-DARKSSON on GitHub.
- **Jaan Pullerits** — Senior Developer — full-stack ja arhitektuur (Java/JavaScript, PostgreSQL, DevOps). Joined April 2026.

---

## Pilot & Domain Experts

- **Riho Kens** — Pilot farmer, domain expert. Viraito OÜ, 942 ha, Põltsamaa. Joined 2025.
- **[[Single-Farm Pilot]]** — MVP scope: one Estonian farm (Viraito OÜ, intentional MVP constraint)

---

## Farmers & Users

- **Farm operators** — Daily users (demo SPA: logging activities, viewing compliance)
- **Farm managers** — Decision-making (compliance, subsidy planning, fertility recommendations)

---

## Government & Data Sources

- **[[PRIA]]** — Estonian Farm Registry (parcel data, crop history, subsidy programs)
- **[[e-Äriregister]]** — Business registry (business lookup, registration codes)
- **[[X-tee]]** — Estonian data exchange backbone (secure government data access)
- **Keskkonnaamet** — Environmental Agency (restriction layer data: NTA, water zones, protected areas)
- **Maaeluministeerium** — Ministry of Rural Affairs (policy, subsidy frameworks)

---

## Cloud & Infrastructure Providers

- **[[Railway]]** — Backend hosting (API, Worker, WhatsApp Bot)
- **[[Vercel]]** — Frontend hosting (Demo SPA, marketing Web, platform project)
- **[[PostgreSQL + PostGIS]]** — Database provider (geospatial analytics)

---

## Technology Partners

- **[[Anthropic]]** — Claude LLM (WhatsApp bot, development)
- **[[Leaflet]]** — Map rendering library
- **[[Chart.js]]** — Data visualization
- **[[Open-Meteo]]** — Weather data (free API)

---

## Market & Investors

- **[[Estonian Agriculture Sector]]** — Market TAM (3500+ farms, EU CAP subsidies EUR 200M+ annually)
- **Potential investors** — EU climate tech funds, AgTech VCs, Estonian government support
- **Competitors** — PRIA native tools, other EU compliance platforms

---

## Roles

| Role              | Responsibility                          | Internal?                 |
| ----------------- | --------------------------------------- | ------------------------- |
| Founder/CEO       | Vision, strategy, user research         | LEISSON                   |
| Backend Engineer  | API, database, integrations             | Claude (agent)            |
| Frontend Engineer | Demo SPA, demo deployer                 | Claude (agent)            |
| DevOps            | CI/CD, Railway/Vercel setup, monitoring | Claude (agent)            |
| Product Manager   | Roadmap, prioritization, user feedback  | TBD                       |
| Design            | UX/UI design system, accessibility      | TBD (demo DS v3.0 seeded) |

---

## Communication Style

- **Founder:** Terse, pragmatic, shows output not plans, values shipping
- **Technical team:** Estonian domain terms, GitHub-first, test-driven
- **Government:** Formal, regulation-focused, data privacy paramount
- **Farmers:** Plain language, mobile-first (offline support), practical benefits

---

## Key Relationships

```
AgroNutikas (platform)
  ├── PRIA WFS (read parcel data, crop history)
  ├── e-Äriregister SOAP (lookup businesses)
  ├── X-tee (secure data exchange)
  ├── Keskkonnaamet (restriction layers)
  ├── Farmers (daily users)
  ├── Vercel + Railway (hosting)
  └── Claude/Anthropic (LLM, development)
```

---

## Stats

- **Core team:** 2 (Gert Leisson, Jaan Pullerits)
- **Pilot/domain experts:** 1 (Riho Kens, Viraito OÜ)
- **Government sources:** 3 (PRIA, e-Äriregister, X-tee)
- **Cloud providers:** 2 (Railway, Vercel)
- **Target users:** 1 farm (MVP — Viraito OÜ, 942 ha), 3500+ farms (TAM)
- **Defined roles:** 6

---

## Navigation

[[Business]] | [[deliverables/_index]] | [[intel/_index]]
