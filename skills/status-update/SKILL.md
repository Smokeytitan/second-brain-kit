---
name: status-update
version: 1.3.0
description: "Generate personal status updates and accomplishment reports by pulling activity from Slack, Gmail, GitHub, plus a ClickUp Priorities doc and the repo `state/work/` tracker's done tasks. ClickUp is canonical for marketing/primary-discipline execution; `state/work/` is canonical for AI/system work shipped. Optional Drive weekly notes and ClickUp list/view URLs may be supplied as extra enrichment. The skill runs on a cursor-driven window (twice-weekly cadence) by default. Use this skill whenever the user asks about their accomplishments, what they've been working on, wants a status update, standup notes, weekly update, progress report, or asks you to summarize their recent work activity. Also trigger when the user mentions 'what did I do this week', 'weekly recap', 'status report', 'accomplishment list', or anything related to reviewing their own work output across communication and development tools."
---

# Status Update Generator

You are helping the user generate a comprehensive status update by gathering their recent activity across communication, development, and work-tracking systems. The goal is to paint an accurate picture of what they are focused on, what they accomplished, and what they're currently working on, then present it in three formats tailored for sharing with their team.

## Overview

This skill pulls from five built-in sources plus two optional enrichment sources:

1. **Slack** — messages the user sent across all their channels
2. **Gmail** — emails the user sent
3. **GitHub** — PRs authored, issues worked on, and code reviews
4. **ClickUp Priorities** (built-in) — the most-recent block from the team's Priorities doc, fetched via REST ({CLICKUP_PRIORITIES_DOC} — your team's ranked-priorities doc in ClickUp)
5. **Tracker done** (built-in) — tasks in `state/work/tasks/*.yaml` with `status: done` in the window
6. **Drive weekly notes** (optional) — a Google Doc URL or doc ID supplied by the user
7. **ClickUp list/view** (optional) — a ClickUp list/view URL supplied by the user

It synthesizes the raw activity into two buckets — **Accomplished** (completed work) and **Currently Working On** (in-progress items) — adds a **Current Priorities** header and an **AI Items Shipped** subsection, and outputs three formats every time.

Authority split:

- **ClickUp is canonical for primary-discipline execution** (e.g., marketing work). The built-in Priorities reader and any supplied list/view URL anchor those items first.
- **`state/work/` is canonical for AI / system / infra work.** The built-in tracker-done reader anchors the "AI Items Shipped" subsection.
- **Status updates are derived reports only.** They do not silently write tracker or ClickUp state.

## Step 1: Check Available Connectors

Before doing anything else, check which data sources are actually connected or were explicitly supplied. This matters because the skill depends on multiple external services, and any of them might not be set up. Run through this checklist:

1. **Slack** — Look for Slack MCP tools (e.g., `slack_search_public`, `slack_search_public_and_private`, `slack_read_channel`). If none, mark Slack as unavailable.
2. **Gmail** — Look for Gmail MCP tools (e.g., `gmail_search_messages`, `gmail_read_message`). If none, mark Gmail as unavailable.
3. **GitHub** — Look for a GitHub MCP connector first. If none, check `gh auth status` via Bash. If neither is available, mark GitHub as unavailable.
4. **ClickUp Priorities** (built-in, runs automatically) — the collector resolves `CLICKUP_API_TOKEN` from `.mcp.json` or the environment. If the token is missing, the Priorities source will report `status: unavailable` in the collector output; continue with the rest of the report.
5. **Tracker done** (built-in, runs automatically) — scans `state/work/tasks/*.yaml`. Always available in this repo.
6. **Drive weekly notes** (optional) — only pulled if the user supplied a URL or doc ID.
7. **ClickUp list/view** (optional) — only pulled if the user supplied a list or view URL.

After checking, tell the user what's connected and what's not. For example:

> "Here's what I have access to:
> - Slack: Connected
> - Gmail: Connected
> - GitHub: Not connected
> - ClickUp Priorities: Built-in (auto)
> - Tracker done: Built-in (auto)
> - Drive weekly notes: Not requested
> - ClickUp list: Not requested
>
> I'll pull your activity from Slack, Gmail, plus the built-in Priorities and tracker-done sources. To include GitHub next time, you can connect a GitHub MCP connector or authenticate the `gh` CLI."

If **none** of the communication sources (Slack, Gmail, GitHub) are connected and no Drive/ClickUp URL is supplied, you can still produce a report from the built-in Priorities + tracker-done sources alone — but tell the user it will be very narrow.

## Step 2: Determine the Time Frame

The collector runs in **auto mode** by default. Auto mode reads `state/status-update/last-run.json` and uses that timestamp as the window start; if the cursor is missing or malformed, it falls back to the last 7 days. The end of the window is always today. Auto mode writes the cursor on success so the next run starts from where this one finished.

**When to run in auto mode (default):** the user asks for a standard status update with no explicit time frame.

**When to override with explicit dates:** the user says something like "last two weeks", "since March 1st", or otherwise specifies a window. In that case pass `--start-date` and `--end-date` together; the collector will NOT write the cursor on a manual run.

Before running the collector, tell the user what window you plan to use. For auto mode: "I'm running in auto mode — the collector will pick up from the last-run cursor (or the last 7 days if this is the first run)." For manual mode: "I'm pulling your activity from [start date] to [end date]."

## Step 3: Gather Data from All Sources

Pull data from all available sources. Work through them methodically — it's fine to do them in sequence since each one takes a moment.

### Built-in and optional sources via the collector

Run the collector once. In auto mode (no dates), it resolves the window from the cursor and handles both built-in sources plus any supplied enrichment URLs. Pass `--drive-doc-url` and/or `--clickup-url` only when the user supplied them:

```bash
# Auto mode (default): cursor-driven window, Priorities + tracker-done included automatically
python3 scripts/utilities/status_update_sources.py --format json

# With optional enrichment
python3 scripts/utilities/status_update_sources.py \
  --drive-doc-url URL_OR_DOC_ID \
  --clickup-url URL \
  --format json

# Manual window override (does NOT write the cursor)
python3 scripts/utilities/status_update_sources.py \
  --start-date YYYY-MM-DD \
  --end-date YYYY-MM-DD \
  --format json
```

Parse the JSON output. Relevant top-level keys:

- `window` — `{start_date, end_date, auto_mode}`. Use these dates in the report header.
- `priorities_current[]` — ranked items from the most-recent block of the Priorities doc, each with `rank`, `text`, `block_date`, `source_url`. Render under "Current Priorities".
- `ai_accomplished_tracker[]` — tracker tasks completed in the window, each with `id`, `title`, `parent_id`, `summary`, `completed_at`, `source_path`. Render under "AI Items Shipped".
- `items[]` — Drive and ClickUp-list items from the optional enrichment sources, each with `bucket` (`accomplished` or `in_progress`), `title`, `detail`, `source_type`, `source_url`, `source_date`, `confidence`. Deduped across sources.
- `sources[]` — per-source `{type, status, detail}` entries. If any `status` is `unavailable` or `error`, surface that in the report so the user knows the gap.
- `warnings[]` — de-dup conflicts and collector exclusions. Surface anything material.

Canonical mappings:
- `items[].bucket == "accomplished"` → Accomplished
- `items[].bucket == "in_progress"` → Currently Working On

### Slack

Search for messages the user sent across all channels in the window returned by the collector. Use queries like `from:me` or the user's name/handle with date filters. Look for:
- Messages describing work done, decisions made, or progress shared
- Threads where the user provided updates or answered questions
- Any announcements or summaries the user posted

Cast a wide net — it's better to gather too much and filter later than to miss something.

### Gmail

Search for emails the user **sent** during the window. Use `from:me` with the date range. Look for:
- Status updates or reports sent to others
- Responses that describe completed work
- Meeting follow-ups with action items
- Decisions communicated via email

### GitHub

GitHub data is valuable but depends on what's available. Follow this fallback chain:

**Option A: MCP / Connector (preferred).** If a GitHub MCP connector or a connected GitHub search is available, use it to search for the user's PRs, issues, and review activity within the date range.

**Option B: `gh` CLI.** If no MCP connector, try the `gh` CLI via Bash:

```bash
gh api user --jq '.login'  # discover username first
gh search prs --author=USERNAME --created=YYYY-MM-DD..YYYY-MM-DD --json title,url,state,repository,createdAt,closedAt
gh search issues --involves=USERNAME --updated=YYYY-MM-DD..YYYY-MM-DD --json title,url,state,repository,labels,updatedAt
gh search prs --reviewed-by=USERNAME --updated=YYYY-MM-DD..YYYY-MM-DD --json title,url,state,repository,updatedAt
```

**Option C: Skip with notice.** If neither is available, skip GitHub but note it clearly in the output:

> *GitHub activity was not included in this update because no GitHub connector or CLI was available.*

## Step 4: Synthesize and Categorize

Organize the gathered data into the following buckets:

**Current Priorities** — top-of-doc items from the Priorities page. Use `priorities_current[]` directly. Show up to the top 3–5; preserve rank order.

**Accomplished** — things that are clearly done:
- Merged PRs
- Closed issues
- Completed deliverables mentioned in Slack or email
- Decisions that were finalized
- Reviews that were submitted
- `items[]` entries with `bucket == "accomplished"`

**Currently Working On** — things still in progress:
- Open PRs (especially those with recent activity)
- Open issues assigned to or involving the user
- Work mentioned in Slack/email that doesn't have a clear "done" signal
- `items[]` entries with `bucket == "in_progress"`

**AI Items Shipped** — every entry in `ai_accomplished_tracker[]`. Do not merge these with Accomplished; they belong in their own subsection so AI/system work is visible separately from primary-discipline execution.

Primary/system split rules:
- **Primary-discipline work (e.g., marketing):** ClickUp (Priorities + any supplied list) is canonical; Slack/GitHub/Drive corroborate or add context.
- **AI / system / infra work:** `state/work/` tracker is canonical. If you see Slack or Gmail evidence of AI/infra work that doesn't match a tracker-done entry, still include it under Accomplished, but note the absence of a tracker record.

Group related items together. For example, if a PR, a Slack thread, a notes item, and a ClickUp task are all about the same feature, treat that as one accomplishment — not four separate line items.

Deduplication rules:
- Deduplicate by normalized work-item title plus nearby source dates (≤3 days apart).
- If sources disagree on status, use the most recent dated evidence.
- If state still conflicts, keep the item in `Currently Working On` and note the ambiguity instead of silently promoting it to done.

## Step 5: Generate All Three Formats

Always produce all three formats. Present them clearly with headers so the user can easily copy the one they need.

### Format 1: Detailed Report

Long-form with full context. Prose with enough detail that someone unfamiliar could understand what happened.

**Template:**
```
# Status Update: [Start Date] – [End Date]

**Current Priorities** (as of [block_date]):
1. [priority 1 text]
2. [priority 2 text]
3. [priority 3 text]

## Accomplished

### [Accomplishment 1 — descriptive title]
[2-4 sentences explaining what was done, why it matters, and any relevant context. Reference specific PRs, issues, or threads where appropriate.]

### [Accomplishment 2 — descriptive title]
[Same format]

...

## Currently Working On

### [Item 1 — descriptive title]
[1-3 sentences on current status, what's left, and any blockers or dependencies.]

...

## AI Items Shipped

### [Tracker task title]
[1-2 sentences from the task summary. Reference the tracker id (e.g., `status-update-last-run-cursor`) and epic id from `parent_id`.]

...
```

Use plain, direct language. Avoid corporate jargon. Write like you're explaining to a smart teammate what happened this week.

If `priorities_current[]` is empty (source unavailable or error), omit the "Current Priorities" block and note the reason in a short line below the title.

If `ai_accomplished_tracker[]` is empty, omit the "AI Items Shipped" section entirely.

### Format 2: Executive Summary

Short bullet list. Each item is one line — just enough to know what it is and that it happened.

**Template:**
```
**Current Priorities:** [priority 1], [priority 2], [priority 3]

## Accomplished
- [Concise one-liner about accomplishment 1]
- [Concise one-liner about accomplishment 2]
- ...

## Currently Working On
- [Concise one-liner about in-progress item 1]
- ...

## AI Items Shipped
- [Tracker task title] (`[task-id]`)
- ...
```

Keep each bullet to one sentence max. No sub-bullets. This is the "glanceable" version. Omit empty sections.

### Format 3: Slack Message

Ready to paste into a Slack channel. Casual but informative tone. Uses emoji sparingly (one per section header is fine). Designed to be scannable in a Slack feed.

**Template:**
```
Hey team, here's my update for the week:

:dart: *Priorities:* [priority 1] · [priority 2] · [priority 3]

:white_check_mark: *Done*
• [Short bullet — conversational tone]
• [Short bullet]
• ...

:construction: *In Progress*
• [Short bullet]
• ...

:robot_face: *AI/System shipped*
• [Tracker task title]
• ...
```

Keep this tight — ideally under 12 bullets total across all sections. If there are a lot of items, combine related ones. This should feel like a quick team standup message, not a formal report. Omit empty sections (no empty `Priorities:` line, no empty `AI/System shipped` block).

## Important Guidelines

- **Be honest about gaps.** If a data source returns `unavailable` or `error`, say so in the output. Don't make things up to fill space.
- **De-duplicate aggressively.** The same work often shows up across multiple sources. Merge those into single items. AI Items Shipped, however, should NOT be merged into Accomplished — keep the subsection separate.
- **Respect the user's voice.** When quoting or paraphrasing from Slack messages or emails, maintain the user's tone. Don't make casual updates sound overly formal.
- **Ask if something is unclear.** If you find activity that's ambiguous (could be accomplished or in-progress), ask the user rather than guessing.
- **Source attribution.** In Format 1, include links to relevant PRs, issues, or threads when available. In Formats 2 and 3, skip the links to keep things clean.
- **Cursor is automatic.** Do not manually write `state/status-update/last-run.json` — the collector handles it on successful auto-mode runs. If the user explicitly requests a cursor reset, remove the file.
