```yaml
doc_type: REF
normative: false
requires: []
```

# New-Mode Competitor Registry — Stub Template (v1.0)

**Status:** STUB — placeholder entries need live data from a baseline run
**Owner:** {SKILL_OWNER}
**Mode:** `{new-mode}` — invoked via "run {new-mode} comp intel", "{new-mode} competitive scan", etc.

Use this stub when you spin up a NEW product-line mode and want the skill
runnable before a full baseline exists. The stub encodes what you already know
conservatively; the baseline run replaces placeholders with sourced data.

---

## Scope

[1-2 sentences: which product line this mode covers and which competitor
category it tracks.]

Your product context (as of {date}):
- **Launched:** [date or target]
- **Model:** [1-2 sentences on how the product works]
- **Distribution/venues:** [where it lives, key integrations]
- **Differentiators:** [the 2-4 claims you believe are true — mark `[DRAFT]` until confirmed]

---

## Registry

> Placeholder entries — fill in during the baseline run. Follow the format in
> `REF-competitor-registry-v1.0.md` for the full column set (Current narrative,
> Narrative changed, Pricing page URL, etc.).

Seed rules:
- Keep only claims already implied by materials you have validated; use
  `[DRAFT — NEEDS REVIEW]` where a counter or interpretation has not been
  validated in a baseline run.
- It is fine to seed with **reference/benchmark** competitors (category leaders
  that define the buyer's mental model) alongside direct competitors — mark
  which is which.

### {CATEGORY_LEADER} (reference/benchmark, not direct competitor)
- **Active:** Yes (reference only)
- **Why track:** Category leader. Defines the mental model for buyers and integrators. Narratives to watch: [list].
- **Last updated:** _TBD_

### {DIRECT_COMPETITOR_1}
- **Active:** [TBD]
- **Why track:** [the overlap with your product]
- **Last updated:** _TBD_

### Direct competitors on your platform
> If none identified yet, state that explicitly, define what a competitor would
> need to match (your qualifying criteria), and add entries here if they emerge.

---

## Your counter-claims vs the category

(Mirror the format in `REF-competitive-positioning-v1.0.md`. All claims
`[DRAFT — NEEDS REVIEW]` until the owner confirms.)

### vs {CATEGORY_LEADER}-style incumbents
- [structural differentiator 1 — `[DRAFT]`]
- [structural differentiator 2 — `[DRAFT]`]

### vs non-overlapping adjacent players
- [why they mostly don't threaten your positioning; what narrative spillover to watch for]

---

## Slack channels to scan for this mode's signals

(To add to `REF-comp-intel-channels-v1.0.md` under a new mode section):

- `{MODE_PROJECT_CHANNEL}` — the internal project channel for this product
- `{MODE_WG_CHANNEL}` — the working group channel
- `{MARKETING_WG_CHANNEL}` — marketing working group
- DMs with the partnership/integration owners for this product (by role)

---

## Web research targets (per-competitor)

- {CATEGORY_LEADER}: {blog URL}, @{x_handle}, {public metrics dashboards}
- {DIRECT_COMPETITOR_1}: {blog URL}, @{x_handle}

---

## Changelog

### v1.0
Initial stub template. Needs a live baseline run (`{new-mode} comp intel baseline
run`) to populate with current data from Slack scan + web research. Counter-claims
all `[DRAFT]` until the owner confirms the differentiators hold vs each competitor.
