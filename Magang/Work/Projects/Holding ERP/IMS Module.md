---
title: IMS Module
type: module-spec
project: "[[Holding ERP]]"
status: stub
created: 2026-10-13
tags: [internship, erp, module, ims]
---

# IMS Module

Stub. Only the general idea is here, and it is subject to change. The full spec comes later, from the Module Spec template, when its turn comes (Phase 4 in [[Pandu Roadmap]]). Part of [[Holding ERP]]. All modules: [[Module Overview]].

Inventory Management System.

## General idea
- Production Planning and Inventory Control (PPIC) *, shared with [[Production Module]]
- Inventory Control and Analyze for Production
- Inventory Control and Analyze for Sales

## Pandu baseline
- What Pandu does here today:
- What Wening changes (batches, expiry, formulas):

## Colleague's design
How the colleague's repo builds this, for reference only: [[Colleague Architecture#IMS]].

## Open questions
- [ ] PPIC is shared with [[Production Module]]. Which module owns the records?
- [ ] Which parts does Pandu need first?
- [ ] Costing: one moving average per item for the whole company (colleague), or per location? Built as the colleague's on branch `claude/production` (stock core, see [[Production Module]]) *(assumed)*
- [ ] When a finished good runs low, should it suggest production instead of a PR? The colleague's repo always makes a PR
- [ ] Slow moving and ABC: count production use too, not only sales?
- [ ] Who approves a stock count difference? The colleague sends every difference to the Super Admin
- [ ] Batches without expiry: issue by arrival date (FIFO)? The colleague's repo goes by batch number. Built: earliest expiry first, then the oldest batch *(assumed)*
