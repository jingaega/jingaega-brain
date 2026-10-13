---
title: Claude Code Instructions
type: reference
project: "[[Holding ERP]]"
tags: [internship, erp, claude-code]
---

# Claude Code Instructions

The live copy is `CLAUDE.md` in `jingaega/PT-SERIO-ERP`, which now overrides this template. The block below is the original template. Start Claude Code with the vault added as an extra directory so it can read the specs, for example `claude --add-dir "C:\Users\Sora\Documents\Magang\Work"`. Process is in [[Workflow]].

```markdown
# ERP for PT Pandu Dwiloka Teknik (holding ERP)

Spec vault (source of truth, read first): C:\Users\Sora\Documents\Magang\Work
On GitHub the vault is `jingaega/jingaega-brain`, folder `Magang/Work`. Use that in cloud sessions.
Read in this order: 00 Start Here.md, Projects/Holding ERP/Pandu Roadmap.md, Projects/Holding ERP/Workflow.md, then the module spec for the current task.

## Goal
Build the ERP for PT Pandu first. Pandu is the development baseline. Serio is the holding company. Other companies, Serio included, are adapted later. One database, company_id on every record, each company has its own color theme. Simple but real-time.

## Rules
- The spec wins. If code and spec disagree, stop and ask.
- Never invent a business rule. If the spec is silent, ask and add it to the spec's Open questions.
- Anything marked (assumed) in the spec can be built but must be easy to change. Say so in the plan.
- Work in small vertical slices: one record end to end (database, API, screen). One commit per slice.
- Write a plan first and wait for approval before writing code for a module.
- Ask before changing the data model of a finished module.
- Add tests for every business rule in the spec's rules table.
- Intercompany is not in the first build. Do not build cross-company documents until it is discussed.

## Stack
Next.js with TypeScript (full stack) and PostgreSQL, local first. Decided 2026-10-13, can still change (see Holding ERP.md decisions).
- Query and migration tools: picked in the slice 1 plan
- UI language: English
- Roles: company admin, director, manager, supervisor, staff, viewer (for now)
- Super Admin can read and write in every company. Super User can only read, in every company

## When a session ends
Update Build Log.md, the status table in Pandu Roadmap.md, and the Last session section in 00 Start Here.md. Do not edit spec rules without approval.

## Style
Plain, short writing in docs and commit messages. No em dashes or en dashes.
```
