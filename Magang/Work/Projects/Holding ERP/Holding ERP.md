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
| 2026-10-13 | S&M builds the tempo (non-CBD) flow first. CBD and Kopken come later | Tempo is the default flow and the best specced one |
| 2026-10-13 | Phase 2 goal is a Pandu tempo order from quote to signed BAST, not to paid | Stock (Phase 4) and Finance (Phase 5) are built later, so stock check and invoice are only simple steps until then |

## Open questions
- What does the holding role of Serio need in the ERP (group view, who sees what, the 2 super users)?
- Which modules beyond Sales & Marketing and FAHCGA are actually in scope?
- Where does the ERP run later (Pandu server or cloud), and do we need a staging copy? Local first for now
- How do we relate to the colleague's repo `Difsyy/Serio-Intern`? It builds the same ERP and is much further along. Contribute to it, or build our own? See [[Colleague Repo Comparison]]
- Maraloka is called "Sibama" in the colleague's repo. Which short name is right?

## Tasks
- [ ] Confirm the items on the [[Company Roles and Modules]] checklist
- [ ] Update the deck to say Pandu is a soap manufacturer (Wening)
- [ ] Write the FAHCGA module spec (use the Module Spec template)

## Links and resources
- ERP repo: `jingaega/PT-SERIO-ERP` on GitHub
- Vault repo: `jingaega/jingaega-brain` on GitHub, folder `Magang/Work`
- Colleague's ERP repo: `Difsyy/Serio-Intern` on GitHub (see [[Colleague Repo Comparison]])
