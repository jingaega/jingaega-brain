---
title: Procurement Module
type: module-spec
project: "[[Holding ERP]]"
status: notes captured
created: 2026-10-13
tags: [internship, erp, module, procurement]
---

# Procurement Module

Captured from earlier whiteboard sessions. Not a full spec yet. Turn it into one with the Module Spec template when its turn comes in [[Pandu Roadmap]]. Part of [[Holding ERP]].

## Scope
- Covers procurement and vendor management
- Goal: every purchase is recorded in the system, so manual nota reports are no longer needed
- Each step auto-posts accounting journals. Corrections use authorized reversal journals that reverse the flow

## Standard flow
PR, Sourcing, PO. If approved: GR, IR, Payment, Closed. If not approved: back to Sourcing.

## COD version
Wanted: a cash on delivery version of the flow. Details not captured yet.

## Down payment (uang muka) flow
PO number, uang muka, payment of uang muka, belanja (the buyer shops after the advance is paid), update PO, GR, IR, settlement, Closed.

## PO status tracking with partial receipt
- PO filled in, then GR, IR, Payment
- GR and IR only
- GR before the desired quantity is reached
- GR for the remaining quantity

## Open questions
- [x] A Manager may correct a goods receipt. Every reversal is done by a Manager. Jingga, 2026-10-14
- [x] Approvals above Rp 100 jt: the Super Admin approves first, then a Super User (a director) gives the final approval. Jingga, 2026-10-14
- [ ] Approval amounts up to Rp 100 jt: who approves which band? The colleague's repo: up to 5 jt Manager, up to 25 jt GM, up to 100 jt Super Admin
- [x] Currencies: purchases can be in IDR, CNY, USD, SGD, AUD, EUR or JPY. Jingga, 2026-10-13
- [x] Exchange rate: the rate normally used, Bank Indonesia's middle rate (kurs tengah BI) *(assumed reading of "the normally used one")*. The rate is locked when the purchase is paid and never changes after that. Jingga, 2026-10-13
- [x] Each vendor has its own currency, and its purchases use it. Jingga, 2026-10-13
- [ ] Before payment, purchases show in IDR at which rate: today's BI rate, or the rate on the PO date? Only the paid rate is fixed so far
- [ ] COD flow: where does payment happen, and does it skip IR?
- [ ] Adding a vendor while sourcing: the same vendor record and rules, with an Add vendor shortcut on the sourcing screen (Jingga expects this, 2026-10-13). Who may do it there: anyone who sources, or only managers? The colleague's repo lets staff add vendors, also one-time online sellers
- [x] Payment terms stay free per vendor (CBD, or Net with any number of days), up to Net 180. Purchases follow the vendor's own terms (vendor SOP). Meeting [[2026-10-14]]. Built 2026-10-14: the form and the database allow Net 1 to 180
- [ ] Approval rules: who approves, and above what amount?
- [ ] What does Pandu buy, and does each need a different path? From the supplier list: chemicals, packaging and printing (vendor categories since slice 8)
- [ ] Which journals post at which step?
