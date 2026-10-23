---
title: Start Here
type: hub
updated: 2026-10-21
tags: [internship, hub]
---

# Start Here

Main hub for the internship. This is also the first note Claude reads at the start of every session, so keep "Current focus" and "Last session" up to date.

## What this is
Internship project: one ERP that connects a holding with its six companies, plus building or tidying each company's website. Full details in [[Holding ERP]].

## Current focus
- [x] Phase 0: stack, deadline and repo chosen (see [[Pandu Roadmap]]). Hosting still open
- [x] Phase 1: Foundation built (slices 1 to 8, 10 and 11). Its open questions still wait, see [[Foundation Module]]
- [ ] Meet the supervisor with the questions below, starting with the ones marked "Ask supervisor" and the first S&M questions
- [x] Write the S&M build plan once the S&M questions that block Pandu are answered (approved 2026-10-14)
- [x] Sales and Marketing module: answer the open questions that block Pandu in [[Sales and Marketing Module]]
- [x] Build S&M: S0 to S9 done (quotes, price proposals, customer PO, suggested SPPR, sales order, price above the SPPR, PI and payments, DO / BAST, tempo credit check and handed to FAT, progress payment plan, returns with QC and kirim), plus the costing change R1 to R5. All planned slices built (see [[Sales and Marketing Module#Slices]])
- [ ] Ask supervisor: the new 2026-10-21 questions in [[Sales and Marketing Module]] (lower PO price needs Marketing, tighter PO term, `SO` prefix, price above the SPPR without waiting, prices per segment or customer)
- [x] Decide one shared approval engine before Procurement: built first in Finance (F0), Jingga 2026-10-22
- [ ] Finance (Phase 5, next, before Stock and Procurement): whole module planned and approved 2026-10-22, slices F0 to F16 in [[Finance Accounting Tax Module]]. F0 to F11 built. Next: F12 to F14
- [ ] Ask supervisor and FAT: the Finance open questions (auto invoice on signed BAST, chart of accounts, PPN rate, Om Leo's journals, approval bands)
- [ ] Merge `claude/stoic-volta-cs2r54` into `main` in the code repo
- [ ] Ask supervisor: the colleague's revised Marketing numbers (GP 27 / 17, FOH from the ledger, who sees cost) against ours, in [[Colleague Repo Comparison]], and the S6 photo retention
- [ ] Ask supervisor: the 4 S5 questions in [[Sales and Marketing Module]] (PPN rate and how it shows, paying a PI in parts, correcting a wrong payment, cancelling an order that has a payment)
- [ ] Ask supervisor: the S&M items marked *(ask supervisor)* and the 3 new S3 questions (order of payment terms, whether a PO with a different qty or price needs approval, Director deciding terms) in [[Sales and Marketing Module]]. Also: what does Pandu call its quotes today, to confirm the `SQ` in `SQ-1026-001`
- [ ] Get supervisor sign-off on the fresh repo, invite the colleague to both repos (see [[Colleague Repo Comparison]])
- [ ] Ask supervisor: what "real-time" means and whether live screens are wanted now (see [[Pandu Roadmap#Additional features]]), whether the import may be looser than the form (in [[Foundation Module]])
- [x] Cost method answered by Jingga: MAP builds the COGM, VA is value added (see [[Finance Accounting Tax Module#Costing and pricing]])
- [ ] Ask supervisor: the "super finance" role (Om Leo) and which journals it approves, and approval bands up to Rp 100 jt (in [[Finance Accounting Tax Module]] and [[Procurement Module]])
- [ ] Plan Excel export
- [ ] Later, from the architecture review: history labels per record, one login module, split user admin, merge the small option files
- [x] Set up a start-up script for cloud sessions: `.claude/hooks/session-start.sh` in the code repo installs packages and starts the machine's PostgreSQL 16 (not Docker), then runs `pnpm db:setup`. Works for all sessions once merged into `main`
- [ ] Ask HR how new NRPs are numbered (until then automatic ones look like PDT-00004)
- [ ] Answer the last slice 6 question in [[Foundation Module]]: demo items, once Pandu's real item list is known
- [ ] Answer the 3 new *(assumed)* user admin rules from slice 5 in [[Foundation Module]]
- [ ] Confirm the *(assumed)* items in [[Company Roles and Modules]]
- [ ] Finalize the full module list for [[Holding ERP]], built around PT Pandu first as the baseline (first draft of the other modules in [[Module Overview]])
- [ ] Company websites: list what each one needs in [[Company Websites]]
- [ ] Update both decks: Pandu is a soap manufacturer (Wening), not "industrial manufacturing"

## Ground rules
- Serio is the holding company. PT Pandu is still the baseline for development: build for Pandu first, then adapt Serio and the others from it

## Last session
**2026-10-23 (fourteenth session, continued)**
- Batches 2 and 3 done: F10 bank (reconciliation, bank and cash entries, statement CSV import), F11 tax (tax codes by date, PPN out, Coretax export), F12 actual FOH %, F13 Finance dashboard, F14 marketing cost
- Security fix: row security was off on 10 Finance tables (migration 0074), now checked by a test for every table
- Every slice had the strict review and a browser click through. 415 tests pass. All on `main`
- New questions in [[Finance Accounting Tax Module#Open questions]] (FOH % basis, income tax account, typed cost as COGM, marketing campaigns)
- Batch 4 too: F15 fixed assets, F16 year end and cash flow. Finance is done (F0 to F16), 425 tests pass, on `main`
- Production ported from the colleague's repo on branch `claude/production` (P1 to P4), pushed, not merged. What changed and why: [[Production Module]]
- Then a stock core on the same branch: warehouses, stock per batch, moving average in the item's one cost, opening stock; production orders now move real stock
- Production and the stock core merged to `main` (db4d861) on Jingga's word, 442 tests pass
- Still open: Next.js 16.4.0 prints a security advisory and stops build and dev once (a retry continues). Upgrade not run, Jingga to decide
- Next: answers to the questions in [[Production Module#Open questions]] and [[Finance Accounting Tax Module#Open questions]], then the rest of Stock (Phase 4) or Procurement

**2026-10-22 (fourteenth session)**
- Finance comes next, before Stock and Procurement (Jingga). Whole module planned and approved, slices F0 to F16
- Built F0 to F6 and F7a: the shared approval engine with a My approvals inbox, Finance setup (chart, banks and cash, tax codes), the journal engine, manual journals, month close and reopen, the statements, customer deposits with payment reversal, refunds on cancelled orders, and the invoice a signed BAST posts (with cancel, invoice again and the Faktur Pajak number), receipts with AR aging, and credit notes
- Every slice had the strict review, its fixes, and a browser click through. 388 tests pass. All on `main`
- New questions for the supervisor are in [[Finance Accounting Tax Module#Open questions]]
- Next: batch 2, F10 bank reconciliation and F11 tax

**2026-10-22 (thirteenth session, continued)**
- Built the costing change R1 to R3 from the colleague's branch, confirmed by Jingga: GP 27 / 17 with a refusal under the minimum, the SPPR floor (no more GM SPPR approval), cost seen only by the GM, Finance and Marketing Managers and the supers
- FOH: pricing keeps a standard FOH % set by Finance, the books follow PSAK 202 in Phase 5. Adopted, flagged for the supervisor
- Built S6 DO / BAST for CBD orders: create once the PI is fully paid, deliver, sign with photos or reject, order status from its lines (Closed for CBD once all signed), photos removed after 365 days. Stock going out when goods leave is flagged for the supervisor
- Then R4 (SPPR valid from a day, with history) and R5 (sales Areas with their cities, a customer's area from its city)
- Then S7 tempo: credit check on tempo orders with Sales Manager approval, tempo DO / BASTs, signed BASTs handed to FAT with a due date, a To invoice list
- Then S8 progress: a payment plan per order (DP, then parts with fixed due dates, following the colleague), the down payment PI before delivery, handed to FAT
- The S&M spec was split (history in [[Sales and Marketing Build Notes]]) and the colleague's repo is graphed each session; the vault is not graphed (not worth the tokens)
- Then S9 returns: Warehouse records the return of a rejected BAST, Quality records QC rounds (rework done by Quality or Production, counted), a pass is sent out again on a new DO / BAST. The four return questions and the permissions are flagged for the supervisor
- Then change M: Marketing is its own module in the app (Price monitor, Prices to approve, Pricing), like the colleague's repo
- 322 tests pass, clicked through in a browser
- Next: all planned S&M slices are built. Meet the supervisor with the flagged questions, then plan the next phase

**2026-10-21 (thirteenth session)**
- Installed graphify and set it up for the code repo (skill, code only graph, rule in the repo's `CLAUDE.md`, hooks). The session start hook now installs it and builds the graph in every cloud session, so the earlier "installed for this session only" is replaced. To cut tokens, Claude queries the graph before grepping
- `main` of the code repo was behind: S3c, S4 and S2d were only on a branch. Merged into `main` (fast forward)
- S5 compared with the colleague's repo, ERPNext and Odoo, then built: a CBD order gets a proforma invoice (order total plus 11 % PPN *(assumed)*), Finance records payments in parts (open, partly paid, paid), void with a reason, cancel voids an unpaid PI, and an order with a payment can't be cancelled. Strict review fixed 5 findings. 283 tests pass, clicked through in a browser
- 4 new questions for the supervisor, in [[Sales and Marketing Module]]
- Next: S6 DO / BAST, which needs the PI fully paid on a CBD order. Plan first, wait for approval

**2026-10-21 (twelfth session)**
- Analyzed the colleague's repo, ERPNext and Odoo from their code. New reference notes: [[Colleague Architecture]], [[ERPNext Architecture]], [[Odoo Architecture]], and [[ERP Best Practices]] comparing all of them per module. Every phase now starts with that comparison (step 0 in [[Workflow]], rule in the code repo's CLAUDE.md)
- Margins settled: 20 % default and 10 % minimum are GP. Built S3c: a suggested SPPR from the default GP (set by Marketing Managers), and an SPPR below the minimum GP waits for the GM
- Directors see costs and margins. Elimination added to the [[Glossary]]: the holding view crosses out sales between group companies as its own line, each company's books stay the same
- Built S4 sales order: S4a (a lower price on the customer's PO waits for Marketing), S4b (Create Sales Order from the accepted quote, lines from the PO), S4c (Warehouse confirms stock, Sales Manager cancels with a reason)
- Compared the colleague's Marketing plan (MK1 to MK5). Kept ours for who sets the SPPR and who sees margin. Built S2d: a price above the SPPR applies at once with a reason and Marketing sees it. Price lists per segment and customer planned, waiting for the supervisor. Market price survey, SPPR start date and Marketing reports added as ideas
- Strict review and a browser click-through after every slice. 271 tests pass. All work is on `claude/stoic-volta-cs2r54`, not merged into `main` yet
- The database stops when the cloud worker restarts: run the session start hook again
- Next: S5 PI and CBD. Before Procurement: decide one shared approval engine

**2026-10-14 (eleventh session)**
- Session start hook for cloud sessions merged into `main`: installs packages, starts PostgreSQL 16, runs `pnpm db:setup`
- S&M questions answered with Jingga (compared with the colleague's repo and two office whiteboards), Inbox sorted into the module notes, S&M build plan written and approved (S0 to S9, CBD first)
- Costing model settled: COGM (materials at MAP), COGS = COGM + FOH rate %, GP on the selling price, SPPR set by Marketing. Sales only sees the SPPR and proposes other prices. Marketing, Finance, the GM and the supers see costs and margins
- Built and merged into `main`: S0 (Marketing and Quality functions, customer terms, credit limit, NPWP, delivery addresses, item cost), S1 (quotes with versions, sent, revised, lost, expiry), S2 (Pricing page, price proposals, To approve), S3 (customer PO checked against the quote, looser term to the GM). Strict review after each. 246 tests pass
- Address, PIC name and PIC phone are now required for customers and vendors
- Quote numbers follow the group's official style, `SQ-1025-001`, from the PO templates Jingga shared. Those templates are Procurement POs (ours to vendors); S3's customer PO is the one Pandu receives
- GitHub Codespaces set up so the ERP runs in a browser tab, since it won't run on Jingga's laptop
- A dev server started right after `pnpm build` can get stuck reloading the login page. Restart `pnpm dev`
- CI for the S3 merge was queued at wrap up; S2's CI passed
- Next: S4 sales order (from the customer PO, payment type and term with their source, manual stock confirmed, cancel). It must wait for a pending term decision

**2026-10-13 to 14 (tenth session)**
- Slice 11 built: levels (Director, GM, Manager, SPV, Lead, OPR) and departments with a function instead of the 6 roles, the Company Admin switch, Roles per function and a Departments screen. Strict reviews after 11a and 11b. 188 tests pass, CI green
- Master data owners answered (Jingga): only Managers and up add and change on the master data screens. Vendors: Procurement. Customers: Sales and Finance. Items and units: Finance, Production and Warehouse. Adding from each module's own screens comes later. Migrations 0021 to 0023
- Customer payment term is for Sales and Finance. Customer NPWP is optional with the 15 or 16 digit check. Both arrive with Sales and Marketing
- Fixed a test race: the levels test added Sibama vendors while the vendors test counted their codes
- Earlier answers recorded: every sale in IDR, Net up to 180 days free per vendor, Director is read only plus approvals, Super User approvals above Rp 100 jt after Super Admin, vendor bank accounts seen by everyone who sees vendors
- Merged the slice 6 to 8 branch into `main` in both repos. CI green on `main`. Docker works, its service just needs starting in a new cloud session
- Asked whether live updates are needed at all. Decided: "real-time" means data is current when saved *(assumed, supervisor question)*, and slice 9 builds live master data lists anyway
- Grilling session on slice 9, the Excel import and the colleague's repo. Their import is no looser than their form, so ours makes the same checks as Add vendor
- Read Pandu's real supplier list. New rules from it: fax and several bank accounts kept (bank accounts hidden until Procurement), several PIC emails, the bank cell gets split
- Slice 10 plan (vendor import) written and approved in [[Foundation Module]]
- Jingga: skip slice 9 for now, build slice 10. Later moved live lists to [[Pandu Roadmap#Additional features]], so Foundation's build is done
- Built slice 10: Fax and several PIC emails on the forms, bank accounts kept but hidden, and Import from Excel on the Vendors list. Pandu's real supplier list imports as 7 new vendors with no errors
- Strict code review fixed 3 findings. 163 tests pass, CI green
- Architecture review of Foundation: 7 candidates. Built 3: one shared master record module, reads check permissions themselves (rule added to the code repo's CLAUDE.md), vendors and bank accounts written in one place. Strict review fixed 3 more findings. 168 tests pass, CI green
- Listed every open question for the supervisor, grouped by what each one unblocks (see Current focus)
- Docker in the cloud session needs a restart after the session's worker restarts (stale containerd files)
- Next: take the supervisor questions to the supervisor, then write the S&M build plan

**2026-10-13 (ninth session)**
- Built Foundation slice 6: items and units, plus an audit log at Jingga's request. Plan, defaults and questions are in [[Foundation Module]]
- Units and code format come from the colleague's repo: 11 starter units per company, codes like `PCS` and `WN-500`
- Every change to items and units is logged by PostgreSQL itself, and the item page shows its history
- Open question noted: seed demo items outside production? Not until Pandu's real item list is known
- Strict code review on slice 6, all 6 findings fixed. 98 tests pass, lint, typecheck and build pass
- CI did not run: it only runs on main and pull requests, and this work is on the working branch
- Jingga confirmed: everyone who can see an item sees its history. The Users dialogs now keep what was typed after a refused save
- Built Foundation slice 7, customers: codes numbered by the system (C-0001), group company link, order type (PO first by default) and rework report. Plan and answers in [[Foundation Module]]
- Strict code review on slice 7: shared list and record page layouts, ready for vendors. 111 tests pass
- Supervisor answers: codes fixed, Excel import and export needed, departments later, new users get an automatic NRP plus phone, KTP number and KTP scan
- Built slice 5d: automatic NRPs (PDT-00004), personal details apart from the login account, KTP scan only for people allowed to see it, every view logged. Review fixed 4 findings. 122 tests pass
- HR's employee sheet: name, NRP, NIK, phone and company at account creation, the rest later by HR. KTP scan switched off. Employee ID renamed NRP everywhere
- Built slice 5e: existing NRPs, optional email for staff (admins and managers need one), 24 hour temporary passwords, Reset Password, 15 minute lock after 5 wrong passwords. Review fixed 4 findings. 136 tests pass
- Passwords of 10 characters, a login lasts one work day. Tidied the super users (they belong to the holding)
- Built slice 8, vendors, with category, payment term, currency, NPWP and website from Pandu's supplier list. Review fixed 3 findings. 146 tests pass
- The contact person is called PIC on customers and vendors, like the supplier list. Payment terms stay per vendor (CBD or Net days); a shared list is a supervisor question
- All work is on the branch `claude/vault-slice-6-9vxzpp` in both repos, not yet merged into `main` (the decision is to push to `main`). CI only runs on `main` and pull requests, so it hasn't run on this work; lint, typecheck, 146 tests and the build pass locally
- Next: merge the branch into `main` in both repos, then slice 9 (real time, plan first) or the Excel import of Pandu's 7 suppliers. Ask HR how new NRPs are numbered

**2026-10-13 (chat, not a build session)**
- Added [[Module Overview]]: general ideas for Finance Accounting Tax, HR, GA, Production, WMS, IMS and Integrated System, plus RND, Engineering, Quality and MMS as names only
- Flagged overlaps to settle (Production Cost, PPIC) in that note
- Every module now has a linked spec note in the module tables, stubs for the ones with only a general idea

**2026-10-13 (eighth session)**
- Built Foundation slice 5, user admin, in three parts, each clicked through in a browser: Users list, Add user with an emailed set your password link, and Manage (change role, remove access, deactivate) plus the Roles screen
- Strict code review on slice 5, all 6 findings fixed. The biggest: permission checks now sit with the rules, and new tests prove a viewer and a Super User are refused every change
- New *(assumed)* rules to confirm in [[Foundation Module]]: nobody changes their own access, deactivate can be undone, no guard against an admin removing their own Roles permission
- New note [[Architecture]]: how the built code fits together, with links between the parts
- Installed graphify (turns a folder into a knowledge graph) for this session only. Not worth running yet, try `/graphify .` on the code repo after slice 9. A new session needs it installed again
- CI green on every slice 5 commit
- Next: slice 6, items and UoM. Start a new session and write the plan first

**2026-10-13 (seventh session)**
- Ran the app with pnpm dev and fixed the dev only warnings and the Next.js badge covering the sidebar
- Vault CLAUDE.md: Glossary in the read order, pointer to the code repo's CLAUDE.md, folder map updated
- Built Foundation slice 4: roles, permissions, and PostgreSQL keeping each company's data apart. Picker shows only your companies, another company shows "no access", Super User has a Read only badge
- Strict code review on slice 4, all 4 findings fixed
- Next: slice 5, user admin

**2026-10-13 (sixth session)**
- Renamed Maraloka to Sibama everywhere, logged the 404 status issue
- Strict code review of slices 1 and 2, all 5 findings fixed
- Built Foundation slice 3, login: email or NRP, forgot password by email (Mailpit locally), two-step login for Super Admin and Super User with backup codes
- New open questions in [[Foundation Module]]: password rules, how long a login lasts, account lockout
- Next: slice 4, roles and permissions

**2026-10-13 (fifth session)**
- Phase 0 closed in a grilling session: Better Auth, shadcn/ui, AWS Jakarta, company owned accounts, login by email or NRP, two-step login for the holding accounts. All in [[Holding ERP]]
- Company colors approved: the logo based themes from the colleague's repo
- Commits now show as jingaega, no Claude lines
- Built Foundation slice 2: the 6 companies with their own colors and logos, a company picker, and a themed page per company
- Next: slice 3, login

**2026-10-13 (fourth session)**
- Grilling session settled it: fresh repo `jingaega/PT-SERIO-ERP` with the colleague, his repo as reference only, plain PostgreSQL on AWS later
- Foundation slice 1 is built: Next.js, PostgreSQL in Docker, Drizzle, health check, CI, `CLAUDE.md`. Pushed to the repo's working branch
- Added the [[Glossary]] and moved the key terms into it
- Next: supervisor sign-off, then slice 2 (companies with themes)

**2026-10-13 (third session)**
- Compared the vault with the colleague's repo `Difsyy/Serio-Intern`, which builds the same ERP and is much further along. See [[Colleague Repo Comparison]]
- New calls logged in [[Holding ERP]]: Next.js and PostgreSQL (local first), English UI, Super Admin and Super User, intercompany later, minimum margin check instead of floor price, Wening confirmed, the 6 role templates for now
- Next: decide with the colleague and supervisor whether we contribute to their repo or build our own

**2026-10-13 (second session)**
- Phase 0 closed: stack approved (can change), deadline flexible, repo is `jingaega/PT-SERIO-ERP`, UI in Indonesian. All logged in [[Holding ERP]]
- Wrote [[Foundation Module]] with placeholders for the super users, colors and roles, plus the Phase 1 build plan
- S&M spec: tempo first, added the Pandu baseline (group company orders start at the SO), fixed contradictions, new open questions
- Phase 2 goal is now quote to signed BAST, since Stock and Finance come later
- Next: answer the Foundation open questions, approve the build plan, then slice 1 (scaffold the repo)

**2026-10-13**
- Restructured the vault into folders and added this note
- Filled the company notes, fixed broken links, settled that Serio is the holding company and Pandu is the development baseline
- Sales and Marketing module spec is drafted, with 8 open questions waiting
- Added the workflow, Pandu roadmap, build log and Claude Code instructions
- Next: stack and deadline, then the S&M build plan

## Map
- Projects: [[Holding ERP]], [[Company Websites]]
- How we work: [[Workflow]], [[Pandu Roadmap]], [[Build Log]]
- For Claude Code: [[Claude Code Instructions]]
- Module specs: [[Foundation Module]], [[Sales and Marketing Module]] (history of its finished slices in [[Sales and Marketing Build Notes]]), [[Procurement Module]]
- Other modules (stubs, general ideas only): [[Module Overview]], [[Finance Accounting Tax Module]], [[HR Module]], [[GA Module]], [[Production Module]], [[WMS Module]], [[IMS Module]], [[Integrated System Module]], [[RND Module]], [[Engineering Module]], [[Quality Module]], [[MMS Module]]
- Reference: [[Glossary]], [[Company Roles and Modules]], [[Companies]], [[Colleague Repo Comparison]]
- Meetings: [[Meetings]]
- Inbox: [[Inbox]]

## Folders
| Folder | What goes in it |
|---|---|
| Inbox | Raw dumps: meeting scribbles, whiteboard photos, chat pastes. New notes land here by default |
| AI-Draft-Notes | Drafts Claude writes that you haven't reviewed yet. Move them out once they're approved. Created only when needed |
| Projects | One folder per project. Project note, decisions log, module specs |
| Companies | One note per PT, plus the Companies index |
| Meetings | One note per meeting, named `YYYY-MM-DD Topic` |
| Daily | Daily notes, named `YYYY-MM-DD` (made by the Daily notes button). Created only when needed |
| Templates | Daily, meeting and module spec templates |
| Attachments | Images and files pasted into notes. Created only when needed |

## Conventions
- *(assumed)* means my own reading, not confirmed yet. When someone confirms it, remove the marker and link the meeting note where the answer came from
- Every decision goes in the decisions log in [[Holding ERP]], with date and reason
- One note per company, per module, per meeting. Each one links back to its project note
- Plain and short writing. No em dashes or en dashes
- Plain markdown, callouts and mermaid only. No Dataview or other plugins, since Claude only sees the raw text

## Working with Claude
- Start a session with "read the vault, we are on [module]"
- Drop raw material in Inbox, then ask Claude to process it into the right notes
- At the end of a session, say "wrap up" and Claude updates Last session, the roadmap and the build log
- Full process in [[Workflow]]
