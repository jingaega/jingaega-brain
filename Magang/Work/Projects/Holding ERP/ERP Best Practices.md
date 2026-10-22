---
title: ERP Best Practices
type: reference
project: "[[Holding ERP]]"
created: 2026-10-21
tags: [internship, erp, reference, best-practices]
---

# ERP Best Practices

What mainstream ERPs (SAP, Oracle, Dynamics, NetSuite) and the rules behind them usually do per module, next to what two open source ERPs actually do in their code ([[ERPNext Architecture]], [[Odoo Architecture]], read 2026-10-21), compared with our vault and build and with the colleague's repo. Researched 2026-10-21. Part of [[Holding ERP]]. Our design is in the module notes and [[Architecture]], the colleague's in [[Colleague Architecture]].

> [!warning] General practice, not our rules
> "General" here means what most ERPs and their guides do. It is a checklist and a source of good questions, never a rule for us. A row only changes our build once Jingga or the supervisor decides it in the module's own note. Most sources are vendor guides, so numbers in them (tolerances, KPI targets) are examples, not standards. Laws and tax rules must be checked against the official text before building.

## How to use this note
This note is step 0 of the loop in [[Workflow]]. Before a phase's build plan:
1. Refresh that phase's table below: general, ERPNext, Odoo, ours (vault and code), colleague's. When ERPNext and Odoo agree with each other and differ from us, look twice
2. Each row gets a verdict: **Same** (no work), **Ours wins** (we decided, keep it), **Gap** (general practice we don't have, decide if Pandu needs it), **Decide** (we differ and haven't chosen)
3. Every **Gap** and **Decide** row becomes an open question in the module's note, or a line in its build plan
4. The table stays here. The module note links to its section, so the comparison lives in one place

## Company types in the group
One ERP has to fit very different businesses. What each type needs most, from general practice:

| Type | Our companies | What general ERPs stress for this type |
|---|---|---|
| Process manufacturer (formulas, batches) | [[PT Pandu Dwiloka Teknik|Pandu]] (Wening soap) | Formula versions, batch scaling and yield, lot and expiry tracking, quarantine until QC release, recall in minutes, regulator registration per formula, overhead absorbed into product cost |
| Discrete manufacturer (machining) | [[PT Serio Prima Indotek|Serio]] *(assumed)* | Multi level BOM, routings and work centers, capacity scheduling, serial numbers, scrap per operation |
| Trader or distributor | [[PT Siar Bagja Maraloka|Sibama]], [[PT Anggoan Karya Narasena|Anggoan]] | Price lists and discounts, POS, fast picking, ABC and reorder points, landed cost on imports, credit control |
| Service company | [[PT Sancang Anugerah Nusantara|Sancang]] | Service orders and contracts, time and materials, technician scheduling, two way match (no goods receipt), spare parts van stock |
| Advisory | [[PT Ekatama Tiara Advisindo|ETA]] | Finance only: projects or engagements, time sheets, billing |
| Holding | [[PT Serio Prima Indotek|Serio]] as holding | Group dashboards, shared master data, intercompany matching, eliminations and consolidation |

So the modules we build for Pandu cover the manufacturer and most of the trader. Service orders (Sancang), POS (Sibama, Anggoan) and engagements (ETA) are separate pieces to plan at phase 9.

## Foundation and shared engines

| Topic | General | ERPNext | Odoo | Ours | Colleague | Verdict |
|---|---|---|---|---|---|---|
| Company data apart | Each legal company has its own books in one system; master data may be shared | Company on every record, user permissions per company | Company on every record, record rules; branches share the parent's chart and taxes | One database, company on every record, enforced by PostgreSQL | Same, with Supabase | Same |
| Roles | Role based access plus separation of duties: whoever asks can't approve, whoever keeps bank details can't pay | Roles per document action (read, write, submit, cancel, amend), override roles in settings | Groups and record rules; manager group approves | Levels and departments, Company Admin switch, Director level | Levels without Director | Ours wins. **Gap**: no general "you can't approve your own" rule yet |
| Approvals | One shared workflow engine: rules per document type and amount band, steps, an inbox, maker and checker | Authorization Rule blocks submit above an amount; Workflow configurable, none shipped | PO double approval above an amount; general approvals are Enterprise | One table per feature (price proposals, payment terms, SPPRs) | One shared engine with bands and inbox | **Decide**: build one shared engine before Procurement. Above Rp 100 jt it must do Super Admin then Super User |
| Document numbers | Per company, per document type, per period, no gaps for legal documents. Tax invoice numbers come from the tax office | Naming series per type, with year, month, company; returns and amends get suffixes | Sequence per journal, no gaps, reset by month or year | `SQ-1026-001`, per company and month | `SQ-PDT-2026-10-0001` | Same idea. **Gap**: holding lists must show the company next to the number |
| Audit trail | Who changed what and when, on master data and documents, with approvals logged | Version history on documents, activity log | Chatter log on records; hash chain on posted entries | PostgreSQL logs every change on master data and quotes | Same | Same |
| Posted documents | Never edited or deleted; corrections are reversals, back dated postings watched | Submitted frozen; cancel writes reversal rows; amend makes a -1 copy | Reversal instead of edit; hashed entries can't return to draft | Vault rule: reversals by a Manager | Reversals by anyone from OPR up | Ours wins |
| Month closing | A closed month refuses postings; reopening needs approval | Accounting period per document type, frozen date, period closing voucher | Lock dates (global, tax, sales, purchase), irreversible hard lock, time boxed exceptions | Not built | Built, reopening needs a reason only | **Gap** for Phase 5 |
| Master data control | One owner per record type, changes to sensitive fields (vendor bank) approved by a second person | Customer and supplier naming and hold settings; scorecard can block suppliers | Basic; bank account trust check before payment | Owners per department, bank numbers kept out of the log | Many people can add | Ours wins. **Gap**: second person for vendor bank changes |
| Currency | Document currency plus the company's currency; open AP and AR revalued at month end, gains and losses booked | Document and company currency; month end Exchange Rate Revaluation, reversed next day | Realised differences automatic; month end revaluation is Enterprise | 7 currencies per vendor, rate locked when paid | IDR only | **Gap**: month end revaluation (PSAK 10) |
| Intercompany | One transaction makes both sides (PO at buyer, SO at seller), due to and due from, eliminated at consolidation | Mirror documents by button (SO to PO, DN to PR, SI to PI), linked both ways; no elimination | Transit location only; automatic mirror documents are Enterprise | Later | Later | **Gap, high**: most of Pandu's sales go to Anggoan |

## Phase 2: Sell (order to cash)

| Topic | General | ERPNext | Odoo | Ours | Colleague | Verdict |
|---|---|---|---|---|---|---|
| Quote | Versions, validity, price from a price list, discounts with approval | Revisions, expiry by daily job, lost reasons; order from an expired quote needs a setting | Templates with optional lines, validity, online signature | Versions, 14 days, price from the SPPR, a lower price needs Marketing, a higher one a reason (S2d, 2026-10-21) | Inquiry first, price lists, Sales types prices | Ours wins |
| Price list | A base price plus price lists per customer group or customer, with start and end dates; the most specific wins | Price lists with item prices and dates, pricing rules by customer, group, qty and date | Pricelists per customer with rules and date ranges | One SPPR per item; segment and customer prices planned on top, waiting for the supervisor | Price lists per customer, segment or general, with dates; most specific wins | **Decide** with the supervisor: does Pandu price per segment? |
| Price change | The pricing owner changes list prices with a second person approving and a start date; history kept | Sales Manager edits item prices, no approval | Sales Manager edits pricelists, no approval | Marketing sets the SPPR now, GM approves below the minimum GP; history in the change log only | Any Manager edits price lists; their plan MK3: Sales requests, Marketing Manager approves | **Gap**: a start date (idea, open question) |
| Margin control | Floor price or minimum margin, approval below it | Can block selling below the purchase or valuation rate | Not built in | Minimum GP 10 %, default 20 % for the suggested SPPR, GM approves SPPRs below it | Minimum markup 10 % | Ours wins |
| Customer PO | Recorded against the quote, differences flagged, a lower price needs approval | PO number and date fields on the order | Customer reference field | Own record, line by line check, looser term to the GM | A few fields on the SO | Ours wins |
| Sales order | Entered from the customer's confirmed PO, checked against the quote; status worked out from delivered and billed qty; cancel blocked once delivered | Made from the quote by a button, lines link back to quote lines; status from % delivered and % billed; Close and On Hold by hand; cancel blocked while deliveries exist | The confirmed quotation is the order; lock after confirm; cancel blocked once delivered or invoiced | Button on the accepted quote; lines from the customer's PO, a lower price than quoted needs Marketing, other differences flagged *(ask supervisor)*; term from the quote or an approved looser PO term (Jingga, 2026-10-21; S4 plan) | Made from the quote; customer PO as a few fields | Ours chosen. Taken from both: line links to the source line, status worked out not typed (from S6) |
| Credit check | Before the order is released: limit, overdue, open orders; hold with approval | Blocks at order (or at delivery and invoice), overdue limit, credit controller role passes | Warning only | Planned at S7 (tempo) | On the SO, hold to the Manager | Same plan |
| Stock check | Available to promise: on hand minus reserved, plus planned receipts | Projected qty, stock reservation per order | Reservation on confirm, forecast per product | Manual "stock confirmed" until Phase 4 | Reserves batches at the SO | Ours for now, **Gap** at Phase 4 |
| Delivery | Pick, pack, ship, proof of delivery | Delivery note, pick list, packing slip, returns | Pick, pack, ship steps | DO / BAST, signed or rejected | DO, delivered or refused | Same |
| Revenue timing | When control passes to the customer (PSAK 72): delivery or acceptance, per contract | COGS on delivery (or a 'delivered not billed' account), revenue on invoice | Invoice books revenue and COGS together (perpetual at invoicing) | Revenue and COGS on the signed BAST | COGS when goods leave, revenue on the invoice | Ours wins (one moment for both) |
| Tax invoice | Output VAT invoice through the tax office system (Coretax), DPP 11/12 at 12 % for most goods | PPN 11 % template only, no e-Faktur | PPN 12 % set as 11 % label 12 %, WAPU; no e-Faktur | Not yet | PPN worked out, no Coretax | **Gap** for both, Phase 5 |
| Returns | Return authorization, QC decides: restock, rework or scrap; credit note | Return delivery and credit note documents | Return transfers, credit notes | From a rejected BAST, QC by Quality, rework loop | Any delivery, one rework, QA by Sales | Ours wins. **Gap**: scrap or write off is missing |
| Collections | AR aging, reminders, payment matched to invoices | Payment schedule, overdue status daily, dunning | Follow up levels, aging | Phase 5 | Built | Phase 5 |
| CRM | Lead (not yet qualified), then opportunity (need, budget, timing) with stages, lost reasons and competitors, a pipeline forecast; a won deal becomes a quote or order without retyping | Lead, opportunity, prospect; status follows the quote | Leads to opportunities, stages, rotting, scoring | Quote lost reasons only; CRM named for later | Inquiries and price requests, no pipeline | **Gap**, later. Pandu's quotes start by hand |
| Website leads | Web forms land as leads, checked against existing customers so no duplicates | Web form to lead | Website module creates leads | Named in the S&M idea, websites are their own project ([[Company Websites]]) | None | Phase 9 |
| POS | Counter sale lowers ERP stock at once; opening float, cash in and out, end of shift count against expected; works offline | Opening, closing with expected vs counted cash, consolidated into invoices | Session with cash control, one sales receipt per session | Sibama and Anggoan need it | None | Phase 9 |

## Phase 3: Buy (procure to pay)

| Topic | General | ERPNext | Odoo | Ours | Colleague | Verdict |
|---|---|---|---|---|---|---|
| Request to order | PR, sourcing or RFQ, PO, approvals by amount | Material request, RFQ with portal, supplier quotation, PO | RFQ, PO; blanket orders; RFQ comparison as a module | Same flow in the notes | Built | Same |
| Down payment to vendors | Down payment request, paid before receipt, cleared against the final invoice | Advance against the PO, optional separate advance account, cleared on the invoice | Down payment lines on the PO | Wanted (DP 50 %, TT 100 %, CBD) | Not possible, only employee advances | **Decide**: how a vendor DP works |
| PO changes | Change orders keep the number with a revision, approval again if the amount grows | Update items on a submitted PO (workflow can limit); or cancel and amend -1 | Edit until locked; manager can unlock | R1, R2 seen on real POs | Draft only | **Decide** |
| Three way match | PO, receipt and invoice compared; tolerance by item and value; holds go to an owner | Over receipt and over billing % per item, role may go over; invoice hold until a date | Bill on ordered or received qty per product; PO to bill matching view | Not built | 0 % tolerance, Manager decides holds | **Decide** tolerance |
| Services | Two way match (PO and invoice) or a service confirmation, since there is no receipt | Service items billed without receipt | Bill on ordered qty for services | Not in the notes | Same three way flow | **Gap**, matters for Sancang |
| Imports | Landed cost: freight, duty, bank fees added to the item's cost | Landed cost voucher, spread by qty or amount | Landed costs, spread by qty, cost, weight or volume | Imports seen on Sibama POs | None | **Gap** |
| Withholding tax | PPh 23 withheld on services, proof given to the vendor | Tax withholding categories (TDS), not Indonesian | Not in Community for Indonesia | Not in the notes | None | **Gap** |
| Separation of duties | The person who receives goods doesn't post the invoice; vendor bank changes checked by a second person | Roles per action; no built in 'not your own' | Groups; no built in rule | Not set | Not enforced | **Gap** |

## Phase 4: Stock (IMS and WMS)

| Topic | General | ERPNext | Odoo | Ours | Colleague | Verdict |
|---|---|---|---|---|---|---|
| Costing | Moving average for bought materials; standard cost with variances for made goods, or actual batch cost | FIFO, moving average, LIFO or standard cost, per item and warehouse | Standard, AVCO or FIFO per product category; periodic or perpetual | Typed cost now, moving average (MAP) planned | Moving average for everything, one per item | **Decide** for finished goods |
| Lower of cost or NRV | Stock written down when it can't be sold for its cost (PSAK 14) | Not built in | Not built in | Not in the notes | None | **Gap** |
| Stock status | Available, quarantine, blocked, expired, per batch | Rejected warehouse; batch can be disabled; no quarantine status | QC location in 3 step receipt; scrap location | Not built | Free, reserved, quarantine | Gap to plan |
| Batch and expiry | Per item, FEFO for expiring goods, FIFO otherwise | Shelf life gives expiry; expired blocked; global FIFO, LIFO or FEFO | Expiry, best before, removal, alert dates per lot; FEFO per category | Wening needs it | FEFO, batches without expiry by batch number | **Gap** in theirs: FIFO by arrival |
| Counting | Cycle counts by ABC class (A often, C rarely), recount above a tolerance | Stock reconciliation sets the balance | Counts on stock records, cyclic per location | Not in the notes | Blind counts, any difference to the Super Admin | **Decide** |
| Reorder | Reorder point, safety stock, lead time; made goods go to production, bought goods to purchasing | Level and qty per warehouse; daily job makes purchase, transfer or manufacture requests | Min and max rules, demand based; route picks buy or manufacture | "Make" items go to Production (S&M rule) | Every item makes a PR | Ours wins |
| Analysis | ABC and slow moving on consumption, including production use | Stock ageing, projected qty, batch expiry reports | Forecast and reporting | "Control for production" wanted | Sales only | Ours wins |
| Put away and picking | Rules by zone and capacity, scan to confirm, FEFO picking | Put away rules with capacity; pick list follows the batch rule | Put away rules by category, package, capacity; removal strategies | Scan after GR wanted | Largest bin first, no scanning | **Gap** for both |
| Labels | Item, batch and location barcodes, pallet IDs | Barcodes on items, batches | Barcodes, packages, SSCC | Open question | None | **Decide** |

## Phase 5: Money (Finance A&T)

| Topic | General | ERPNext | Odoo | Ours | Colleague | Verdict |
|---|---|---|---|---|---|---|
| Automatic journals | Every business step posts its own balanced journal | Every submit posts GL; stock GL from the stock ledger | Invoices and closings post; receipts and deliveries don't (v20) | Planned, F2: event codes, mappings, balance checked by PostgreSQL | Built, with mappings per company, deferred balance check | Same, taken from his |
| Manual journals | Maker and checker above a threshold | Journal entry; approval through workflow | Journal entries; hash chain | Om Leo approves, as holder of `journals.approve`, through the shared engine (F0, F3) | Super Admin approves | Ours wins |
| Chart of accounts | One group chart makes consolidation easy | Child company copies the parent's chart; Indonesian chart available | Indonesian chart template; branches share the parent's | His starter chart, extended, same codes in every company (decided 2026-10-22) | Starter chart per company | Decided |
| VAT | Input and output VAT, Coretax tax invoices, monthly VAT return | PPN 11 % template | PPN taxes and WAPU; no e-Faktur | PPN out report and Coretax export file planned (F11); tax codes by date | PPN 11/12 worked out, no Coretax | Planned. Rate open (ask FAT) |
| Withholding | PPh 21, 23, 4(2) with monthly returns | TDS framework, not Indonesian; payroll tax India shaped | None for Indonesia in Community | PPh 23 with Procurement, PPh 21 with HR | PPh 21 only | **Gap** until those modules |
| Fixed assets | Register with depreciation; book method and the tax groups of PMK 72/2023 (4, 8, 16, 20 years; straight line or declining) kept apart | Finance books (book and tax), straight line, declining, WDV; daily posting | Enterprise | Planned F15: book and tax schedules | Register, no depreciation | Planned |
| Bank | Reconciliation against the statement | Statement import, matching, create missing entries | Statement lines, reconcile models | Planned F10: statement import, every bank line, petty cash | Ticks customer and vendor payments only | Ours planned wider |
| Costing model | Absorption costing for stock value (fixed overhead in product cost), management views on top | BOM cost with operating cost; standard cost variances | Workcenter hour cost; components plus labour | COGM, DL, FOH, GP, SPPR; actual FOH % from the ledger (F12); COGS posts with Stock | Materials only | Ours wins on the model; posting with Stock |
| Customer invoice | From the delivery or the order, revenue on the invoice | Sales invoice from order or delivery | Invoice from order, by ordered or delivered qty | Posted by itself on the signed BAST, one per BAST (F7, *ask supervisor*) | Made by Finance from received deliveries | Ours chosen |
| Advances | Down payment held as a liability, cleared on the invoice | Advance against the order, cleared on the invoice | Down payment invoice deducted later | PI payments as customer deposits, used up by the invoice (F6, F7) | Same, customer deposits | Same, taken from his |
| Receipts | One receipt can pay several invoices; unapplied cash kept | Payment ledger, allocation to many invoices | Reconciliation of many lines | One receipt, several invoices of one customer (F8) | One receipt, one invoice | Ours wins |
| Month closing | Closed months refuse postings; reopening controlled | Accounting period, frozen date | Lock dates, hard lock | Close by a Finance Manager, reopening approved (F4) | Close and reopen with a reason | Ours wins |
| Year end | P&L closed into retained earnings | Period closing voucher | Enterprise | Planned F16 | None | Planned |
| Cash flow | Statement of cash flows | Report built in | Enterprise | Planned F16 | None | Planned |
| FX | Revaluation of open items at month end | Exchange rate revaluation | Enterprise | With Procurement | IDR only | **Gap** until Procurement |
| Group reports | Consolidation with eliminations | Consolidated report, one column per company, no elimination | Enterprise | Later | Planned dashboard only | Phase 9 |

## Phase 6: Make (Production and Quality)

| Topic | General | ERPNext | Odoo | Ours | Colleague | Verdict |
|---|---|---|---|---|---|---|
| Formula | Versioned and approved; a batch scales from it; every batch matches the formula registered with the regulator (BPOM CPKB for cosmetics, Kemenkes for household products) | BOM submitted and versioned; process loss %; no registration link | BOM versions in Community; change control (PLM) is Enterprise | Needed for Wening | BoM versions, no approval, no registration link | **Gap, high**: approval and a registration number per formula |
| Production order | Plan, issue (in parts), finish with yield; scrap and by products; leftovers returned | Work order, transfer to WIP, job cards, manufacture with process loss and by products | MO with consumption warning, by products, scrap, unbuild | Not built | Built, no scrap, no returns | **Decide** |
| Batch record | Every batch keeps materials and their batches, who, when, QC results | Material batches mapped to the finished batch; traceability report | Lot traceability through move lines | Needed | Issues logged, material batches not on the order | **Gap**: trace back to material batches |
| Planning | Master schedule, then materials planning, rough capacity check, campaigns for products that share a line | Production plan nets demand against stock, makes work orders and requests; one period | Reorder rules and routes; make to order | PPIC owner open | Restock list only | **Decide** what Pandu needs |
| Production cost | Labour and overhead absorbed into each batch at a rate, variances reported | Materials + operating cost from job cards + extras, over good qty | Components + work order time x hour cost + extras | FOH as a % of COGM in pricing | Materials only | **Decide** with Finance (Phase 5) |
| QC specs | Test parameters per item with limits, versioned, linked to the formula | Inspection template with numeric, text or formula criteria | Enterprise | Not in the notes | Specs per item with history | **Gap** |
| Incoming QC | Materials stay in quarantine until checked; sampling plans; vendor CoA checked against the spec | Inspection required per item before purchase; Stop or Warn | 3 step receipt with QC location; checks are Enterprise | Not in the notes | A switch, off by default, raw material and packaging only | **Decide** |
| QC release | Finished batches stay in quarantine until Quality (not Production) releases them (ISO 22716, CPKB) | No release status; inspection on the finished good if the BOM says so | Enterprise | Required for Wening | A switch, off by default; blocked batches stuck | **Gap**: always on for Pandu, rework or destroy flow |
| CoA | Certificate of analysis made from the batch's results, sent with deliveries when a customer asks | Inspection results; no CoA document | Enterprise | Not in the notes | CoA PDF per batch | **Gap** |
| Deviations and CAPA | Non conformance record with severity and decision (use, rework, return, destroy), root cause and corrective action, followed up | Non conformance and quality action records, not linked to batches | Enterprise | Not in the notes | None | **Gap** |
| Supplier quality | Reject rate and late deliveries per vendor feed a vendor rating | Supplier scorecard with weighted criteria, can block | Not in Community | Not in the notes | Reject rate by value | **Gap** |
| Complaints and recall | Complaints investigated, all affected batches found, recalled goods kept apart, regulator told | Batch traceability report; no recall document | Lot traceability; no recall document | Recall wanted | None | **Gap, high** |

## Phase 7: People and places (HR, GA)

| Topic | General | ERPNext | Odoo | Ours | Colleague | Verdict |
|---|---|---|---|---|---|---|
| Employee record | HR owns the employee and staff number; a login is linked to it | Employee in ERPNext; a user is linked to it; number by series | Employee linked to a user | NRP on the user, HR owns it later | Employee first, generated numbers | **Decide** before HR |
| Contracts | PKWT and PKWTT with dates, probation, pay; reminders before a PKWT ends | Not a separate record; joining date, promotion and transfer records | Contract on the employee in Enterprise payroll | Contract Management in the HR idea | Built, never edited, a new one ends the old | Same idea |
| Attendance | Check in and out (device, GPS or face), shifts, late and early, corrections by HR with a reason | Check ins from device or phone with location, auto attendance per shift | Kiosk, app with GPS, manual; overtime rules | Absensi in the HR idea | GPS against work locations, HR corrects | **Decide** how Pandu clocks in |
| Leave | Balances per year, carry over, cuti bersama, request and approval, calendar | Leave ledger: allocation, use, carry over, expiry rows; earned leave, encashment | Approvals by HR, manager or both; accrual plans | Not in the notes | Built, policy by the Super Admin | **Decide** who sets the policy |
| Overtime, THR | PP 35/2021, Permenaker 6/2016 | Overtime types and slips; THR as additional salary | Overtime rules; payroll Enterprise | Inbox notes | Built | Same |
| Payroll | Period locked before payment, payslips, bank transfer file, PPh 21 TER, BPJS with yearly caps | Salary structure formulas, three journals; PPh 21 TER doesn't fit, needs a hook | Enterprise | Inbox notes match | Built; JP cap typed in as the 2025 figure | Same source. **Gap**: caps must be settings, updated yearly |
| Payslip privacy | Pay seen only by HR payroll, the person and approvers; never in general audit views | Permissions per document; payslip PDF can be password protected | Enterprise | Not in the notes | Payroll rights per permission | **Gap** to plan |
| Claims, loans, advances | Request, approve, pay, settle; advances declared with receipts | Expense claims and employee advances with journals; loans in a separate app | Expenses posted as receipts; advances limited | Inbox notes | Built, advances share the UM account | Same idea |
| Grades | Career paths, points, assessments, promotion changes pay with history | Grades, appraisals with goals; not linked to pay | Not in Community | Inbox notes and meeting [[2026-10-14]] | Built | Same idea |
| Assets | One register shared by GA (where, who holds it, handover BAST) and Finance (cost, depreciation) | Asset register with location and custodian, movements | Enterprise | GA idea only | GA register, no depreciation | **Gap** (see Phase 5) |
| Supplies | Office stock issued to departments, cost to their cost center, restock request | Material issue from stock to a cost center | Internal transfers and consumption | Not in the notes | Built, no approval | **Decide** |
| Repairs and bookings | Facility requests with status; rooms and cars booked without double booking | Asset repair; no room booking | Maintenance requests; booking is a separate app | Facility Management in the GA idea | Built | **Decide** what Pandu needs |
| Licences and documents | Company permits and contracts with expiry reminders (NIB, product registrations, vehicle STNK and KIR) | Not built in | Not built in | Not in the notes | Built, 60, 30, 7 day reminders | **Gap**: product registrations belong here too |
| Bills | Utilities per meter and month, approved, paid, booked to a cost center | Purchase invoice with expense account and cost center | Vendor bills | Not in the notes | Built, PPN not split | **Decide** |

## Phase 8: Rest (R&D, Engineering, MMS)

| Topic | General | ERPNext | Odoo | Ours | Colleague | Verdict |
|---|---|---|---|---|---|---|
| R&D projects | Stages from idea to launch with targets (cost, price, date) | Projects and tasks | Projects | Stub, nothing yet | Projects with stages and targets | **Decide** what Pandu's R&D does |
| Trials | Lab batches from a formula version, materials used booked as R&D cost | Not built in | Not built in | Stub | Trials use real stock, R&D expense | **Decide** |
| Stability | Accelerated and real time storage, checks like pH, viscosity, odor, micro; result sets the shelf life | Not built in | Not built in | Stub | Test rounds with storage days | **Gap**: shelf life from stability results |
| Launch | Only after stable results and the regulator's registration; the formula goes to Production as an approved version | Not built in | Enterprise PLM | Stub | Launch by the GM, a failed test only warns | **Gap**: registration and passed tests before launch |
| Change control | Change request, review, approval, released revision with an effective date; impact checked on open orders | BOM update tool replaces or recosts; no change request | Enterprise PLM (change orders) | Stub | ECR, approval by Manager then GM, BoM change made by hand | **Gap**: the approved change should create the new formula version |
| Machines and lines | Machine list with technical data, linked to the asset register | Workstations with hour rate and status; assets separate | Work centers with OEE; equipment in maintenance | Production idea lists machines | Engineering owns machines and lines | **Decide** owner |
| Preventive plans | By calendar or running hours, checklists, parts kit | Asset maintenance tasks with periodicity | Preventive requests repeat every N days | MMS meaning open | Built, due list made by a button | **Decide** |
| Work orders | Breakdown and preventive, downtime, parts used, technician | Asset maintenance log, asset repair | Maintenance requests (corrective, preventive) | MMS meaning open | Built, no approval | **Decide** |
| KPIs | PM compliance (leading), MTBF, MTTR, OEE = availability x performance x quality (lagging) | Downtime on repairs | MTBF, MTTR, next failure; OEE on work centers | Not in the notes | MTBF, MTTR, availability, no OEE | **Gap**: OEE needs output per line |
| Who maintains | In house or a contractor with service orders | Maintenance schedules and visits for customers' equipment | Maintenance teams | Sancang does Pandu's plant | In house | **Decide**, ties to intercompany |

## Integrated System

| Topic | General | ERPNext | Odoo | Ours | Colleague | Verdict |
|---|---|---|---|---|---|---|
| One inbox | Everything waiting for me across modules, in one list | To do list and assignments | Activities and to dos | Per module (Sales To approve) | One approvals inbox | **Gap**, comes with the shared approval engine |
| Notifications | In app, by email for urgent ones, made by the system when something waits or is decided | Notifications, email alerts, email digest | Activities, chatter, email | Email for login only | In app bell from approvals | **Gap** |
| My Workspace | One page per person: my requests, approvals, leave, assets | Workspaces per role | Dashboards per app | Not in the notes | Built | **Decide** |
| Document chain | From any document, see the ones before and after it (quote, PO cust, SO, DO, invoice, payment). Refreshed for S4 (2026-10-21) | Each line links back to its source line; connections panel | Smart buttons to linked documents | Quote shows its PO cust | Planned, not built | **Gap** to plan from S4 |
| Import and export | Excel import with all or nothing checks; export of every list | Data import tool, export on lists | Import and export on every list | Vendor import built, export planned | Import for items and vendors | Same idea. Export still open |
| Search | One search across records | Global search | Global search | Not in the notes | None | Later |
| Holding view | Group dashboard for the supers: sales, AR, AP, stock per company and in total | Consolidated report | Enterprise | Phase 9 | Company cards only | Phase 9 |

## Phase 9: Extend (other companies and the group)

| Topic | General | ERPNext | Odoo | Ours | Colleague | Verdict |
|---|---|---|---|---|---|---|
| Intercompany sale | One transaction makes the buyer's PO and the seller's SO, with due to and due from; prices at arm's length | Mirror documents by button, no elimination | Enterprise | Later, "PO becomes SO at the other company" | Not in phase 1 | **Gap, high** for Pandu to Anggoan |
| Intercompany service | Service order at the provider, cost at the receiver (Sancang maintaining Pandu) | Same mirror invoices | Enterprise | Later | None | **Gap** |
| Consolidation | Each company's books stay whole; the group view eliminates intercompany sales, balances and profit in stock | Consolidated statements per company column, no elimination | Enterprise | Holding view shows each company, the between group line and the group total side by side; see [[Glossary]] Elimination (Jingga, 2026-10-21) | Planned dashboard only | **Gap** |
| Shared master data | One group chart of accounts and item codes make consolidation easier | Child company copies chart; items shared | Shared records and accounts across companies | Items per company now | Per company | **Decide** |
| Trader (Sibama, Anggoan) | POS, price lists and discounts, landed cost on imports, fast picking | POS, pricing rules, landed cost, pick lists | POS, pricelists, landed costs, routes | Named in [[Company Roles and Modules]] | Sales, POS not built | Phase 9 plan |
| Service company (Sancang) | Service orders and contracts tied to equipment and site, technician schedule, mobile close out offline, invoice from actual work, two way match for parts | Maintenance schedules and visits, SLA, warranty claims | Field service is Enterprise; repairs in Community | Named | None | Phase 9 plan |
| Discrete maker (Serio) | Multi level BOM, routings and work centers, scrap per operation | Multi level BOM, routings, job cards, capacity | Routings, work centers, OEE | Named *(assumed)* | BoM for process only | Phase 9 plan |
| Advisory (ETA) | Engagements, time sheets, billing | Projects, timesheets billed to invoices | Projects and timesheets | Finance only | None | Phase 9 plan |
| New company | Adding a company is configuration, not code | Company setup creates defaults | Company setup with chart template | Rule in the repo | Rule in the repo | Same |

## Biggest gaps for Pandu
Sorted by how much they matter for Pandu's real flow. None is a rule yet.
1. **Intercompany Pandu to Anggoan.** Most of Pandu's sales are to Anggoan. Group ERPs make one transaction create both sides and eliminate it at group level. Worth planning early, even if built later
2. **Formula approval tied to the registration.** BPOM pulls products whose batches don't match the registered formula. Each formula version should carry its registration number, and a change should be approved
3. **QC release and recall.** Quarantine until Quality releases, a way to rework or destroy a blocked batch, and a trace from a finished batch to customers and back to material batches
4. **One shared approval engine** before Procurement, with "you can't approve your own"
5. **Vendor down payments** and PO revisions, seen on the group's real POs
6. **Tax: Coretax output invoices, PPh 23, fixed asset depreciation (book and tax)**
7. **Month end: closing, FX revaluation, stock written down to NRV**

## Ideas from ERPNext and Odoo
Patterns both open ERPs use, or one uses very well. Each is a question for its phase, not a decision.
1. **Status worked out from progress.** A sales order is "to deliver and bill", "to bill" or "completed" from its lines, never typed. Both do this. Fits S4 onwards
2. **Every line links to its source line.** Partial deliveries, invoices and returns then add up by themselves, and the document chain comes free. Both do this. Fits S4 onwards
3. **Frozen once posted, cancel and amend.** ERPNext makes an amended copy `-1`; Odoo reverses. Both never edit. Matches our "reversals, never edits"
4. **Advances against an order.** Both let a payment sit on a sales or purchase order and clear on the invoice. Answers our vendor down payment question in a simple way
5. **Reorder routes decide buy or make.** Both send a low "make" item to production and a low "buy" item to purchasing. Matches our S&M rule
6. **Expiry per lot and FEFO as a setting.** Odoo also has best before, removal and alert dates. Fits Wening
7. **Receipt steps as a setting.** Odoo's "receive, quality control, store" gives a QC location without a separate module. A simple first step to quarantine
8. **Locks.** Odoo's hard lock date that only moves forward and time boxed exceptions; ERPNext's frozen date with one role that may still post. Fits Phase 5
9. **Finance books.** ERPNext keeps one depreciation schedule for the books and one for tax. Fits PMK 72/2023
10. **Analytic plans or dimensions.** Report by department, product line or project without new accounts. Fits Phase 5
11. **Leave as a ledger of rows.** Balance is the sum of allocations, uses and expiries. Use reversals, not deletes (ERPNext deletes, a weak spot)
12. **A country hook for Indonesian rules.** ERPNext's regional hook is how PPh 21 TER and BPJS would plug in. For us: keep tax rules in one place per country

What both still lack, so we design it ourselves: batch release by Quality, recall, formula registration, Indonesian e-Faktur, PPh 21 TER, BPJS and elimination of intercompany sales.

## Sources
- ERPNext and Frappe HR code: [frappe/erpnext](https://github.com/frappe/erpnext), [frappe/hrms](https://github.com/frappe/hrms), read 2026-10-21. Notes in [[ERPNext Architecture]]
- Odoo 20.0 Community code: [odoo/odoo](https://github.com/odoo/odoo), read 2026-10-21. Notes in [[Odoo Architecture]]
- Process manufacturing: [Blytheco, process manufacturing ERP](https://blytheco.com/blog/posts/a-deep-dive-into-process-manufacturing-erp-for-formula-based-production), [BatchMaster, personal care and cosmetics](https://www.batchmaster.co.in/en-au/erp-software-for-personal-care-and-cosmetics), [Deacom overview](https://www.erpresearch.com/en-us/deacom-erp), [BatchMaster, inventory valuation](https://www.batchmaster.com/blog/inventory-valuation-process-manufacturing/)
- Cosmetics GMP: [Registrar Corp, ISO 22716 batch traceability](https://www.registrarcorp.com/blog/cosmetics/iso-22716/iso22716-batch-traceability/), [SG Systems, ISO 22716](https://sgsystemsglobal.com/?p=16831), [Ecomundo, cosmetics GMP](https://ecomundo.eu/en/blog/understanding-gmp-cosmetic-products-guide)
- Indonesia, products: [BPOM, 21 notifications revoked](https://www.pom.go.id/siaran-pers/bpom-cabut-21-izin-edar-kosmetik-dengan-komposisi-tidak-sesuai-dengan-yang-didaftarkan), [Cekindo, cosmetic registration](https://www.cekindo.com/id/blog/proses-registrasi-produk-kosmetik-di-indonesia), [izin.co.id, PKRT](https://izin.co.id/blog/pkrt-adalah/), [Kemenkes, PKRT licensing guide](https://farmalkes.kemkes.go.id/unduh/pedoman-bimbingan-teknis-perizinan-perbekalan-kesehatan-rumah-tangga/)
- Indonesia, tax and payroll: [IKPI, DPP nilai lain in Coretax](https://ikpi.or.id/?p=13685), [Ortax, Coretax invoices](https://ortax.org/membuat-faktur-pajak-besaran-tertentu-di-coretax), [Pajak.com, PMK 72/2023](https://www.pajak.com/pajak/rincian-pokok-pmk-72-2023-soal-penyusutan-dan-amortisasi-pajak/), [RSM, PMK 72/2023](https://rsm.global/indonesia/sites/default/files/media/events/2023/RSM%20Webinar%20-%20Implementation%20of%20PMK%20No.%2072%20Year%202023%20and%20The%20Accounting%20Impact.pdf), [Talenta, JP wage cap](https://www.talenta.co/blog/iuran-jaminan-pensiun-terbaru/), [BPJS JP cap letter 2025](https://ramco.com/hubfs/compliance-portal/surat-informasi-ketentuan-batasan-upah-jaminan-pensiun-tahun-2025.pdf)
- Buying and selling: [Cleverence, 3 way match and AP control](https://www.cleverence.com/articles/sage-documentation/accounting-behind-the-screens-purchases-4827/), [SAP Insider, P2P controls](https://sapinsider.org/articles/9-key-controls-to-mitigate-purchase-to-pay-compliance-concerns/), [Cleverence, sales order to invoice](https://www.cleverence.com/articles/for-business/sales-order-to-invoice-process-flow-4829/), [Stripe, order to cash](https://stripe.com/en-hu/resources/more/what-is-order-to-cash-o2c-heres-how-it-works)
- Stock and costing: [Cleverence, standard costing](https://www.cleverence.com/articles/for-business/standard-costing-inventory-valuation-4827/), [Cleverence, warehouse inventory process](https://www.cleverence.com/articles/for-business/warehouse-inventory-management-process-4827/), [Cleverence, put away strategy](https://www.cleverence.com/articles/3pl-business/how-to-implement-an-effective-put-away-strategy-5827)
- Planning and maintenance: [Cleverence, production planning and control](https://www.cleverence.com/articles/for-business/how-to-do-production-planning-and-control-4821/), [OxMaint, maintenance KPIs](https://oxmaint.com/article/mtbf-mttr-oee-maintenance-kpis)
- Controls and group: [Hyperbots, ERP financial controls](https://www.hyperbots.com/glossary/erp-financial-controls), [SafePaaS, segregation of duties](https://www.safepaas.com/?p=11725), [Trintech, intercompany eliminations](https://www.trintech.com/blog/what-are-intercompany-eliminations-a-guide-to-accurate-financial-consolidation/), [Apideck, multi entity ERP](https://developers.apideck.com/guides/multi-entity-erp-behavior)
- Quality and change control: [ERP Research, quality management](https://erpresearch.com/erp/erpnext/quality-management), [AlisQI, supplier deviations](https://www.alisqi.com/en/modules/supplier-quality/supplier-deviation-management), [Acumatica, engineering change control](https://www.acumatica.com/media/2020/05/Acumatica_Manufacturing_Engineering_Change_Control_Datasheet_200916.pdf), [Guideflow, engineering change management](https://www.guideflow.com/blog/engineering-change-management-software)
- Stability testing: [Cosmetics and Toiletries, stability guidance](https://www.cosmeticsandtoiletries.com/testing/method-process/article/21837281/stability-testing-guidance-for-product-safety-and-shelflife-insight), [ALS, stability storage and testing](https://www.alsglobal.com/en/quality-control/stability-storage-and-testing), [CTPA, stability testing](https://www.ctpa.org.uk/e-learning/stability-testing-for-cosmetics)
- Service, POS and CRM: [ERP Research, field service](https://www.erpresearch.com/erp-modules/field-service), [SAP, field service management](https://www.sap.com/products/scm/field-service-management/what-is-field-service-management.html), [DMSI, POS for Business Central](https://dmsiworks.com/blog/pos-systems-for-business-central-what-to-look-for-and-what-most-get-wrong), [ERP Research, POS](https://www.erpresearch.com/erp-add-ons/pos), [ERPNext, sales pipeline](https://docs.frappe.io/erpnext/sales-pipeline.md), [SuperOffice, lead and opportunity management](https://superoffice.com/blog/lead-and-opportunity-management)
- ERP products: [ERP Research, Odoo vs SAP Business One](https://www.erpresearch.com/compare/odoo-vs-sap-business-one-for-manufacturing)
