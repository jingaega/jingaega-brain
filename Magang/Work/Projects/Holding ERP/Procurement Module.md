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

## The group's PO templates
Six real POs shared by Jingga, 2026-10-14 (Serio 2022, Sibama 2023 and 2025, Sancang 2024, Pandu 2025, Anggoan 2025). Only their layout is kept here, never the vendors' data.

**Two layouts**
- **Older** (Serio 2022, Sancang 2024): header No. PO, date, "Your Ref" (the vendor's quote), delivery time. A Vendor box and a Customer box (us), each with address, phone, Attn and email. Lines: no, part number, description, qty, unit, unit price, total. Subtotal, PPN, total. A Terms and Conditions list (payment, delivery term and place, delivery time, packing, origin, documents like COO, COA). The vendor signs and stamps it to acknowledge; Prepared by and Approved by sign for us
- **Newer** (Sibama, Pandu, Anggoan, 2023 on): our company name, address and Tax ID (NPWP) on top. No., date, delivery date, quote ref. "Order to" the vendor with address, phone, Tax ID, Att and phone or email. Lines: no, item code, description (with a sub line like "COA included"), qty, UoM, unit price, amount. Total, discount %, tax 11 %, grand total; imports add freight and bank fee. Notes: term of payment, incoterms (like "Dooring PT Pandu ..." or EXW), HS code, down payment. Signature blocks for us and the vendor. Anggoan's adds a payment term in the header and a "Delivery to" block with a PIC, phone and email

**What they tell us**
- PO numbers today: `PO.007/SERIO/I/2022`, `PO.2407007/SAN/FAR`, `LK-0825-008` (Pandu, local, month and year then a number), `POLK-000040R1` (Sibama, local, revision 1), `POIM-000012` (Sibama, import), `POLK-AKN-0106` (Anggoan, local). So **local and import POs are numbered apart**, and a **revised PO keeps its number with R1, R2**
- POs can be in **IDR, USD or CNY** (matches the currencies decided 2026-10-13)
- Payment terms seen: CBD, TT 100 % in advance, full payment when goods are ready, 14 days after invoice, DP 50 % then the rest after receipt, immediately. They match CBD, Net days and down payment
- Each PO shows the vendor's Tax ID, an incoterm or delivery place, a requested delivery date, and the vendor's quote reference
- PPN is shown on the PO (11 %, 10 % in 2022), not in the unit prices

**Open questions from them**
- [ ] Should the ERP number POs like today (local and import apart, per company), or one new format for every company?
- [ ] Is the PO printed from the ERP in the newer layout, the same for every company with its own header?
- [ ] Who are Prepared by and Approved by on a PO: the buyer and their Manager, or the approval chain (up to Rp 100 jt and above)?

## Colleague's design
How the colleague's repo builds this, for reference only: [[Colleague Architecture#Procurement]].

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
- [ ] Inbox note (2026-10-14): "Procurement has 4 types: cost, expense, inventory," and the fourth is missing. Is it fixed asset? The colleague's repo books a goods receipt to inventory, fixed asset or expense by item type. And what is the difference between cost and expense here?
- [ ] Which journals post at which step?
- [ ] How does a down payment to a vendor work (DP 50 %, TT 100 % in advance)? The colleague's repo only has an employee advance (UM) and pays vendors after receipt
- [ ] Is "update PO" a real PO revision (R1, R2), or only the price fixed on the invoice like the colleague's repo?
- [ ] Invoice price tolerance: the colleague uses 0 % with a Manager deciding on a hold. What tolerance, and who decides?
- [ ] Is over receipt ever allowed? The colleague's repo refuses it
- [ ] Partial payments of one invoice: allowed? The colleague allows them
