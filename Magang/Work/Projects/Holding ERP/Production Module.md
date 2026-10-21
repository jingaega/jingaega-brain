---
title: Production Module
type: module-spec
project: "[[Holding ERP]]"
status: stub
created: 2026-10-13
tags: [internship, erp, module, production]
---

# Production Module

Stub. Only the general idea is here, and it is subject to change. The full spec comes later, from the Module Spec template, when its turn comes (Phase 6 in [[Pandu Roadmap]]). Part of [[Holding ERP]]. All modules: [[Module Overview]].

Wening needs formulas (BOM), batch and expiry tracking, and QC per batch, see [[Company Roles and Modules]].

## General idea
- Master Data (raw material, machine, and so on)
- Production Planning and Inventory Control (PPIC) *, shared with [[IMS Module]]
- Machine Monitoring System
- Maintenance Management System
- Production Report
- Production Cost

## Pandu baseline
- What Pandu does here today:
- What Wening changes (batches, expiry, formulas):

## Colleague's design
How the colleague's repo builds this, for reference only: [[Colleague Architecture#Production]].

## Open questions
- [ ] Production Cost is also listed in [[Finance Accounting Tax Module]]. Which module owns it?
- [ ] PPIC is shared with [[IMS Module]]. Which module owns the records?
- [ ] Which parts does Pandu need first?
- [ ] MMS is also its own module in [[Module Overview]]. Do machines and maintenance belong here or in [[MMS Module]] and [[Engineering Module]] like the colleague's repo?
- [ ] Does a finished batch carry DL and FOH, or materials only like the colleague's repo?
- [ ] Production orders: need approval? Allow partial finish, scrap, and return of unused materials?
- [ ] Planning: does Pandu need a schedule or MRP, or only a restock list like the colleague's repo?
