```yaml
doc_type: REF
normative: false
requires: []
```

# Competitor Registry — Default Mode (v1.0)

**Status:** Template — populate via baseline run
**Owner:** {SKILL_OWNER}
**Consumers:** Claude AI Agent
**Last full update:** [PENDING baseline run]
**Change control:** Updated in-place each comp-intel run. Registry entries are
  appended when new competitors are confirmed in scope. Entries are never deleted —
  set Status to `dormant` if a competitor goes quiet.

---

## How to Read This Registry

This is the master competitor map for your default product line. Each row represents one
competitor. Fields are updated in-place each run — only changed fields are
updated; stable facts persist. The `Last updated` column shows when each row
was last touched.

**Status definitions:**
- `active` — mentioned in Slack or had a web signal in the last 14 days
- `monitor` — in scope, no recent signal; watch for changes
- `dormant` — previously active, no signal in 14+ days; de-prioritize but keep

---

## Registry Table

<!-- One column per competitor. Fill in via the baseline run; keep entries sourced and dated. -->

| Field | {COMPETITOR_1} | {COMPETITOR_2} | {COMPETITOR_3} |
|---|---|---|---|
| **Product area** | [which of your product lines they compete with] | ... | ... |
| **Core claim** | [their primary positioning claim, in their own language] | ... | ... |
| **Key strengths** | [sourced strengths: compliance posture, brand, DX, distribution, pricing, install base] | ... | ... |
| **Key weaknesses** | [sourced weaknesses: gaps in their stack, pricing complaints, onboarding friction, coverage gaps] | ... | ... |
| **Pricing signals** | [published pricing, or "not publicly disclosed"; include internal BD pricing signals with channel/date/author cite] | ... | ... |
| **Audience** | [who they sell to] | ... | ... |
| **BD deal context** | [dated, sourced win/loss/objection context from BD channels and call transcripts — the highest-value field in the registry; every entry cites channel + date + author] | ... | ... |
| **Current narrative** | [their homepage headline framing — used by Step 2f narrative shift detection] | ... | ... |
| **Narrative changed** | [date + old → new + change type, when Step 2f detects a shift] | ... | ... |
| **Pricing page URL** | [stable URL for the Step 2b unconditional pricing check] | ... | ... |
| **Last updated** | [PENDING baseline run] | ... | ... |
| **Status** | monitor | monitor | monitor |

---

## Extended Competitor Profiles

One profile section per competitor. Populate during the baseline run.

### {COMPETITOR_1}

**Full name:** ...
**Founded:** ...
**Headquarters:** ...
**What they do:** [2-3 sentences: their product, their notable clients]

**Why they matter to your product:** [Why this competitor is in scope: which
buyers they contest, which capability overlap matters most]

**Key differentiator vs your product:** [Where they lead; where you lead. Be
specific — this feeds the battlecard.]

**Live BD intelligence:** [The deal-level pattern: why they win, why they lose,
with cites. E.g., "loses deals on three vectors: pricing, onboarding friction,
customer relationships — sourced from {channel}, {date}."]

<!-- Repeat per competitor -->

---

## Competitor Watchlist (not yet in registry — flag if they appear in Slack)

| Name | Why watching | Scope |
|---|---|---|
| {WATCHLIST_COMPETITOR_1} | [the signal that put them on the watchlist, with source] | [product line] |
| {WATCHLIST_COMPETITOR_2} | ... | ... |

**Promotion rule:** When a watchlist competitor surfaces in a BD deal signal or
ships something that contests your positioning, promote them to the registry
with a full profile and run their first web-research pass.
