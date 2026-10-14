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
| 1. Foundation | Login, companies with color themes, 2 super users, roles and authorization, master data (items, customers, vendors) | [[Foundation Module]] | Building, slices 1 to 8 and 10 done, slice 9 skipped for now |
| 2. Sell | A Pandu tempo order goes from quote to signed BAST | [[Sales and Marketing Module]] | Spec drafted |
| 3. Buy | A Pandu purchase goes from request to paid | [[Procurement Module]] | Notes captured |
| 4. Stock | Stock is correct after every sale, purchase and return | IMS, WMS | Not started |
| 5. Money | Finance posts journals automatically from phases 2 to 4 | Finance A&T | Not started |
| 6. Make | Wening production with formulas, batches, expiry and QC | Production, Quality | Not started |
| 7. People and places | Staff, payroll, facilities | HR, GA | Not started |
| 8. Rest | Remaining modules for Pandu | RND, Engineering, MMS | Not started |
| 9. Extend | Serio (holding) and the other four companies come online, intercompany flows and the holding view switched on | all | Not started |

Why this order: Sell and Buy are already the best specced. Stock sits right after them because both depend on it. Finance comes after the flows it posts from, so its posting rules are known. Foundation comes before everything because even a quote needs a customer and an item.

> [!note] Phase 2 before Stock and Money
> Sell is built before IMS/WMS and Finance exist. Until then, check stock is a manual "stock confirmed" step on the SO, and a signed BAST only sets the order to "handed to FAT". Real stock and the invoice and payment are wired in at Phases 4 and 5, which is when "quote to paid" works.

## Module status (12 modules)
| Module | Spec | Build | Notes |
|---|---|---|---|
| Foundation | Draft, open questions waiting | Slices 1 to 8 and 10 done, 9 skipped for now | Comes before the 12, not counted in them |
| Sales & Marketing | Draft, open questions waiting | Not started | First module. Tempo flow first |
| Procurement | Notes captured | Not started | Covers vendor management too |
| IMS | [[IMS Module]] (stub, general idea) | Not started | |
| WMS | [[WMS Module]] (stub, general idea) | Not started | |
| Finance A&T | [[Finance Accounting Tax Module]] (stub, general idea) | Not started | Auto-posts journals, reversal journals for corrections |
| Production | [[Production Module]] (stub, general idea) | Not started | BOM, batch, expiry, QC |
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
- [ ] Slice 9: live master data lists (plan approved, skipped for now, ask supervisor)
- [x] Slice 10: Excel import of vendors, with Pandu's supplier list
- [ ] Close the S&M open questions that block Pandu (see below)
- [ ] Write the S&M build plan

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
