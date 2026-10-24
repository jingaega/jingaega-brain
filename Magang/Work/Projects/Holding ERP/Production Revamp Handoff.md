---
title: Production Revamp Handoff
type: handoff
project: "[[Holding ERP]]"
created: 2026-10-24
tags: [internship, erp, handoff, production]
---

# Production Revamp Handoff

What the next session needs to finish the Production revamp. Back to [[Production Module]], [[00 Start Here]], [[Pandu Roadmap]].

## Where the work is

- Code repo: `jingaega/PT-SERIO-ERP`
- Branch: **`feature/production-revamp`** (not committed yet)
- Vault: this note, plus [[Production Module]], [[Quality Module]], [[Glossary]], [[Pandu Roadmap]], [[Build Log]] and [[00 Start Here]] are updated

## What is built

| Slice | What | Where |
|---|---|---|
| P5 | the dummy Wening recipe book in Pandu | `db/seed.ts` |
| P6 | the item class (raw material, packaging, other) | `db/schema/master-data.ts`, `lib/item-supply.ts` |
| P7 | the planner, "500 L of Cupir, what can we make?" | `lib/production-planner.ts`, `lib/production-planner-queries.ts`, `app/[company]/production/planner/page.tsx` |
| P8 | the shift and the production code | `db/migrations/0084_production-shift-code.sql`, `lib/production-options.ts` |
| P9 | planned against actual, with the susut measured | `lib/production-orders.ts`, `app/[company]/production/orders/[id]/page.tsx` |
| P10 | quality readings per item, with limits | `lib/production-quality.ts`, `db/schema/quality.ts`, `db/migrations/0085_production-quality.sql` |
| P11 | the report, one line per shift | `lib/production-reports.ts`, `app/[company]/production/reports/page.tsx` |

Migrations: 0083 item class, 0084 shift and code, 0085 quality. All applied to the local database.

## What is left

1. **The thermo-nuclear code review** on the branch, per the repo's `CLAUDE.md` rule: every finished slice is followed by the strict review on that slice's commits, and its findings are fixed (or answered) before the slice is called done. The repo has the skill at `.claude/skills/thermo-nuclear-code-quality-review`
2. **A browser click through**, the way every slice before has been done:
   - Production -> **What can we make**: pick `CUP-BASE` and `500`, and check the alternatives: 12 jerrycans of 40 L, 98 of 5 L, with the chemical and packaging cost, the COGS, the selling price, the leftover and the estimated susut
   - Plan an order for one of them, pick shift A, and check the code comes out as `CUP-40L-2610A20`
   - Plan the same product, shift A, same day: refused with a clear message
   - Issue the materials, then finish: type the liquid in, the good product, the rejects and what was left over, and check the susut follows from them
   - The order page: the planned table beside the actual table, and the quality readings (pH, TDS, Bau on the liquid; pH, Warna, Busa on the soap)
   - Production -> Reports: the new **Per shift** section, one row per shift
3. **The commit.** Nothing on this branch is committed yet. The `.claude/` changes (the session start hook, the graphify guard, the launch config) were made earlier and are in the working tree too: decide whether they go in the same commit or their own
4. **The supervisor questions**, in [[Production Module#Open questions]]

## The prompt to start the next session

```
Read the vault at C:\Users\Sora\Documents\GitHub\jingaega-brain\Magang\Work, starting with
00 Start Here.md, then Projects/Holding ERP/Production Module.md and the Production Revamp
Handoff note.

We are on PT-SERIO-ERP, branch feature/production-revamp: the Production revamp (P5 to P11) is
built but not reviewed, not clicked through and not committed. Run the thermo-nuclear code
review on the branch, fix what it finds, click through the planner and a shift in the browser,
then commit.

Local setup: Docker Desktop is running, the database and Mailpit are up, pnpm 10.28.0 is
installed, and .claude/hooks/session-start.sh works on Windows. The dev server is on :3000.
```

## Skills worth using

| Skill | When |
|---|---|
| `thermo-nuclear-code-quality-review` | after the slices, before the commit. The repo's own rule |
| `graphify` | for any question about the code: `graphify explain "<name>"` first, `graphify query` only without a name. The graph is in `graphify-out/`, the colleague's in `~/graphs/serio-intern/` |
| `codebase-design` | if the review finds the production code grew too many seams |
| `domain-modeling` | if the susut, the shift or the planner need a clearer vocabulary in [[Glossary]] |
| `web-design-guidelines` | for the planner table, which is a new kind of screen in this app |

## Two things to know before starting

- **Two tests fail that are not from this work**: `tests/auth.test.ts` (the Super Admin sign in) and `tests/year-end.test.ts` (the year end close). Both fail on a clean tree, verified by stashing the branch on 2026-10-24. Do not chase them as if the revamp broke them, but they are worth a look at some point
- **The colleague's app is stopped.** His repo was run locally to read his costing and Quality code, then stopped to free the machine. His Supabase stack is stopped too, with a backup. Start it again with `pnpm bootstrap` in `C:\Users\Sora\Documents\GitHub\Serio-Intern` (needs Node 24 and pnpm 12: `npx -y pnpm@12.10.1`)

## The local machine, in short

- The dev server is on **:3000**, the colleague's was on :3001 (stopped)
- `pnpm dev` stops **once** on the Next.js 16.4.0 security advisory; a retry continues. Not a bug in our code
- The C: drive was nearly full (1.8 GB) on 2026-10-24 and killed Docker mid-pull. Jingga freed a lot after that; there is room now. Watch it if the Supabase stack is started again
- `pnpm build` needs the same retry
