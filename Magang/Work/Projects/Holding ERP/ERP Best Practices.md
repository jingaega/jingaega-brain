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
| CRM | Lead (not yet qualified), then opportunity (need, budget, timing) with stages, lost reasons and competitors, a pipeline forecast; a won deal becomes a quote or order without retyping | Quote lost reasons only; CRM named for later | Inquiries and price requests, no pipeline | **Gap**, later. Pandu's quotes start by hand |
| Website leads | Web forms land as leads, checked against existing customers so no duplicates | Named in the S&M idea, websites are their own project ([[Company Websites]]) | None | Phase 9 |
| POS | Counter sale lowers ERP stock at once; opening float, cash in and out, end of shift count against expected; works offline | Sibama and Anggoan need it | None | Phase 9 |

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

## Phase 6: Make (Production and Quality)

| Topic | General | Ours | Colleague | Verdict |
|---|---|---|---|---|
| Formula | Versioned and approved; a batch scales from it; every batch matches the formula registered with the regulator (BPOM CPKB for cosmetics, Kemenkes for household products) | Needed for Wening | BoM versions, no approval, no registration link | **Gap, high**: approval and a registration number per formula |
| Production order | Plan, issue (in parts), finish with yield; scrap and by products; leftovers returned | Not built | Built, no scrap, no returns | **Decide** |
| Batch record | Every batch keeps materials and their batches, who, when, QC results | Needed | Issues logged, material batches not on the order | **Gap**: trace back to material batches |
| Planning | Master schedule, then materials planning, rough capacity check, campaigns for products that share a line | PPIC owner open | Restock list only | **Decide** what Pandu needs |
| Production cost | Labour and overhead absorbed into each batch at a rate, variances reported | FOH as a % of COGM in pricing | Materials only | **Decide** with Finance (Phase 5) |
| QC specs | Test parameters per item with limits, versioned, linked to the formula | Not in the notes | Specs per item with history | **Gap** |
| Incoming QC | Materials stay in quarantine until checked; sampling plans; vendor CoA checked against the spec | Not in the notes | A switch, off by default, raw material and packaging only | **Decide** |
| QC release | Finished batches stay in quarantine until Quality (not Production) releases them (ISO 22716, CPKB) | Required for Wening | A switch, off by default; blocked batches stuck | **Gap**: always on for Pandu, rework or destroy flow |
| CoA | Certificate of analysis made from the batch's results, sent with deliveries when a customer asks | Not in the notes | CoA PDF per batch | **Gap** |
| Deviations and CAPA | Non conformance record with severity and decision (use, rework, return, destroy), root cause and corrective action, followed up | Not in the notes | None | **Gap** |
| Supplier quality | Reject rate and late deliveries per vendor feed a vendor rating | Not in the notes | Reject rate by value | **Gap** |
| Complaints and recall | Complaints investigated, all affected batches found, recalled goods kept apart, regulator told | Recall wanted | None | **Gap, high** |

## Phase 7: People and places (HR, GA)

| Topic | General | Ours | Colleague | Verdict |
|---|---|---|---|---|
| Employee record | HR owns the employee and staff number; a login is linked to it | NRP on the user, HR owns it later | Employee first, generated numbers | **Decide** before HR |
| Contracts | PKWT and PKWTT with dates, probation, pay; reminders before a PKWT ends | Contract Management in the HR idea | Built, never edited, a new one ends the old | Same idea |
| Attendance | Check in and out (device, GPS or face), shifts, late and early, corrections by HR with a reason | Absensi in the HR idea | GPS against work locations, HR corrects | **Decide** how Pandu clocks in |
| Leave | Balances per year, carry over, cuti bersama, request and approval, calendar | Not in the notes | Built, policy by the Super Admin | **Decide** who sets the policy |
| Overtime, THR | PP 35/2021, Permenaker 6/2016 | Inbox notes | Built | Same |
| Payroll | Period locked before payment, payslips, bank transfer file, PPh 21 TER, BPJS with yearly caps | Inbox notes match | Built; JP cap typed in as the 2025 figure | Same source. **Gap**: caps must be settings, updated yearly |
| Payslip privacy | Pay seen only by HR payroll, the person and approvers; never in general audit views | Not in the notes | Payroll rights per permission | **Gap** to plan |
| Claims, loans, advances | Request, approve, pay, settle; advances declared with receipts | Inbox notes | Built, advances share the UM account | Same idea |
| Grades | Career paths, points, assessments, promotion changes pay with history | Inbox notes and meeting [[2026-10-14]] | Built | Same idea |
| Assets | One register shared by GA (where, who holds it, handover BAST) and Finance (cost, depreciation) | GA idea only | GA register, no depreciation | **Gap** (see Phase 5) |
| Supplies | Office stock issued to departments, cost to their cost center, restock request | Not in the notes | Built, no approval | **Decide** |
| Repairs and bookings | Facility requests with status; rooms and cars booked without double booking | Facility Management in the GA idea | Built | **Decide** what Pandu needs |
| Licences and documents | Company permits and contracts with expiry reminders (NIB, product registrations, vehicle STNK and KIR) | Not in the notes | Built, 60, 30, 7 day reminders | **Gap**: product registrations belong here too |
| Bills | Utilities per meter and month, approved, paid, booked to a cost center | Not in the notes | Built, PPN not split | **Decide** |

## Phase 8: Rest (R&D, Engineering, MMS)

| Topic | General | Ours | Colleague | Verdict |
|---|---|---|---|---|
| R&D projects | Stages from idea to launch with targets (cost, price, date) | Stub, nothing yet | Projects with stages and targets | **Decide** what Pandu's R&D does |
| Trials | Lab batches from a formula version, materials used booked as R&D cost | Stub | Trials use real stock, R&D expense | **Decide** |
| Stability | Accelerated and real time storage, checks like pH, viscosity, odor, micro; result sets the shelf life | Stub | Test rounds with storage days | **Gap**: shelf life from stability results |
| Launch | Only after stable results and the regulator's registration; the formula goes to Production as an approved version | Stub | Launch by the GM, a failed test only warns | **Gap**: registration and passed tests before launch |
| Change control | Change request, review, approval, released revision with an effective date; impact checked on open orders | Stub | ECR, approval by Manager then GM, BoM change made by hand | **Gap**: the approved change should create the new formula version |
| Machines and lines | Machine list with technical data, linked to the asset register | Production idea lists machines | Engineering owns machines and lines | **Decide** owner |
| Preventive plans | By calendar or running hours, checklists, parts kit | MMS meaning open | Built, due list made by a button | **Decide** |
| Work orders | Breakdown and preventive, downtime, parts used, technician | MMS meaning open | Built, no approval | **Decide** |
| KPIs | PM compliance (leading), MTBF, MTTR, OEE = availability x performance x quality (lagging) | Not in the notes | MTBF, MTTR, availability, no OEE | **Gap**: OEE needs output per line |
| Who maintains | In house or a contractor with service orders | Sancang does Pandu's plant | In house | **Decide**, ties to intercompany |

## Integrated System

| Topic | General | Ours | Colleague | Verdict |
|---|---|---|---|---|
| One inbox | Everything waiting for me across modules, in one list | Per module (Sales To approve) | One approvals inbox | **Gap**, comes with the shared approval engine |
| Notifications | In app, by email for urgent ones, made by the system when something waits or is decided | Email for login only | In app bell from approvals | **Gap** |
| My Workspace | One page per person: my requests, approvals, leave, assets | Not in the notes | Built | **Decide** |
| Document chain | From any document, see the ones before and after it (quote, PO cust, SO, DO, invoice, payment) | Quote shows its PO cust | Planned, not built | **Gap** to plan from S4 |
| Import and export | Excel import with all or nothing checks; export of every list | Vendor import built, export planned | Import for items and vendors | Same idea. Export still open |
| Search | One search across records | Not in the notes | None | Later |
| Holding view | Group dashboard for the supers: sales, AR, AP, stock per company and in total | Phase 9 | Company cards only | Phase 9 |

## Phase 9: Extend (other companies and the group)

| Topic | General | Ours | Colleague | Verdict |
|---|---|---|---|---|
| Intercompany sale | One transaction makes the buyer's PO and the seller's SO, with due to and due from; prices at arm's length | Later, "PO becomes SO at the other company" | Not in phase 1 | **Gap, high** for Pandu to Anggoan |
| Intercompany service | Service order at the provider, cost at the receiver (Sancang maintaining Pandu) | Later | None | **Gap** |
| Consolidation | Each company's books stay whole; the group view eliminates intercompany sales, balances and profit in stock | Later | Planned dashboard only | **Gap** |
| Shared master data | One group chart of accounts and item codes make consolidation easier | Items per company now | Per company | **Decide** |
| Trader (Sibama, Anggoan) | POS, price lists and discounts, landed cost on imports, fast picking | Named in [[Company Roles and Modules]] | Sales, POS not built | Phase 9 plan |
| Service company (Sancang) | Service orders and contracts tied to equipment and site, technician schedule, mobile close out offline, invoice from actual work, two way match for parts | Named | None | Phase 9 plan |
| Discrete maker (Serio) | Multi level BOM, routings and work centers, scrap per operation | Named *(assumed)* | BoM for process only | Phase 9 plan |
| Advisory (ETA) | Engagements, time sheets, billing | Finance only | None | Phase 9 plan |
| New company | Adding a company is configuration, not code | Rule in the repo | Rule in the repo | Same |

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
- Quality and change control: [ERP Research, quality management](https://erpresearch.com/erp/erpnext/quality-management), [AlisQI, supplier deviations](https://www.alisqi.com/en/modules/supplier-quality/supplier-deviation-management), [Acumatica, engineering change control](https://www.acumatica.com/media/2020/05/Acumatica_Manufacturing_Engineering_Change_Control_Datasheet_200916.pdf), [Guideflow, engineering change management](https://www.guideflow.com/blog/engineering-change-management-software)
- Stability testing: [Cosmetics and Toiletries, stability guidance](https://www.cosmeticsandtoiletries.com/testing/method-process/article/21837281/stability-testing-guidance-for-product-safety-and-shelflife-insight), [ALS, stability storage and testing](https://www.alsglobal.com/en/quality-control/stability-storage-and-testing), [CTPA, stability testing](https://www.ctpa.org.uk/e-learning/stability-testing-for-cosmetics)
- Service, POS and CRM: [ERP Research, field service](https://www.erpresearch.com/erp-modules/field-service), [SAP, field service management](https://www.sap.com/products/scm/field-service-management/what-is-field-service-management.html), [DMSI, POS for Business Central](https://dmsiworks.com/blog/pos-systems-for-business-central-what-to-look-for-and-what-most-get-wrong), [ERP Research, POS](https://www.erpresearch.com/erp-add-ons/pos), [ERPNext, sales pipeline](https://docs.frappe.io/erpnext/sales-pipeline.md), [SuperOffice, lead and opportunity management](https://superoffice.com/blog/lead-and-opportunity-management)
- ERP products: [ERP Research, Odoo vs SAP Business One](https://www.erpresearch.com/compare/odoo-vs-sap-business-one-for-manufacturing)
