---
title: Workflow
type: process
project: "[[Holding ERP]]"
updated: 2026-10-13
tags: [internship, erp, workflow]
---

# Workflow

How Jingga, Claude (chat) and Claude Code work together on the ERP. Goal and order of work are in [[Pandu Roadmap]]. Back to [[00 Start Here]].

## Roles
| Who | Does | Does not |
|---|---|---|
| Jingga | Decides, answers open questions, runs meetings, reviews and accepts each module | |
| Claude (chat) | Writes and tidies specs, turns Inbox dumps into notes, plans builds, reviews work, keeps the vault current | Writes production code |
| Claude Code | Implements one module slice at a time from the spec, writes tests, keeps the repo clean | Changes a spec rule without asking, invents business rules |

## The loop (one per module)
0. **Compare.** At the start of each phase, refresh that phase's table in [[ERP Best Practices]]: what ERPs generally do, what we have, what the colleague's repo does ([[Colleague Architecture]]). Every Gap or Decide row becomes an open question in the module note or a line in its build plan. Jingga's rule, 2026-10-21
1. **Spec.** The module note in the vault is the source of truth. Use the Module Spec template. Open questions are listed, not hidden
2. **Plan.** Before any code, Claude writes a build plan at the bottom of the spec: data model, screens, API, and a task list cut into small slices. Jingga approves it
3. **Build.** Claude Code takes one slice at a time. A slice is one record working end to end (database, API, screen), not a whole layer
4. **Verify.** Run the acceptance checks written in the spec. Jingga clicks through it as the user
   - After every slice, Claude Code runs the strict code review (thermo-nuclear skill) on that slice and fixes what it finds before the next slice starts. Jingga's rule, 2026-10-13
5. **Log.** Update the status in [[Pandu Roadmap]], add decisions to [[Holding ERP]], and write the handoff in [[00 Start Here]]

Do not start the next module until the current one passes step 4. A module is done when its definition of done (below) is fully ticked.

## Definition of done (every module)
- [ ] Every record and status in the spec exists and works
- [ ] Every business rule in the spec is enforced, not just shown
- [ ] Acceptance checks pass, with the result noted in the spec
- [ ] It runs for Pandu with Pandu's real flow, using the company color theme
- [ ] Intercompany behavior is written down for the other companies, even if not built yet
- [ ] No *(assumed)* item is hiding inside the build. Each is either confirmed or listed as a known risk
- [ ] Roadmap status, decisions and handoff updated

## Rules for the AI
1. Read [[00 Start Here]], then [[Pandu Roadmap]], then the module spec, before doing anything
2. The spec wins. If the code and the spec disagree, stop and ask which one is right
3. Never invent a business rule. If the spec is silent, add it to Open questions and ask
4. Anything *(assumed)* can be built, but keep it easy to change and say so in the plan
5. An open question only blocks the slice that depends on it. Build the rest
6. Small commits, one slice each, with a message that names the slice
7. Ask before changing the data model of a finished module
8. At the end of a session, update the handoff and the Roadmap status

## Session start and end
**Start (Jingga says one line):** "Read the vault, we are on [module]."
Claude reports: current phase, what is done, the next slice, and any blockers.

**End:** "Wrap up." Claude updates:
- Last session in [[00 Start Here]]
- Status table in [[Pandu Roadmap]]
- Decisions and open questions
- [[Build Log]] with what was built or changed

## How information moves
| From | To | How |
|---|---|---|
| Meeting or chat | Inbox | Jingga dumps raw notes |
| Inbox | Right notes | Claude processes, ticks confirmations, logs decisions |
| Spec | Repo | Claude Code reads the vault through `CLAUDE.md` (see [[Claude Code Instructions]]) |
| Repo | Vault | Claude writes results into [[Build Log]] and the spec's acceptance results |

## Where things live
- Specs, decisions, status: this vault
- Code, tests, migrations: the repo
- Never keep the same fact in both. The vault says what and why, the repo says how
