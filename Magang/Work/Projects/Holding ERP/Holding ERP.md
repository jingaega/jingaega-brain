---
title: Holding ERP
type: project
status: active
created: 2026-10-05
updated: 2026-10-13
tags: [internship, erp, project]
---

# Holding ERP

Main project note for the ERP. Part of [[00 Start Here]]. Who does what is in [[Company Roles and Modules]].

## What it is
An ERP that connects the holding with its six companies. Scope also includes building or tidying each company's website (see [[Company Websites]]).

## Scope
- [[PT Serio Prima Indotek|Serio]] is the holding company. PT Pandu is the development baseline: build for Pandu first, then adapt Serio and the others
- Simple but real-time
- One ERP for all companies, each company with its own color theme
- Two super users: Super Admin (can change anything in every company) and Super User (can see every company, change nothing)
- The ERP belongs to [[PT Pandu Dwiloka Teknik]] and uses all modules. The other companies access it through authorization
- Company websites connect to the ERP, so website contacts get recorded in Sales & Marketing

## Companies
| Company | Role in the ERP |
|---|---|
| [[PT Pandu Dwiloka Teknik]] | Baseline company, makes [[Wening]] soap, owns the ERP |
| [[PT Anggoan Karya Narasena]] | Main dealer for Wening, needs marketing, CRM and POS |
| [[PT Ekatama Tiara Advisindo]] | Consultant, uses Finance & Accounting, invoices every company |
| [[PT Serio Prima Indotek]] | Holding company. Valve maker and machining *(assumed)* |
| [[PT Siar Bagja Maraloka]] | Spare parts trader *(assumed)* |
| [[PT Sancang Anugerah Nusantara]] | Maintenance services, AC and HVAC |

## Modules
The full list is 12 modules. Order of work and status are in [[Pandu Roadmap]]. Process is in [[Workflow]].

| Module | Spec | Status |
|---|---|---|
| Foundation (login, companies, users, roles, master data). Not one of the 12, it comes first | [[Foundation Module]] | Draft |
| Sales & Marketing (POS, CRM, inventory elements, maybe supply chain) | [[Sales and Marketing Module]] | Draft |
| FAHCGA is the group name for Finance A&T, HR and GA | See below | |
| Procurement (includes vendor management) | [[Procurement Module]] | Notes captured |
| Production, WMS, IMS, Finance A&T, HR, GA, RND, Engineering, Quality, MMS | Not started | |

## Intercompany rules
> [!note] To discuss later
> Not in the first build. The rules below are what we were told, but how intercompany works in the ERP gets discussed later.

- A PO from one company becomes an SO at the supplying company automatically
- Intercompany sales are left out of the holding view. Only sales to outside customers count as group revenue

## Decisions
| Date | Decision | Why |
|------|----------|-----|
| 2026-10-05 | One ERP for all companies, own color theme each | Simple, real-time, easier to maintain |
| 2026-10-13 | Serio is the holding company, but PT Pandu is the development baseline (corrects an earlier note saying there was no head holding) | Pandu owns the ERP and uses every module, so everything else is adapted from it |
| 2026-10-13 | Stack is Go (Fiber), Next.js, PostgreSQL. Can still change. Smaller picks: sqlc with pgx for queries, goose for migrations, session cookie login, SSE for real-time | Jingga has used this stack before on a POS project, and it fits a real-time, multi-company system |
| 2026-10-13 | Deadline is flexible, no fixed date | Phases are ordered by dependency in [[Pandu Roadmap]], not by date |
| 2026-10-13 | ERP repo is `jingaega/PT-SERIO-ERP` on GitHub | The name follows the holding. The ERP is still built for Pandu first |
| 2026-10-13 | UI language is Indonesian | Jingga's call |
| 2026-10-13 | Quotes have item lines (item, qty, price), the total is their sum | So the PO cust can be checked line by line, the margin check works per item, and lines copy into the SO |
| 2026-10-13 | Stack changes to Next.js (TypeScript, full stack) and PostgreSQL, local first. Replaces the Go (Fiber) stack above, and the sqlc, goose and SSE picks with it | Jingga's call after seeing the colleague's repo (Next.js). Plain PostgreSQL running locally first |
| 2026-10-13 | UI language is English. Replaces the Indonesian decision above | Jingga corrected it |
| 2026-10-13 | The 2 super users are Super Admin (read and write in every company) and Super User (read only in every company) | Same split as the colleague's repo, Jingga confirmed |
| 2026-10-13 | Intercompany is discussed later, not in the first build | Jingga's call |
| 2026-10-13 | Sales starts at a quote made by hand. The ERP only records it | Jingga confirmed. The colleague's repo starts at an inquiry inside the ERP, which is wrong for us |
| 2026-10-13 | Floor price becomes a minimum margin check: a quote line below the minimum margin needs manager approval | Jingga's call. Replaces the floor price warning idea |
| 2026-10-13 | Pandu's product is Wening soap | Jingga confirmed. The colleague's repo lists hand soap, dish soap, tire cleaner and glass cleaner instead |
| 2026-10-13 | Use the colleague's 6 role templates for now: company admin, director, manager, supervisor, staff, viewer | Jingga's call, until the real role list exists |
| 2026-10-13 | Build a fresh repo `jingaega/PT-SERIO-ERP` together with the colleague. His repo `Difsyy/Serio-Intern` is reference only | His repo is too big, unreviewed and built on unconfirmed rules, and dropping Supabase for AWS means rewriting its core anyway. Supervisor sign-off pending. Full reasoning in the repo's `docs/adr/0001-fresh-repo.md` |
| 2026-10-13 | Hosting later is plain PostgreSQL on AWS (RDS) with our own login, no Supabase | Jingga's call, standard parts any developer can maintain |
| 2026-10-13 | Database tools: Drizzle ORM with the `pg` driver and drizzle-kit migrations | Claude's pick in Foundation slice 1, mainstream and close to plain SQL. Can change |
| 2026-10-13 | One [[Glossary]] note in the vault holds all terms. The repo's `CLAUDE.md` points to it | Jingga's call in the grilling session, so terms live in one place |
| 2026-10-13 | Phase 0 closed. Login with Better Auth (users and sessions in our own PostgreSQL). Screens with shadcn/ui, UI can still change | Grilling session on Phase 0. A login library is safer than writing our own, shadcn/ui is mainstream and easy to hand over |
| 2026-10-13 | The company owns the AWS account from day one. The code repo moves to a company GitHub organization before go-live | Nothing stays stuck on Jingga's personal accounts after the internship |
| 2026-10-13 | AWS Jakarta region (ap-southeast-3). Local now, staging on AWS once Foundation works, production when Pandu goes live | Personal data (HR, payroll) stays in Indonesia under UU PDP. No AWS spending before Foundation works |
| 2026-10-13 | Login with email or [[Glossary\|Employee ID]]. Use Pandu's existing staff number if there is one | Jingga's call. The fallback format is *(assumed)* |
| 2026-10-13 | Two-step login (authenticator app) for Super Admin and Super User. Normal users have none for now | Both can see every company's data |
| 2026-10-13 | Forgot password sends a reset link by email. Mailpit catches emails locally, AWS SES on staging and production | Works the same everywhere without real email setup on a laptop |
| 2026-10-13 | Push code straight to `main` for now, no pull requests | Jingga's call, until Jingga and the colleague agree how they work together |
| 2026-10-13 | Plan for a general web developer to take over after the internship | Keeps the tools mainstream. Handover details come later |
| 2026-10-13 | Company colors and logos: the 6 logo based themes from the colleague's repo | Approved, per Jingga |
| 2026-10-13 | PT Siar Bagja Maraloka's short name is Sibama | Jingga's call. The company's own logo says sibama |
| 2026-10-13 | Every finished slice is followed by the strict code review (thermo-nuclear skill), and its findings are fixed before the next slice | Jingga's rule. Keeps AI written code from piling up unreviewed |
| 2026-10-13 | Slice 4 plan approved with its defaults: people with one company go straight in, only company admin manages users and roles *(assumed)*. One role per person per company is not settled, so the database allows several | Jingga's call. Exceptions to one role may exist |
| 2026-10-13 | Slice 5 plan approved with its defaults: new users set their own password from an emailed link (3 days), a company admin can give access to someone with an account in another group company, only Super Admin deactivates a whole account, Employee ID optional *(assumed)* | Jingga's call. Employee ID required or not: ask the supervisor |
| 2026-10-13 | Slice 6 plan approved with its defaults. Starter units and the code format come from the colleague's repo. An audit log is added, written by PostgreSQL on every change | Jingga's call. Units and code format are ideas, not business rules, and can change |
| 2026-10-13 | Everyone who can see an item can see its history | Jingga's call |
| 2026-10-13 | Slice 7 plan approved: one address and one contact per customer, the colleague's customer code format, at most one customer per group company, new customers start as PO first. Who manages customers and whether codes can change go to the supervisor | Jingga's call. Order type (PO first or work first) is a customer setting; payment type (tempo, CBD) stays on the quote |
| 2026-10-13 | User admin is not HR. Users and Roles live under Settings; employees belong to the HR module, which will own the Employee ID | Who can sign in is a different job from who works for the company |
| 2026-10-13 | PostgreSQL row level security keeps companies apart. The app connects as a limited database user, the owner is for migrations and the seed only | A bug in the app code can't show another company's data |
| 2026-10-13 | S&M builds the tempo (non-CBD) flow first. CBD and Kopken come later | Tempo is the default flow and the best specced one |
| 2026-10-13 | Phase 2 goal is a Pandu tempo order from quote to signed BAST, not to paid | Stock (Phase 4) and Finance (Phase 5) are built later, so stock check and invoice are only simple steps until then |

## Open questions
- What does the holding role of Serio need in the ERP (group view, who sees what, the 2 super users)?
- Which modules beyond Sales & Marketing and FAHCGA are actually in scope?
- How do Jingga and the colleague work together in the repo (branches, reviews)?
- Handover plan: who maintains the ERP after the internship
- Company GitHub organization for the code repo

## Tasks
- [ ] Confirm the items on the [[Company Roles and Modules]] checklist
- [ ] Update the deck to say Pandu is a soap manufacturer (Wening)
- [ ] Write the FAHCGA module spec (use the Module Spec template)

## Links and resources
- ERP repo: `jingaega/PT-SERIO-ERP` on GitHub. How the code is put together: [[Architecture]]
- Vault repo: `jingaega/jingaega-brain` on GitHub, folder `Magang/Work`
- Colleague's ERP repo: `Difsyy/Serio-Intern` on GitHub (see [[Colleague Repo Comparison]])
