---
doc_type: RUN
normative: true
title: Guidance Review Workflow
version: 1.0
requires:
  - REF-guidance-registry-v1.0
  - REF-guidance-review-output-template-v1.0
---

# Guidance Review Workflow

## Overview

Review marketing copy against one or more registered guidance documents. Produce a
checklist summary (pass/fail per rule category) and inline annotations on flagged
sections with the specific rule violated and a suggested fix.

**Input:** Marketing copy — blog draft, social post, one-pager, brief, deck copy,
or any marketing deliverable. Provided as pasted text or a file path.

**Output:** Two-section review per `REF-guidance-review-output-template-v1.0.md`.

---

## Invocation Modes

| Mode | Trigger phrases | Behavior |
|------|----------------|----------|
| Review (default) | "review for legal", "review for compliance", "guidance review for [slug]" | Steps 0–5. Full review against matched guides. |
| Multi-guide | "review for legal and brand-guidelines" | Steps 0–5. Loads all matched guides, merges rules. |

---

## Step 0 — Accept Input

Accept the content to review. Valid inputs:

1. **Pasted text** — the user pastes the content directly in the conversation
2. **File path** — the user provides a path to a markdown or text file in the repo

If no content is provided alongside the trigger, ask:

> Paste the content you want reviewed, or provide a file path.

If a file path is provided, read the file. If the file does not exist, report the
error and stop.

---

## Step 1 — Resolve Guides

Parse the trigger phrase for guide keywords. Resolution order:

1. **Registry lookup:** For each keyword in the trigger, check `trigger_keywords`
   lists in `REF-guidance-registry-v1.0.md`. If a keyword matches, record the
   `slug` and `source_doc`.

2. **Convention fallback:** If no registry match, scan `guidance/*/GUIDE.md` for a
   folder name matching the keyword.

3. **No match:** If the keyword resolves to nothing, list all available guides
   (slug + display_name from registry) and ask the user to select.

4. **No keyword provided:** If the trigger contains no guide keyword (e.g., bare
   "run guidance review"), list all available guides and ask the user to select.

For each resolved guide, read its `GUIDE.md`. Extract all `## Category` sections
and their `### Rule` entries. Build a merged rules table:

| Guide Slug | Category | Rule Name | Check | Approved | Prohibited | Required Text | Severity |
|------------|----------|-----------|-------|----------|------------|---------------|----------|

If multiple guides are loaded, keep their categories separate in the output (one
checklist table per guide).

---

## Step 2 — Segment Content

Split the input content into reviewable segments for annotation referencing:

| Segment type | Detection |
|-------------|-----------|
| Headline | First H1 or the most prominent opening line |
| Subheadline | H2, H3 headings |
| Body paragraph | Sequential text blocks (numbered by position) |
| CTA | Buttons, link text, calls to action |
| Footnote/Disclaimer | Text after a `---` separator, or explicitly labeled disclaimers |
| Comparison table | Markdown tables comparing products, features, or entities |
| Metadata | Structured headers in briefs (e.g., "Audience:", "Objective:") |

Each segment gets a label for use in inline annotations (see output template).

---

## Step 3 — Rule-by-Rule Review

For each rule in the merged rules table, evaluate all segments:

### 3a. Prohibited language check
Scan for prohibited terms, phrases, or close semantic variants. A violation occurs
when a segment:
- Contains an exact prohibited phrase
- Uses a close variant or synonym that conveys the same prohibited meaning
- Implies a prohibited claim even without using the exact words

### 3b. Approved language verification
When content makes a claim in a rule's domain, verify it uses approved language or
substantively equivalent framing. A violation occurs when the content addresses a
topic covered by the rule but uses neither approved phrasing nor a defensible
equivalent.

### 3c. Missing required elements
Check whether the content omits a required element (disclaimer, footnote, qualifier).
A violation occurs when the content's subject matter triggers a requirement from the
guide and that requirement is absent.

### 3d. Relevance filter
Before flagging a violation, confirm the content actually touches the rule's domain.
Do not flag a social post about one product line against another product line's
disclaimer rules. A category is marked N/A when the content does not engage with any
rule in that category.

### For each violation, record:

- **Segment label** + quoted excerpt (the specific flagged passage)
- **Rule name** + guide slug + category
- **Violation type:** prohibited term used / approved language missing / required element missing
- **Severity:** inherited from the rule definition
- **Suggested fix:** rewrite using approved language, or the exact required text to add

---

## Step 4 — Disclaimer Check

Special pass for rules in "Required Disclaimers" categories across all loaded guides:

1. For each required disclaimer rule, check whether the content includes the
   required text (exact or substantively equivalent wording).
2. If present, verify it matches the approved wording. Flag deviations that change
   the legal meaning.
3. If absent and the content's subject matter triggers the requirement, flag as a
   violation with the exact required text as the suggested fix.

---

## Step 5 — Assemble Output

Follow `REF-guidance-review-output-template-v1.0.md` exactly:

1. **Section A — Checklist Summary:** One table per guide. Categories listed with
   PASS/FAIL/N/A status, violation count, and max severity.
2. **Section B — Inline Annotations:** Violations in document order (by segment
   position). Each annotation shows the quoted excerpt, rule reference, issue
   description, and suggested fix.

If zero violations across all loaded guides, emit the all-PASS checklist and a
one-line confirmation. No Section B.

**Do not add** preamble, commentary, or recommendations beyond the structured output.
The review stands on its own.
