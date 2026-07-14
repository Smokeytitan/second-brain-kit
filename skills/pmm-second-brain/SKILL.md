---
name: pmm-second-brain
description: "Product Marketing Manager skill for managing a PARA-based second brain system. Use this skill whenever the user asks to: create or update project files, run /daily-brief or /weekly-review or /meeting-prep commands, build GTM campaigns for product launches, prep for meetings (pulling calendar events, meeting notes from cloud docs, and project-tracker tasks), process meeting notes into project files, track workstreams and blockers, draft outreach or messaging, update the Dashboard, or anything related to managing their PMM workflow. Also trigger when the user mentions product launches, GTM strategy, campaign planning, competitive analysis, messaging docs, content calendars, or says things like 'update my notes', 'what's on my plate', 'prep me for my meeting', 'pull the latest notes', or 'add this to my project file'. This skill should trigger for virtually any work-related task the user brings up, since their second brain is the hub for all of it."
---

# PMM Second Brain Skill

## Overview

This skill powers an integrated PMM workflow. It connects a PARA-based notes vault, your calendar, your cloud docs (auto-generated meeting notes), and your project tracker into a single operating system for product marketing.

## User Context

Read `references/user-context.md` for full details on the user's role, team, projects, and preferences. Fill that file in for your own situation. Key fields it defines:
- **Role:** the user's title and remit
- **Reports to:** {MANAGER} — their manager
- **Key partner:** {PRODUCT_LEAD} — closest product counterpart
- **Mentor:** {PMM_MENTOR} — GTM campaign guidance
- **Constraints:** e.g., budget limits that shape channel strategy
- **Input style:** if the user often uses voice-to-text, keep responses action-oriented

## File Structure

The second brain lives in a notes vault (Obsidian or any markdown folder) with this structure:

```
Vault/
├── claude.md          # Claude's context file — role, projects, commands, preferences
├── Dashboard.md       # Quick overview of priorities, project status, blockers
├── Projects/          # One file per active project (has end date)
├── Areas/             # Ongoing responsibilities (no end date)
├── Resources/         # Reference materials, templates, guides
└── Archives/          # Completed/inactive items
```

## Custom Commands

### `/daily-brief`
1. Check the calendar for today's events
2. Read Dashboard.md and all active project files
3. Produce a concise brief:
   - Today's meetings (with relevant context from project files)
   - Top 3 priorities
   - Active blockers
   - Any deadlines this week

### `/weekly-review`
1. Read all project files and Dashboard.md
2. Check calendar for the past and upcoming week
3. Produce:
   - What got done this week
   - What's still open
   - Blockers and risks
   - Priorities for next week
   - Any project file updates needed

### `/meeting-prep [meeting name or topic]`
1. Check the calendar for the meeting details (attendees, time, description)
2. Search cloud docs for related meeting notes (auto-generated notes if your workspace produces them)
3. Check relevant project files for context
4. Check the project tracker for related tasks/subtasks
5. Produce:
   - Meeting context (who, what, when)
   - What happened last time (from previous meeting notes)
   - Key questions to ask
   - Relevant data points and links
   - Open items to raise

## PMM Frameworks

These frameworks are battle-tested approaches for product marketing. They should inform how Claude helps the user with content creation, stakeholder communication, and launch planning.

### SCQR — Stakeholder Communication
When the user needs to communicate with stakeholders (e.g., designers, social leads, leadership), structure the message using:
- **Situation:** Immediate context — what's happening
- **Complication:** The challenge or blocker
- **Question:** The key question that needs answering
- **Resolution:** The proposed solution or ask

Use this framework when drafting chat messages, briefs, or async updates to design, social, content, or leadership counterparts.

### 3 Core Messages
Every piece of product content should tie back to one of the user's **3 core messages** — the things people should associate with the product when they hear the name. When creating any content:
1. Check which core message this content supports
2. Include supporting proof points (metrics, data, product updates)
3. If a product update doesn't map to a core message, flag it — it may not be worth amplifying

The user defines these core messages themselves. They act as a filter: if content doesn't reinforce one of the three, question whether it's worth creating.

### Content Development Flow
When creating campaign content, follow this progression — it forces clarity at each stage:
1. **Start with a tweet** — if you can't say it in 280 characters, you don't understand it yet
2. **Blog outline** — Title + 3 key points (TL;DR) + 3 topic sentences + conclusion
3. **Full content** — Expand the outline into the final piece

This approach prevents bloated content and ensures every piece has a clear, concise core message.

### Product Launch Foundation
Before building any GTM or launch content, always start with these questions:
1. **Who is my audience?**
2. **What is the goal?**
3. **What are the risks if this goes poorly?**

These three questions should appear at the top of every new GTM project file. They ground the entire campaign and prevent scope creep.

### Content Ownership Model
The user doesn't create everything themselves — they brief and draft, then hand off to specialists:
- **Social (X/LinkedIn):** user drafts → {SOCIAL_LEAD} refines and publishes
- **Blog:** user creates brief → {BLOG_WRITER} writes
- **Landing page / website:** user owns product copy directly
- **Design:** user creates briefs using SCQR → {DESIGNER} executes

When helping the user create content, match the format to the handoff target. A social draft should be tweet-ready. A blog brief should have the outline structure. A design brief should use SCQR.

### Messaging Constraints
Codify any legally approved, non-negotiable constraints for your product in `references/user-context.md`. For example, if your product involves a regulated asset:
- Discuss the product's **role and utility**, not price or investment value
- Focus on metrics, adoption, and ecosystem function
- Never make predictions or imply investment value

When reviewing or creating content, flag anything that strays into constrained territory.

## Core Workflows

### Processing Meeting Notes
When the user says "pull the notes from [meeting]" or shares meeting updates:
1. Search cloud docs for auto-generated notes by meeting name and date
2. Fetch and read the notes
3. Extract: decisions made, action items (with owners), blockers raised, key updates
4. Update the relevant project file's "Running Notes" section with a date-stamped entry
5. Update Dashboard.md if priorities or blockers changed
6. Flag any action items assigned to the user

### Updating Project Files
When the user shares an update (often via voice-to-text):
1. Identify which project the update belongs to
2. Add a date-stamped entry to "Running Notes"
3. Update task checkboxes if items were completed
4. Add new tasks to "Next Steps" if mentioned
5. Update Dashboard.md if the project status changed
6. If a blocker was resolved or a new one appeared, update the Blockers section

### Building a GTM Campaign
When the user needs to build a GTM for a product launch:
1. Read `references/gtm-template.md` for the framework
2. Create a new project file in Projects/ with all GTM sections
3. Research the product: pull the PRD from cloud docs, check the project tracker for tasks
4. Pre-populate sections with known data (metrics, competitive landscape, timelines)
5. Identify gaps and flag them for the user
6. Structure follows: Marketing Overview → Market Opportunity → Positioning → Audience → Messaging → Competitive Comparison → GTM Checklist → Social → Blog → Email → Design Brief → Follow-on Campaign

### Campaign Content Creation
When the user needs to create campaign content:
1. Read the relevant project file for full context
2. Read `references/gtm-template.md` for structure and tone examples
3. Match the tone to the channel (X = platform-native, LinkedIn = professional, community chat = conversational)
4. Reference specific data points and metrics from project files
5. Keep the user's voice — match their established personal handle style

## Tool Usage

### Calendar
- **List events:** Check today's schedule, upcoming meetings, find free time
- **Get event details:** Pull attendee lists, descriptions, meeting links
- **Find meeting times:** When scheduling new meetings
- Always use the user's timezone ({USER_TIMEZONE})

### Cloud Docs (e.g., Google Drive)
- **Search for meeting notes:** Query full text for the meeting name, filtered to document types, with a semantic query for context
- **Fetch documents:** Pull PRDs, messaging docs, campaign docs, meeting notes
- **Key recurring docs to know about:** <!-- Fill in your own -->
  - {WEEKLY_PRODUCT_MEETING_DOC} — recurring product meeting agenda and notes
  - Auto-generated meeting notes (note your workspace's naming pattern, e.g. "[Meeting Name] - [Date] - Notes")
  - {PRODUCT_PRD_DOC}, {MESSAGING_FAQ_DOC}

### Project Tracker (e.g., ClickUp, Linear, Asana)
- **Get tasks:** Pull task details, subtasks, assignees, due dates, status
- **Search:** Find tasks by name or keyword
- **Update tasks:** Add comments, update status (with the user's permission)
- Key parent task for the flagship launch: `{PRODUCT_LAUNCH_TASK_ID}` — your tracker's launch epic

### Web Search
- Use for competitive analysis, market data, community sentiment research
- Check your industry's analytics dashboards when metrics are needed

## Response Style

- **Concise and actionable** — respect the user's time, especially if they use voice-to-text
- **Bullet points for lists** — most users prefer them
- **Reference specific files** — link to project files, docs, tracker tasks
- **Flag blockers prominently** — always surface what's stuck
- **Date-stamp everything** — running notes should always have dates
- **Don't over-explain** — match the user's pace
