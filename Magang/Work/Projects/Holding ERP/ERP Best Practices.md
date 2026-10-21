---
title: ERP Best Practices
type: reference
project: "[[Holding ERP]]"
created: 2026-10-21
tags: [internship, erp, reference, best-practices]
---

# ERP Best Practices

What mainstream ERPs (SAP, Oracle, Dynamics, NetSuite, Odoo) and the rules behind them usually do per module, compared with our vault and build and with the colleague's repo. Researched 2026-10-21. Part of [[Holding ERP]]. Our design is in the module notes and [[Architecture]], the colleague's in [[Colleague Architecture]].

> [!warning] General practice, not our rules
> "General" here means what most ERPs and their guides do. It is a checklist and a source of good questions, never a rule for us. A row only changes our build once Jingga or the supervisor decides it in the module's own note. Most sources are vendor guides, so numbers in them (tolerances, KPI targets) are examples, not standards. Laws and tax rules must be checked against the official text before building.

## How to use this note
This note is step 0 of the loop in [[Workflow]]. Before a phase's build plan:
1. Refresh that phase's table below: general, ours (vault and code), colleague's
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

| Topic | General | Ours | Colleague | Verdict |
|---|---|---|---|---|
| Company data apart | Each legal company has its own books in one system; master data may be shared | One database, company on every record, enforced by PostgreSQL | Same, with Supabase | Same |
| Roles | Role based access plus separation of duties: whoever asks can't approve, whoever keeps bank details can't pay | Levels and departments, Company Admin switch, Director level | Levels without Director | Ours wins. **Gap**: no general "you can't approve your own" rule yet |
| Approvals | One shared workflow engine: rules per document type and amount band, steps, an inbox, maker and checker | One table per feature (price proposals, payment terms, SPPRs) | One shared engine with bands and inbox | **Decide**: build one shared engine before Procurement. Above Rp 100 jt it must do Super Admin then Super User |
| Document numbers | Per company, per document type, per period, no gaps for legal documents. Tax invoice numbers come from the tax office | `SQ-1026-001`, per company and month | `SQ-PDT-2026-10-0001` | Same idea. **Gap**: holding lists must show the company next to the number |
| Audit trail | Who changed what and when, on master data and documents, with approvals logged | PostgreSQL logs every change on master data and quotes | Same | Same |
| Posted documents | Never edited or deleted; corrections are reversals, back dated postings watched | Vault rule: reversals by a Manager | Reversals by anyone from OPR up | Ours wins |
| Month closing | A closed month refuses postings; reopening needs approval | Not built | Built, reopening needs a reason only | **Gap** for Phase 5 |
| Master data control | One owner per record type, changes to sensitive fields (vendor bank) approved by a second person | Owners per department, bank numbers kept out of the log | Many people can add | Ours wins. **Gap**: second person for vendor bank changes |
| Currency | Document currency plus the company's currency; open AP and AR revalued at month end, gains and losses booked | 7 currencies per vendor, rate locked when paid | IDR only | **Gap**: month end revaluation (PSAK 10) |
| Intercompany | One transaction makes both sides (PO at buyer, SO at seller), due to and due from, eliminated at consolidation | Later | Later | **Gap, high**: most of Pandu's sales go to Anggoan |

## Phase 2: Sell (order to cash)

| Topic | General | Ours | Colleague | Verdict |
|---|---|---|---|---|
| Quote | Versions, validity, price from a price list, discounts with approval | Versions, 14 days, price from the SPPR, other prices need Marketing | Inquiry first, price lists, Sales types prices | Ours wins |
| Margin control | Floor price or minimum margin, approval below it | Minimum GP 10 %, default 20 % for the suggested SPPR, GM approves SPPRs below it | Minimum markup 10 % | Ours wins |
| Customer PO | Recorded against the quote, differences flagged | Own record, line by line check, looser term to the GM | A few fields on the SO | Ours wins |
| Credit check | Before the order is released: limit, overdue, open orders; hold with approval | Planned at S7 (tempo) | On the SO, hold to the Manager | Same plan |
| Stock check | Available to promise: on hand minus reserved, plus planned receipts | Manual "stock confirmed" until Phase 4 | Reserves batches at the SO | Ours for now, **Gap** at Phase 4 |
| Delivery | Pick, pack, ship, proof of delivery | DO / BAST, signed or rejected | DO, delivered or refused | Same |
| Revenue timing | When control passes to the customer (PSAK 72): delivery or acceptance, per contract | Revenue and COGS on the signed BAST | COGS when goods leave, revenue on the invoice | Ours wins (one moment for both) |
| Tax invoice | Output VAT invoice through the tax office system (Coretax), DPP 11/12 at 12 % for most goods | Not yet | PPN worked out, no Coretax | **Gap** for both, Phase 5 |
| Returns | Return authorization, QC decides: restock, rework or scrap; credit note | From a rejected BAST, QC by Quality, rework loop | Any delivery, one rework, QA by Sales | Ours wins. **Gap**: scrap or write off is missing |
| Collections | AR aging, reminders, payment matched to invoices | Phase 5 | Built | Phase 5 |

## Phase 3: Buy (procure to pay)

| Topic | General | Ours | Colleague | Verdict |
|---|---|---|---|---|
| Request to order | PR, sourcing or RFQ, PO, approvals by amount | Same flow in the notes | Built | Same |
| Down payment to vendors | Down payment request, paid before receipt, cleared against the final invoice | Wanted (DP 50 %, TT 100 %, CBD) | Not possible, only employee advances | **Decide**: how a vendor DP works |
| PO changes | Change orders keep the number with a revision, approval again if the amount grows | R1, R2 seen on real POs | Draft only | **Decide** |
| Three way match | PO, receipt and invoice compared; tolerance by item and value; holds go to an owner | Not built | 0 % tolerance, Manager decides holds | **Decide** tolerance |
| Services | Two way match (PO and invoice) or a service confirmation, since there is no receipt | Not in the notes | Same three way flow | **Gap**, matters for Sancang |
| Imports | Landed cost: freight, duty, bank fees added to the item's cost | Imports seen on Sibama POs | None | **Gap** |
| Withholding tax | PPh 23 withheld on services, proof given to the vendor | Not in the notes | None | **Gap** |
| Separation of duties | The person who receives goods doesn't post the invoice; vendor bank changes checked by a second person | Not set | Not enforced | **Gap** |

## Phase 4: Stock (IMS and WMS)

| Topic | General | Ours | Colleague | Verdict |
|---|---|---|---|---|
| Costing | Moving average for bought materials; standard cost with variances for made goods, or actual batch cost | Typed cost now, moving average (MAP) planned | Moving average for everything, one per item | **Decide** for finished goods |
| Lower of cost or NRV | Stock written down when it can't be sold for its cost (PSAK 14) | Not in the notes | None | **Gap** |
| Stock status | Available, quarantine, blocked, expired, per batch | Not built | Free, reserved, quarantine | Gap to plan |
| Batch and expiry | Per item, FEFO for expiring goods, FIFO otherwise | Wening needs it | FEFO, batches without expiry by batch number | **Gap** in theirs: FIFO by arrival |
| Counting | Cycle counts by ABC class (A often, C rarely), recount above a tolerance | Not in the notes | Blind counts, any difference to the Super Admin | **Decide** |
| Reorder | Reorder point, safety stock, lead time; made goods go to production, bought goods to purchasing | "Make" items go to Production (S&M rule) | Every item makes a PR | Ours wins |
| Analysis | ABC and slow moving on consumption, including production use | "Control for production" wanted | Sales only | Ours wins |
| Put away and picking | Rules by zone and capacity, scan to confirm, FEFO picking | Scan after GR wanted | Largest bin first, no scanning | **Gap** for both |
| Labels | Item, batch and location barcodes, pallet IDs | Open question | None | **Decide** |

## Phase 5: Money (Finance A&T)

| Topic | General | Ours | Colleague | Verdict |
|---|---|---|---|---|
| Automatic journals | Every business step posts its own balanced journal | Planned | Built, with mappings per company | Same plan |
| Manual journals | Maker and checker above a threshold | Om Leo approves | Super Admin approves | Ours wins |
| Chart of accounts | One group chart makes consolidation easy | Not decided | Starter chart per company | **Decide**: one group chart? |
| VAT | Input and output VAT, Coretax tax invoices, monthly VAT return | Not built | PPN 11/12 worked out, no Coretax | **Gap** |
| Withholding | PPh 21, 23, 4(2) with monthly returns | PPh 21 with HR only | PPh 21 only | **Gap** |
| Fixed assets | Register with depreciation; book method and the tax groups of PMK 72/2023 (4, 8, 16, 20 years; straight line or declining) kept apart | GA idea only | Register, no depreciation | **Gap** |
| Bank | Reconciliation against the statement | Not built | Match and mark | Phase 5 |
| Costing model | Absorption costing for stock value (fixed overhead in product cost), management views on top | COGM, DL, FOH, GP, SPPR on the whiteboard | Materials only | Ours wins on the model, **Gap** in posting it |
| Group reports | Consolidation with eliminations | Later | Planned dashboard only | Phase 9 |

## Phase 6: Make (Production, Quality, R&D, Engineering, MMS)

| Topic | General | Ours | Colleague | Verdict |
|---|---|---|---|---|
| Formula | Versioned and approved; a batch scales from it; every batch matches the formula registered with the regulator (BPOM CPKB for cosmetics, Kemenkes for household products) | Needed for Wening | BoM versions, no approval, no registration link | **Gap, high**: approval and a registration number per formula |
| Production order | Plan, issue (in parts), finish with yield; scrap and by products; leftovers returned | Not built | Built, no scrap, no returns | **Decide** |
| Batch record | Every batch keeps materials and their batches, who, when, QC results | Needed | Issues logged, material batches not on the order | **Gap**: trace back to material batches |
| Planning | Master schedule, then materials planning, rough capacity check | PPIC owner open | Restock list only | **Decide** what Pandu needs |
| QC release | Incoming and finished goods stay in quarantine until Quality releases them (ISO 22716, CPKB) | Required for Wening | A switch, off by default; blocked batches stuck | **Gap**: always on for Pandu, rework or destroy flow |
| Complaints and recall | Complaints investigated, all affected batches found, recalled goods kept apart, regulator told | Recall wanted | None | **Gap, high** |
| R&D | Formula trials, stability tests, then registration before launch | Stub | Trials, tests, launch by the GM | **Gap**: registration step before launch |
| Maintenance | Preventive plans, work orders, PM compliance, MTBF, MTTR, OEE, critical spares | MMS meaning open, Sancang does Pandu's plant | Built, no OEE | **Decide** who records it |

## Phase 7: People and places (HR, GA)

| Topic | General | Ours | Colleague | Verdict |
|---|---|---|---|---|
| Employee record | HR owns the employee and staff number; a login is linked to it | NRP on the user, HR owns it later | Employee first, generated numbers | **Decide** before HR |
| Payroll | Period locked before payment, payslips, bank transfer file, PPh 21 TER, BPJS with yearly caps | Inbox notes match | Built; JP cap typed in as the 2025 figure | Same source. **Gap**: caps must be settings, updated yearly |
| Overtime, THR, leave | PP 35/2021, Permenaker 6/2016, UU 13/2003 | Inbox notes | Built | Same |
| Assets | One register shared by GA (where, who holds it) and Finance (cost, depreciation) | GA idea only | GA register, no depreciation | **Gap** (see Phase 5) |

## Biggest gaps for Pandu
Sorted by how much they matter for Pandu's real flow. None is a rule yet.
1. **Intercompany Pandu to Anggoan.** Most of Pandu's sales are to Anggoan. Group ERPs make one transaction create both sides and eliminate it at group level. Worth planning early, even if built later
2. **Formula approval tied to the registration.** BPOM pulls products whose batches don't match the registered formula. Each formula version should carry its registration number, and a change should be approved
3. **QC release and recall.** Quarantine until Quality releases, a way to rework or destroy a blocked batch, and a trace from a finished batch to customers and back to material batches
4. **One shared approval engine** before Procurement, with "you can't approve your own"
5. **Vendor down payments** and PO revisions, seen on the group's real POs
6. **Tax: Coretax output invoices, PPh 23, fixed asset depreciation (book and tax)**
7. **Month end: closing, FX revaluation, stock written down to NRV**

## Sources
- Process manufacturing: [Blytheco, process manufacturing ERP](https://blytheco.com/blog/posts/a-deep-dive-into-process-manufacturing-erp-for-formula-based-production), [BatchMaster, personal care and cosmetics](https://www.batchmaster.co.in/en-au/erp-software-for-personal-care-and-cosmetics), [Deacom overview](https://www.erpresearch.com/en-us/deacom-erp), [BatchMaster, inventory valuation](https://www.batchmaster.com/blog/inventory-valuation-process-manufacturing/)
- Cosmetics GMP: [Registrar Corp, ISO 22716 batch traceability](https://www.registrarcorp.com/blog/cosmetics/iso-22716/iso22716-batch-traceability/), [SG Systems, ISO 22716](https://sgsystemsglobal.com/?p=16831), [Ecomundo, cosmetics GMP](https://ecomundo.eu/en/blog/understanding-gmp-cosmetic-products-guide)
- Indonesia, products: [BPOM, 21 notifications revoked](https://www.pom.go.id/siaran-pers/bpom-cabut-21-izin-edar-kosmetik-dengan-komposisi-tidak-sesuai-dengan-yang-didaftarkan), [Cekindo, cosmetic registration](https://www.cekindo.com/id/blog/proses-registrasi-produk-kosmetik-di-indonesia), [izin.co.id, PKRT](https://izin.co.id/blog/pkrt-adalah/), [Kemenkes, PKRT licensing guide](https://farmalkes.kemkes.go.id/unduh/pedoman-bimbingan-teknis-perizinan-perbekalan-kesehatan-rumah-tangga/)
- Indonesia, tax and payroll: [IKPI, DPP nilai lain in Coretax](https://ikpi.or.id/?p=13685), [Ortax, Coretax invoices](https://ortax.org/membuat-faktur-pajak-besaran-tertentu-di-coretax), [Pajak.com, PMK 72/2023](https://www.pajak.com/pajak/rincian-pokok-pmk-72-2023-soal-penyusutan-dan-amortisasi-pajak/), [RSM, PMK 72/2023](https://rsm.global/indonesia/sites/default/files/media/events/2023/RSM%20Webinar%20-%20Implementation%20of%20PMK%20No.%2072%20Year%202023%20and%20The%20Accounting%20Impact.pdf), [Talenta, JP wage cap](https://www.talenta.co/blog/iuran-jaminan-pensiun-terbaru/), [BPJS JP cap letter 2025](https://ramco.com/hubfs/compliance-portal/surat-informasi-ketentuan-batasan-upah-jaminan-pensiun-tahun-2025.pdf)
- Buying and selling: [Cleverence, 3 way match and AP control](https://www.cleverence.com/articles/sage-documentation/accounting-behind-the-screens-purchases-4827/), [SAP Insider, P2P controls](https://sapinsider.org/articles/9-key-controls-to-mitigate-purchase-to-pay-compliance-concerns/), [Cleverence, sales order to invoice](https://www.cleverence.com/articles/for-business/sales-order-to-invoice-process-flow-4829/), [Stripe, order to cash](https://stripe.com/en-hu/resources/more/what-is-order-to-cash-o2c-heres-how-it-works)
- Stock and costing: [Cleverence, standard costing](https://www.cleverence.com/articles/for-business/standard-costing-inventory-valuation-4827/), [Cleverence, warehouse inventory process](https://www.cleverence.com/articles/for-business/warehouse-inventory-management-process-4827/), [Cleverence, put away strategy](https://www.cleverence.com/articles/3pl-business/how-to-implement-an-effective-put-away-strategy-5827)
- Planning and maintenance: [Cleverence, production planning and control](https://www.cleverence.com/articles/for-business/how-to-do-production-planning-and-control-4821/), [OxMaint, maintenance KPIs](https://oxmaint.com/article/mtbf-mttr-oee-maintenance-kpis)
- Controls and group: [Hyperbots, ERP financial controls](https://www.hyperbots.com/glossary/erp-financial-controls), [SafePaaS, segregation of duties](https://www.safepaas.com/?p=11725), [Trintech, intercompany eliminations](https://www.trintech.com/blog/what-are-intercompany-eliminations-a-guide-to-accurate-financial-consolidation/), [Apideck, multi entity ERP](https://developers.apideck.com/guides/multi-entity-erp-behavior)
- ERP products: [ERP Research, Odoo vs SAP Business One](https://www.erpresearch.com/compare/odoo-vs-sap-business-one-for-manufacturing)
