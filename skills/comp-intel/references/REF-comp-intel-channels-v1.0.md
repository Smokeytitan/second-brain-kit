```yaml
doc_type: REF
normative: false
requires: []
```

# Competitive Intelligence — Channels & Keywords (v1.0)

**Status:** Active v1.0
**Owner:** {SKILL_OWNER}
**Consumers:** Claude AI Agent
**Change control:** Update when channels are added/removed or new competitors
  are confirmed in scope.

---

## Overview

This file defines: (1) the Slack channels to read for internal competitor
signals, and (2) the keyword list for each competitor. Used in Step 1 of the
workflow (Slack scan).

<!-- Fill in your own channels, keywords, and competitor targets throughout. -->

---

## Section 1 — Slack Channels

### 1a — Primary channels (read in every run)

These channels have the highest concentration of competitor mentions across BD,
product, and exec activity. Typical high-yield channel types: sales-material
requests, product working groups, BD enablement, marketing working group.

| Channel | Channel ID | Why it matters |
|---|---|---|
| `{SALES_MATERIAL_CHANNEL}` | {CHANNEL_ID} | BD team requests — competitive objections surface here |
| `{PRODUCT_CHANNEL}` | {CHANNEL_ID} | Product discussions — competitor comparisons in product reviews |
| `{MARKETING_WG_CHANNEL}` | {CHANNEL_ID} | Marketing working group — narrative and positioning discussions |
| `{BD_ENABLEMENT_CHANNEL}` | {CHANNEL_ID} | BD enablement — most direct source of competitive objections |

### 1b — Secondary channels (read in baseline run; keyword-scan in standard runs)

| Channel | Channel ID | Why it matters |
|---|---|---|
| `{CUSTOMER_ONBOARDING_CHANNEL}` | {CHANNEL_ID} | Customer onboarding — win/loss context |
| `{PRICING_CHANNEL}` | (search by name) | Rate card and pricing discussions — competitor pricing signals |
| `{REGIONAL_BD_CHANNEL}` | (search by name) | Regional BD — regional competitor signals |
| `{EXEC_BROADCAST_CHANNEL}` | (search by name) | {EXEC}'s primary broadcast channel — competitor mentions go here |

### 1c — Executive channels (read in baseline run; scan for competitor keywords in standard runs)

{EXEC} actively uses these channels and may post competitor references directly:

| Channel | Why it matters |
|---|---|
| `{EXEC_BROADCAST_CHANNEL}` | {EXEC}'s broadcast posts often reference competitors by name |
| `{EXEC_PRODUCT_CHANNEL}` | {EXEC} posts shipping pressure signals here |
| `{PRICING_CHANNEL}` | {EXEC} enforces the rate card here — competitor pricing context |

### 1d+ — Mode-specific primary channels

For each additional product-line mode, add a section listing the channels to
read IN ADDITION to §1a when that mode is active:

| Channel | Channel ID | Why it matters |
|---|---|---|
| `{MODE_2_PRODUCT_CHANNEL}` | {CHANNEL_ID} | Primary product channel for that line — competitor mentions, feature comparison requests, BD objections |
| `{MODE_2_ENG_CHANNEL}` | {CHANNEL_ID} | Engineering channel — competitor SDK mentions in technical context |

---

## Section 2 — Competitor Keyword List

### Master keyword list (search across all primary channels)

Run all keywords below in a single batch at the start of Step 1. Each keyword
is searched with `after:AFTER_DATE` in Slack.

**Competitor names (exact):**
```
"{COMPETITOR_1}"
"{COMPETITOR_1_ALT_SPELLING}"
"{COMPETITOR_2}"
"{COMPETITOR_3}"
...
```

For competitor names that collide with common words, add a disambiguation
filter in parentheses, for example:
```
"{AMBIGUOUS_NAME}" (filter: flag only in [your category] context — exclude unrelated uses)
```

**Product/positioning terms that may signal competitor context:**
```
"competitor"
"competition"
"competitive"
"battlecard"
"vs {COMPETITOR_1}"
"vs {COMPETITOR_2}"
"versus"
"they offer"
"they have"
"switching from"
"switched from"
"why not {COMPETITOR_1}"
"objection"
"alternative"
"comparison"
"losing to"
"lost to"
"won against"
"beat them"
```

**Category-specific terms (per mode):**
```
"{CATEGORY_TERM_1}"     # protocol names, standards, feature-category phrases
"{CATEGORY_TERM_2}"     # that signal competitive context without naming a competitor
"{CATEGORY_TERM_3}"
...
```

### Mode-specific keyword lists

For each additional product-line mode, add:
- an exact-name list for that mode's competitors (with disambiguation filters)
- a context-term list for that mode's category vocabulary, including migration
  phrases ("switching from {COMPETITOR}", "migration from {COMPETITOR}",
  "already on {COMPETITOR}") — migration phrasing is the highest-signal pattern
  for displacement opportunities.

---

## Section 3 — Per-Competitor Web Research Targets

Used in Step 2 (web research). For each active competitor, list the sources to
fetch. One block per competitor:

### {COMPETITOR_1}
- Homepage: {competitor domain}
- Blog/announcements: {blog URL}
- Pricing: {pricing URL, or "not public — check homepage for published rate references"}
- LinkedIn company page (for funding/partnership announcements)
- Watch for: {the 2-4 specific change types that matter for this competitor}

### {COMPETITOR_2}
- Homepage: ...
- Blog: ...
- Changelog/docs: ...
- GitHub: {org URL} (SDK releases), if applicable
- Watch for: ...

<!-- Repeat per competitor, per mode. -->

### Blog domain quick reference (for Step 2e)

One row per active competitor with a confirmed blog domain, per mode:

| Competitor | Blog domain for Step 2e search |
|---|---|
| {COMPETITOR_1} | {blog domain} |
| {COMPETITOR_2} | {blog domain} |

---

## Section 4 — BD People Map Reference

The following BD people are most likely to post competitor references in Slack.
When a competitor keyword search returns a hit, check if the author is in this
list — BD-sourced signals (especially win/loss context) should be flagged in
the `BD deal context` field of the registry.

<!-- Fill in your own BD people map -->

| Name | Slack ID | Products they sell | Competitor context they typically carry |
|---|---|---|---|
| {BD_PERSON_1} | {SLACK_ID} | {product lines} | {competitors they most often reference} |
| {BD_PERSON_2} | {SLACK_ID} | {product lines} | {competitors} |
| {PRODUCT_EXEC} | (not BD but high-signal) | All products | Synthesizes competitive context into product decisions unprompted |
| {EXEC} | (CEO — not in BD map) | All | The competitors they track explicitly (from their profile) |

---

### §4b Developer Community Sources

Used by Step 1d (developer community web search). Search these contexts for developer-level
competitive signals that don't appear in internal Slack.

| Platform | Search approach |
|---|---|
| Reddit | `WebSearch` with `site:reddit.com` constraint; list the subreddits relevant to your category |
| Hacker News | `WebSearch` with `site:news.ycombinator.com` constraint; HN comment sections often contain "I tried X vs Y" developer comparisons |
| Dev.to | `WebSearch` with `site:dev.to` constraint; tutorial and comparison posts |
| Stack Overflow | `WebSearch` for specific error messages or "how do I migrate from [competitor] to" patterns |
| GitHub issues | Check competitor SDK issue tracker for recurring pain point labels: "bug", "feature request", "migration" |

**Developer community signal types:**
- `migration-signal` — Developer explicitly mentions moving from a competitor to your product or vice versa
- `developer-pain-point` — Developer describes a specific limitation with a competitor's SDK
- `competitive-comparison` — Developer compares two or more products for evaluation purposes

**Gap filing rule:** Do NOT file developer community signals as battlecard gaps unless the pain
point maps to a capability your product has already shipped. A developer complaint about a
competitor's pricing is that competitor's weakness, not your gap. A developer complaint about
a capability your product lacks IS a gap.

**Competitor GitHub SDK repos (for issue tracker monitoring):**

| Competitor | GitHub org / repo | What to check |
|---|---|---|
| {COMPETITOR_1} | {github org URL} | SDK issues; migration-related discussions |
| {COMPETITOR_2} | {github org URL} | SDK issues; feature requests; integration pain points |

---

## Section 5 — Social Handle Reference (for Step 2g)

Used by Step 2g (targeted social signal check). One row per competitor with known
X handle. Only searched when Step 2g triggers (notable signal found).

| Competitor | X handle |
|---|---|
| {COMPETITOR_1} | @{handle} |
| {COMPETITOR_2} | @{handle} |

**Note:** Handles marked with uncertainty should be confirmed by searching
`site:x.com "[competitor name]"` during Step 2g.
