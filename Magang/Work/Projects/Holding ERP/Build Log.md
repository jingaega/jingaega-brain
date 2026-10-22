---
title: Build Log
type: log
project: "[[Holding ERP]]"
tags: [internship, erp, log]
---

# Build Log

One entry per working session, newest on top. Written by Claude at wrap up. Process is in [[Workflow]].

## 2026-10-22 (thirteenth session, continued)
- Jingga confirmed the colleague's revised Marketing rules, then asked to check FOH against general practice. Adopted my recommendation (pricing keeps a standard FOH %, the books follow PSAK 202), flagged for the supervisor. Stock going out when goods leave is built but flagged too
- Built R1 to R3 (GP 27 / 17 with a refusal under the minimum, the SPPR floor that removes the GM's SPPR approval, cost seen only by the GM, Finance and Marketing Managers and the supers). Migrations 0046 to 0048. Strict review: one exact GP check for screens and PostgreSQL
- Built S6a and S6b (DO / BAST for CBD orders with a paid PI: create, cancel, Mark Delivered, Sign with photos, Reject, order status from its lines, photos removed after 365 days). Migrations 0049 and 0050. Strict review fixed 4 things. 293 tests pass, lint, typecheck and build pass, clicked through in a browser
- The worker restarted mid session; nothing was lost. PostgreSQL needs `pg_ctlcluster 16 main start` after a restart
- Built R4 (SPPR valid from a day, with history) and R5 (Areas with cities, a customer's city and area). Strict review: one fix. 298 tests pass, clicked through in a browser. A new test raced with the customers test over Pandu's codes; it uses ETA now
- Next: S7 tempo (credit check on the SO, tempo DO / BAST, handed to FAT). Plan first

## 2026-10-21 (thirteenth session)
- Installed graphify (`graphifyy` on PyPI) and set it up for the code repo: `/graphify` skill, a code only graph (no API key), a rule in the repo's `CLAUDE.md`, PreToolUse hooks that make Claude query the graph before grepping, and git hooks that rebuild it. The session start hook now installs graphify and builds the graph in every cloud session, without ever blocking the session. A scoped query costs about 600 tokens
- Found `main` of the code repo was at S3: S3c, S4 and S2d lived only on the branch `claude/stoic-volta-cs2r54`. Fast forwarded `main` to it. CI for that push not checked
- S5 compared with the colleague's repo, ERPNext and Odoo first (PPN on the PI, partial payment, who creates, cancel after a payment). Jingga: take the colleague's way for PPN with an ask flag, partial payments with a partly paid status, Sales creates, "idk" on cancelling after a payment (my call, assumed)
- Built S5a (proforma invoice for a CBD order: amount copied from the order plus 11 % PPN *(assumed)*, frozen in PostgreSQL, one live per order, void with a reason, voided when its order is cancelled, Proforma invoices list and page) and S5b (payments: several per PI, never more than its amount, status worked out from them, a PI with a payment can't be voided and its order can't be cancelled). Migrations 0044 and 0045
- Strict review fixed 5 findings: one shared order total expression, one reason dialog for cancel and void, order changes take an options object, the order page's PI part split out, a magic "0.00" compare. 283 tests pass, lint, typecheck and build pass, clicked through in a browser
- Found: a dev server started right after `pnpm build` gets stuck on the login page again (as noted 2026-10-14). Delete `.next` and restart `pnpm dev`
- Wrote the S6 plan (DO / BAST) and compared stock timing, signature and photo, and tempo and progress gates with the colleague's repo, ERPNext and Odoo. S6 is CBD only. Jingga: photos should disappear after some time, planned as 365 days after signing *(assumed)*
- Read the colleague's other branches. `feat/marketing-sppr` (not on his `main`) has a revised Marketing plan that differs from ours on GP numbers (27 / 17 against 20 / 10), FOH, and who sees cost. Written in [[Colleague Repo Comparison]], 2 new questions for the supervisor, nothing in the code changed
- Next: S6a once Jingga approves the plan

## 2026-10-21 (twelfth session)
- Read the colleague's repo in depth and wrote [[Colleague Architecture]] for the modules we haven't built, plus a clash table in [[Colleague Repo Comparison]]
- Researched general ERP practice per module and per company type, and read ERPNext and Odoo from their code: [[ERP Best Practices]], [[ERPNext Architecture]], [[Odoo Architecture]]. New rule: each phase's plan starts with this comparison
- Margins: default 20 % and minimum 10 % are GP. Built S3c (suggested SPPR = COGS / (1 - default GP), default GP set by Marketing Managers, an SPPR below the minimum waits for the GM). Migration 0038
- Director sees costs and margins (migration 0039). Elimination explained and added to the Glossary
- S4 plan approved after comparing general practice, ERPNext, Odoo and the colleague. Built S4a (lower PO price waits for Marketing, migration 0040), S4b (sales orders from the accepted quote, numbered `SO-MMYY-NNN`, lines locked in PostgreSQL, migration 0041), S4c (stock confirmed by Warehouse, cancel by a Sales Manager with a reason, migration 0042)
- Compared the colleague's Marketing plan. Decisions: SPPR stays with Marketing, margin stays hidden from Sales, a price above the SPPR needs a reason but no approval, price lists planned. Built S2d (migration 0043)
- Strict review after every slice. Main fixes: typed decision sides, one rule for when a quote can become an order, totals summed in SQL, one guard for order changes, one shared proposal query. Logged, not fixed: approvals now come in four shapes, so a shared engine is a Decide item before Procurement
- Browser click-through after every slice. Demo data removed from the dev database after
- 271 tests pass, lint, typecheck and build pass. Branch `claude/stoic-volta-cs2r54` not merged into `main` yet
- Next: S5 PI and CBD

## 2026-10-14 (eleventh session)
- Session start hook for cloud sessions in the code repo: installs packages, starts the machine's PostgreSQL 16 (not Docker), runs `pnpm db:setup`. Merged into `main`
- S&M questions answered with Jingga, compared with the colleague's repo and the office whiteboard (GP % on the selling price, cost methods). Inbox processed into the module notes
- S&M build plan written and approved: S0 to S9, CBD first
- Built S0 (Foundation changes for S&M) in three commits plus review fixes: Marketing and Quality functions, customer payment term, credit limit, NPWP and delivery addresses, typed item cost seen only by Marketing, the GM and Finance, enforced in PostgreSQL
- Fixed a database hole: payment term checks let Net or tempo through with no days
- Jingga: address, PIC name and PIC phone required for customers and vendors
- Clicked through in a browser; found and fixed a server page passing a plain function to a client component. 206 tests pass
- Jingga: Finance sees costs and margins too. Costing model from Jingga: COGM (materials at MAP) + DL + FOH = COGS, GP on the price, SPPR set by Marketing. Sales only sees the SPPR and proposes other prices, approved by Marketing Managers
- GitHub Codespaces set up (`.devcontainer`), so the ERP runs in a browser tab. Fixed two sign in problems behind the Codespaces proxy (it sends the origin as localhost:3000)
- Built S1 (quotes) in three commits plus review fixes: SPPR on items, quotes with versions and lines priced from the SPPR, mark sent with the file, revise, mark lost, expiry. A sent version's lines are fixed in PostgreSQL. 225 tests pass, clicked through in a browser
- S0 and S1 merged into `main` in the code repo
- Found: a dev server started right after `pnpm build` can get stuck reloading the login page. Restarting `pnpm dev` fixes it
- Second costing whiteboard read: FOH in pricing is a standard % of COGM (COGS = COGM x 115 %), SPPR = COGS / (1 - GP)
- Built S2 (price proposals) in two commits plus review fixes: Pricing page (FOH rate %, minimum GP), COGS and GP on the item page, Propose Price on quote lines, To approve for Marketing Managers. Fixed a test race over Serio's pricing. 235 tests pass, merged into `main`
- Jingga shared the group's PO templates (layouts and columns only kept, in [[Procurement Module#The group's PO templates]]). Quote numbers now follow the official style, `SQ-1025-001`
- Built S3 (customer PO) in two commits plus review fixes: Record Customer PO on a sent quote, lines checked against the quote before PPN and flagged, the quote Accepted; a looser payment term waits for the GM (or a Director) on To approve, and a refusal sends the quote back to waiting. Review: quote changes now lock the quote, and approvals are one statement, so two at once can't both win. 246 tests pass, clicked through in a browser
- Cleared up with Jingga: the PO templates are Procurement documents (POs our companies send to vendors). S3's customer PO is the one Pandu receives from a customer; the templates only showed what such a PO carries
- Wrap up: CI for the S3 merge was queued; S2's passed. Next: S4 sales order

## 2026-10-13 to 14 (tenth session)
- Jingga: customers belong to Sales and Finance, Managers and up. Migration 0023. Master data owners now all answered
- Jingga: items and units belong to Finance, Production and Warehouse. Starting grants moved, migration 0022. Fixed a test race found on the way: the levels test added Sibama vendors while the vendors test counted Sibama's codes; it uses Serio now
- Jingga: master data screens are for Managers and up only; adding from modules comes later. Starting grants changed, migration 0021 takes back OPR, Lead and SPV add rights. 188 tests pass
- Slice 11b: Settings, Departments (add, rename, deactivate, people count). Function fixed after adding, people keep a deactivated department *(both assumed)*. Strict review: 3 small fixes. 188 tests pass, CI green on 11a
- Found while writing up: the slice 11 master data owners differ from Jingga's 2026-10-13 answers on customers and items. Put in Open questions in [[Foundation Module]] for Jingga
- Slice 11a: levels (Director, GM, Manager, SPV, Lead, OPR) and departments with a function instead of the 6 roles. Company Admin is a switch. Master data permissions split into add and change by owning function. Add user and Manage ask for level and department. The Roles screen has a tab per function (so 11c is done too). Demo people moved; Company Admin became GM with the switch
- Strict review of 11a: 5 findings fixed (one rule for what a place may do, one function name guard, no casts, fewer queries when saving roles, form parsing in the server actions). Kept the hand written check lists in the schema, like the rest of the repo. 183 tests pass
- Architecture review of Foundation (`/improve-codebase-architecture`): 7 candidates. Built 3 of them:
  - One shared module for master data records, 5 copies of the same logic removed
  - Reads now check permissions themselves, like changes, for master data and Settings. Rule added to the code repo's `CLAUDE.md`
  - Vendors and bank accounts are written in one place
- Strict review of that work fixed 3 findings: the permission check came after the field checks in two places, the two download routes repeated one guard, an unused type. 168 tests pass
- Left for later from the review: history labels per record, one login module, split user admin, merge the small option files
- Merged the slice 6 to 8 branch into `main` in both repos. CI green
- Grilling session on slice 9, the Excel import and the colleague's repo. Slice 9 planned, then skipped for now (Jingga)
- Read Pandu's real supplier list. It drove fax, several PIC emails and the bank account split
- Built slice 10 in two commits: 10a fax, several PIC emails, bank accounts table with numbers kept out of the audit log; 10b the vendor import with template, preview and all or nothing save
- Fixed while clicking through: long form dialogs now scroll, so Add vendor's button is reachable on a laptop
- Strict code review, 3 findings fixed. 163 tests pass, CI green
- Docker needed restarting twice after the cloud worker restarted: remove the stale containerd pid and socket files, then start dockerd

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
- Jingga confirmed history is visible to everyone who can see the item. Moved the Users dialogs onto the shared form dialog, so a refused Add User or Manage keeps what was typed
- Slice 7 plan approved after comparing with the colleague's customers: system numbered codes (C-0001), one address and one contact, at most one customer per group company, PO first by default. Who manages customers goes to the supervisor
- Built slice 7 in two commits, each clicked through in a browser: 7a customers list and Add customer, 7b customer page with Edit, Deactivate, Reactivate and History
- Strict code review on slice 7: shared list and record page layouts and one list filter, so vendors reuse them. 111 tests pass, lint, typecheck and build pass
- Supervisor answers came in: item and unit codes fixed, Excel import and export needed, who manages items and customers needs departments (later), new users get an automatic NRP with phone, KTP number and KTP scan
- Built slice 5d (the user revisions) before vendors, at Jingga's request: company codes, automatic NRPs, personal details kept apart from the login account, KTP scan upload. Only people with `users.personal_data` see them, every scan view is logged
- Strict code review on 5d, 4 findings fixed. 122 tests pass, lint, typecheck and build pass, migrations checked on a fresh database
- HR's employee sheet reviewed (columns only kept): name, NRP, NIK, phone and company at account creation, the rest later by HR. KTP scan switched off, only the NIK
- Employee ID renamed NRP everywhere (code and vault)
- Built slice 5e: typed or automatic NRP, optional email for staff and viewers (required for admins and managers), 24 hour temporary passwords with a choose your password page, Reset Password in Manage, 15 minute lock after 5 wrong passwords. All logged in the audit log
- Strict code review on 5e, 4 findings fixed. 136 tests pass, lint, typecheck and build pass, migrations checked on a fresh database
- Passwords now need 10 characters, a login lasts one work day (9 hours, not renewed)
- Tidied the vault: the 2 super users belong to the holding
- Pandu's supplier list reviewed (columns only kept): added category, payment term, NPWP, website and notes to the vendor plan
- Built slice 8, vendors: codes V-0001, currency, category, payment term, one per group company. Customers and vendors now share their common fields
- Strict code review on 8, 3 findings fixed. 146 tests pass, lint, typecheck and build pass, migrations checked on a fresh database
- Contact person renamed PIC on customers and vendors. Payment terms stay per vendor; a shared list (like the colleague's) is a supervisor question in [[Procurement Module]]
- All of this session's work is on `claude/vault-slice-6-9vxzpp` in both repos, not merged into `main` yet, so CI hasn't run on it
- Next: merge into `main`, then slice 9, real time (plan first), or the Excel import for Pandu's real suppliers

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
  - 3a login and logout with Better Auth, email or NRP, placeholder Super Admin and Super User, every page needs a login
  - 3b forgot password, same answer whether or not the account exists, one hour one time link, Mailpit in docker compose
  - 3c two-step login for the holding accounts with QR code and 10 backup codes
- Found and fixed a bug while testing 3c: the backup codes were never shown, because the setup page reloaded after the code was accepted. They now show before the last step
- 30 tests pass, CI green on 3a and 3b
- New rule: the strict code review runs after every slice. Ran it on slice 3 and fixed all 5 findings: one shared error handler instead of six copies, one place that turns an email or NRP into an account, one password length rule, a shared page frame, and each action next to its page. 37 tests pass, all three login flows re-checked in a browser
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
