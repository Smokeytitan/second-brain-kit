# [Agent Name]: [One-line job description]

**Owner:** [You]
**Cadence:** [e.g. Every Monday, 9:00 AM local (auto-run), plus on-demand]
**Delivery:** [e.g. Dated brief saved to `Resources/[Agent-Name]/YYYY-MM-DD-brief.md` AND a short summary DM]
**Last updated:** [date] (v1)

## Why this exists
[The gap this closes. What you'd miss without it, and which meetings/decisions it feeds.]

## Sources (pull all, every run)

| # | Source | Where | What to pull |
|---|--------|-------|--------------|
| 1 | [PRIMARY source of truth] | [Doc/list/channel + `{ID}`] | [What, exactly] |
| 2 | [Cross-check source] | [`{ID}`] | [Treat as incomplete; cross-check only] |
| 3 | [Chat channel] | [`{CHANNEL_ID}`] | [Signals to watch] |

## Method
1. Build the master list from the primary source.
2. Cross-reference against the other sources.
3. Flag: (a) items missing from the tracker, (b) items with no owner/coverage, (c) conflicts between sources, (d) slips since last run.
4. Classify each item by tier (below).
5. Write the dated brief, then send the headline summary.

## Triage lens (apply to every item)
- [Question 1 — e.g. does it change what customers/users must do?]
- [Question 2 — e.g. does it strengthen our core story?]
- [Question 3 — e.g. does it open or close a competitive gap?]

## Tiering
- **Major:** [criteria] → full treatment
- **Minor:** [criteria] → light touch
- **Sensitive/private:** [criteria] → NO public comms

## Output format (the brief)
Sections, in order: Needs your attention (next 14 days), Upcoming (~30 days table), Surfaced outside the tracker, On the roadmap (undated), Recently shipped, Coverage check, Data hygiene, Sources.

## Rules
- [House style rules the output must follow]
- [Escalation rule: what warrants an immediate ping vs. waiting for the brief]
