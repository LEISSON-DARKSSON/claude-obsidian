---
type: domain
title: "AgroNutikas System Architecture"
created: 2026-04-23
updated: 2026-04-23
tags:
  - architecture
  - mode-b
  - system-design
  - domain
status: seed
related:
  - "[[modules/_index]]"
  - "[[components/_index]]"
  - "[[flows/_index]]"
  - "[[decisions/_index]]"
  - "[[dependencies/_index]]"
---

# AgroNutikas System Architecture

Monorepo with 6 workspaces: API, Worker, Demo SPA, Web, Bot, Contracts.

---

## High-Level Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    AgroNutikas Platform                      │
└─────────────────────────────────────────────────────────────┘

┌──────────────────────┐     ┌──────────────────────┐
│   Demo SPA           │     │    Marketing Web     │
│ (Vanilla JS, Vite)   │     │ (HTML/CSS/JS, static)│
│ demo.agronutikas.ee  │     │ agronutikas.ee       │
└──────────┬───────────┘     └──────────┬───────────┘
           │                           │
           └─────────────┬─────────────┘
                         │
                    ┌────▼────────┐
                    │  Vercel CDN  │
                    │ (Frontend)   │
                    └─────────────┘

                         ▼
        ┌────────────────────────────────┐
        │   NestJS API (v1)              │
        │   Fastify 5                    │
        │   api.agronutikas.ee           │
        │   20 pilot modules             │
        └────┬──────────────────────┬────┘
             │                      │
      ┌──────▼───────┐      ┌──────▼──────┐
      │  PostgreSQL  │      │  WhatsApp   │
      │  16+PostGIS  │      │  Bot        │
      │  35+ tables  │      │  (Claude)   │
      └──────────────┘      └─────────────┘

        ┌──────────────────────┐
        │  Outbox + Worker     │
        │  (TypeScript, pg)    │
        │  PRIA submission     │
        └──────────────────────┘

        ┌────────────────────────────────┐
        │  External Integrations         │
        │ ┌──────────────┐               │
        │ │ PRIA (WFS)   │ (R/O parcel) │
        │ ├──────────────┤               │
        │ │ e-Äriregister│ (SOAP query) │
        │ ├──────────────┤               │
        │ │ X-tee        │ (data exch.) │
        │ ├──────────────┤               │
        │ │ Open-Meteo   │ (weather)    │
        │ └──────────────┘               │
        └────────────────────────────────┘
```

---

## Deployment Topology

| Component    | Tech                       | Hosting      | Region     | Auto-Deploy         |
| ------------ | -------------------------- | ------------ | ---------- | ------------------- |
| API          | NestJS 11 + Fastify 5      | Railway      | EU         | Git tag             |
| Worker       | TypeScript + pg            | Railway      | EU         | Git tag             |
| Demo SPA     | Vite 6 + Vanilla JS        | Vercel       | Global CDN | Git tag (sync repo) |
| Web          | Static HTML/CSS/JS         | Vercel       | Global CDN | Push main           |
| WhatsApp Bot | TypeScript + Anthropic SDK | Railway      | EU         | Git tag             |
| Contracts    | OpenAPI 3.1 → TS/Dart      | CI-generated | N/A        | PR check            |

---

## Monorepo Structure

```
agronutikas-platform/
├── apps/
│   ├── api/              NestJS backend (20 pilot modules)
│   ├── worker/           Outbox processor (PRIA submission)
│   ├── demo/             Vanilla JS SPA (20+ views)
│   ├── web/              Marketing site (11 pages)
│   ├── whatsapp-bot/     Claude assistant bot
│   └── contracts/        OpenAPI specs + generated clients
├── db/
│   └── init/             PostgreSQL migrations (001-036)
├── scripts/              Build, migration, smoke tests
├── docs/                 Design system, runbooks, audits
├── .github/              CI workflows, branch protection
└── .claude/              Agents, commands, enforcement rules
```

---

## Backend Architecture (API)

### Layer Pattern

```
Request (HTTP)
  ↓
Controller (HTTP handler, validation, PilotAuthGuard)
  ↓
Service (business logic, orchestration, transaction boundary)
  ↓
Repository (SQL/PostGIS only, parameterized queries)
  ↓
PostgreSQL (tables with pilot_ prefix, constraints, triggers)
```

### Module Structure (20 pilot modules)

**Core:** auth, farm, data, crop-history, activities

**Domain:** compliance, rules, fertility, organic (20+16 rules), subsidies, weather

**Integration:** xtee, business-registry, import, history, alerts, feedback, automation, setup

**Observability:** analytics (UX events ingestion + summary, migration 036)

**Aggregator:** pilot (imports all, registered in app.module.ts)

### Database

- **PostgreSQL 16** — Single source of truth
- **PostGIS 3.4** — Geospatial queries (ST_Intersect, ST_Area)
- **036 migrations** — Idempotent (IF NOT EXISTS, CREATE OR REPLACE); next free is 037 (008 + 035 unused)
- **pilot\_ prefix** — All tables scoped
- **Constraints:** FOREIGN KEY ON DELETE CASCADE/SET NULL, spatial indexes (GiST)

### Authentication & Session

```
PBKDF2-SHA256 password hashing
  ↓
HttpOnly signed cookie (sessionid)
  ↓
PilotAuthMiddleware (Fastify preHandler hook)
  ↓
request.pilotSession available globally
  ↓
PilotAuthGuard protects routes
```

### Error Handling

- **Errors explicit** — try-catch at every boundary
- **Logs structured** — `app` + `app_version` in every line
- **User-friendly** — API response envelope: status, data, error, metadata

---

## Frontend Architecture (Demo SPA)

### View Switching

```
HTML: 20+ .view-panel sections
  ↓
switchView(viewId) — show 1, hide others
  ↓
Navigation: data-view attributes + overflow menu
  ↓
Each view: initView() + renderView() exports
```

### API Clients (src/api/client.js)

```
AuthClient
  ├─ login, logout, user status
  └─ session validation

DataClient (WFS + cache)
  ├─ getParcelsByFarm (WFS fallback)
  ├─ getRestrictionLayers (7 layers)
  └─ getCropHistory

FarmClient
  ├─ getFarmData
  ├─ updateParcel
  └─ createFarm

FarmActivityClient
  ├─ logActivity
  └─ simulateAction (compliance check)

AdminClient
  ├─ getUsers
  ├─ approveUser
  └─ farmManagement
```

### State Management

```
Module-scoped variables (no Redux/Zustand)
  ├─ authClient.user (global auth state)
  ├─ sessionStore (selected parcel)
  ├─ preferences (user settings, localStorage)
  └─ orgContext (current organization)

Each view maintains its own _state object
  ├─ todayView._state (today forecast)
  ├─ mapView._state (map pan/zoom)
  └─ ...
```

### Offline & Resilience

```
IndexedDB (offlineQueue)
  ├─ Queued mutations (activities, updates)
  ├─ Synced on reconnect
  └─ Exponential backoff retry

Bundled GeoJSON fallback
  ├─ 8 restriction layer GeoJSON
  ├─ ~2.5 MB, loaded on init
  └─ Replaces WFS if offline

EventSource token injection
  ├─ SSE cannot use fetch monkey-patch
  ├─ Token passed as ?token= query param
  └─ Progress streaming for import/export
```

---

## Compliance Rules Engine

### Conventional Rules (20)

- Nitrogen limits (NTA enforcement, crop-specific)
- Field restrictions (water zones, protected areas)
- Cropping rules (rotation, grassland)
- Activity restrictions (timing, method)

### Organic Rules (16)

- EU 2018/848 compliance (crop restrictions, transitions, inputs)
- Grassland detection (5+ years = permanent grassland)
- Input eligibility (PTA register validation)
- Rotation requirements

### Fertility Scoring (6 components)

1. Soil pH
2. Phosphorus (P)
3. Potassium (K)
4. Organic matter (OM)
5. Cation exchange capacity (CEC)
6. Soil texture

---

## Integration Points

### Read-Only (No Writeback)

- **PRIA WFS** — Parcel boundaries, crop history, subsidy programs
- **e-Äriregister SOAP** — Business lookup by registration code
- **X-tee** — Secure government data exchange
- **Open-Meteo API** — Weather forecast (free, no auth)
- **Ilmateenistus** — Estonian meteorological service

### Outbox Pattern (Eventual Consistency)

```
API writes outbox table
  ↓
Worker polls (every 5s)
  ↓
PRIA submission API
  ↓
Retry with exponential backoff
  ↓
DLQ if fails 3x
  ↓
pilot_deploy_log records event
```

---

## Versioning & Deployment

### Auto-Versioning (Conventional Commits)

```
Commit message: feat(pilot-auth): add oauth
  ↓
CI parses conventional commit
  ↓
Detect feat: → minor bump
  ↓
Bump package.json + version.json
  ↓
Create git tag: api-v1.2.0
  ↓
Railway/Vercel auto-deploys on tag
  ↓
Changelog JSON generated + pushed to web repo
```

### Version Footer

All UIs display version + git SHA:

- Demo: `Demo v0.2.0 · a3f7c2d` + API version
- Web: reads version-meta.json
- API: `/health` returns version + env

---

## CI/CD Pipelines

| Pipeline               | Trigger                     | Required Checks                                                           |
| ---------------------- | --------------------------- | ------------------------------------------------------------------------- |
| required-backend-check | Push main, PRs              | migrations check, contracts drift, API/worker build+tests, security audit |
| demo-design-system     | Push main, PRs (demo paths) | DS verifiers, contract tests, visual tests, screenshot regression, build  |
| web-ci                 | Push main, PRs              | type-check, build                                                         |
| auto-version           | Push main (conv. commits)   | Parse commit, bump versions, create tags, push changelog                  |
| post-deploy-smoke      | After deploy                | Public runtime + demo site + internal write-path smoke tests              |
| lighthouse             | Push main, PRs (demo paths) | Lighthouse CI performance audits                                          |
| codeql                 | Push main, PRs, weekly      | Security scanning (JavaScript/TypeScript)                                 |
| gitleaks               | Push main, PRs              | Secret detection                                                          |

---

## Key Metrics

- **Backend:** 20 pilot modules, 24+ tables (pilot\_ prefix), 036 migrations (next 037), 27 ADRs
- **Frontend:** 20+ views, 40+ components, 458+ tests (80%+ coverage)
- **Rules:** 20 conventional + 16 organic, 6-component fertility, grassland detection
- **Integrations:** 5 (PRIA, e-Äriregister, X-tee, Open-Meteo, Ilmateenistus)
- **Deployment:** Railway (backend), Vercel (frontend), GitHub Actions CI

---

## Navigation

[[Business]] | [[modules/_index]] | [[components/_index]] | [[flows/_index]] | [[decisions/_index]]
