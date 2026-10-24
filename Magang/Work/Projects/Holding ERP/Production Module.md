---
title: Production Module
type: module-spec
project: "[[Holding ERP]]"
status: built, merged to main
created: 2026-10-13
updated: 2026-10-23
tags: [internship, erp, module, production]
---

# Production Module

Part of [[Holding ERP]]. All modules: [[Module Overview]].

> [!note] Built ahead of its phase, merged to `main` 2026-10-23
> Jingga, 2026-10-23: copy the colleague's whole Production module (`Difsyy/Serio-Intern`, `docs/plan-production.md`) and make it work on ours; questions after. Built on branch `claude/production`, merged by Jingga the same day. Everything below marked *(assumed)* waits for the supervisor.

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

## Production revamp (P5 to P9, started 2026-10-24)
Jingga asked for the plant's real flow: "we have 500 L of Cupir, what can we make?". The ported module could not answer that, so this revamp adds it, plus the shift, the actuals and the quality readings.

### P5. The dummy Wening recipe book
Pandu only, seeded by `db/seed.ts`: one base liquid per family packaged several ways, so planning an order never says "no BoM in use" any more.
- Base liquids: `CUP-BASE` (Cairan dasar Cupir), `SAB-BASE` (Bahan dasar sabun cair), both raw materials
- Packaging: `PKG-JRC40`, `PKG-JRC5`, `PKG-PC125`, `PKG-BTL500`
- Made: `CUP-40L` (jerigen 40 L), `CUP-5L` (Jirgen 5 L), `CUP-125ML` (Puch 125 ml), `SAB-500` (sabun 500 ml)
- Two products (Cupir, Sabun) with four variants, one active BoM each, with the loss on the liquid line
- Costs and SPPR are **dummy numbers** so the screens can be clicked through. Pandu's real item list and prices come later

### The item class (new column on `items`)
One class for the item, in every BoM and every report, so a BoM can split its cost between chemicals and packaging: `raw_material` (the liquid or chemical going in), `packaging` (the bottle, jerrycan or pouch), `other`. Set on the item page and in the seed. This is a change to the data model of Foundation (slice 6), additive only: a new column with a default, no data rewritten.

### P7. The planner, "what can we make?"
New screen: **Production -> What can we make**. Type a base liquid and how much of it there is (500 L of Cupir), and the table refreshes with one row per **alternative**: never a mix.
- Each row: product, how much can be made, chemical cost, packaging cost, cost each, COGS each, selling price, left over (sisa) and the estimated susut
- **Whole packages only**: you cannot sell half a jerrycan, so 500 L fills 12 jerrycans of 40 L, not 12,25
- The susut in the planner is the **recipe estimate**. The real susut is measured after the shift (P9)
- Costs only for people who may see costs, like everywhere else
- It is a calculator. It does not create an order; planning an order stays where it was

### P8. Shift and the production code
- A production order is **one product on one shift** (A, B or C), on its planned day. One order per product, shift and day; a second one is refused with a clear message
- The production code is the product code, the date and the shift: `WN-500-2610A20` (product code, YY, MM, shift, DD). Jingga's format with one separator after the product code, so the code reads clearly
- The date in the code is the **planned** day, fixed when the order is planned and never changed, like every other document number

### P9. Planned against actual, per shift
- The order page shows **two tables**: the plan (frozen, never changes) beside what the shift measured
- The shift records: the base liquid in, the good product out, the rejects (NG) and what was left over (sisa)
- The **susut is measured**, never guessed: input less good product less rejects less leftover. The database works it out, so nobody can type a loss that does not follow from the balance
- A finished order never changes: the measurements are frozen with it

### P10. Quality readings
Tests are set **per item**, not fixed columns, so a new test is a row and not a migration.
- The base liquid carries **pH, TDS and Bau**; a soap variant carries **pH, Warna and Busa**
- A reading is taken with the shift it belongs to, with the limits copied onto it, so a later change to a test does not rewrite the past
- A reading outside its limits is **flagged, not blocked**. What happens to a batch that fails stays open (see the questions below)

### P11. The report, one line per shift
The production report gained a **Per shift** section: one line per shift of production, with the day, the shift, the code, what was planned, what was made, the liquid in, the good product, the rejects, the susut, the yield, the quality readings and the cost. The three existing sections stay as they were. CSV for the new section too.

## The rules Jingga confirmed on 2026-10-24
| Rule | Answer |
|---|---|
| The planner takes | a base liquid you already hold, e.g. 500 L of Cupir |
| The rows of the planner | **alternatives, OR not AND** |
| Whole packages or decimals | whole packages only |
| Chemical against packaging cost | classify the **item** once, inherited by every BoM line |
| Quality attributes | tests per item with limits, not fixed columns |
| The shift | on the order, one order per product per shift per day, a repeat refused |
| The susut | **measured after production**, from the volume balance |

The shift rule is confirmed by Jingga but still to ask the supervisor.

## Open questions
- [ ] *(ask supervisor, P8)* One production order per product per shift per day, a repeat refused. Jingga confirmed 2026-10-24, still to ask
- [ ] *(ask supervisor, P8)* The production code `WN-500-2610A20`: product code, YY, MM, shift, DD. The separator after the product code is Jingga's call
- [ ] *(ask FAT, stock core)* Average cost to the cent, or 4 decimals like the colleague (a few rupiah between stock value and the books)?
- [ ] *(ask supervisor, stock core)* Who enters opening stock (Warehouse and Finance Managers now), and one MAIN warehouse to start?
- [ ] *(ask supervisor, P3)* Return of unused materials and scrap: not built. Now also: what happens to the stock and the cost of the **rejects**?
- [ ] *(ask supervisor, P4)* The 98 % yield mark (the colleague's, to confirm). The susut is now measured, so the mark may become the susut the plant uses
- [ ] *(ask supervisor, P3)* QC release of a finished batch (the colleague's Q3 holds it in quarantine): wait for Quality?
- [ ] *(ask supervisor, P5)* The dummy items, costs and SPPR: when does Pandu's real item list arrive?
- [ ] *(ask supervisor)* Permissions above
- [ ] Production Cost is also listed in [[Finance Accounting Tax Module]]. Which module owns it?
- [ ] PPIC is shared with [[IMS Module]]. Which module owns the records?
- [ ] Which parts does Pandu need first?
- [ ] MMS is also its own module in [[Module Overview]]. Do machines and maintenance belong here or in [[MMS Module]] and [[Engineering Module]] like the colleague's repo?
- [ ] Does a finished batch carry DL and FOH, or materials only like the colleague's repo?
- [ ] Production orders: need approval? Allow partial finish, scrap, and return of unused materials?
- [ ] Planning: does Pandu need a schedule or MRP, or only a restock list like the colleague's repo?

## Open questions (from the original port)
- [x] Merge the ported Production now, or wait until Stock? Merged with the stock core (Jingga, 2026-10-23)
- [ ] *(ask supervisor)* Permissions above
