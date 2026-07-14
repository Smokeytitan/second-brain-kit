# Partner Roundup Drafter — Workflow

**Version:** 1.0.0
**Skill:** [partner-roundup-drafter](../SKILL.md)

This document defines the exact step-by-step workflow. Follow it in order. Do
not improvise mid-flow.

---

## Invocation Modes

| Mode | Trigger contains | Steps run |
|------|------------------|-----------|
| Full draft (default) | (any other trigger phrase) | 0–6 |
| Collection only | "collect" / "collection" | 0–3 (writes signals cache, stops) |
| Resume from cache | "resume" / "continue" | 4–6 (uses cached signals) |

---

## Step 0 — Window setup

Determine the publishing week:
- **End date** = the upcoming Friday (or today if today is Friday).
- **Start date** = end date − 7 days.
- **Output filename** = `YYYY-MM-DD-partner-roundup.md` where YYYY-MM-DD is the end date.
- Save to `../../Resources/Partner-Roundups/`.

If a file with this name already exists, append `-v2`, `-v3`, etc. Never
overwrite.

Print: "Drafting partner roundup for week {start} → {end}, output: {filename}".

---

## Step 1 — Pull the partner scanner output

Source: the partner-scanner Slack channel (`{PARTNER_SCANNER_CHANNEL}` — the
channel where your partner-scanner bot or teammate posts partner signals) AND
the most recent DM from the scanner owner ({PARTNER_SCANNER_OWNER}) that
contains a partner-roundup signal list. The owner sometimes drops the curated
list directly in DM rather than the channel.

Tools:
- `slack_read_channel` on `{PARTNER_SCANNER_CHANNEL}` for the past 7 days.
- If the channel is sparse, search DMs from {PARTNER_SCANNER_OWNER} for the
  last 7 days containing "partner" / "roundup" / "scanner".

Capture every partner item posted in the window. Save raw to:
`{output-dir}/.cache/{week}-raw-scanner-signals.json`

Each signal: `{partner_handle, beat, source_url, posted_at, scanner_note}`.

If zero signals found, halt and notify: "No partner signals from the scanner
this week — confirm the bot is running before drafting."

---

## Step 2 — Cross-channel Slack scan

Search Slack channels for additional partner activity not in the scanner.
Channels (fill in your own):

<!-- Fill in your own cross-channel scan list -->
- `{SOCIAL_FEEDS_CHANNEL}` — RT/QT candidates flagged by the team
- `{SOCIAL_TEAM_CHANNEL}` — social team coordination
- `{MARKETING_WG_CHANNEL}` — marketing working group
- `{COMMUNITY_CHANNEL}` — community/partner overview posts
- `{ADVOCACY_CHANNEL}` — employee-advocacy amplification flags

Search query patterns (run for each channel, last 7 days):
- "partner" OR "shipped" OR "live on {COMPANY}" OR "launched on {COMPANY}"
- partner handle mentions from `REF-partner-roundup-sources-v1.0.md` shortlist

Capture additional signals. Save to:
`{output-dir}/.cache/{week}-raw-cross-channel.json`

For each new signal, dedup against the scanner's list. Mark as
`cross_channel_only` if only present here.

---

## Step 3 — Dashboard fetches

Fetch the two reference dashboards:

### 3a. Flagship product dashboard
- URL: `{PRODUCT_DASHBOARD_URL}` — your flagship product's public metrics dashboard (e.g., a Dune dashboard)
- Pull metrics: product supply/TVL, holders, market share, rate metrics, days
  since launch — whatever the dashboard exposes for your product.
- Capture freshness timestamp (e.g., "11h ago").

### 3b. North-star metrics dashboard
- URL: `{NORTHSTAR_DASHBOARD_URL}` — your company's headline network/business metrics dashboard
- Pull metrics: headline supply/volume figures + 30d MoM change, transfer or
  transaction volume (all-time + 30d), activity counts (30d + 30d % growth),
  unique users/wallets (30d + 30d % growth), TVL or equivalent + 30d % change.
- Capture freshness timestamp per metric.

Save to: `{output-dir}/.cache/{week}-dashboard-snapshots.json`

**If a dashboard MCP is unavailable:** WebFetch the dashboard URL and parse what's
visible. If neither works, use the most recent cached snapshot from the prior
week's roundup file with a `⚠ freshness: stale (last updated {date})` flag.

After Step 3, signals are cached. **Collection-only mode stops here.**

---

## Step 4 — Dedup against last week + apply legal flags

### 4a. Last-week dedup
- Read the most recent file in `../../Resources/Partner-Roundups/` AND the most
  recent `Partner-Roundup-*.md` in `../../Resources/Digests/`.
- For each partner in this week's signals, check if it appeared last week.
- If yes AND the beat is the same: **drop**. Don't repeat.
- If yes AND the beat is materially new: **keep with `evolution: true` flag**.

### 4b. Legal flag pass (load `../../memory/context/legal-flags.md`)
- Scan every signal for your company's legal-flag categories. Typical examples
  (adapt to your own file):
  - **Yield/APY mentions** → drop or flag `⚠ yield claims blocked — {LEGAL_REVIEWER} signoff pending`.
  - **TVL / AUM / supply numbers** → keep but flag `⚠ verify with {METRICS_VERIFIER}`.
  - **Contract-language rules** → use the approved phrasing your legal team specified.
  - **Competitor names** → drop the bullet entirely.
  - **Confidential figures** → drop.
  - **Product naming rules** → ensure the approved full product name is used, not shorthand.

### 4c. Dedup duplicate beats from same partner
- If the same partner has 2+ items, collapse to the strongest beat unless they
  are genuinely distinct (e.g., a launch + a metric milestone).

Save deduped + legal-flagged list to:
`{output-dir}/.cache/{week}-deduped-signals.json`

---

## Step 5 — Compose the draft

Load `references/REF-partner-roundup-format-v1.0.md` for format spec.

### 5a. Pick the angle
Based on the dominant theme in this week's signals + dashboard snapshots, write a
1-line "this-week angle" for the file header. Examples:
- "The flagship product is 9 days old. The ecosystem keeps compounding around it."
- "Payments drove the week. Three partner launches, big supply growth."
- "Wallet integrations dominated this week's beats."

### 5b. Build the file structure
Sections in order:
1. Header (date range, sources, destination, angle)
2. Flagship product snapshot (table from dashboard 3a)
3. Network snapshot (split into "Use these" / "Handle with care" tables based on positive vs negative direction; per format spec)
4. Strategic takeaway (2–3 sentences)
5. Deduped partner items (numbered list with handle, beat, source URL)
6. Cross-check from the social-feeds channel (if any cross_channel_only items)
7. Version A (final) — publish-ready bullets
8. Optional Version B (thread) — only if 8+ items justify
9. Alternate opening hooks (3–4 options)
10. Algorithm notes (posting time, link rules, image rules)

### 5c. Compose Version A bullets
Use the established published format from last week:
- `■` bullet character (NOT `•`, NOT `-`).
- Each line: `@handle [verb] on/for {COMPANY} [optional concrete detail]`.
- 1-2 flagship product or company milestone bullets at the end (not partner items).
- Closing line: punchy 1-line summary in the user's voice.

Example pattern:
```
■ @PartnerA launches X on {COMPANY}
■ @PartnerB ships Y for {COMPANY} users
■ @PartnerC reports Z volume on {COMPANY}
■ {FLAGSHIP_PRODUCT} crosses {milestone}, {N} days post-launch
■ {COMPANY} {headline metric} tops ${N}, up {N}% this month

The rails are live. Every week, more ships.
```

### 5d. Apply the brand voice
- No em dashes. Colons, commas, periods, semicolons.
- No marketing-speak ("revolutionizing," "game-changing," etc.).
- Active voice. Short sentences.
- Closing line punchy, not preachy.

---

## Step 6 — Save and notify

### 6a. Write the file
Save to `../../Resources/Partner-Roundups/{filename}` with the full structure.

### 6b. Notify the user
Print to chat:
```
✅ Partner roundup drafted for week {start} → {end}.

File: Resources/Partner-Roundups/{filename}
Version A:

{paste Version A text inline}

Flags for review before posting:
- {list any ⚠ items — TVL/AUM/yield pending verification, freshness warnings, dedup notes}

Next: review the file, edit if needed, copy Version A into your posting tool.
```

### 6c. Emit run telemetry (if a telemetry emitter is wired)
Telemetry record structure:
```yaml
skill: partner-roundup-drafter
version: 1.0.0
mode: full | collection | resume
window_start: YYYY-MM-DD
window_end: YYYY-MM-DD
output_file: Resources/Partner-Roundups/{filename}
signals_scanner: {count}
signals_cross_channel: {count}
signals_after_dedup: {count}
dashboard_freshness: {ok | stale | unavailable}
legal_flags_raised: {count}
status: success | partial | failed
```

If no telemetry emitter is installed, skip telemetry (no error).

---

## Halt conditions

Halt and notify the user in any of these cases:

- **No signals found** in Step 1 (scanner empty AND no DM signals).
- **Slack MCP unavailable** AND no recent cached signals to fall back on.
- **Legal flags fail to load** (Step 4b).
- **`legal-flags.md` flags a confidentiality risk** that can't be auto-resolved
  (a confidential number leaked into a partner bullet).
- **Output file write fails.**

---

## Notes

- The skill is invokable manually now. Once scheduled-task wiring is in place,
  set cadence to **Friday morning** (giving the user time to review +
  forward to the social lead for Monday morning posting per the algorithm notes).
- After 3+ runs, audit which Slack channels in Step 2 actually surface
  high-quality signals. Drop low-yield channels from the scan list to save
  context.
- Future enhancement: pull the scanner channel directly from the bot's
  output stream via webhook rather than Slack search — would eliminate the
  DM fallback path.
