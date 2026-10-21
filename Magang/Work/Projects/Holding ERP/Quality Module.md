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
