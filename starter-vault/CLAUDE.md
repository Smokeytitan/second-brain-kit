# Claude Profile

## About Me
- **Name:** [Your Name]
- **Role:** [Your Role] at [Your Company]
- **Reports to:** [Manager Name]
- **Works with:** [Key collaborators — names and roles]
- **Key constraint:** [Any constraints worth knowing — budget, timeline, dependencies]

## Active Projects
<!-- Add one line per active project. These should match files in your Projects/ folder. -->
- **[Project Name]** — [One-line description and current status]
- **[Project Name]** — [One-line description and current status]

## Ongoing Areas
<!-- These are responsibilities with no end date — things you're always doing. -->
- **[Area Name]** — [What this covers]
- **[Area Name]** — [What this covers]

## File Structure
- **Dashboard.md** — Quick overview of all projects, priorities, and blockers
- **Projects/** — One file per active project with tasks, links, and running notes
- **Areas/** — Ongoing responsibilities (no end date)
- **Resources/** — Reference materials, templates, guides
- **Archives/** — Completed projects
- **memory/** — Long-term knowledge: people, projects, glossary, decisions, legal flags

## Custom Commands
- `/daily-brief`: Read Dashboard.md and project files, check my calendar, summarize today's priorities, deadlines, and blockers.
- `/weekly-review`: Review all project files, check calendar for past and upcoming week, list completed tasks, open items, blockers, and priorities for next week.
- `/meeting-prep [topic]`: Check calendar for meeting details, search my docs for related notes, check relevant project files, check my task tracker for related tasks. Give me context, what happened last time, questions to ask, and open items.

## Preferences
- Prefer concise, actionable summaries
- Highlight blockers, wins, and urgent tasks
- Use bullet points for lists
- Reference specific project files when possible
- Date-stamp all running notes entries
- When updating project files, also check if Dashboard.md needs updating

## Workflow
1. I talk through updates via voice or text.
2. Claude organizes updates into the right project file.
3. Each morning, run `/daily-brief` for priorities.
4. Every Friday, run `/weekly-review` to reflect and plan.
5. When working on a project, Claude reads the relevant project file first for context.

## How to Handle Meeting Notes
When I say "pull notes from [meeting]":
1. Search my docs for auto-generated meeting notes (e.g. "Meeting Name - Date - Notes")
2. Read the notes
3. Extract: decisions, action items with owners, blockers, key updates
4. Add a date-stamped entry to the relevant project file's Running Notes
5. Update Dashboard.md if priorities or blockers changed
6. Flag any action items assigned to me

## How to Handle Updates
When I give you an update:
1. Figure out which project it belongs to
2. Add a date-stamped entry to Running Notes
3. Check off completed tasks in Next Steps
4. Add new tasks if I mention them
5. Update Dashboard.md if status changed
6. If a blocker was resolved or new one appeared, update Blockers
