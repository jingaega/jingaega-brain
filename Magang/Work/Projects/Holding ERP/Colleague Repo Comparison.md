---
title: Colleague Repo Comparison
type: reference
project: "[[Holding ERP]]"
created: 2026-10-13
tags: [internship, erp, reference]
---

# Colleague Repo Comparison

Comparison between this vault and the colleague's ERP repo `Difsyy/Serio-Intern`, read on 2026-10-13. Part of [[Holding ERP]].

## What the repo is
The same ERP for the same 6 companies, with Pandu as the pilot. Next.js with Supabase (PostgreSQL). Much further along than this vault: per its changelog, Foundation, Procurement, Sales, Warehouse, Inventory, Finance, Production and HR already work for Pandu, with database tests and a staging deploy.

## Where we differ, and Jingga's call
| Topic | Vault | Colleague's repo | Jingga's call (2026-10-13) |
|---|---|---|---|
| Stack | Go, Next.js, PostgreSQL | Next.js, Supabase | Next.js and PostgreSQL, local first |
| UI language | Indonesian | English default, Indonesian available | English |
| Super users | 2 super users | Super Admin (read and write) and Super User (read only) | Same as the repo |
| Intercompany | PO becomes SO at the other company | Not in phase 1 | Discuss later |
| Where sales starts | Quote made by hand, ERP records it | Inquiry inside the ERP, missing prices go to Procurement | By hand. The repo is wrong for us |
| Floor price | Warning below the floor price | Minimum margin check, manager approves below it | Minimum margin check with manager approval |
| Pandu's products | Wening soap | Hand soap, dish soap, tire cleaner, glass cleaner. No Wening | Wening. The repo is wrong |
| Roles | Not decided | 6 templates: company admin, director, manager, supervisor, staff, viewer | Use the 6 templates for now |
| Maraloka short name | Maraloka | Sibama | Sibama (the company's own logo says sibama) |

All calls are logged in the decisions table in [[Holding ERP]].

## Outcome (grilling session, 2026-10-13)
Fresh repo `jingaega/PT-SERIO-ERP`, built together with the colleague. His repo is reference only. Plain PostgreSQL on AWS later, no Supabase. Logged in [[Holding ERP]].

Answered by Jingga:
- Same ERP, same supervisor. The company will run one ERP
- The colleague is open to changes, and Jingga has access to his repo
- Goal is an ERP the company actually uses
- His repo: too big, unneeded files, nobody reviewed it, unconfirmed rules
- The vault is Jingga's notes from discussions, partly confirmed and partly still unclear
- One [[Glossary]] in the vault

Not answered, Claude's suggestion used for now *(assumed)*:
- Rule status: only rules confirmed by the supervisor win over code. *(assumed)* ones can be built but must be easy to change. Open ones block only their slice
- Build Foundation together, then split by module (colleague on Procurement, Jingga on S&M)
- Company data isolation is enforced inside PostgreSQL, not only in the app

## Still to settle
- [ ] Supervisor signs off on the fresh repo decision
- [ ] Invite the colleague to `jingaega-brain` (vault) and `jingaega/PT-SERIO-ERP`
- [x] Maraloka or Sibama: Sibama

## Clashes found 2026-10-21
From a deep read of his repo for the modules we haven't built yet. His design per module is in [[Colleague Architecture]]. "Ours" is the vault or our code. Status: **ours** means we already decided, **open** means it waits for Jingga or the supervisor.

**Roles, access and login**
| Topic | His repo | Ours | Status |
|---|---|---|---|
| Director level | Retired, directors are Super Users | Director level kept: sees all, changes nothing, approves | Ours |
| Marketing function | None | Marketing owns SPPR and margins | Ours |
| Who sees cost | Every view right goes to everyone, so Sales reads average cost and BoM margins | Sales never sees cost or margin | Ours |
| Reversals | OPR and up cancel GR, IR, payments, UM, manual journals | Only a Manager reverses | Ours |
| Master data owners | Items: procurement, warehouse, production. Customers: Sales, staff may add. Vendors: OPR adds | Items: Finance, Production, Warehouse Managers. Customers: Sales and Finance Managers. Vendors: Procurement Managers | Ours |
| NRP | Always generated `PDT-00001`, lives on the HR employee | Lives on the user, legacy NRPs typed, automatic `PDT-00004` | Open: settle before HR |
| NIK and personal details | HR and the employee can edit | Typed once, the app can't change it | Open: settle before HR |
| Password, session | Must mix upper, lower, digit. 30 min idle sign out | 10 characters, no mix. 9 hour login | Ours |
| Two step login | Super Admin only | Super Admin and Super User | Ours |
| Attendance | HR only, GM can't see | GM sees everything except Settings | Open |
| Leave policy | Super Admin only | Company HR admin | Open |

**Approvals and numbering**
| Topic | His repo | Ours | Status |
|---|---|---|---|
| Above Rp 100 jt | One Super Admin step from Rp 25 jt up, Super User never approves | Super Admin, then a Super User gives the final approval | Ours |
| Bands up to Rp 100 jt | Manager to Rp 5 jt, GM to Rp 25 jt | Not set | Open (supervisor) |
| Journal approval | Manual journals only, by the Super Admin | Om Leo approves journals of every company | Open (which journals) |
| Payroll, THR, promotions, stock counts | Super Admin approves | Would fit the Director level | Open |
| Approval engine | One shared engine for every document | Per feature so far (price proposals, term decisions) | Open: build one shared engine before Procurement |
| Document numbers | `PO-PDT-2026-10-0012`, unique in the group | `SQ-1026-001`, per company, no company code | Ours. Holding level lists need the company shown next to the number |
| PO numbers | One sequence, no revisions | Local and import apart, revisions R1, R2 | Open |

**Sales**
| Topic | His repo | Ours | Status |
|---|---|---|---|
| Start of a sale | Inquiry in the ERP, price requests to Procurement | Quote made by hand, ERP records it | Ours |
| Price | Price lists per customer or segment, Sales types prices | SPPR per item, Sales only proposes | Ours |
| Margin | Markup on cost, default 20 %, minimum 10 % | GP on the selling price, COGS = COGM x (1 + FOH) | Ours. Same numbers, but ours are GP (Jingga, 2026-10-21) |
| Payment type | On the customer | On each quote version | Ours |
| Customer PO | A few fields on the SO | Its own record, checked line by line against the quote | Ours |
| Progress balance due | 30 days after the order date | 30 days after delivery | Ours |
| Who marks a PI paid | Sales staff or accountant | Finance Manager and up | Ours |
| Stock shortage | Draft PR | Make items go to Production first | Ours |
| Revenue and COGS | COGS when goods leave, revenue on a manual invoice | Both on the signed BAST | Ours |
| Returns | From any delivery, QA by Sales or Warehouse, one rework pass, no re QC | From a rejected BAST, QC by Quality, rework loops back to QC | Ours |
| Pandu's products | Hand soap, dish soap, tire cleaner, glass cleaner | Wening | Ours |

**Procurement and Finance**
| Topic | His repo | Ours | Status |
|---|---|---|---|
| Currencies | IDR only | 7 currencies, one per vendor, BI rate locked when paid | Ours |
| Payment terms | CBD or Net 0 to 365 | CBD or Net 1 to 180 | Ours |
| Down payment to a vendor | Not possible, only an employee advance (UM) after which the PO is paid on receipt | DP 50 %, TT 100 % in advance and CBD seen on real POs | Open: how a vendor DP works |
| Update PO | Not a PO edit, the price change lands on the invoice hold | Down payment flow has "update PO" | Open |
| Vendor required fields | Name only | Address, PIC name, PIC phone | Ours |
| Bank numbers in the audit log | Logged | Never logged | Ours |
| "Cost" item type | Goes to an asset account "costs to be capitalised" | 4 types: cost, expense, inventory, and a missing fourth | Open |
| Invoice edits | Invoice never edited, only cancelled and entered again. Part payments allowed | Payment can't be edited, invoice can | Open |
| PO print | No company NPWP, quote ref, discount, incoterms, freight, signatures | The group's newer layout has them | Open |
| COGS | Materials only, no labour or overhead | COGM + DL + FOH | Ours |

**Stock, production and the rest**
| Topic | His repo | Ours | Status |
|---|---|---|---|
| Machines and maintenance | Machines in Engineering, maintenance in its own MMS module | Listed under Production, MMS also its own module | Open: our notes contradict each other |
| Who maintains Pandu's plant | Pandu's own department | Sancang, as an intercompany service | Open |
| Restocking finished goods | PR, like any item | Pandu makes them | Open: needs a "make" path |
| PPIC | No planning at all, only a restock list | Shared by IMS and Production | Open |
| QC per batch | A switch, off by default. Blocked batch stays stuck | Required for Wening | Open: rework or write off flow |
| Recall | No trace of a batch to customers or materials | Every batch traceable for recall | Open |
| Analysis | Slow moving and ABC count sales only, so every raw material looks slow | Inventory control for production too | Open |
| QR or barcode after GR | None | Scanning after GR | Open |
| Picking order | Largest bin first | Not set | Open |

## Their Marketing plan (2026-10-21)
The colleague's Claude read our vault and planned Marketing for his ERP in five steps (MK1 margin rule and monitor, MK2 market price survey, MK3 price change requests, MK4 quote approval below the SPPR, MK5 margin reports). How it lands for us:

| Topic | Their plan | Ours | Verdict |
|---|---|---|---|
| COGS | Adds the FOH % to materials | COGM x (1 + FOH %) | Same now |
| Margin | Switches from markup to GP on the price | GP on the price | Same now |
| Price lists | Per segment or customer | One SPPR per item, segment and customer prices planned on top | Open, ask supervisor (decision 2026-10-21 in [[Holding ERP]]) |
| Who changes the SPPR | Sales requests, Marketing Manager approves (MK3) | Marketing sets it from the suggested SPPR; Sales proposes per quote line | Ours, Jingga 2026-10-21 |
| Price above the SPPR | No approval | A reason, no wait (S2d) | Same now *(ask supervisor)* |
| Price below the SPPR | Marketing Manager approves (MK4) | Marketing approves | Same |
| Who sees cost and margin | Sales sees each line's margin | Marketing, Finance, GM, Director, super roles; never Sales | Ours, confirmed 2026-10-21 |
| Market price survey | MK2 | Not yet | Idea taken, open question |
| SPPR start date and history | MK3 | Change log only | Idea taken, open question |
| Margin and voice of customer reports | MK5 | Wanted, not in the first build | Idea taken, open question |

Their read of our vault was out of date on three points: the Director sees costs too, an SPPR below the minimum GP waits for the GM (S3c), and the holding view eliminates sales between group companies as its own line instead of just leaving them out.

