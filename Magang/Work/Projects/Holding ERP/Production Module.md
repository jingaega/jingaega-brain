---
title: Production Module
type: module-spec
project: "[[Holding ERP]]"
status: built on a branch, not merged
created: 2026-10-13
updated: 2026-10-23
tags: [internship, erp, module, production]
---

# Production Module

Part of [[Holding ERP]]. All modules: [[Module Overview]].

> [!note] Built ahead of its phase, on branch `claude/production`, not merged
> Jingga, 2026-10-23: copy the colleague's whole Production module (`Difsyy/Serio-Intern`, `docs/plan-production.md`) and make it work on ours; questions after. Not merged until Jingga says so. Everything below marked *(assumed)* waits for the supervisor.

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

## What was built (P1 to P4, ported)
| Slice | What | Migration |
|---|---|---|
| P1 | Material prices, the colleague's "Daftar Pembelian Material": supplier, material, unit bought in and what it holds (1 drum = 200 kg), qty, price, amount, price per base unit. One current price per supplier, material and unit; a new one keeps the old one as history. Added by hand, or pasted from Excel with a row by row check, saved all or none. Unit ZAK added | 0078 |
| P2 | Products group their variants (an item the company makes). Each variant has BoM versions: batch size and unit, what one batch makes, materials with a loss % on top. Draft, then active (one per variant, the old one archived) or cancelled; only a draft changes. New version, copy to another variant. Cost per batch, per batch unit and per item, with the SPPR margin | 0079 |
| P3 | Production orders (MO): N batches (decimals allowed) from the BoM in use. Materials issued in parts: Dr work in progress, Cr inventory. Finish records the qty made, batch (the MO number by default) and expiry: Dr inventory, Cr work in progress at the real cost (everything issued over the qty made). Cancel only with nothing issued, with a reason | 0080 |
| P4 | Reports for a period: per product orders, planned and made, yield (under 98 % marked), real cost per item; materials planned against issued; per variant the BoM cost at today's typed costs against the last real cost, with the SPPR margin. CSV downloads | none |

PostgreSQL keeps: company rules on every table, prices never edited (only replaced), only a draft BoM changes, an order's plan never changes, issued only grows. 438 tests pass, clicked through in a browser.

## Stock core (S1, S2, built 2026-10-23 on the same branch)
Jingga: "create stock for production", the stock core on this branch, then: one cost table like the colleague's, and his batch and expiry switches on Items.
- Warehouses (master data, MAIN in every company *(assumed)*), a stock ledger of every movement (never changed), stock on hand per item, warehouse and batch. Only the database function `stock_move` writes stock
- `item_costs` is the item's one cost: Finance types it while the item has no stock; once it has stock it is the moving average (the colleague's formula), kept by stock moves, and can't be typed any more. Quotes, the SPPR floor, Pricing, the dashboard, BoMs and production all read it
- Items get the colleague's switches: the stock keeps a batch, or a batch and expiry. Fixed once the item has stock
- Opening stock (Warehouse and Finance Managers *(assumed)*): Dr inventory, Cr opening balances. The only way in until receiving is built
- Production orders pick a warehouse, show free stock and shortages, issue earliest expiry first at the average (refused beyond free stock), and put what was made into stock at the real cost
- Migrations 0081, 0082. 442 tests pass, clicked through

## Different from the colleague's (on purpose, because we have no Stock or AP yet)
- Costs are the moving average once an item has stock, Finance's typed cost before (the colleague shows no cost then)
- The average is kept to the cent (the colleague: 4 decimals), so a stock value can read a few rupiah off the books (Rp 11 on Rp 9,5 jt in the test) *(assumed)*
- No goods receipt, transfers, counts, quarantine or reservations yet, and no "shortage to purchase request" button. These come with Stock (Phase 4) and Procurement
- The price list isn't updated by vendor invoices (no AP yet)
- The import only matches suppliers and items that exist; it doesn't create them. Paste from Excel, no .xlsx upload *(assumed)*
- Variants link to items in their own table instead of new columns on items (items stay as they are)
- Costs stay hidden from Production staff (our rule: only GM, Finance and Marketing Managers and the supers see costs). People who issue get the costs from a database function, for the journal only
- Money and quantities in exact BigInt math, like the rest of ours

## Permissions *(all assumed)*
- Material prices: Production, Procurement and Finance see them; their Managers keep them
- Products and BoMs: Production, Quality and Finance see them; a Production Manager writes them
- Orders: Production, Warehouse and Finance see them; Production SPVs and up plan and cancel; every level in Production issues and finishes
- No approval for BoMs or orders (the colleague's)

## Compared with other ERPs
- ERPNext: BOM with operations and workstations, work orders with job cards and routing, materials moved to a WIP warehouse, cost by valuation rate. We have no routing, no labor or machine cost, a WIP account but no WIP warehouse
- Odoo MRP: BoM kits and phantom BoMs, manufacturing orders with work centers, by-products, scrap, master production schedule. We have none of those yet
- Both cost from stock. Ours waits for Stock (Phase 4) and uses the typed cost until then

## Colleague's design
How the colleague's repo builds this, for reference only: [[Colleague Architecture#Production]].

## Open questions
- [ ] *(ask supervisor, P1 to P4)* Merge the ported Production now, or wait until Stock (Phase 4) so issue and finish move real stock?
- [ ] *(ask FAT, stock core)* Average cost to the cent, or 4 decimals like the colleague (a few rupiah between stock value and the books)?
- [ ] *(ask supervisor, stock core)* Who enters opening stock (Warehouse and Finance Managers now), and one MAIN warehouse to start?
- [ ] *(ask supervisor, P3)* Return of unused materials and scrap: not built (the colleague's says to confirm too)
- [ ] *(ask supervisor, P4)* The 98 % yield mark (the colleague's, to confirm)
- [ ] *(ask supervisor, P3)* QC release of a finished batch (the colleague's Q3 holds it in quarantine): wait for Quality?
- [ ] *(ask supervisor)* Permissions above
- [ ] Production Cost is also listed in [[Finance Accounting Tax Module]]. Which module owns it?
- [ ] PPIC is shared with [[IMS Module]]. Which module owns the records?
- [ ] Which parts does Pandu need first?
- [ ] MMS is also its own module in [[Module Overview]]. Do machines and maintenance belong here or in [[MMS Module]] and [[Engineering Module]] like the colleague's repo?
- [ ] Does a finished batch carry DL and FOH, or materials only like the colleague's repo?
- [ ] Production orders: need approval? Allow partial finish, scrap, and return of unused materials?
- [ ] Planning: does Pandu need a schedule or MRP, or only a restock list like the colleague's repo?
