```yaml
doc_type: REF
normative: false
requires: []
```

# PMM Launch Scaffolder — Changelog

**Owner:** {PMM_LEAD}
**Change control:** PR Review

---

## v1.6.0

### Deduplication moved before approval gate

- Subtask dedup check now runs during Step 1, before the Step 1b prompt.
- For existing tasks: `clickup_get_task` (subtasks: true) is called as soon as the task
  ID is known — whether via URL, search result, or direct ID.
- Step 1b now shows the exact delta (✓ will be created, ⚠ already exists — skipped)
  so "go" approves a specific list, not a template in the abstract.
- If all template subtasks already exist, Step 1b surfaces this explicitly and asks
  whether to proceed (e.g. to set product line) or cancel.
- Step 2b no longer calls the API — uses the cached set from Step 1.
- Motivation: two duplicate "Brief" subtasks were created because an interrupted run
  left one behind and the dedup check ran too late to catch it.

---

## v1.5.0

### Intake gate (Step 0)

- Added Step 0 — Intake Check before Step 1. Before any search or input prompt, the
  agent asks three questions: (1) Is the feature approved / at staging? (2) Who is the
  target audience? (3) Is there a launch date or target window?
- If all three are answered, the workflow proceeds as before.
- If any answer is missing, the agent creates a single "pending clarity" task
  with the unanswered questions as the description and `status: needs-product-input` tag,
  then stops. No subtasks are created.
- Motivation: a prior launch sequence produced a full brief before the product decision
  cleared. This gate would have caught it at step 1 and prevented the rework.

---

## v1.0.0

### Initial build

**Skill created. Status: Draft (pending first end-to-end test run).**

**Purpose:** Automate creation of PMM launch scaffolds in the project tracker. Given a
task name, product, launch date, and template type, creates a parent task + all standard
subtasks with correct assignees, due dates, tags, and custom fields in one shot.
Eliminates manual one-by-one subtask creation.

**Reference files created:**

| File | Purpose |
|---|---|
| `RUN-pmm-launch-scaffolder-workflow-prd-v1.6.0.md` | Execution workflow, lookup tables, templates |
| `REF-pmm-launch-scaffolder-changelog.md` | This file |

**Templates defined:** `feature-launch`, `public-release`, `case-study`.

**Pipeline layer:** L4 — Standalone. Reads no external data sources. Operates exclusively
via ClickUp MCP tools on list `{PMM_LIST_ID}`.

**Known pending items at build time:**
- Custom field IDs for Product label and Product line: marked `[PENDING]` — confirm via
  `clickup_get_list` or `clickup_get_task` on a known task before first production run.
- Materials label option IDs: marked `[PENDING]` — confirm via same method.
- Tracker user IDs for all people: listed with approximate values — confirm via
  `clickup_get_workspace_members` before first run.

**Writing examples:** N/A — this skill creates tracker tasks, not editorial content.
Voice register / writing style guidelines do not apply.

---

## v1.2.0

### Search-before-prompt workflow fix

- Moved `clickup_search` to Step 1 (before the input prompt), renamed input gate to Step 1b.
- Search results are now surfaced inline in the Step 1b prompt so the user can choose to
  attach subtasks to an existing task or create a new one before any fields are filled.
- Previously the search happened in Step 2, after the approval gate — meaning the user
  confirmed inputs before knowing a matching task already existed.

---

## v1.4.0

### Product line field fix

- Removed a non-existent "Product label" field from Step 2a — this field did not
  exist in the target list (confirmed via `clickup_get_task`).
- `product line` is the only product selector in this list. Step 2a now sets
  only this field. Value format: option ID string passed directly as `value`.
- Added Step 2c: after deduplication, call `clickup_update_task` on the parent task to set
  `product line`. Applies to both new and pre-existing parent tasks. If product not known,
  prompt user — do not silently skip.
- Added a new product option to the product lookup table.

---

## v1.3.0

### Subtask naming convention

- All subtask names now follow `{Type}: {slug}` where slug = parent task name lowercased
  with spaces replaced by hyphens (e.g. "Launch Widget Pro" → "launch-widget-pro").
- Type labels: Brief, Design Brief, Blog, Socials, PR, Design, Brand, Community.
- Slugified names make subtasks self-describing, unique across the tracker, and more
  reliable for deduplication.

### Weekend guard

- Step 3 now moves any computed due date that falls on Saturday or Sunday back to the
  preceding Friday before setting the `due_date` field.

---

## v1.1.0

### People map + Newsletter label

- Replaced placeholder People lookup with confirmed tracker user IDs for all 8 PMM roles
  ({PMM_LEAD}, {SOCIAL_LEAD}, {CONTENT_LEAD}, {PR_LEAD}, {DESIGNER}, {BRAND_LEAD},
  {COMMUNITY_LEAD_1}, {COMMUNITY_LEAD_2}).
- Updated all three templates to use role-correct assignees.
- Removed newsletter subtasks from all templates — the PMM team does not have a newsletter.
- Moved Materials field from parent task to subtasks — parent is a launch event, subtasks are deliverables.
- feature-launch: 3 subtasks, 4-day sprint (offsets −4, −3, −1).
- public-release: 4 subtasks, 2-week runway; PR coordination ({PR_LEAD}) added.
- case-study: 7 subtasks, 3-week runway; design, brand, and community explicit rows.
- Brief label added to Materials — used for Marketing Brief and Design Brief subtasks.
- Post = social copy and PR only. Blog = long-form. Graphic = design. Brief = planning docs.
