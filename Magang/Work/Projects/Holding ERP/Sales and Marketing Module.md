---
title: Sales and Marketing Module
type: module-spec
project: "[[Holding ERP]]"
status: draft
created: 2026-10-13
tags: [internship, erp, module, sales-marketing]
---

# Sales and Marketing Module

Part of [[Holding ERP]]. Company context is in [[Company Roles and Modules]].

## Overview and scope

The S&M module starts when a quotation is recorded. Everything before that (RFQ, pricing, PMS sourcing, making the quotation) is done manually and stays outside the ERP.

- **In the ERP:** quote record, customer PO, SO, DO / BAST, delivery and receive status, returns and rework
- **Handed to other modules:** stock (IMS/WMS), purchasing or production (Proc/Prod), invoice and payment (FAT)
- **Flows covered:** Tempo (default, credit terms), CBD (cash before delivery), and the Kopken work-first template
- **First build:** CBD first, then tempo and progress (down payment), all starting at Quote recorded. COD and Kopken come later (decisions in [[Holding ERP]], 2026-10-14)
- **Not in Pandu's first release:** POS, CRM and website leads. They are needed later for Anggoan's own selling

> [!warning] Consequence
> Lost-deal reports only cover quotes that were actually recorded. Deals lost before a quote exists are never seen by the ERP.

## Key terms
All terms are in [[Glossary]].

## Records the module owns

Five records carry a deal from quote to delivery. Each one links to the one before it, so any order can be traced back to its quote. Orders from group companies are discussed later (see Pandu baseline).

| Record | Main fields | Statuses | Links to |
|---|---|---|---|
| Quote record | Customer, version (v1, v2...), lines (item, qty, price), total (sum of the lines), payment type (required), validity date, date sent, attached file | Awaiting reply, Accepted, Revised (superseded), Lost | Customer |
| PO cust | PO number, items, qty, price, payment term, delivery address, scan or file | Received, Matched, Mismatch flagged | Accepted quote version |
| SO | Items with our item codes, qty, price, payment type (optional, falls back to quote), delivered qty, invoiced qty, paid qty | Confirmed, Partially delivered, Delivered, Closed, Cancelled | PO cust |
| DO / BAST | Items and qty sent, delivery date, photo proof, customer signature | Created, Delivered, Signed, Rejected | SO |
| Return | Items returned, reason, WMS received date, QC result, rework count | Received, In QC, In rework, Passed, Sent out again | Rejected DO / BAST |

Notes on the quote record:
- Logged at the moment it is sent, so the sent date and validity date are real
- It has item lines, not just a total. That lets the ERP check the PO cust line by line, run the minimum margin check per line, and copy the lines into the SO
- Nego happens outside the ERP. The sales person only records the outcome: accepted (PO cust comes in), revised (new version entered, old one superseded), or lost (reason required)
- Expiry is automatic. Validity date passes with no reply, status becomes Lost (expired). Jingga, 2026-10-14: keep it
- The sales person is warned 3 days before the validity date *(assumed number)*
- An expired quote is never revived. If the customer still wants it, sales makes a new quote version with fresh prices

## Tempo flow (default)

Deliver first, invoice after the BAST is signed, customer pays by the due date from their term. Read it top down. The ERP starts at Quote recorded.

```mermaid
flowchart TD
  subgraph Manual["Manual, outside the ERP"]
    RFQ --> Pricing["Pricing, PMS sourcing"] --> Quotation["Quotation made"]
  end
  Quotation --> Quote["Quote recorded<br/>awaiting customer reply"]
  Quote -->|revised, new version| Pricing
  Quote -->|lost| Lost["Lost<br/>reason required"]
  Quote -->|accepted| PO["PO cust<br/>locks the quote"]
  PO --> SO["SO<br/>payment type falls back"]
  SO --> Stock["Check stock<br/>IMS, WMS"]
  Stock -->|enough| DO["DO / BAST<br/>photo proof needed"]
  Stock -->|not enough| Proc["Proc / Prod<br/>buy or make"]
  Proc --> DO
  DO --> Delivery["Delivery<br/>WMS"]
  Delivery --> Receive["Receive<br/>BAST signed or rejected"]
  Receive -->|rejected| Return["Return loop<br/>QC, rework, kirim"]
  Return -->|kirim, new DO / BAST| DO
  Receive -->|sale booked| Invoice["Invoice<br/>FAT, due date by term"]
  Invoice --> Payment["Payment<br/>FAT"]
  Payment --> Closed["Closed<br/>all paid"]
```

Step notes:
1. **Quote recorded.** Copy of the quote sent to the customer, status awaiting reply. Nego happens outside the ERP. Outcome is one of accepted, revised (new version, back to manual pricing) or lost
2. **PO cust.** Scanned or input, linked to the accepted quote version, which then locks. Differences from the quote (qty, price, term) are flagged
3. **SO.** Built from the PO with our item codes. Tracks delivered, invoiced and paid quantities, including partial deliveries
4. **Check stock (IMS, WMS).** Enough goes to DO / BAST. Not enough goes to Proc/Prod (buy or make flag on the item), which then feeds DO / BAST
5. **DO / BAST.** One document, statuses created, delivered, signed. Can't be signed without a photo
6. **Delivery (WMS), then Receive.** Signed BAST books the sale and triggers the invoice. Rejected goes to the return loop
7. **Invoice and payment (FAT).** Due date from the effective payment term. Order closes when everything is paid

## Return, QC and rework loop

A rejected delivery comes back through WMS and has to pass QC before it ships again. Nothing goes back out without passing QC.

```mermaid
flowchart LR
  Receive["Receive<br/>BAST rejected"] --> Return["Return<br/>WMS received"] --> QC["QC<br/>pass or defect"]
  QC -->|pass| Kirim["Kirim<br/>new DO / BAST"]
  QC -->|defect| Rework["Rework<br/>fix, then recheck"]
  Rework -->|fixed| QC
```

QC is one step with two exits: defect goes to rework, pass goes to kirim. Rework always returns to the same QC step.

- Kirim creates a new DO / BAST linked to the original order. The original BAST stays Rejected, so the history is visible
- The return record keeps a rework count. The limit before scrap or a supplier claim is still open
- After QC, the customer gets the goods again (kirim) or a **credit note**: a document that takes the returned goods' value off what they owe, or leaves them a credit if they already paid. Jingga, 2026-10-14. The credit note needs invoices, so it comes with FAT (Phase 5). Until then only kirim. Who may issue a credit note and whether it needs approval is open
- This is the default loop. Kopken uses a Rework Report instead and gets no new BAST (see the Kopken template below)

## CBD flow

> [!note] First build
> Built first, before tempo (Jingga, 2026-10-14). It is also the default payment type.

In CBD the customer pays before anything ships, so there is no AR and no credit risk.

1. RFQ comes in (on the whiteboard this flow starts at RFQ, see open questions)
2. Check stock. If not enough, go to Prod/Proc, then back to check stock
3. Issue the PI (proforma invoice)
4. Customer pays the PI in full
5. Create the DO / BAST only after payment is confirmed
6. Delivery, customer signs the BAST
7. Order closed (prepaid, nothing left to collect)

> [!important] Rule
> The system blocks DO / BAST creation until the PI is marked paid.

## Kopken work-first template

> [!note] Later
> Not in the first build. Kopken belongs to Sancang, not Pandu.

For Kopken (handled by [[PT Sancang Anugerah Nusantara]], AC service jobs) the work is done first and priced after, the reverse of tempo.

1. Kopken supervisor reports the AC problem to Kopken head office (pusat)
2. Pusat assigns the job to Sancang. Sancang opens an RFQ / WO (the AC job ticket)
3. Work is done. DO / BAST is created with photo proof. No photo, no BAST
4. Quotation is made after the work, with the actual price
5. If Kopken complains, Sancang makes a Rework Report (photos, what was fixed) and goes back to the WO. No new BAST
6. No complaint: Kopken issues their PO
7. Invoice for the finished job, then AR and payment as normal

Build notes:
- Make this an order type on the customer ("work first" vs "PO first"), not hardcoded for Kopken. Same documents, different order
- The Rework Report requirement is also a customer setting
- Add an aging flag on BASTs with no PO yet. Work is already done, so a missing PO is money at risk
- Create the SO automatically when Kopken's PO arrives, so billing still starts from an SO

## Business rules

| Rule | How the system enforces it |
|---|---|
| Payment type lives on the quote | Required field on every quote version (cash, tempo + days, down payment, installment) |
| SO payment type comes from the quote | Taken from the accepted quote version, or from an approved PO cust mismatch (rule below) *(assumed, ask supervisor)* |
| Term change only through the PO cust | The SO has no free term field. Its term comes from the accepted quote. If the PO cust term differs (tempo 45 vs quoted 30), the mismatch is flagged: the GM approves it (Director if no GM) and it becomes the order's term, or sales makes a new quote version *(assumed, ask supervisor)* |
| Quote locks on PO | Once PO cust is linked, that quote version can't be edited |
| Loss needs a reason | Price (customer says too expensive) *(assumed reading of "price too low")*, customer cancelled, no response, expired (auto) |
| Minimum margin check | A quote line with a GP % ((price - COGS) / price) below the company minimum (10 %) needs approval by a Manager or up in Marketing before the quote is sent. Only Marketing and the supers see the margin. Decided 2026-10-13, details 2026-10-14 from the whiteboard and the colleague's repo, supervisor to confirm |
| No photo, no BAST | DO / BAST can't move to Signed without at least one photo |
| Stock goes out when goods leave | Stock drops when the goods leave the warehouse, so nobody else is promised them *(assumed, ask supervisor and FAT)*. Manual until stock exists (Phase 4) |
| Sale is booked on BAST | Finance books revenue and its COGS together when the BAST is signed, not when the PO arrives or the goods leave. Signed BAST also triggers the invoice (tempo). COGS at the same moment is *(assumed, ask supervisor and FAT)* |
| DO / BAST is one document | One record with statuses (created, delivered, signed, rejected), not two |
| Nothing ships without passing QC | Returned goods go through QC. Rework always goes back to QC before kirim |
| Kirim uses a new DO / BAST | Linked to the original order. The original BAST stays Rejected for history |
| Tempo invoice due date | Comes from the effective payment term (SO or quote) |

## Handoffs to other modules

> [!info] The split that matters
> PMS only gets a price before the customer commits. Proc/Prod spends real money, so it only opens after the SO is confirmed.

| Module | What it does for S&M | Where in the flow |
|---|---|---|
| PMS | Vendor price inquiry when there's no price yet. Nothing gets bought | Before the quote, manual, outside S&M |
| IMS / WMS | Stock check, DO / BAST, delivery, receiving, returns received | Check stock through receive, and the return loop |
| Proc / Prod | Buys or makes the goods when stock is short. Decided by a buy-or-make flag on the item | After SO, when check stock says no |
| FAT | Invoice, AR, payment, books the sale on signed BAST | After receive |

## Pandu baseline
- **Who Pandu sells to:** outside customers and other group companies. Anggoan is the main one, a B2B buyer, and goes through S&M
- **Group company orders:** intercompany is discussed later. Until then Anggoan is handled like any other customer
- **Outside customers:** full tempo flow, starting at Quote recorded
- **Before Stock and Finance exist (Phase 2):** check stock is a manual "stock confirmed" step on the SO, and a signed BAST sets the order to "handed to FAT". Real stock and invoices are wired in at Phases 4 and 5 (see [[Pandu Roadmap]])
- **What Wening changes:** batch and expiry on DO / BAST lines later, when Production and Stock exist (Phase 6)
- **What the holding view excludes:** Pandu's sales to group companies. Only sales to outside customers count as group revenue

## Open questions
- [ ] Does CBD also start at quote recorded, or at RFQ like the whiteboard? Decides if both flows share one entry point
- [x] Credit gate before the SO for tempo customers, like the colleague's repo. Jingga, 2026-10-14. Each customer has a credit limit (0 by default). Exposure = open SOs not yet invoiced + unpaid invoices + the new SO. Over the limit: a warning, the SO waits for approval by a Manager or up in Sales, and no stock is held until approved. Overdue invoices are not checked separately *(assumed)*
- [ ] Rework limit: after how many failed QC rounds does it go to scrap or a supplier claim?
- [x] Check stock is automatic, like the colleague's repo: available stock is held for the SO when it is confirmed, and a shortage goes to Prod/Purch. Same for every payment type. Jingga, 2026-10-14
- [x] Until stock exists (Phase 4), check stock stays a manual "stock confirmed" step on the SO. It becomes automatic in Phase 4. Jingga, 2026-10-14
- [x] A lost deal counts as Closed, like the whiteboard. Jingga, 2026-10-14 *(assumed, ask supervisor to confirm)*
- [ ] Kopken revenue timing: the price only exists after BAST, so when does finance book the sale?
- [x] Minimum margin: settings from the colleague's repo, formula from the office whiteboard ([[2026-10-14 Whiteboard costing and pricing]]). Jingga, 2026-10-14. **Ask supervisor to confirm.** Margin is gross profit on the selling price: GP % = (price - COGS) / price x 100. COGS is the item's moving average cost, like the colleague's repo *(assumed, ask supervisor)*. One % per company: default 20 %, minimum 10 %, both changeable per company *(colleague's numbers)*. The GP formula is the whiteboard's *(ask supervisor to confirm)*. Below the minimum, a Manager or up in Marketing approves before the quote is sent
- [ ] **Ask supervisor:** confirm moving average as the cost method for every item (the whiteboard also lists standard price and one that reads "VA"). Claude's view: moving average is the usual choice in Indonesia, allowed by PSAK 14 and by tax rules (average or FIFO, used consistently), and simple. Picking the oldest batch first for expiry is a warehouse rule and works with any cost method. Standard price can be added later for finished goods if Production wants to compare planned and real cost. Jingga reads the "15 to 22" on the whiteboard as nothing important
- [x] Margin and cost are seen **only by Marketing and the Super Admin and Super User**. Nobody in Sales sees them, so they can't see the profit and push for a raise. Jingga, 2026-10-14, from the Inbox note "Marketing may have a margin, sales may not" *(ask supervisor to confirm)*. A quote line below the minimum only shows "needs approval". The colleague's repo differs: every sales staff who sees or makes a quote sees each line's margin
- [x] A quote below the minimum margin is approved by a Manager or up in Marketing, since Sales can't see margins. Jingga, 2026-10-14 *(ask supervisor to confirm)*
- [x] Until stock exists (Phase 4), Finance types a cost on each item (a standard price) and the margin check uses it. Jingga, 2026-10-14 *(assumed, to change in Phase 4 when the moving average cost exists)*
- [x] Down payment is its own payment type, called progress: a down payment before delivery, then the rest in parts, like the colleague's repo. Built in the first build. Jingga, 2026-10-14. COD comes later. Was: Is down payment its own payment type, or tempo with a first installment? Partly answered in meeting [[2026-10-14]]: sales payment types are ours to decide, default CBD, with COD, progress payment and Net up to 180 days. Which ones S&M builds first still to decide
- [ ] Later, with intercompany: does Anggoan (or another group buyer) get a quote first, or does its PO go straight to an SO?
- [ ] What is PMS? It is not one of the 12 modules. Is it sourcing inside Procurement?
- [ ] Is "cash" on the quote the same as CBD? Installment has no flow yet
- [x] SO term override is approved by the GM. Jingga, 2026-10-14. **Ask supervisor to confirm** where it happens: Claude suggested on the PO cust mismatch, not as a free field on the SO, and Jingga agreed (2026-10-14). Only a looser term (more days, or tempo instead of CBD) needs approval *(assumed)*. A company with no active GM falls back to a Director *(assumed)*
- [ ] Credit note after a return: who issues it, and does it need approval? (comes with Phase 5)
- [ ] Can a BAST be partly rejected, with some lines accepted and invoiced?
- [x] Every sale is in IDR. Meeting [[2026-10-14]]
- [ ] Does a customer need several delivery addresses for the DO / BAST? Foundation keeps one address and one contact per customer for now. The colleague's repo has several of each
- [ ] Later: who owns the PI and its paid status in CBD, S&M or FAT? CBD has no PO cust or SO in the flow, but DO / BAST links to an SO
- [ ] Later: Kopken creates the BAST before any PO or SO exists, and the WO and Rework Report are not records yet. How do they link?

## Acceptance checks
Written before the build. Each one is something Jingga can click through.
- [ ] 

## Build plan
Filled in by Claude and approved before any code.
- Data model:
- Screens:
- API:
- Slices (small, one record end to end):
  - [ ] 
