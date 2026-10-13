---
title: Build Log
type: log
project: "[[Holding ERP]]"
tags: [internship, erp, log]
---

# Build Log

One entry per working session, newest on top. Written by Claude at wrap up. Process is in [[Workflow]].

## 2026-10-13 (ninth session)
- Wrote and got approval for the slice 6 plan, with the colleague's 11 starter units and code format, and an audit log (Jingga's call)
- Built slice 6 in three commits, each clicked through in a browser:
  - 6a: audit log written by PostgreSQL on every change, units screen, 11 starter units per company, `items.view` and `items.manage`
  - 6b: items list with search and Show inactive, Add item
  - 6c: item page with Edit, Deactivate, Reactivate and History
- The database refuses deletes on items and units, and an item using another company's unit
- Found while clicking through: a refused save cleared the form. Fixed for master data, logged as a known issue for the Users dialogs
- Strict code review on slice 6, all 6 findings fixed: one shared form dialog, one edit dispatcher, one item query, one code check. 331 lines removed, 225 added
- 98 tests pass. Lint, typecheck and build pass locally. CI only runs on main and pull requests, so it did not run on the working branch
- Docker Hub was rate limited in the cloud session, so the local database ran on the machine's own PostgreSQL 16
- Next: slice 7, customers

## 2026-10-13 (eighth session)
- Built slice 5 in three commits, each clicked through in a real browser:
  - 5a shadcn/ui with the company theme colors, Settings section, Users list with search
  - 5b Add user: new email gets an account and a set your password link (3 days), an existing account from another company just gets access. Account, access and link saved together
  - 5c Manage: change role, remove access (one company only), give access back. Super Admin deactivates or reactivates an account: no sign in anywhere, signed out at once. Roles screen with a tick grid per company
- Bugs found while testing: the form crashed when no role was picked, roles were listed alphabetically instead of in template order, two test files changed the same demo user at the same time, the seed would have undone Roles screen changes. All fixed
- Strict code review on slice 5, all 6 findings fixed: permission checks with the rules, one rule for who can't be changed, success messages that can't be faked by a link, one table for the Manage buttons, simpler user and permission types
- 72 tests pass, CI green on every commit
- Wrote [[Architecture]]: the main parts of the code (login, access and permissions, company isolation, users and roles, mail, database, tests) and how they link
- Installed graphify for this session. Only useful once the code is bigger, plan is to run it after slice 9
- Next: slice 6, items and UoM

## 2026-10-13 (seventh session)
- Ran the app with pnpm dev. Fixed: Next.js instant navigation warnings on every per request page (each page now opts out on its own), and the dev badge covering the sidebar links
- Built slice 4: permissions, roles (6 per company), role permissions and company access tables. PostgreSQL row level security keeps companies apart, the app connects as a limited user, the owner only for migrations and seed. Several roles per person per company allowed
- Demo users: PDT-00001 Pandu viewer, PDT-00002 staff in Pandu and Anggoan (not in production)
- Tests prove a Pandu person can't read or change Anggoan's rows even with hand written queries, Super User can't change anything, Super Admin can. 52 tests pass
- Strict code review on slice 4: one company lookup in one transaction, data and page code split, typed seed users, proper rollback in tests
- Next: slice 5

## 2026-10-13 (sixth session)
- Built slice 3 in three commits, each clicked through in a real browser:
  - 3a login and logout with Better Auth, email or Employee ID, placeholder Super Admin and Super User, every page needs a login
  - 3b forgot password, same answer whether or not the account exists, one hour one time link, Mailpit in docker compose
  - 3c two-step login for the holding accounts with QR code and 10 backup codes
- Found and fixed a bug while testing 3c: the backup codes were never shown, because the setup page reloaded after the code was accepted. They now show before the last step
- 30 tests pass, CI green on 3a and 3b
- New rule: the strict code review runs after every slice. Ran it on slice 3 and fixed all 5 findings: one shared error handler instead of six copies, one place that turns an email or Employee ID into an account, one password length rule, a shared page frame, and each action next to its page. 37 tests pass, all three login flows re-checked in a browser
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
