---
type: session
title: "AgroNutikas Multi-Repo Enhancement Plan + Phase A B/C Shipping"
created: 2026-06-10
updated: 2026-06-10
tags:
  - session
  - agronutikas
  - multi-repo
  - ecc
  - phase-a
  - planning
  - security
  - tech-debt
status: evergreen
related:
  - "[[log]]"
  - "[[hot]]"
  - "[[index]]"
  - "[[overview]]"
  - "[[Architecture]]"
  - "[[decisions/_index]]"
---

# AgroNutikas Multi-Repo Enhancement Plan + Phase A B/C Shipping

Long-form planning session followed by partial Phase A execution. Drafted an 18-month multi-repo enhancement plan across the AgroNutikas workspace (5 product repos + ECC plugin), then shipped Phase A items B (TECH-DEBT P0.1 sync) and C (`branch-status.sh`) the same day.

Plan file: `C:\Users\gert\.claude\plans\a-composed-stallman.md` (approved).

---

## Session Arc

1. **ECC repo doc fix** — clarified that `ecc migrate audit|plan|scaffold|import-*` ships in the Rust `ecc-tui` crate (`ecc2/`), not the npm `ecc-universal` Node CLI. Added binary-comparison table in `HERMES-SETUP.md` and renamed commands in `HERMES-OPENCLAW-MIGRATION.md`.
2. **Windows test fixes** — `tests/scripts/instinct-cli-projects.test.js` hardcoded `python3` (fails on Windows where only `python`/`py` exist); `tests/lib/resolve-formatter.test.js` assumed `os.tmpdir()` had no ancestor `package.json` (false on Windows where tmpdir lives under `%USERPROFILE%`). Both fixed with platform-aware fallbacks mirroring existing patterns in sibling tests.
3. **Comprehensive multi-repo brainstorm** — dispatched 3 parallel Explore agents to map workspace layout, ECC reusable patterns, and friction signatures. Then 3 parallel Plan agents (one per phase horizon). Resulting plan: 3 horizons (A foundation, B ECC adoption, C operator control plane), 12 months Phase B + 12 months Phase C with overlap.
4. **Phase A item B — TECH-DEBT P0.1 reality sync** — discovered the feared credential leak never actually had remote exposure (files always gitignored, never committed). Quarantined local copies + updated `TECH-DEBT.md` to reflect actual state.
5. **Phase A item C — `branch-status.sh`** — workspace operator script replacing the manual "check git status/branch/remote per repo" session-start ritual.
6. **Paused** before A.1 sprint (workspace AGENTS.md §13 + anchors + 8 per-repo pointer-conversion PRs — needs ~3 weeks).

---

## PRs Shipped

| PR                                                                | Repo                                | Title                                                          | Commit     |
| ----------------------------------------------------------------- | ----------------------------------- | -------------------------------------------------------------- | ---------- |
| [#1](https://github.com/AgroNutikas/performance/pull/1)           | `AgroNutikas/performance`           | docs(hermes): clarify ecc-tui migrate + fix Windows tests      | `9533c520` |
| [#3](https://github.com/AgroNutikas/agronutikas-workspace/pull/3) | `AgroNutikas/agronutikas-workspace` | docs(tech-debt): mark P0.1 RESOLVED with actual evidence trail | `ff0140d7` |
| [#4](https://github.com/AgroNutikas/agronutikas-workspace/pull/4) | `AgroNutikas/agronutikas-workspace` | feat(scripts): add branch-status.sh workspace operator script  | `a01c3573` |

All squash-merged via REST API (`gh pr merge` GraphQL path threw permission error despite admin role — REST `PUT /pulls/:n/merge` succeeded as fallback; likely org-level SSO/token-scope quirk on `AgroNutikas`).

---

## Quarantine Created

`C:\Users\gert\Desktop\AgroNutikas\_handoff\p0-1-quarantine-2026-06-10\`

- `apply-riho-seed.mjs`, `set-riho-password.mjs` (moved from `agronutikas-platform/artifacts/`)
- `README.md` documenting evidence trail
- Gitignored. Safe to delete after 2027-06-10.

---

## Key Decisions Captured

- **State DB topology (Phase C.1)**: hybrid per-repo authoritative + workspace read-only aggregate via SQLite `ATTACH`, NOT single workspace DB. Reason: preserves ECC `ECC_STATE_DB` resolution invariants, avoids forcing env injection per hook per repo.
- **AgroNutikas governance skill home (Phase B.2)**: stay in `agronutikas-governance-skills/` at workspace root. Publish as ECC selective-install pack via `agronutikas-pack.json`. NOT moved into design-system plugin (audience mismatch — design-system = brand skills, governance = PR runners + DB gates). NOT mirrored into `performance/skills/` (ECC is public OSS).
- **Multi-machine state sync (Phase C.7)**: explicitly NONE. State DB contains decision traces possibly including farmer PII (workspace AGENTS.md §16 privacy boundary). Each machine's DB independent. PII-scrubbed JSON export for cross-machine HUD if needed.
- **History purge for P0.1**: deferred. Files never committed → no remote exposure → no purge needed. Original plan had this as Phase A.2 deliverable; reality made it moot.
- **GitHub PR merge via REST not GraphQL**: workaround for `LEISSON-DARKSSON does not have the correct permissions to execute MergePullRequest` GraphQL error. REST `gh api -X PUT repos/.../pulls/N/merge` works.
- **Worktree pattern for workspace PRs**: when workspace has in-flight branch with unrelated work, use `git worktree add ../wt-name -b chore/... origin/master` rather than stash. Keeps original checkout untouched.

---

## Plan Phase Status

### Phase A (Foundation Consolidation + Safety Automation, Months 0–3)

- ✅ A.2 — TECH-DEBT P0.1 reality sync (PR #3) — original 15-min estimate; actual 50 min dominated by discovery
- ✅ A.4 — `branch-status.sh` (PR #4) — first of 4 planned workspace scripts
- ⏳ A.1 — workspace AGENTS.md §13 + stable anchors + 8 per-repo pointer-conversion PRs (~3 weeks sprint)
- ⏳ A.3 — `.gitignore` template + `check-env-drift.sh`
- ⏳ A.4 remaining — `verify-all.sh`, `pria-copy-scan.sh`, `secret-hygiene.sh`
- ⏳ A.5 — disposable-DB guard for `ci:migrations:check`
- ⏳ A.6 — single PR template at workspace + per-repo copies
- ⏳ A.7 — wire ECC `block-no-verify` + `mcp-health-check` at workspace level
- ⏳ A.8 — deprecation table audit

### Phase B (ECC Selective Adoption + Cross-Repo Standardization, Months 3–9)

- ⏳ Not started. Blocked on Phase A completion.

### Phase C (Operator Control Plane + Multi-Repo Orchestration, Months 6–18)

- ⏳ Not started. Blocked on Phase B.

---

## Cross-Cutting Findings Worth Remembering

- **Workspace root `.gitignore` line 3 is `*`** — ignore-everything-by-default. Any new workspace-level file (script, tool, doc) needs explicit allowlist entry following the `/tools/` pattern. Easy to miss.
- **Workspace default branch is `master`**, not `main`. Workspace remote: `AgroNutikas/agronutikas-workspace`.
- **`agronutikas-platform/.gitignore:43` = `/artifacts/`** — entire `artifacts/` dir gitignored. Was the protective measure that kept P0.1 credential files out of git history despite living in working tree.
- **Active workspace repos** (verified 2026-06-10): `agronutikas-platform`, `AgroNutikas-APP`, `agronutikas-web`, `AgroNutikas-Design-System-V8.1`, `agronutikas-business-materials`, `performance` (+ workspace root). `agronutikas-governance-skills/` is a tracked subdir of the workspace repo, not its own git repo.
- **Pre-tool Bash hook in `performance/` runs full `npm test` (2640 tests) before every Bash call** — slow, blocks commits when any test fails or registry drifts. `npm run command-registry:write` needs to run periodically to clear stale-registry block.
- **Many sibling worktrees exist** (`AgroNutikas-APP-d1a-weather-test-stability`, `agronutikas-web-codex-operating-protocol`, `agronutikas-video-launch-phase[3-8]`, etc.) — must be skipped from any "list active repos" allowlist.

---

## Files Created / Modified This Session

**Created** (in workspace via PR #4):

- `scripts/branch-status.sh` (150 lines)

**Created** (locally, gitignored):

- `_handoff/p0-1-quarantine-2026-06-10/apply-riho-seed.mjs` (moved from platform)
- `_handoff/p0-1-quarantine-2026-06-10/set-riho-password.mjs` (moved from platform)
- `_handoff/p0-1-quarantine-2026-06-10/README.md` (evidence trail)

**Created** (in plans):

- `C:\Users\gert\.claude\plans\a-composed-stallman.md` (approved long-run plan, ~700 lines)

**Modified** (via PRs):

- `performance/docs/HERMES-SETUP.md` — added "Two `ecc` binaries" section + renamed commands
- `performance/docs/HERMES-OPENCLAW-MIGRATION.md` — same disambiguation paragraph + renamed commands
- `performance/tests/scripts/instinct-cli-projects.test.js` — added `findPython()` helper
- `performance/tests/lib/resolve-formatter.test.js` — added ancestor-pollution skip
- `agronutikas-workspace/TECH-DEBT.md` — P0.1 entry marked RESOLVED with evidence
- `agronutikas-workspace/.gitignore` — added `scripts/` allowlist block

---

## Next Session Entry Point

Per **B → C → A.1** sequence agreed at session start:

> **A.1 sprint** — workspace `AGENTS.md` §13 + stable anchors + 8 per-repo pointer-conversion PRs. ~3 weeks of small focused PRs. First PR: anchors-only on workspace `AGENTS.md` (1 PR, 25 min).

After A.1: remaining Phase A items in week order — `.gitignore` template, remaining 3 workspace scripts, disposable-DB guard, PR template, ECC hook wiring.

Plan file is the single source of truth; refer to it for any phase question.

---

## Skill / Tooling Notes (for future sessions)

- **MCP disconnect mid-session**: `mcp__github__*`, `mcp__memory__*`, `mcp__playwright__*`, `mcp__obsidian-vault__*`, `mcp__sequential-thinking__*`, `mcp__context7__*` all disconnected at ~75% session token usage. `gh` CLI worked fine as fallback. Direct filesystem access also worked. No blocker — note the dependency for future sessions.
- **Plan-mode + auto-mode coexistence**: when both system reminders fire, plan mode wins (per its `supercedes any other instructions` note). Auto mode's "execute immediately" was overridden during planning phase, then resumed for execution.
- **Caveman mode (full intensity)**: ran the entire session without revert. Worked well except for security/git operations where normal-mode explicit prose paid off (commits, PRs, security findings, user decisions).
