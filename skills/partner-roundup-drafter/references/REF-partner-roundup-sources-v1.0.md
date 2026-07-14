# Partner Roundup — Sources Reference

**Version:** 1.0.0

The canonical list of input sources for the partner roundup. Update this file
when channels, dashboards, or partner shortlists change.

<!-- Fill in your own sources throughout this file -->

---

## Slack channels (in priority order)

### Primary

| Channel | Why | Owner |
|---------|-----|-------|
| `{PARTNER_SCANNER_CHANNEL}` | Your partner-scanner bot drops signals here. THE primary source. | {PARTNER_SCANNER_OWNER} |
| Scanner-owner DM | Sometimes the curated weekly list lands in DM rather than the channel. Check both. | {PARTNER_SCANNER_OWNER} |

### Cross-channel scan (Step 2)

| Channel | Signal type |
|---------|-------------|
| `{SOCIAL_FEEDS_CHANNEL}` | RT/QT candidates flagged by team — high yield for partner moves |
| `{SOCIAL_TEAM_CHANNEL}` | Social team coordination |
| `{MARKETING_WG_CHANNEL}` | Marketing working group, partner posts surface here |
| `{COMMUNITY_CHANNEL}` | Community/partner overview posts |
| `{ADVOCACY_CHANNEL}` | Employee-advocacy amplification flags |

---

## Metrics dashboards

### Flagship product dashboard
- **URL:** `{PRODUCT_DASHBOARD_URL}` — your flagship product's public metrics dashboard (e.g., Dune)
- **Owner:** {DASHBOARD_OWNER}
- **Metrics to pull:**
  - Product supply (formatted with thousand separators)
  - TVL (USD, formatted as $X.XM)
  - Holders (count)
  - Product share of its category (%)
  - Rate/exchange metric (4 decimal places)
  - Category total network-wide
  - Days since launch (computed from the product's launch date)
- **Refresh cadence:** ~hourly. Capture timestamp for "freshness" column.

### North-star dashboard
- **URL:** `{NORTHSTAR_DASHBOARD_URL}` — your company's headline metrics dashboard
- **Owner:** {DASHBOARD_OWNER}
- **Metrics to pull:**
  - Headline supply metric (total + 30d MoM %)
  - Transfer volume (all-time + 30d)
  - Trading volume (30d + all-time)
  - Transactions (all-time + 30d + 30d % growth)
  - Unique users (30d + 30d % growth)
  - TVL or equivalent (total + 30d % change)
- **Split rule:** Metrics with positive 30d direction → "Use these" table. Metrics with negative 30d direction → "Handle with care" table (don't lead with growth claims if direction is negative).

### Future dashboard additions
- Additional product or category dashboards as they mature
- Per-product TVLs

---

## Partner shortlist

This is the list of partner accounts the agent should specifically watch for in
cross-channel scans. Update when the partner ecosystem changes. Source:
your partner-scanner spreadsheet + your community team's tracked accounts list.

<!-- Fill in your own partner shortlist -->

### Tier 1 — High-frequency partner accounts (always watch)
- @{PARTNER_HANDLE_1}
- @{PARTNER_HANDLE_2}
- @{PARTNER_HANDLE_3}
- ... (the 10–15 accounts that most often produce roundup-worthy beats)

### Tier 2 — Watchlist (mention triggers attention but not always include)
- Other protocols/apps in your ecosystem
- Other complementary products (only if relevant to your company)
- Partner exchanges/platforms — only if the beat is relevant to your company

### Tier 3 — Ecosystem coverage (track but rarely include)
- Industry media accounts — coverage of your company, not partner moves

---

## Dedup rules

### Cross-week dedup
- Read previous week's roundup (most recent in `Resources/Partner-Roundups/`).
- If a partner appeared with beat X last week AND has beat X this week: DROP.
- If a partner appeared with beat X last week AND has beat Y this week: KEEP with `evolution: true` flag.

### Same-week dedup (within scanner signals)
- If a single partner has 2+ signals within the week, collapse unless distinct events:
  - Distinct: launch + metric milestone (e.g., PartnerA launches X + PartnerA reports $470K subsidies — both keep).
  - Not distinct: launch + retweet of launch (collapse to one).

### Cross-source dedup (scanner vs cross-channel)
- If the same beat is flagged by the scanner AND in `{SOCIAL_FEEDS_CHANNEL}`: keep the scanner's, note the cross-channel confirmation as a strength signal in the takeaway.

---

## Items typically EXCLUDED

- Pure ecosystem coverage (e.g., a media article about your company) — that's PR territory, not partner roundup.
- Internal team posts (execs, founders, etc.) — those go in their own social calendar.
- {COMPANY_HANDLE} RTs that aren't standalone partner moves.
- Generic "listed/integrated" without a partner-driven story.

---

## Cross-References
- **Skill:** [../SKILL.md](../SKILL.md)
- **Workflow:** [RUN-partner-roundup-drafter-workflow-prd-v1.0.md](RUN-partner-roundup-drafter-workflow-prd-v1.0.md)
- **Format spec:** [REF-partner-roundup-format-v1.0.md](REF-partner-roundup-format-v1.0.md)
- **Legal flags:** [../../../memory/context/legal-flags.md](../../../memory/context/legal-flags.md)
- **Partner Scanner spreadsheet:** `{PARTNER_SCANNER_SPREADSHEET}` — your partner-tracking spreadsheet
