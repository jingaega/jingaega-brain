---
title: Pandu Roadmap
type: roadmap
project: "[[Holding ERP]]"
status: active
updated: 2026-10-13
tags: [internship, erp, roadmap]
---

# Pandu Roadmap

[[PT Serio Prima Indotek|Serio]] is the holding company, but PT Pandu is the development baseline. The goal is a working ERP for [[PT Pandu Dwiloka Teknik]] first, then Serio and the other companies are adapted from it. Process is in [[Workflow]]. Back to [[00 Start Here]].

## Goal
A working, real-time ERP that Pandu uses for its own Wening flow, from the first quote or purchase to the closed and paid order, with finance posting automatically. Every module is built once for Pandu, then extended to the other five companies.

## Deadline
**Flexible, no fixed date.** Phases are ordered by dependency, not by date.

## Phases
| Phase | Outcome | Modules | Status |
|---|---|---|---|
| 0. Decide | Stack, deadline, repo and environment are chosen | none | Done (3 follow ups open in [[Holding ERP]]) |
| 1. Foundation | Login, companies with color themes, 2 super users, roles and authorization, master data (items, customers, vendors) | [[Foundation Module]] | Built (slices 1 to 8, 10 and 11: levels and departments). Live lists moved to [[#Additional features]] |
| 2. Sell | A Pandu CBD, tempo or progress order goes from quote to signed BAST | [[Sales and Marketing Module]] | Done: S0 to S9 built (all planned slices), costing rules R1 to R5 changed. Supervisor questions open |
| 3. Buy | A Pandu purchase goes from request to paid | [[Procurement Module]] | Notes captured |
| 4. Stock | Stock is correct after every sale, purchase and return | IMS, WMS | Not started. Comes right after Finance (Jingga, 2026-10-22) |
| 5. Money | Finance posts journals automatically from phases 2 to 4 | [[Finance Accounting Tax Module]] | **Next, built before 3 and 4** (Jingga, 2026-10-22). Whole module planned and approved (F0 to F16). AP, COGS and payroll parts come with their modules |
| 6. Make | Wening production with formulas, batches, expiry and QC | Production, Quality | Not started |
| 7. People and places | Staff, payroll, facilities | HR, GA | Not started |
| 8. Rest | Remaining modules for Pandu | RND, Engineering, MMS | Not started |
| 9. Extend | Serio (holding) and the other four companies come online, intercompany flows and the holding view switched on | all | Not started |

Order changed 2026-10-22 (Jingga): after Sell comes Money (Finance), then Stock, then Buy. Finance first finishes quote to paid for sales; its posting engine is ready for Stock and Procurement when they come. Original reasoning: Sell and Buy are already the best specced. Stock sits right after them because both depend on it. Finance comes after the flows it posts from, so its posting rules are known. Foundation comes before everything because even a quote needs a customer and an item.

> [!note] Phase 2 before Stock and Money
> Sell is built before IMS/WMS and Finance exist. Until then, check stock is a manual "stock confirmed" step on the SO, and a signed BAST only sets the order to "handed to FAT". Real stock and the invoice and payment are wired in at Phases 4 and 5, which is when "quote to paid" works.

## Module status (12 modules)
| Module | Spec | Build | Notes |
|---|---|---|---|
| Foundation | Draft, open questions waiting | Built, open questions left. Live lists moved to Additional features | Comes before the 12, not counted in them |
| Sales & Marketing | Build plan approved 2026-10-14, supervisor questions marked | S0, S1 (quotes), S2 (price proposals), S3 (customer PO), S3c (suggested SPPR, SPPR approval), S4 (sales order), S5 (PI and payments), S6 (DO / BAST) S7 (tempo credit check, handed to FAT), S8 (progress payment plan and down payment), S9 (returns, QC and rework, kirim), the costing change R1 to R5 and Marketing as its own module (change M) built. All planned slices done; supervisor questions open | First module. CBD first, then tempo and progress |
| Procurement | Notes captured | Not started | Covers vendor management too |
| IMS | [[IMS Module]] (stub, general idea) | Not started | |
| WMS | [[WMS Module]] (stub, general idea) | Not started | |
| Finance A&T | [[Finance Accounting Tax Module]], whole module plan approved 2026-10-22 | F0 to F16 built, module done (approval engine, setup, journals, manual journals, periods, statements, deposits and refunds, invoices from signed BASTs, receipts and AR aging, credit notes, bank reconciliation, bank and cash entries, statement import, PPN out and Coretax export, actual FOH %, Finance dashboard, marketing cost, fixed assets, year end, cash flow) | Auto-posts journals, reversal journals for corrections. Built before Procurement and Stock |
| Production | [[Production Module]], ported from the colleague's repo | P1 to P4 built on branch `claude/production`, not merged (Jingga decides) | Material prices, BoM per variant, production orders through WIP, reports, and a stock core (warehouses, stock per batch, moving average) so orders move real stock |
| Quality | [[Quality Module]] (stub, name only) | Not started | |
| HR | [[HR Module]] (stub, general idea) | Not started | Owns employees and the NRP. User accounts (Foundation) link to employees, not the other way round |
| GA | [[GA Module]] (stub, general idea) | Not started | |
| RND | [[RND Module]] (stub, name only) | Not started | |
| Engineering | [[Engineering Module]] (stub, name only) | Not started | |
| MMS | [[MMS Module]] (stub, name only) | Not started | |

## Now
**Phase 1 prep, with the S&M spec in parallel.**
- [x] Choose the stack (logged in [[Holding ERP]], can still change)
- [x] Set a deadline (flexible)
- [x] Pick the repo: `jingaega/PT-SERIO-ERP`
- [ ] Answer the Foundation open questions that block slices (see [[Foundation Module]])
- [x] Slice 1: scaffold the repo and add `CLAUDE.md` from [[Claude Code Instructions]]
- [x] Slice 2: companies with themes
- [x] Slice 3: login
- [x] Slice 4: roles and permissions
- [x] Slice 5: user admin
- [x] Slice 6: items and units, with the audit log
- [x] Slice 7: customers
- [x] Slice 8: vendors
- [x] Slice 10: Excel import of vendors, with Pandu's supplier list
- [x] Slice 11: levels and departments, Company Admin switch, Departments screen, master data by owning department
- [x] Architecture review of Foundation, 3 of 7 candidates built (see [[Build Log]])
- [x] Close the S&M open questions that block Pandu (answered by Jingga 2026-10-14, some still to confirm with the supervisor)
- [x] Write the S&M build plan (approved 2026-10-14)
- [x] S&M S0: Foundation changes for S&M
- [x] S&M S1: quotes
- [x] S&M S2: price proposals and approvals
- [x] S&M S3: customer PO
- [x] S&M S3c: suggested SPPR and SPPR approval below the minimum GP
- [x] S&M S2d: a price above the SPPR applies with a reason
- [x] S&M S4: sales order (lower PO price approval, create from the quote, stock confirmed, cancel)
- [x] S&M S5: PI and CBD (proforma invoice with PPN, partial payments recorded by Finance, void, cancel rules, Proforma invoices list)
- [x] S&M S8: progress (payment plan with fixed due dates, down payment PI before delivery, handed to FAT)
- [x] S&M S9: returns (return received, QC rounds, rework count, kirim on a new DO / BAST)
- [x] S&M S7: tempo (credit check with Sales Manager approval, tempo DO / BAST, handed to FAT, To invoice list)
- [x] S&M R4 and R5: SPPR valid from a day with history, sales areas and a customer's area from its city
- [x] S&M R1 to R3: GP 27 / 17 with a refusal, the SPPR floor, who sees cost (from the colleague's branch, confirmed by Jingga)
- [x] S&M S6: DO / BAST (create, deliver, sign with photos or reject, order status, photos removed after a year)
- [ ] Phase 4: the margin check moves from the typed item cost to the moving average cost
- [ ] Phase 4: make S&M check stock automatic (hold stock on the SO, shortage to Prod/Purch), replacing the manual "stock confirmed" step

## Additional features
Not needed to finish a module. Built later, when a screen gains from it or someone asks. Each one keeps its plan where it was written.
- [ ] Live lists: lists update by themselves when someone else changes a record. Plan approved, in [[Foundation Module#Slice 9 plan: real-time lists]]. Ask the supervisor if live screens are wanted
- [ ] Live record pages and a live Users list (same plan, after the lists)
- [ ] "Added by X" on a new or changed row in a live list (Jingga likes it)
- [ ] Marketing: market price survey, competitor and shop prices per item against the SPPR. Open question in [[Sales and Marketing Module#Open questions this plan raises]]
- [ ] Marketing: SPPR with a start date and history, set ahead of time. Same note. First step towards price lists
- [ ] Marketing: price lists per segment and per customer on top of the SPPR, only after the supervisor confirms Pandu prices that way (decision 2026-10-21 in [[Holding ERP]]). The traders need them in Phase 9
- [ ] Marketing: margin and voice of customer reports, and a Marketing home. Same note

## Stack
Decided, see the decisions table in [[Holding ERP]].

## S&M questions that block Pandu first
These decide how Pandu's tempo flow behaves, so answer them before the build plan:
- [ ] Credit gate for tempo customers before the SO?
- [x] Floor price: replaced by a minimum margin check with manager approval (details still open in the spec)
- [ ] Does a lost deal count as Closed?

CBD and Kopken are built later, so their questions (CBD entry point, check-stock difference, down payment type, Kopken revenue timing, rework limit) can wait.

## Pandu baseline checklist
Things every module spec must answer for Pandu before building:
- [ ] What does Pandu do in this module today, on paper or in Excel?
- [ ] What does Wening change about it (batches, expiry, formulas)?
- [ ] Which records cross into Anggoan (intercompany, discussed later)?
- [ ] What does the holding view exclude?
