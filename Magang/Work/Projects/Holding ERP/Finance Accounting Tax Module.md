---
title: Finance Accounting Tax Module
type: module-spec
project: "[[Holding ERP]]"
status: stub
created: 2026-10-13
tags: [internship, erp, module, finance]
---

# Finance Accounting Tax Module

Stub. Only the general idea is here, and it is subject to change. The full spec comes later, from the Module Spec template, when its turn comes (Phase 5 in [[Pandu Roadmap]]). Part of [[Holding ERP]]. All modules: [[Module Overview]].

Called Finance A&T in other notes. Part of FAHCGA, the group name for Finance Accounting Tax, HR and GA.

## General idea
- Cost Control and Analyze
- Tax Analyze
- Marketing Cost
- Production Cost
- Distribution Cost
- Cost Structure
- Salary Structure

## Pandu baseline
- What Pandu does here today:
- What Wening changes (batches, expiry, formulas):

## Rules from Jingga
- Om Leo (a director, "super finance") approves the journals of every company, and gives finance people access to make journal entries. Jingga, 2026-10-14
- Every reversal (correcting a posted entry) is done by a Manager. Jingga, 2026-10-14

## Costing and pricing
From Jingga, 2026-10-14 (explaining the office whiteboard, [[2026-10-14 Whiteboard costing and pricing]]). Finance owns this model; S&M and Procurement use it. Words are in [[Glossary]].

**The parts of a product's cost**
- **COGM** (cost of goods material, here *not* "manufactured"): all the materials that make the product, like chemical + packaging + looks (sticker, cap and so on). Seen per material ("head to head") or as one sum
- **MAP** (moving average price): the costing method. Each material's price is its moving average, and those prices build the COGM
- **DL** (direct labor): what the people making it cost per month, like operator Rp 5.000.000 and factory SPV Rp 7.000.000
- **FOH** (factory overhead): electricity, water, rent and so on, per month
- **COGS** (cost of goods sold) = COGM + DL + FOH

**Margin and price**
- **GP** (gross profit, the margin) = (selling price - COGS) / selling price
- **SPPR** (selling price publish rate, HET on the whiteboard) = COGS + GP

**Two ways to work it out**
1. **Finance's way** (Finance advises with this one):
   - revenue - COGM = **VA** (value added)
   - VA - DL - FOH = GP
2. **Marketing's way**: DL and FOH are grouped as one standard rate, just called FOH. Then SPPR = COGM + % FOH + % margin. This is the same as SPPR = COGS + GP, broken down

Finance and Marketing work it out together: Finance advises from the numbers, Marketing knows the market.

**Who sees what**
- Sales only knows the SPPR, never cost or margin
- Procurement knows the MAP (material prices)
- **Cost and margin are seen only by the GM, the Super Admin, the Super User and the Finance manager** (Jingga, 2026-10-21, replaces "Marketing and Finance see it"; the code changes in S&M change plan R3). Marketing Manager and Director are open there
- **FOH from the ledger** (Jingga, 2026-10-21): the month's indirect costs from the general ledger, spread over the month's products by sales value, last month's FOH % for pricing. The standard % of COGM below stays until this is built in Phase 5. A COGS adjustment by Finance never goes below the system COGS

**Open questions on this**
- [x] FOH is a rate: whatever DL and FOH cost that month is split over the units you want to look at (per bottle, per liter and so on), giving a rupiah amount per unit. Jingga, 2026-10-14
- [x] In pricing, the FOH rate is a standard **% of COGM**, one for the company, like 15 %: COGS = COGM x 115 %. Finance works it out from the monthly DL and FOH split (per PC, per L, per batch, per month or per machine hour). Second whiteboard, 2026-10-14 *(Claude's reading, ask supervisor)*. Example: COGM Rp 10.000, FOH 15 %, GP 40 %: COGS Rp 11.500, SPPR = 11.500 / 60 % = Rp 19.167
- [x] The margin in Marketing's way is the GP on the price: SPPR = COGS / (1 - GP). Example: COGS Rp 12.000 at 25 % GP gives Rp 16.000. Jingga, 2026-10-14
- [x] One FOH rate for the whole company: the month's total, split per unit. Jingga, 2026-10-14. Finance calculates it (Jingga, 2026-10-14). Until the Finance module (Phase 5) works it out, Finance Managers type the rate per unit *(assumed)*
- [x] One SPPR per item. Dis A Surabaya and Dis B Bandung on the whiteboard are probably about something else. Jingga, 2026-10-14
- [x] Marketing sets the SPPR with Finance's advice. Jingga, 2026-10-14
- [x] Sales can't type a price, only propose one with a reason, approved by a Manager or up in Marketing (Finance not needed). Sales sees only the SPPR, never the COGS or margin. Pandu has no bottom price. Jingga, 2026-10-14

## Notes from the Inbox
From Jingga's Inbox, processed 2026-10-14. Raw notes, not rules yet.
- A recorded payment can't be edited. It must match its invoice. The invoice is what can be edited *(assumed: before it is paid)*
- Down payment and progress payments are received before the invoice. The payment and the invoice must match in the end (see progress in [[Sales and Marketing Module]])

## Colleague's design
How the colleague's repo builds this, for reference only: [[Colleague Architecture#Finance A&T]].

## Open questions
- [ ] Which journals does Om Leo approve: every journal, or only the ones typed by hand (automatic ones from purchases and sales post on their own)?
- [ ] Production Cost is also listed in [[Production Module]]. Which module owns it?
- [ ] Which parts does Pandu need first?
- [ ] Use one journal engine for every module (each step posts its own journal, corrections by reversal), like the colleague's repo?
- [ ] Month closing: lock a closed month so nothing can be posted in it? Who closes and who reopens?
- [ ] Chart of accounts: does Pandu have one today, or start from the colleague's starter list?
- [ ] Purchase price differences on invoices: to a price variance account (colleague) or into the item's cost?
- [ ] PPh 23 on services bought: needed?
