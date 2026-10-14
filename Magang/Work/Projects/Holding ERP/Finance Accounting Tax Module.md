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
- Marketing and Finance see cost and margin (built in S&M slice S0)

**Open questions on this**
- [ ] % FOH and % margin in Marketing's way: a % of what? Example: COGM Rp 10.000, FOH 20 %, margin 25 %. Is SPPR 10.000 x 1,2 = 12.000 COGS, then 12.000 / (1 - 0,25) = Rp 16.000 (GP on the price)? Or 10.000 + 20 % + 25 % of COGM = Rp 14.500?
- [ ] Is the % FOH one rate for the company, or per product? Who sets it, Finance?
- [ ] Is the SPPR one price per item, or per distributor or region (the whiteboard shows Dis A Surabaya and Dis B Bandung under it)?
- [ ] Who sets and changes the SPPR: Marketing, with Finance's advice? Does it need approval?
- [ ] When sales quotes below the SPPR, does that need approval (Sales can't see the margin, so the margin check would show up as "below publish rate")?

## Notes from the Inbox
From Jingga's Inbox, processed 2026-10-14. Raw notes, not rules yet.
- A recorded payment can't be edited. It must match its invoice. The invoice is what can be edited *(assumed: before it is paid)*
- Down payment and progress payments are received before the invoice. The payment and the invoice must match in the end (see progress in [[Sales and Marketing Module]])

## Open questions
- [ ] Which journals does Om Leo approve: every journal, or only the ones typed by hand (automatic ones from purchases and sales post on their own)?
- [ ] Production Cost is also listed in [[Production Module]]. Which module owns it?
- [ ] Which parts does Pandu need first?
