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
| Maraloka short name | Maraloka | Sibama | Not asked yet |

All calls are logged in the decisions table in [[Holding ERP]].

## Still to settle
- [ ] Contribute to the colleague's repo, or build our own in `jingaega/PT-SERIO-ERP`? Talk to the colleague and the supervisor first
- [ ] If we build our own: is Supabase OK, since it is PostgreSQL and runs locally too, or plain PostgreSQL only?
- [ ] If we contribute: the two points where the repo is wrong for us (sales start, Wening) need to go to the colleague
- [ ] Maraloka or Sibama
