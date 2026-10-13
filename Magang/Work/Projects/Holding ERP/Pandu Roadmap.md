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
Not set yet. Add it here once known: **TBD**. Until then, phases are ordered by dependency, not by date.

## Phases
| Phase | Outcome | Modules | Status |
|---|---|---|---|
| 0. Decide | Stack, deadline, repo and environment are chosen | none | Not started |
| 1. Foundation | Login, companies with color themes, 2 super users, roles and authorization, master data (items, customers, vendors) | core | Not started |
| 2. Sell | A Pandu order goes from quote to paid | [[Sales and Marketing Module]] | Spec drafted |
| 3. Buy | A Pandu purchase goes from request to paid | [[Procurement Module]] | Notes captured |
| 4. Stock | Stock is correct after every sale, purchase and return | IMS, WMS | Not started |
| 5. Money | Finance posts journals automatically from phases 2 to 4 | Finance A&T | Not started |
| 6. Make | Wening production with formulas, batches, expiry and QC | Production, Quality | Not started |
| 7. People and places | Staff, payroll, facilities | HR, GA | Not started |
| 8. Rest | Remaining modules for Pandu | RND, Engineering, MMS | Not started |
| 9. Extend | Serio (holding) and the other four companies come online, intercompany flows and the holding view switched on | all | Not started |

Why this order: Sell and Buy are already the best specced. Stock sits right after them because both depend on it. Finance comes after the flows it posts from, so its posting rules are known. Foundation comes before everything because even a quote needs a customer and an item.

## Module status (12 modules)
| Module | Spec | Build | Notes |
|---|---|---|---|
| Sales & Marketing | Draft, 8 open questions | Not started | First module |
| Procurement | Notes captured | Not started | Covers vendor management too |
| IMS | None | Not started | |
| WMS | None | Not started | |
| Finance A&T | None | Not started | Auto-posts journals, reversal journals for corrections |
| Production | None | Not started | BOM, batch, expiry, QC |
| Quality | None | Not started | |
| HR | None | Not started | |
| GA | None | Not started | |
| RND | None | Not started | |
| Engineering | None | Not started | |
| MMS | None | Not started | |

## Now
**Phase 0 and Phase 1 prep, with the S&M spec in parallel.**
- [ ] Choose the stack (proposal below)
- [ ] Set a deadline
- [ ] Create the repo and add `CLAUDE.md` from [[Claude Code Instructions]]
- [ ] Close the S&M open questions that block Pandu (see below)
- [ ] Write the S&M build plan

## Stack proposal
**Go (Fiber) + Next.js + PostgreSQL.** Jingga has used this stack before on a full-stack POS project, and it suits a real-time, multi-company system. One database with a `company_id` on every record, plus a per-company theme. Decision is pending, so it is not final until it is logged in [[Holding ERP]].

## S&M questions that block Pandu first
These decide how Pandu's flow behaves, so answer them before the build plan:
- [ ] Does CBD start at the quote or at the RFQ?
- [ ] Credit gate for tempo customers before the SO?
- [ ] Floor price per item, or decided each time?
- [ ] Does a lost deal count as Closed?

The other four (rework limit, check-stock difference, down payment type, Kopken revenue timing) can wait, because Kopken belongs to Sancang and not to Pandu's first release.

## Pandu baseline checklist
Things every module spec must answer for Pandu before building:
- [ ] What does Pandu do in this module today, on paper or in Excel?
- [ ] What does Wening change about it (batches, expiry, formulas)?
- [ ] Which records cross into Anggoan (the intercompany PO to SO)?
- [ ] What does the holding view exclude?
