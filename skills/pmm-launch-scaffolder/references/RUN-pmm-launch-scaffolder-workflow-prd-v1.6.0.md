```yaml
doc_type: RUN
normative: true
requires: []
```

# PMM Launch Scaffolder — Execution Workflow

**Status:** Active v1.6.0
**Owner:** {PMM_LEAD}
**Consumers:** Claude AI Agent
**Change control:** PR Review

---

## 0. Overview

This runbook defines the exact sequence the AI must execute when asked to scaffold
a PMM launch task. The workflow has one human approval gate (Step 1 input confirmation)
and then runs to completion. Do not emit conversational text between steps.

**Trigger phrase:** Any variant of "scaffold [task name]", "create PMM tasks for [name]",
"scaffold a [template] for [name]", "set up launch tasks for [name]".

**Target tracker list:** `{PMM_LIST_ID}` — your project tracker's PMM launch list. Do not
operate outside this list.

---

## Step 0 — Intake Check

Before running any search or presenting any prompt, ask the following three questions
in a single message. Wait for the user's response before proceeding.

```
Before I scaffold this, I need three quick answers:

1. Is the product/feature approved? (Has it shipped to staging, or can the team demo it?)
2. Who is the target audience? (internal, enterprise, consumer — or a specific segment?)
3. Is there a launch date or target window?
```

**If all three are answered:** proceed to Step 1 (full scaffold).

**If any answer is missing or unclear:** do NOT proceed to full scaffold.
Instead, create a single "pending clarity" task in list `{PMM_LIST_ID}`:

| Field | Value |
|---|---|
| `name` | `[task name] — pending clarity` |
| `assignees` | {PMM_LEAD} (`{PMM_LEAD_ID}`) |
| `status` | `backlog` |
| `tags` | [product tag from lookup, if product is known; omit if not] |

Set the task description to the unanswered questions, prefixed with:
> "Scaffolding paused — the following inputs are needed before PMM work begins:"

Then tag the task `status: needs-product-input` and stop. Do not create any subtasks.
Print the task URL and this message:
> "Created a pending-clarity task. Once these questions are resolved, re-run scaffold to create the full task structure."

---

## Step 1 — Pre-Search + Subtask Fetch (runs before showing any prompt)

**If a tracker task URL was provided in the trigger:** extract the task ID, skip search,
call `clickup_get_task` with `subtasks: true` on that task ID, build the dedup set,
then proceed to Step 1b.

**If no URL was provided:** immediately run `clickup_search` with the inferred task name
in list `{PMM_LIST_ID}` before presenting the input prompt.

- If **0 results**: proceed to Step 1b (input prompt), noting no existing task found.
  Dedup set is empty (new task).
- If **1–5 results**: include the matches in the Step 1b prompt. If the user selects an
  existing task, call `clickup_get_task` with `subtasks: true` on that task ID and build
  the dedup set before presenting Step 1b.
- If **>5 results**: show only the top 3 closest matches in the Step 1b prompt.

**If creating a new task:** skip the subtask fetch — dedup set is empty by definition.

---

## Step 1b — Gather Inputs (Human Approval Gate)

Present the following prompt, incorporating any search results from Step 1. Wait for
confirmation before proceeding.

```
Here's what I'll scaffold — confirm or edit before I create anything:

  Task name:    [inferred from trigger, or blank]
  Product:      [inferred, or blank — see product table below]
  Template:     [inferred, or "feature-launch" as default]
  Launch date:  [inferred, or blank]
  DRI:          {PMM_LEAD} (default)

[If existing tasks found]:
  ⚠ Found existing task(s): [name] — [URL]
  Add subtasks to existing task, or create a new one?

Subtasks to be created:
  ✓ [subtask name]   → [assignee]   due [date]
  ✓ [subtask name]   → [assignee]   due [date]
  ...

Already exists — will be skipped:
  ⚠ [subtask name]
  ...

[If all subtasks already exist]:
  ⚠ All template subtasks already exist on this task. Nothing to create.
  Proceed anyway (e.g. to set product line), or cancel?

Type "go" to confirm, or edit any field above.
```

**Required fields with no default** — if blank after "go", re-prompt for that field only:
- Task name
- Launch date (used as parent due date and as anchor for subtask offsets)

**Fields with defaults or derived values:**
- Product: if not stated, ask; do not default silently (affects tag and custom fields)
- Template: default `feature-launch` if not stated
- DRI: always {PMM_LEAD} unless user says otherwise

**Derived at confirmation time (do not ask user):**
- Tag: look up from Product → Tag table (§ Lookup: Product → Tag + Custom Fields)
- Product label: look up from product table
- Product line: look up from product table

---

## Step 2 — Resolve Parent Task

### If attaching to an existing task (chosen in Step 1b)

Use the existing task ID. Proceed to Step 2b.

### If creating a new task

### Step 2a — Create new parent task

Call `clickup_create_task` with:

| Field | Value |
|---|---|
| `list_id` | `{PMM_LIST_ID}` |
| `name` | [task name from Step 1] |
| `assignees` | [{PMM_LEAD} user ID — see § Lookup: People] |
| `tags` | [tag from product lookup] |
| `due_date` | [launch date from Step 1, as Unix ms timestamp] |
| `status` | `backlog` |
| Custom field: product line | [product line option ID from § Lookup: Product → Tag + Custom Fields] |

Capture the returned task ID and URL for the summary.

---

## Step 2b — Subtask Deduplication

Use the subtask set already fetched in Step 1. Do **not** call `clickup_get_task` again.

The delta (what to create vs skip) was already computed and shown in Step 1b.
Proceed directly to Step 2c using that cached result.

---

## Step 2c — Set Product Line on Parent Task

Call `clickup_update_task` on the parent task ID with:

```json
{"custom_fields": [{"id": "{PRODUCT_LINE_FIELD_ID}", "value": "<product-line-option-id>"}]}
```

Use the product line option ID from § Lookup: Product → Tag + Custom Fields.

If the product was not stated in the trigger or confirmed in Step 1b, ask before proceeding:
> "Which product should I assign to this task? (needed for the product line field)"

Do not skip this step or silently leave the field unset.

---

## Step 3 — Create Subtasks

**Subtask name convention:** Each subtask name follows `{Type}: {slug}` where `{slug}` is
derived from the parent task name — lowercased, spaces replaced with hyphens, special
characters removed. Example: "Launch Widget Pro" → `launch-widget-pro`.

**Weekend guard:** If a computed due date falls on a Saturday or Sunday, move it to the
**preceding Friday**. Work should be done before the weekend, not after.
Apply this adjustment before setting the `due_date` field in the API call.

For each non-duplicate subtask in the template, call `clickup_create_task` with:

| Field | Value |
|---|---|
| `list_id` | `{PMM_LIST_ID}` |
| `parent` | [parent task ID] |
| `name` | [subtask name from template] |
| `assignees` | [assignee user IDs from template row] |
| `due_date` | [launch date minus offset days from template row, as Unix ms timestamp] |
| `status` | `backlog` |
| Custom field: Materials | [option ID from template row — see § Lookup: Materials Labels] |

Create subtasks sequentially (not in parallel) so failures can be reported individually.
Capture each returned task URL for the summary.

---

## Step 4 — Summary

After all creates complete, print a summary table:

```
## ✓ Scaffold complete

**Parent task:** [name] — [URL]

| Subtask | Assignee | Due Date | Materials | Status |
|---|---|---|---|---|
| [name] | [name] | YYYY-MM-DD | [label] | ✓ created — [URL] |
| [name] | [name] | YYYY-MM-DD | [label] | ⚠ already exists — skipped |
```

If any subtask failed to create, show `✗ failed — [error message]` and continue.

---

## Error Handling

| Situation | Action |
|---|---|
| API failure on parent create | Halt immediately. Report error. Do not attempt subtasks. |
| Subtask create failure | Report `✗ failed — [error]` in summary for that row. Continue creating remaining subtasks. |
| Search returns >5 results | Show top 3 matches. Ask the user to confirm or provide a task URL directly. |
| Missing task name after "go" | Re-prompt: "What should I name the parent task?" |
| Missing launch date after "go" | Re-prompt: "What is the launch date? (e.g. April 10)" |
| Product not recognized | List valid product names from the lookup table and ask the user to choose. |
| `clickup_get_task` returns no subtasks field | Treat as 0 existing subtasks and proceed. |

---

## § Lookup: People

<!-- Fill in your own team. Confirm user IDs via `clickup_get_workspace_members` before first run. -->

| Person | Tracker User ID | PMM Role |
|---|---|---|
| {PMM_LEAD} | `{PMM_LEAD_ID}` | Marketing Brief, Design Brief, DRI default |
| {SOCIAL_LEAD} | `{SOCIAL_LEAD_ID}` | Socials / social amplification |
| {CONTENT_LEAD} | `{CONTENT_LEAD_ID}` | Blog post |
| {PR_LEAD} | `{PR_LEAD_ID}` | PR |
| {DESIGNER} | `{DESIGNER_ID}` | Design |
| {BRAND_LEAD} | `{BRAND_LEAD_ID}` | Brand Campaign |
| {COMMUNITY_LEAD_1} | `{COMMUNITY_LEAD_1_ID}` | Community |
| {COMMUNITY_LEAD_2} | `{COMMUNITY_LEAD_2_ID}` | Community |

---

## § Lookup: Custom Field IDs

<!-- Fill in your own list's field IDs. Confirm via `clickup_get_task` on a known task in your list before first run. -->

| Custom Field | Field ID | Type |
|---|---|---|
| DRI | `{DRI_FIELD_ID}` | users |
| Materials | `{MATERIALS_FIELD_ID}` | labels |
| Product | `{PRODUCT_FIELD_ID}` | labels |
| product line | `{PRODUCT_LINE_FIELD_ID}` | drop_down |
| RAG Status | `{RAG_STATUS_FIELD_ID}` | drop_down |
| priority | `{PRIORITY_FIELD_ID}` | drop_down |

---

## § Lookup: Product → Tag + Custom Fields

<!-- Fill in your own product lines. Confirm option IDs via `clickup_get_task` on a known task before first run. Add one row per product, including alias spellings users might type. -->

| Product (user input) | Tag | Product field option ID | product line option ID |
|---|---|---|---|
| {PRODUCT_A} / [aliases] | `{product-a-tag}` | `{PRODUCT_A_OPTION_ID}` | `{PRODUCT_A_LINE_OPTION_ID}` |
| {PRODUCT_B} / [aliases] | `{product-b-tag}` | `{PRODUCT_B_OPTION_ID}` | `{PRODUCT_B_LINE_OPTION_ID}` |
| {PRODUCT_C} / [aliases] | `{product-c-tag}` | `{PRODUCT_C_OPTION_ID}` | `{PRODUCT_C_LINE_OPTION_ID}` |

> **Tag note:** Confirm the exact tag spelling (singular vs plural) from a real task in
> your list — trackers are unforgiving about near-miss tag names.

---

## § Lookup: Materials Labels

<!-- Fill in the actual labels that exist in your Materials custom field — do not invent others. Confirm option IDs via `clickup_get_task` on a known task. -->

| Label | Option ID |
|---|---|
| Post | `{POST_LABEL_ID}` |
| Blog | `{BLOG_LABEL_ID}` |
| Graphic | `{GRAPHIC_LABEL_ID}` |
| Video | `{VIDEO_LABEL_ID}` |
| KOL | `{KOL_LABEL_ID}` |
| Report | `{REPORT_LABEL_ID}` |
| Infographic | `{INFOGRAPHIC_LABEL_ID}` |
| Webinar | `{WEBINAR_LABEL_ID}` |
| Brief | `{BRIEF_LABEL_ID}` |

> **Label mapping:** social copy and PR → Post; long-form content → Blog; design assets → Graphic; planning docs → Brief.

---

## § Templates

### Template: `feature-launch`

Use for: new feature availability announcements, platform upgrades, infrastructure activations.
3 subtasks — 4-day sprint.

| Subtask name | Assignee(s) | Due offset (days before launch) | Materials label |
|---|---|---|---|
| `Brief: {slug}` | {PMM_LEAD} | −4 | Brief |
| `Blog: {slug}` | {CONTENT_LEAD} | −3 | Blog |
| `Socials: {slug}` | {SOCIAL_LEAD} | −1 | Post |

### Template: `public-release`

Use for: SDK releases, API version releases, public product releases with external audiences.
4 subtasks — 2-week runway.

| Subtask name | Assignee(s) | Due offset (days before launch) | Materials label |
|---|---|---|---|
| `Brief: {slug}` | {PMM_LEAD} | −14 | Brief |
| `Blog: {slug}` | {CONTENT_LEAD} | −10 | Blog |
| `PR: {slug}` | {PR_LEAD} | −5 | Post |
| `Socials: {slug}` | {SOCIAL_LEAD} | −3 | Post |

### Template: `case-study`

Use for: customer success stories, partner case studies, co-marketing articles.
7 subtasks — 3-week runway. Marketing Brief and Design Brief share the −21 offset (same start day).

| Subtask name | Assignee(s) | Due offset (days before launch) | Materials label |
|---|---|---|---|
| `Brief: {slug}` | {PMM_LEAD} | −21 | Brief |
| `Design Brief: {slug}` | {PMM_LEAD} | −21 | Brief |
| `Blog: {slug}` | {CONTENT_LEAD} | −14 | Blog |
| `Design: {slug}` | {DESIGNER} | −10 | Graphic |
| `Brand: {slug}` | {BRAND_LEAD} | −7 | Post |
| `Community: {slug}` | {COMMUNITY_LEAD_1} + {COMMUNITY_LEAD_2} | −5 | Post |
| `Socials: {slug}` | {SOCIAL_LEAD} | −3 | Post |

---

## Changelog

See `REF-pmm-launch-scaffolder-changelog.md`.
