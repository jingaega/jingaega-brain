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
- [ ] Maraloka or Sibama
