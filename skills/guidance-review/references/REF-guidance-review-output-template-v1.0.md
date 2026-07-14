---
doc_type: REF
title: Guidance Review Output Template
version: 1.0
description: >
  Defines the two-section output format for the guidance-review skill:
  Section A (checklist summary) and Section B (inline annotations).
---

# Guidance Review Output Template

The review output has exactly two sections, emitted in order. No preamble before
Section A. No commentary between sections.

---

## Section A — Checklist Summary

One table per loaded guide. If multiple guides were loaded, emit one table per guide
with its display name as the heading.

```markdown
## Guidance Review Results

### {guide_display_name}

| Category | Status | Violations | Severity |
|----------|--------|------------|----------|
| {category_display_name} | PASS / FAIL / N/A | {count} | {max_severity or --} |
| ... | ... | ... | ... |

**Overall: {n} FAIL, {n} PASS, {n} N/A**
```

**Status logic:**
- **PASS** — zero violations found in this category
- **FAIL** — one or more violations found
- **N/A** — the content does not touch this category's domain at all (e.g., a tweet
  about product performance has no relevance to another product line's "Required
  Disclaimers")

**Severity column** shows the highest severity among violations in that category.
If PASS or N/A, show `--`.

---

## Section B — Inline Annotations

Violations listed in **document order** (by segment position), not grouped by rule.
This lets the reader walk through their content top-to-bottom and see each issue
in context.

```markdown
### Inline Annotations

---

**[{segment_label}]**
> "{quoted excerpt from the content — the specific flagged passage}"

**VIOLATION:** {category_display_name} > "{rule_name}" ({severity})
- **Issue:** {what the violation is and why it matters}
- **Suggested fix:** {rewritten text using approved language, or the required element to add}

---
```

**Segment labels** use the format:
- `[Headline]` — the main title/H1
- `[Subheadline — "{text}"]` — H2/H3 headings
- `[Body — paragraph {n}]` — body paragraphs in order
- `[CTA]` — calls to action, buttons, link text
- `[Footnote/Disclaimer]` — existing disclaimers or footnotes
- `[Missing — Footer]` or `[Missing — Disclaimer]` — for required elements that are absent

If a single passage violates multiple rules, list each violation under the same
segment label (do not repeat the quoted excerpt).

---

## Zero Violations

If no violations are found across all loaded guides:

```markdown
## Guidance Review Results

### {guide_display_name}

| Category | Status | Violations | Severity |
|----------|--------|------------|----------|
| ... | PASS | 0 | -- |

**Overall: 0 FAIL, {n} PASS, {n} N/A**

No violations found against {guide_display_name}.
```

No Section B is emitted.
