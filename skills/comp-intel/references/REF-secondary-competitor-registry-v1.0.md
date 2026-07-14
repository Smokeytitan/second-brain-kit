```yaml
doc_type: REF
normative: false
requires: []
```

# Competitor Registry — Secondary Product Line (v1.0)

**Status:** Template — populate via that mode's baseline run
**Owner:** {SKILL_OWNER}
**Consumers:** Claude AI Agent
**Last full update:** [PENDING baseline run]
**Change control:** Updated in-place each run of this mode. Registry entries are
  appended when new competitors are confirmed in scope. Entries are never deleted —
  set Status to `dormant` if a competitor goes quiet.

---

## How to Read This Registry

This is the master competitor map for a secondary product line ({PRODUCT_LINE_2}).
Each row represents one competitor. Fields are updated in-place each run — only
changed fields are updated; stable facts persist. The `Last updated` column shows
when each row was last touched.

**Status definitions:**
- `active` — mentioned in Slack or had a web signal in the last 14 days
- `monitor` — in scope, no recent signal; watch for changes
- `watchlist` — confirmed in scope for tracking but no baseline run yet; promote to active/monitor after first baseline
- `dormant` — previously active, no signal in 14+ days; de-prioritize but keep

**Cross-registry note:** Competitors that span multiple product lines should be
fully tracked in ONE registry (usually the default), and appear here only as
cross-references with this mode's specific angle. Do not duplicate full profiles —
read the primary registry for their full entries.

---

## Registry Table

<!-- One column per competitor in this product line. -->

| Field | {COMPETITOR_A} | {COMPETITOR_B} | {COMPETITOR_C} |
|---|---|---|---|
| **Product area** | ... | ... | ... |
| **Core claim** | [their primary positioning claim, in their own language] | ... | ... |
| **Key strengths** | ... | ... | ... |
| **Key weaknesses** | ... | ... | ... |
| **Pricing signals** | [published tiers with fetch dates; note when an internal Slack-cited price does not match the public page — likely older or custom quote] | ... | ... |
| **Audience** | ... | ... | ... |
| **Acquisition** | [acquirer + date + source URL, or "None confirmed" — acquisitions reframe narratives and commercial terms; track them explicitly] | ... | ... |
| **BD deal context** | [dated, sourced win/loss/objection context] | ... | ... |
| **Current narrative** | [homepage headline framing] | ... | ... |
| **Narrative changed** | [date + old → new + change type] | ... | ... |
| **Last updated** | [PENDING baseline run] | ... | ... |
| **Status** | watchlist | watchlist | watchlist |

---

## Extended Competitor Profiles

One short profile per competitor:

### {COMPETITOR_A}

**Full name:** ...
**Why they matter to your product:** [e.g., "the most-mentioned competitor in
the BD pipeline; X% of teams you talk to are already on them — meaning they are
a migration source, not just a competitor. Capture the migration friction and
whether you need a published migration-path story."]

**Key differentiator vs your product:** [where they lead; your counters]

**Migration note:** [any active displacement effort and who owns it — role, not name]

<!-- Repeat per competitor -->

---

## Elevated Competitors (promoted from Watchlist)

When a watchlist competitor earns a full entry (BD deal appearance, major ship,
acquisition), add a full field-table block here with the same fields as the
Registry Table, and note the promotion date.

---

## Cross-References to the Primary Registry

For competitors tracked fully in the primary registry, add a short block here
with only this mode's angle:

### {SHARED_COMPETITOR} — {PRODUCT_LINE_2} angle
[2-4 sentences: what about this competitor is specific to this product line,
and what signal types are battlecard-gap-relevant for this mode.]

---

## Competitor Watchlist ({PRODUCT_LINE_2} scope)

| Name | Why watching | Scope | First seen |
|---|---|---|---|
| {WATCHLIST_A} | [the signal that put them on the watchlist, with source] | ... | YYYY-MM-DD |

---

## Battlecard Gap Tracker Reference

Mode-specific battlecard gaps are tracked in the shared file:
`outputs/competitive/battlecard-gaps.md`

Filter for the mode tag (e.g., `[{mode-2}]`), applied starting from this mode's
first baseline run.

**Known pre-baseline gaps (seed — not yet formally filed):**
- [gap 1: e.g., "no published migration guide for teams moving from {COMPETITOR_A}"]
- [gap 2: e.g., "no published counter to {COMPETITOR_B}'s new category framing"]

---

## Buyer Segment Map

If you maintain buyer segments/personas, map each competitor to the segments
where they are strongest, and your win condition per segment. This tells the
run which competitor(s) to surface when a BD deal maps to a specific segment.

| Competitor | Strongest buyer segments | Verticals | Your win condition in segment |
|---|---|---|---|
| {COMPETITOR_A} | {segments} | {verticals} | {the capability or roadmap item that wins the segment} |
| {COMPETITOR_B} | ... | ... | ... |

**Notes:**
- If a win condition depends on an unshipped capability, mark it as contingent
  and do not pre-announce it in BD conversations (respect content holds).

**Narrative change types (used in registry `Narrative changed` field):**
- `pivot` — major category shift
- `expansion` — added a new narrative layer while keeping the existing one
- `consolidation` — narrowed scope (sharper, more targeted story)
- `acquisition-reframe` — narrative changed due to M&A
- `stable` — no change detected this run
