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
| Procurement (includes vendor management) | [[Procurement Module]] | Notes captured |
| Finance Accounting Tax (part of FAHCGA, the group name for Finance A&T, HR and GA) | [[Finance Accounting Tax Module]] | Stub |
| HR (FAHCGA) | [[HR Module]] | Stub |
| GA (FAHCGA) | [[GA Module]] | Stub |
| Production | [[Production Module]] | Stub |
| WMS | [[WMS Module]] | Stub |
| IMS | [[IMS Module]] | Stub |
| Integrated System | [[Integrated System Module]] | Stub |
| RND | [[RND Module]] | Stub |
| Engineering | [[Engineering Module]] | Stub |
| Quality | [[Quality Module]] | Stub |
| MMS | [[MMS Module]] | Stub |

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
| 2026-10-13 | Login with email or [[Glossary\|NRP]]. Use Pandu's existing staff number if there is one | Jingga's call. The fallback format is *(assumed)* |
| 2026-10-13 | Two-step login (authenticator app) for Super Admin and Super User. Normal users have none for now | Both can see every company's data |
| 2026-10-13 | Forgot password sends a reset link by email. Mailpit catches emails locally, AWS SES on staging and production | Works the same everywhere without real email setup on a laptop |
| 2026-10-13 | Push code straight to `main` for now, no pull requests | Jingga's call, until Jingga and the colleague agree how they work together |
| 2026-10-13 | Plan for a general web developer to take over after the internship | Keeps the tools mainstream. Handover details come later |
| 2026-10-13 | Company colors and logos: the 6 logo based themes from the colleague's repo | Approved, per Jingga |
| 2026-10-13 | PT Siar Bagja Maraloka's short name is Sibama | Jingga's call. The company's own logo says sibama |
| 2026-10-13 | Every finished slice is followed by the strict code review (thermo-nuclear skill), and its findings are fixed before the next slice | Jingga's rule. Keeps AI written code from piling up unreviewed |
| 2026-10-13 | Slice 4 plan approved with its defaults: people with one company go straight in, only company admin manages users and roles *(assumed)*. One role per person per company is not settled, so the database allows several | Jingga's call. Exceptions to one role may exist |
| 2026-10-13 | Slice 5 plan approved with its defaults: new users set their own password from an emailed link (3 days), a company admin can give access to someone with an account in another group company, only Super Admin deactivates a whole account, NRP optional *(assumed)* | Jingga's call. NRP required or not: ask the supervisor |
| 2026-10-13 | Slice 6 plan approved with its defaults. Starter units and the code format come from the colleague's repo. An audit log is added, written by PostgreSQL on every change | Jingga's call. Units and code format are ideas, not business rules, and can change |
| 2026-10-13 | Everyone who can see an item can see its history | Jingga's call |
| 2026-10-13 | Slice 7 plan approved: one address and one contact per customer, the colleague's customer code format, at most one customer per group company, new customers start as PO first. Who manages customers and whether codes can change go to the supervisor | Jingga's call. Order type (PO first or work first) is a customer setting; payment type (tempo, CBD) stays on the quote |
| 2026-10-13 | Customer codes are numbered by the system per company (C-0001), like the colleague's repo | Jingga's call. No typos or duplicates |
| 2026-10-13 | Currencies: IDR, plus CNY, USD, SGD, AUD, EUR and JPY for procurement | Jingga's call. Some vendors are abroad |
| 2026-10-13 | Exchange rate: the normally used rate (Bank Indonesia middle rate, assumed reading), locked when the purchase is paid and never changed after. Each vendor has its own currency | Jingga's call. Customer currency is still a question for the supervisor |
| 2026-10-13 | Item and unit codes can't change after saving | Jingga's call |
| 2026-10-13 | Items and units are managed by finance managers and above, customers by finance and procurement managers and above | Jingga's call. Needs departments in the role model first |
| 2026-10-13 | New users get an automatic NRP. The person adding them types name, number, KTP and other details | Jingga's call. Changes slice 5, plan needed |
| 2026-10-13 | Excel import and export are needed | Jingga's call |
| 2026-10-13 | No KTP scan, only the NIK. The upload code stays, switched off | Jingga's call. Can come back when needed |
| 2026-10-13 | The staff ID is called NRP. Existing staff keep theirs, new staff get one automatically. Email is optional; without one, the admin hands over a temporary password | Jingga's call. Not every employee has an email |
| 2026-10-13 | Email required for roles that can change important things (company admin, manager), optional for staff and viewers. Temporary passwords last 24 hours, resets are logged, 5 wrong passwords lock an account for 15 minutes | Jingga's call after asking if optional email weakens security |
| 2026-10-13 | Passwords of at least 10 characters. A login lasts one work day (9 hours, not renewed while in use) | Jingga's call. A computer left signed in is signed out by the next morning |
| 2026-10-13 | User admin is not HR. Users and Roles live under Settings; employees belong to the HR module, which will own the NRP | Who can sign in is a different job from who works for the company |
| 2026-10-13 | PostgreSQL row level security keeps companies apart. The app connects as a limited database user, the owner is for migrations and the seed only | A bug in the app code can't show another company's data |
| 2026-10-13 | S&M builds the tempo (non-CBD) flow first. CBD and Kopken come later | Tempo is the default flow and the best specced one |
| 2026-10-13 | Phase 2 goal is a Pandu tempo order from quote to signed BAST, not to paid | Stock (Phase 4) and Finance (Phase 5) are built later, so stock check and invoice are only simple steps until then |
| 2026-10-13 | "Real-time" means data is current the moment it is saved: no end of day batch, finance posts right away *(assumed, asking supervisor)*. Live lists (slice 9) are extra on top | Jingga's reading. Nobody had said what the scope line meant |
| 2026-10-13 | Build slice 9 (live master data lists) now, then slice 10 (vendor Excel import) | Jingga's call after asking whether live updates are needed at all |
| 2026-10-14 | The ERP can run in GitHub Codespaces (`.devcontainer` in the code repo) | It won't run on Jingga's laptop. Same PostgreSQL 16 and mail catcher as local |
| 2026-10-14 | A customer's PO with a payment term looser than the quote's waits for the GM, or a Director. Refused, the quote waits for the customer again | Jingga's S3 plan. Which terms count as looser, and the Director fallback, are *(assumed)*, see [[Sales and Marketing Module]] |
| 2026-10-14 | Live lists (slice 9) become an additional feature, not part of Foundation. Foundation's build is done with slices 1 to 8 and 10 | Not needed to finish Foundation. Built when a screen gains from it, after the supervisor says what real-time means |
| 2026-10-14 | Every sale is in IDR | Meeting [[2026-10-14]] |
| 2026-10-14 | Payment terms: Net up to 180 days. Sales terms are ours to decide (default CBD, also COD, progress payment, Net up to 180). Purchases follow the vendor's own terms, any number of days per vendor | Meeting [[2026-10-14]] |
| 2026-10-14 | Directors approve big transactions only. A limit that makes them approve something every day is the wrong limit | Meeting [[2026-10-14]]. Approval amounts still to set |
| 2026-10-14 | Both holding people are Super Admin | Jingga |
| 2026-10-14 | The directors are Super User, and also have a Director level in their own company | Jingga |
| 2026-10-14 | Company levels: Director, GM, Manager, SPV, Lead, OPR. Super Admin stays for the holding | Jingga |
| 2026-10-14 | The Director level sees everything in its company and changes nothing, apart from approvals. The GM changes everything except Settings | Jingga |
| 2026-10-14 | Super User only views, except approvals above Rp 100 jt: Super Admin approves first, then a Super User gives the final approval | Jingga |
| 2026-10-14 | Om Leo (director, "super finance") approves journals in every company and gives finance people access to journal entries | Jingga |
| 2026-10-14 | A Manager may correct a goods receipt, and every reversal is done by a Manager | Jingga |
| 2026-10-14 | Vendor bank accounts: seen by everyone who can see vendors, changed by people who can manage vendors, like the colleague's repo | Jingga, after looking at the colleague's repo |
| 2026-10-14 | ERP document numbers follow the group's official style, like Pandu's `LK-0825-008`: prefix, month and year, number. Quotes are `SQ-1025-001` | Jingga, after reading the group's PO templates |
| 2026-10-14 | A customer's PO is checked against our quote line by line on prices before PPN; a different PPN or total is only a note | Jingga |
| 2026-10-14 | Quote prices come from the item's SPPR. Sales can't type a price, only propose one with a reason, approved by a Manager or up in Marketing. Sales never sees cost or margin. Pandu has no bottom price | Jingga. Sales gets incentives on price, so they must not see the profit |
| 2026-10-14 | Costing model: COGS = COGM (materials at MAP) + DL + FOH, one FOH rate per unit for the company, GP on the selling price, SPPR set by Marketing with Finance's advice | Jingga, explaining the office whiteboard |
| 2026-10-14 | Finance sees item costs and margins too, every level. Sales never does | Jingga. Finance types the costs |
| 2026-10-14 | Customers and vendors need an address, a PIC name and a PIC phone. The PIC email stays optional, and the Excel import makes the same check | Jingga |
| 2026-10-14 | Every column in the company's Excel sheets (supplier list, customer list, employee sheet) is a field when adding that record | Jingga. Nothing in their files is lost. Example: customer NPWP is kept although only Finance uses it |
| 2026-10-14 | New department functions: Marketing (next to Sales, both in S&M) and Quality (empty until the Quality module) | Jingga. Only Marketing sees margins. QC on returns goes to Quality |
| 2026-10-14 | S&M first build: CBD first, then tempo and progress (down payment). COD and Kopken later, Kopken as an order type | Jingga, after comparing with the colleague's repo. CBD is the default payment type |
| 2026-10-14 | S&M starts at Quote recorded, and the customer PO is its own record, checked against the quote | Jingga, kept over the colleague's repo, where the PO is a field on the SO |
| 2026-10-14 | Check stock is automatic, like the colleague's repo | Jingga. When it can be built depends on stock (Phase 4), open question in [[Sales and Marketing Module]] |
| 2026-10-14 | Credit gate before the SO for tempo customers, like the colleague's repo | Jingga. Details in [[Sales and Marketing Module]] |
| 2026-10-14 | Minimum margin is gross profit on the selling price: 20 % default, 10 % minimum, per company | Formula from the office whiteboard. Numbers first seen in the colleague's repo, where they are a markup on cost. Jingga confirmed 2026-10-21 that ours are GP on the selling price. Supervisor to confirm |
| 2026-10-21 | The 20 % default GP gives a suggested SPPR for each item. Marketing can take it or type its own | Jingga. How it is worked out (SPPR = COGS / (1 - 20 %), rounded up to the whole rupiah) is *(assumed)*. Built in S&M slice S3c |
| 2026-10-21 | Marketing Managers set the default GP, like the minimum. Below the minimum GP: a warning and an approval. A quote price by a Manager or up in Marketing (as built), an SPPR by the GM, or a Director when there is no GM | Jingga. A Marketing Manager sets the SPPR, so someone above approves it |
| 2026-10-21 | Every phase starts with a three way comparison: ours, general ERP practice, the colleague's repo ([[ERP Best Practices]]) | Jingga. Gaps become open questions before the build plan |
| 2026-10-21 | The holding's combined view eliminates sales and debts between group companies and shows them as their own line. Each company's own books stay the same | Jingga. An honest group total with nothing hidden. Built in Phase 9 |
| 2026-10-14 | The GM approves SO term overrides | Jingga |
| 2026-10-14 | Roles move toward the colleague's model: levels (General Manager, Manager, SPV, Lead, OPR) with a department function, Company Admin as a switch | Jingga. Needs its own plan before building |
| 2026-10-13 | Excel import: same checks as the form, add only, warning on similar names, our template and Pandu's own columns both read. Copy how the colleague's import works, not its rules | Grilling session. Keeps one set of rules, and the import never changes existing vendors |
| 2026-10-13 | Vendors keep fax and several bank accounts. Bank accounts are shown nowhere until Procurement, and the audit log never holds the numbers *(assumed)* | Pandu's supplier list has them, so nothing in the file is lost. Bank details need stricter access |
| 2026-10-13 | A PIC may have several emails, on customers and vendors | Two of Pandu's 7 vendors have two emails |

## Open questions
- What does the holding role of Serio need in the ERP (group view, who sees what)? The 2 super users are settled: the holding's admins
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
