---
title: Start Here
type: hub
updated: 2026-10-13
tags: [internship, hub]
---

# Start Here

Main hub for the internship. This is also the first note Claude reads at the start of every session, so keep "Current focus" and "Last session" up to date.

## What this is
Internship project: one ERP that connects a holding with its six companies, plus building or tidying each company's website. Full details in [[Holding ERP]].

## Current focus
- [x] Phase 0: stack, deadline and repo chosen (see [[Pandu Roadmap]]). Hosting still open
- [ ] Phase 1: answer the open questions in [[Foundation Module]] and approve its build plan
- [ ] Sales and Marketing module: answer the open questions that block Pandu in [[Sales and Marketing Module]]
- [ ] Get supervisor sign-off on the fresh repo, invite the colleague to both repos (see [[Colleague Repo Comparison]])
- [ ] Foundation slice 10: Excel import of vendors with Pandu's supplier list (plan approved, building now)
- [ ] Foundation slice 9: live master data lists. Plan approved, skipped for now (Jingga, 2026-10-13). Build once the supervisor says live screens are wanted
- [ ] Ask supervisor: what "real-time" means and whether live screens (slice 9) are wanted now, whether the import may be looser than the form, who may see vendor bank accounts (in [[Foundation Module]])
- [ ] Decide departments in roles (finance, procurement), needed for who manages items and customers (in [[Foundation Module]])
- [ ] Plan Excel export
- [ ] Ask HR how new NRPs are numbered (until then automatic ones look like PDT-00004)
- [ ] Ask supervisor: payment terms as a shared list (CBD, Net 14/30/45/60) or any number of days per vendor? Ours for now (in [[Procurement Module]])
- [ ] Answer the last slice 6 question in [[Foundation Module]]: demo items, once Pandu's real item list is known
- [ ] Answer the 3 new *(assumed)* user admin rules from slice 5 in [[Foundation Module]]
- [ ] Ask supervisor: can customers have their own currency, or is every sale in IDR? (in [[Foundation Module]])
- [ ] Confirm the *(assumed)* items in [[Company Roles and Modules]]
- [ ] Finalize the full module list for [[Holding ERP]], built around PT Pandu first as the baseline (first draft of the other modules in [[Module Overview]])
- [ ] Company websites: list what each one needs in [[Company Websites]]
- [ ] Update both decks: Pandu is a soap manufacturer (Wening), not "industrial manufacturing"

## Ground rules
- Serio is the holding company. PT Pandu is still the baseline for development: build for Pandu first, then adapt Serio and the others from it

## Last session
**2026-10-13 (tenth session, planning only)**
- Merged the slice 6 to 8 branch into `main` in both repos. CI green on `main`. Docker works, its service just needs starting in a new cloud session
- Asked whether live updates are needed at all. Decided: "real-time" means data is current when saved *(assumed, supervisor question)*, and slice 9 builds live master data lists anyway
- Grilling session on slice 9, the Excel import and the colleague's repo. Their import is no looser than their form, so ours makes the same checks as Add vendor
- Read Pandu's real supplier list. New rules from it: fax and several bank accounts kept (bank accounts hidden until Procurement), several PIC emails, the bank cell gets split
- Slice 10 plan (vendor import) written and approved in [[Foundation Module]]
- Jingga: skip slice 9 for now, build slice 10
- Next: slice 10a, then 10b

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
- Module specs: [[Foundation Module]], [[Sales and Marketing Module]], [[Procurement Module]]
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
