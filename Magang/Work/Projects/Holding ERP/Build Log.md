---
title: Build Log
type: log
project: "[[Holding ERP]]"
tags: [internship, erp, log]
---

# Build Log

One entry per working session, newest on top. Written by Claude at wrap up. Process is in [[Workflow]].

## 2026-10-13 (sixth session)
- Built slice 3 in three commits, each clicked through in a real browser:
  - 3a login and logout with Better Auth, email or Employee ID, placeholder Super Admin and Super User, every page needs a login
  - 3b forgot password, same answer whether or not the account exists, one hour one time link, Mailpit in docker compose
  - 3c two-step login for the holding accounts with QR code and 10 backup codes
- Found and fixed a bug while testing 3c: the backup codes were never shown, because the setup page reloaded after the code was accepted. They now show before the last step
- 30 tests pass, CI green on 3a and 3b
- Next: slice 4

## 2026-10-13 (fifth session)
- Closed Phase 0, logged the decisions in [[Holding ERP]]
- Added the web-design-guidelines skill to both repos
- Rewrote slice 1 so it shows as jingaega. CI passed on it
- Built slice 2: companies table and seed, the 6 approved themes and logos, company picker, themed shell per company. 11 tests pass. A web-design-guidelines review found small accessibility gaps (skip link, focus state, long names), all fixed
- Known limit: an unknown company shows the not found page but with HTTP status 200, because the page streams before the check. Fine for users
- Renamed Maraloka to Sibama everywhere (decision in [[Holding ERP]]), with a migration for databases seeded earlier
- Logged the HTTP 200 on unknown companies under Known issues in [[Foundation Module]]
- Ran the strict code review on slices 1 and 2 and fixed all 5 findings: one company lookup per page instead of three, build works without a database (needed for AWS), one clear home for the company list, shared page shell and sidebar component. Behavior unchanged, all checks pass
- Next: slice 3, login

## 2026-10-13 (fourth session)
- Decided: fresh repo, colleague's repo as reference (see [[Colleague Repo Comparison]])
- Built Foundation slice 1 in `jingaega/PT-SERIO-ERP`: Next.js 16, PostgreSQL 16 in Docker, Drizzle, `/api/health`, Vitest, CI, `CLAUDE.md`, README, ADR 0001
- Checks: lint, typecheck, 1 test and the build all pass. Health check returns 200 with the database up and 503 with it down
- Added [[Glossary]]
- Next: slice 2, companies with themes

## 2026-10-13 (third session)
- No code yet
- Read the colleague's repo and wrote [[Colleague Repo Comparison]]
- Logged Jingga's calls: stack, English UI, super users, intercompany later, minimum margin, Wening, roles. Updated Foundation, S&M and Claude Code Instructions to match
- Next: contribute to the colleague's repo or build our own

## 2026-10-13 (second session)
- No code yet
- Closed Phase 0 in the vault: stack, deadline, repo and UI language logged in [[Holding ERP]]
- Added [[Foundation Module]] with the Phase 1 build plan (9 slices)
- Fixed contradictions in [[Sales and Marketing Module]], added its Pandu baseline, cut Phase 2 to quote to signed BAST
- Quotes now have item lines (decision logged). Floor price question marked to ask the supervisor
- Next: Foundation answers and plan approval, then slice 1

## 2026-10-13
- Set up the vault structure and the workflow
- No code yet
- Next: choose the stack and deadline, then the S&M build plan
