---
title: Module Overview
type: reference
project: "[[Holding ERP]]"
status: draft
created: 2026-10-13
tags: [internship, erp, modules, overview]
---

# Module Overview

Every module has a linked spec note. [[Foundation Module]], Sales & Marketing and Procurement have real specs or notes. The rest are stubs holding only the general idea, from Jingga's first list, **subject to change**. A stub becomes a full spec, from the Module Spec template, when its turn comes in [[Pandu Roadmap]]. Part of [[Holding ERP]].

## At a glance
| Module                 | Main parts                                                               | Spec                           |
| ---------------------- | ------------------------------------------------------------------------ | ------------------------------ |
| Sales & Marketing      | Quote, PO cust, SO, DO / BAST, returns with QC and rework. Tempo flow first, then CBD and Kopken. POS, CRM and website leads later | [[Sales and Marketing Module]] |
| Procurement            | PR, sourcing, PO, GR, IR, payment, vendor management. Down payment and COD flows, partial receipts, auto-posted journals | [[Procurement Module]]         |
| Finance Accounting Tax | Cost control, tax, cost structure, salary structure                      | [[Finance Accounting Tax Module]]             |
| HR                     | HRIS: employees, contracts, attendance, overtime, payroll, reimbursement | [[HR Module]]             |
| GA                     | Assets, facilities                                                       | [[GA Module]]             |
| Production             | Master data, planning, machines, maintenance, report, cost               | [[Production Module]]             |
| WMS                    | Receiving, binning, outgoing, logistics, hub, trucking                   | [[WMS Module]]             |
| IMS                    | Planning and inventory control, inventory analysis                       | [[IMS Module]]             |
| Integrated System      | All modules working together                                             | [[Integrated System Module]]             |
| RND                    |                                                                          | [[RND Module]]                     |
| Engineering            |                                                                          | [[Engineering Module]]                     |
| Quality                |                                                                          | [[Quality Module]]                     |
| MMS                    |                                                                          | [[MMS Module]]                     |

FAHCGA is the group name for Finance Accounting Tax, HR and GA. Finance Accounting Tax is called Finance A&T in other notes.

Each module's general idea lives in its own stub note, linked in the table above.

How the colleague's repo builds each of these, for reference only: [[Colleague Architecture]].

## Overlaps to settle
- **Production Cost** is listed in both Finance Accounting Tax and Production. Which module owns it?
- **PPIC** is marked * in both Production and IMS, so it is shared. Which module owns the records?
- **WMS** serves both production and sales. Does it have one set of records for both?

## Open questions
- [ ] What does "Integrated System" mean: a shared data layer, cross-module dashboards, the holding view, or something else?
- [ ] What do RND, Engineering, Quality and MMS cover?
- [ ] Which parts does Pandu need first, since Pandu is the baseline? (Wening needs formulas, batches, expiry and QC, see [[Company Roles and Modules]])
- [ ] Do the other companies use the same parts, or only some?
