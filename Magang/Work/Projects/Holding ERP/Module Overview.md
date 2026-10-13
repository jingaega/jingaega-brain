---
title: Module Overview
type: reference
project: "[[Holding ERP]]"
status: draft
created: 2026-10-13
tags: [internship, erp, modules, overview]
---

# Module Overview

The general idea of every module other than Foundation, Sales & Marketing and Procurement, which have their own notes. This is a first list from Jingga and **subject to change**. Nothing here is a spec yet. Each module gets one with the Module Spec template when its turn comes in [[Pandu Roadmap]]. Part of [[Holding ERP]].

## At a glance
| Module                 | Main parts                                                               | Spec                           |
| ---------------------- | ------------------------------------------------------------------------ | ------------------------------ |
| Sales & Marketing      |                                                                          | [[Sales and Marketing Module]] |
| Procurement            |                                                                          | [[Procurement Module]]         |
| Finance Accounting Tax | Cost control, tax, cost structure, salary structure                      | General idea below             |
| HR                     | HRIS: employees, contracts, attendance, overtime, payroll, reimbursement | General idea below             |
| GA                     | Assets, facilities                                                       | General idea below             |
| Production             | Master data, planning, machines, maintenance, report, cost               | General idea below             |
| WMS                    | Receiving, binning, outgoing, logistics, hub, trucking                   | General idea below             |
| IMS                    | Planning and inventory control, inventory analysis                       | General idea below             |
| Integrated System      | All modules working together                                             | General idea below             |
| RND                    |                                                                          | Named only                     |
| Engineering            |                                                                          | Named only                     |
| Quality                |                                                                          | Named only                     |
| MMS                    |                                                                          | Named only                     |

FAHCGA is the group name for Finance Accounting Tax, HR and GA. Finance Accounting Tax is called Finance A&T in other notes.

## Finance Accounting Tax
- Cost Control and Analyze
- Tax Analyze
- Marketing Cost
- Production Cost
- Distribution Cost
- Cost Structure
- Salary Structure

## HR
Should follow HRIS (human resource information system).
- Database Karyawan (employee database)
- Contract Management
- Absensi (attendance)
- Lembur (overtime)
- Penggajian (payroll)
- Reimbursement dan Tunjangan (reimbursement and allowances)

HR owns employees and the Employee ID. User accounts in [[Foundation Module]] link to employees, not the other way round.

## GA
- Asset Management
- Facility Management

## Production
- Master Data (raw material, machine, and so on)
- Production Planning and Inventory Control (PPIC) *, shared with IMS
- Machine Monitoring System
- Maintenance Management System
- Production Report
- Production Cost

## Warehouse Management System (WMS)
For both production and sales.
- Receiving
- Binning (putting received goods into storage locations)
- Outgoing
- Logistic and Distribution
- Hub Management
- Trucking Management System

## Inventory Management System (IMS)
- Production Planning and Inventory Control (PPIC) *, shared with Production
- Inventory Control and Analyze for Production
- Inventory Control and Analyze for Sales

## Integrated System
All modules working as one system.

## RND, Engineering, Quality, MMS
Named in the module list. No general idea yet.

Quality is probably where Pandu's QC per batch and the QC step in the S&M return loop land *(assumed)*.

## Overlaps to settle
- **Production Cost** is listed in both Finance Accounting Tax and Production. Which module owns it?
- **PPIC** is marked * in both Production and IMS, so it is shared. Which module owns the records?
- **WMS** serves both production and sales. Does it have one set of records for both?

## Open questions
- [ ] What does "Integrated System" mean: a shared data layer, cross-module dashboards, the holding view, or something else?
- [ ] What do RND, Engineering, Quality and MMS cover?
- [ ] Which parts does Pandu need first, since Pandu is the baseline? (Wening needs formulas, batches, expiry and QC, see [[Company Roles and Modules]])
- [ ] Do the other companies use the same parts, or only some?
