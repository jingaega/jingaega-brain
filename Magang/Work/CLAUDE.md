# Internship vault (Holding ERP, PT Pandu first)

This is Jingga's Obsidian vault for the internship. It is the source of truth for the ERP project. Read this file first, then follow the order below.

## Read order
1. `00 Start Here.md` for current focus and the last session handoff
2. `Projects/Holding ERP/Pandu Roadmap.md` for the goal, phases and module status
3. `Projects/Holding ERP/Workflow.md` for how we work
4. The spec for the module being worked on, in `Projects/Holding ERP/`

## What the project is
One ERP that connects a holding with six companies. PT Serio Prima Indotek is the holding company. PT Pandu Dwiloka Teknik (soap maker, brand Wening) owns the ERP and is the development baseline: build for Pandu first, then adapt Serio and the other companies. Simple but real-time, one database, each company with its own color theme.

## Folder map
- `Inbox/` raw dumps. Process these into the right notes, then tell Jingga what can be deleted
- `Projects/` project notes, module specs, roadmap, workflow, build log
- `Companies/` one note per PT, plus Wening
- `Meetings/` one note per meeting, named `YYYY-MM-DD Topic`
- `Templates/` daily note, meeting note, module spec
- `AI-Draft-Notes/` drafts Claude wrote that Jingga has not reviewed yet
- `Claude Code Instructions.md` (in Projects/Holding ERP) is the template for the repo's own CLAUDE.md. This file is different: it is for working in the vault

## Rules
- *(assumed)* means a guess, not confirmed. Never remove the marker unless Jingga says it is confirmed, then link the meeting note where it was confirmed
- Never invent business rules. If something is unknown, add it to the note's Open questions
- Every decision goes in the decisions table in `Holding ERP.md`, with date and reason
- One fact lives in one place. Link to it, do not copy it
- Specs say what and why. Code says how. Do not put code in the vault except short examples
- Use plain markdown, callouts and mermaid. No Dataview or other plugin syntax, because Claude only sees raw text
- Use the templates in `Templates/` for new notes
- Every note links back to its parent (project, hub or index) and every new note gets linked from one

## Git
- Commit as Jingga: run `git config user.name "jingaega"` and `git config user.email "ganpique@gmail.com"` before the first commit of a session
- No `Co-Authored-By`, `Claude-Session` or "Generated with Claude Code" lines in commits or pull requests
- Push vault changes straight to `main`, so Obsidian picks them up

## Writing style
- Short and plain, like a student writing notes. Relaxed, not corporate
- No em dashes or en dashes anywhere. Use commas, periods or colons

## Session start
When Jingga says "read the vault, we are on [module]", report: current phase, what is done, the next slice of work, and any blockers or open questions that matter.

## Session end ("wrap up")
Update all of these:
1. `Last session` in `00 Start Here.md`
2. The status tables in `Pandu Roadmap.md`
3. New decisions and open questions
4. `Build Log.md`

## Safe editing
- Read a note before editing it. Jingga may have changed it since the last session
- Obsidian can overwrite a note that is open with its old copy. After writing, check the note again. If edits are missing, tell Jingga to close the note and redo the write
- Old duplicate files can exist at the vault root. Do not edit them, and flag them
- Deleting files needs Jingga's approval. Move to a `_to_delete` folder if deletion is not allowed
