---
title: Quality Module
type: module-spec
project: "[[Holding ERP]]"
status: stub
created: 2026-10-13
tags: [internship, erp, module, quality]
---

# Quality Module

Stub. Only the general idea is here, and it is subject to change. The full spec comes later, from the Module Spec template, when its turn comes (Phase 6 in [[Pandu Roadmap]]). Part of [[Holding ERP]]. All modules: [[Module Overview]].

Named in the module list. No general idea yet. Pandu's QC per batch and the QC step in the S&M return loop probably land here *(assumed)*.

## What Production needs, and now has (2026-10-24)
Quality as a module is still a stub. But Production cannot wait for it, so the QC readings Production needs were built inside Production (the revamp, P10). See [[Production Module#Production revamp (P5 to P11, started 2026-10-24)]].

- **Tests per item, with limits.** Not fixed columns, so a new test is a row and not a migration. The base liquid carries **pH, TDS and Bau**; a soap variant carries **pH, Warna and Busa**. The limits are copied onto each reading, so a later change to a test does not rewrite the past
- **A reading is flagged, not blocked.** A reading outside its limits is shown as out of range and the shift still finishes. What happens to a batch that fails is still open, below
- **The tests are set by the plant.** Who may set them, and when a batch is held for Quality, are the questions below

This is deliberately only what Production needs. Quarantine, release, rework, batch recall and the rest stay open until this module is specced.

## General idea
- Nothing yet

## Pandu baseline
- What Pandu does here today:
- What Wening changes (batches, expiry, formulas):

## Colleague's design
How the colleague's repo builds this, for reference only: [[Colleague Architecture#Quality]].

## Open questions
- [ ] What does Quality cover?
- [ ] Which parts does Pandu need first?
- [ ] Incoming QC on every GR, or only for some item classes (the colleague: raw material, packaging, finished goods)?
- [ ] A blocked batch: rework, write off, or something else? The colleague's repo leaves it stuck in quarantine
- [ ] Batch recall: trace a batch forward to customers and back to material batches?
- [ ] Who releases a batch: the Quality Manager, the GM, or both?
