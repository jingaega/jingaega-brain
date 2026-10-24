# Production Revamp: next session

## The prompt to paste

```
Read the vault at C:\Users\Sora\Documents\GitHub\jingaega-brain\Magang\Work, starting with
00 Start Here.md, then Projects/Holding ERP/Production Revamp Handoff and
Projects/Holding ERP/Production Module.md.

We are on PT-SERIO-ERP, branch feature/production-revamp. The Production revamp (P5 to P11) is
built but not reviewed, not clicked through and not committed. Run the thermo-nuclear code
review on the branch, fix what it finds, click through the planner and one shift in the
browser, then commit.

Local setup is done: Docker Desktop runs, PostgreSQL and Mailpit are up, pnpm 10.28.0 is
installed, and .claude/hooks/session-start.sh works on Windows. The dev server is on :3000.
```

## Skills to use

| Skill | When |
|---|---|
| `thermo-nuclear-code-quality-review` | First. The repo's rule: every finished slice gets the strict review before the slice is called done. It lives at `.claude/skills/thermo-nuclear-code-quality-review` in the repo |
| `graphify` | For any question about the code. `graphify explain "<name>"` first, `graphify query` only when there is no name to look up. Our graph is in `graphify-out/`, the colleague's in `~/graphs/serio-intern/` |
| `codebase-design` | If the review finds the production code grew too many seams |
| `domain-modeling` | If the susut, the shift or the planner need clearer words in the Glossary |
| `web-design-guidelines` | The planner table is a new kind of screen in this app |
| `security-review` | Before the commit, since the branch touches permissions and company isolation |

## The three things that will trip you up

1. **Two tests fail that are not from this work.** `tests/auth.test.ts` and `tests/year-end.test.ts` both fail on a clean tree, checked by stashing the branch on 2026-10-24. Do not chase them as if the revamp broke them. Everything else passes: 452 tests, lint and typecheck green
2. **The migrations need `IF EXISTS`.** 0084 and 0085 were written by hand and re-applied several times while the schema was being worked out. They now run from any state, but do not rename their triggers or columns
3. **`pnpm dev` and `pnpm build` stop once** on the Next.js 16.4.0 security advisory. A retry continues. It is not our code

## What the supervisor still has to answer

In `Projects/Holding ERP/Production Module.md`, under Open questions: the shift rule (one order per product per shift per day), the production code format, what happens to the stock and cost of the rejects, the QC release rule, and when Pandu's real item list arrives (the seeded items, costs and SPPR are dummies).

## The rules Jingga confirmed on 2026-10-24, so do not ask again

- The planner takes a base liquid you already hold, e.g. 500 L of Cupir
- Each row of the planner is an **alternative, OR not AND**
- Whole packages only: you cannot sell half a jerrycan
- Chemical against packaging cost: classify the **item** once, inherited by every BoM line
- Quality attributes: tests per item with limits, not fixed columns
- The shift is on the order, one order per product per shift per day, a repeat refused
- The susut is **measured after production** from the volume balance, not guessed from the recipe
