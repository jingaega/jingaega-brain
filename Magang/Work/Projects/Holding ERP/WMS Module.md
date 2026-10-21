---
title: WMS Module
type: module-spec
project: "[[Holding ERP]]"
status: stub
created: 2026-10-13
tags: [internship, erp, module, wms]
---

# WMS Module

Stub. Only the general idea is here, and it is subject to change. The full spec comes later, from the Module Spec template, when its turn comes (Phase 4 in [[Pandu Roadmap]]). Part of [[Holding ERP]]. All modules: [[Module Overview]].

Warehouse Management System, for both production and sales.

## General idea
- Receiving
- Binning (putting received goods into storage locations)
- Outgoing
- Logistic and Distribution
- Hub Management
- Trucking Management System

## Pandu baseline
- What Pandu does here today:
- What Wening changes (batches, expiry, formulas):

## Notes from the Inbox
From Jingga's Inbox, processed 2026-10-14.
- After a goods receipt (GR), goods are scanned by QR code or barcode. Which labels exist today, and who prints them, is open

## Colleague's design
How the colleague's repo builds this, for reference only: [[Colleague Architecture#WMS]].

## Open questions
- [ ] Does WMS have one set of records for both production and sales?
- [ ] Which parts does Pandu need first?
- [ ] Is receiving (GR) in WMS or in Procurement? The colleague keeps it in Procurement
- [ ] Picking order: FEFO, FIFO by arrival, or the colleague's largest bin first?
- [ ] Is a pick and check step required before a DO leaves, or optional like the colleague's repo?
- [ ] Trip costs (fuel, toll): posted to the books, or only recorded like the colleague's repo?
