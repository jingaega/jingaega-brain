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
- [ ] Ask supervisor: is there a fixed floor price per item? (open question in [[Sales and Marketing Module]])
- [ ] Confirm the *(assumed)* items in [[Company Roles and Modules]]
- [ ] Finalize the full module list for [[Holding ERP]], built around PT Pandu first as the baseline
- [ ] Company websites: list what each one needs in [[Company Websites]]
- [ ] Update both decks: Pandu is a soap manufacturer (Wening), not "industrial manufacturing"

## Ground rules
- Serio is the holding company. PT Pandu is still the baseline for development: build for Pandu first, then adapt Serio and the others from it

## Last session
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
- Reference: [[Company Roles and Modules]], [[Companies]]
- Meetings: [[Meetings]]
- Inbox: [[Inbox]]

## Folders
| Folder | What goes in it |
|---|---|
| Inbox | Raw dumps: meeting scribbles, whiteboard photos, chat pastes. New notes land here by default |
| AI-Draft-Notes | Drafts Claude writes that you haven't reviewed yet. Move them out once they're approved |
| Projects | One folder per project. Project note, decisions log, module specs |
| Companies | One note per PT, plus the Companies index |
| Meetings | One note per meeting, named `YYYY-MM-DD Topic` |
| Daily | Daily notes, named `YYYY-MM-DD` (made by the Daily notes button) |
| Templates | Daily, meeting and module spec templates |
| Attachments | Images and files pasted into notes |

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
