```yaml
doc_type: RUN
normative: true
requires:
  - REF-comp-intel-persona-prd-v1.0
  - REF-competitor-registry-v1.0
  - REF-comp-intel-channels-v1.0
```

# Competitive Intelligence — Execution Workflow (v1.0)

**Status:** Active v1.0
**Owner:** {SKILL_OWNER}
**Consumers:** Claude AI Agent
**Change control:** PR Review

---

## 0. Overview

This runbook defines the exact sequence the AI executes when "run comp intel"
or any registered trigger is used. Execute all steps in order. No human gate —
run headlessly from start to finish and present completed output. Do not emit
conversational text between steps.

**Run cadence:** Standard runs look back 3 days. Baseline run (first run only)
looks back 120 days. See §Baseline Run below.

**Lookback computation:**
- Standard: `AFTER_DATE = today - 3 days` (format: `YYYY-MM-DD`)
- Baseline: `AFTER_DATE = today - 120 days` (first run for a mode)

---

## Run Mode Detection

Detect run mode from the trigger phrase before loading any KB files.

| Trigger contains | RUN_MODE | Registry to load | Output file prefix |
|---|---|---|---|
| "{PRODUCT_LINE_2}" | `{mode-2}` | `REF-secondary-competitor-registry-v1.0.md` | `YYYY-MM-DD-comp-intel-{mode-2}-report.md` |
| "{PRODUCT_LINE_3}" | `{mode-3}` | (that mode's registry file) | `YYYY-MM-DD-comp-intel-{mode-3}-report.md` |
| anything else | `default` | `REF-competitor-registry-v1.0.md` | `YYYY-MM-DD-comp-intel-report.md` |

<!-- Add one row per product-line mode you configure. -->

`RUN_MODE` governs: which registry loads (Step 0), which Slack keywords run (Step 1a),
which channels are primary (Step 1b), which web research targets apply (Step 2),
which positioning docs load for gap analysis (Step 4a), and which output path is used (Step 6a).

**Priority note for trigger overlap:** If a trigger matches more than one mode name,
pick the more specific mode; document your tiebreak order here.

---

## Invocation Modes

The skill supports three invocation modes. Detect the mode from the trigger phrase after
resolving `RUN_MODE`.

| Mode | Trigger phrases | Behavior |
|---|---|---|
| **Full run** | "run comp intel", "run comp intel for {PRODUCT_LINE_2}" | Runs Steps 0–6. Writes signals cache, snapshots, report, and run checkpoint. No human gate. |
| **Collection only** | "collect comp intel", "run comp intel collection", "collect {PRODUCT_LINE_2} comp intel" | Runs Steps 0–2 only. Writes signals cache + checkpoint. Stops and notifies. |
| **Resume** | "resume comp intel", "continue comp intel", "resume {PRODUCT_LINE_2} comp intel" | Reads cached signals from most recent collection checkpoint. Runs Steps 3–6. No live Slack or web calls. |

**Collection only** and **Resume** together replace a single long run: trigger collection
(slow, live data), walk away, come back and say "resume comp intel" to run the analysis from
cached signals.

---

## Step 0 — Load Knowledge Base

Read the following files before taking any other action:

1. `references/REF-comp-intel-persona-prd-v1.0.md`
2. The registry file matching `RUN_MODE` (see Run Mode Detection table)
3. `references/REF-comp-intel-channels-v1.0.md`
4. Most recent {EXEC} profile in `outputs/people/` (if you maintain one)

Hold all in context for the entire run. If any file fails to load, halt
and report which file is missing.

---

## Step 1 — Slack Scan (Internal Signals)

Goal: discover which competitors are being discussed internally right now, what
context they came up in, and what the team is concerned about or impressed by.

### 1a — Keyword searches (all accessible channels)

Compute `AFTER_DATE`. Run the following searches using `slack_search_public` or
`slack_search_public_and_private`. Run competitor name searches and context
term searches in parallel.

**Competitor name queries (per mode — take the names from the active registry):**
```
"{COMPETITOR_1}" after:AFTER_DATE
"{COMPETITOR_2}" after:AFTER_DATE
"{COMPETITOR_3}" after:AFTER_DATE
...
```
Include common spelling variants (e.g., both "ZetaCorp" and "Zeta Corp") and add
disambiguation filters for names that collide with common words (see the channels
file §2 for the filter pattern).

**Context term queries (per mode — adapt to your category vocabulary):**
```
"competitor" after:AFTER_DATE
"battlecard" after:AFTER_DATE
"vs {COMPETITOR_1}" after:AFTER_DATE
"vs {COMPETITOR_2}" after:AFTER_DATE
"switching from" after:AFTER_DATE
"why not {COMPETITOR_1}" after:AFTER_DATE
"objection" after:AFTER_DATE
"{CATEGORY_TERM_1}" after:AFTER_DATE
"{CATEGORY_TERM_2}" after:AFTER_DATE
```
Category terms are the technology or market phrases that signal competitive
context even without a competitor name (e.g., protocol names, feature-category
names, standards).

### 1b — Channel reads (primary channels)

Read the primary channels for the active `RUN_MODE` directly for the lookback
window using `slack_read_channel`. Channel lists per mode live in
`REF-comp-intel-channels-v1.0.md §1`.

| Channel | Channel ID |
|---|---|
| `{SALES_MATERIAL_CHANNEL}` | {CHANNEL_ID} |
| `{PRODUCT_CHANNEL}` | {CHANNEL_ID} |
| `{BD_ENABLEMENT_CHANNEL}` | {CHANNEL_ID} |
<!-- Fill in the mode's primary channels from the channels file -->

### 1c — Process Slack results

For each Slack hit:
1. Record: channel, date, author (Slack ID + display name if available), and
   a direct quote or close paraphrase (max 2 sentences).
2. Classify the competitor referenced (map to registry entry).
3. Classify the signal type:
   - `mention` — competitor named in passing
   - `objection` — prospect or BD person raised competitor as an obstacle
   - `win-context` — competitor mentioned in a deal your company won
   - `loss-context` — competitor mentioned in a deal your company lost or is at risk
   - `product-comparison` — team compared your product and competitor features
   - `pricing-signal` — competitor pricing mentioned
4. Note if the author is in the BD people map (check
   `REF-comp-intel-channels-v1.0.md §4`) — BD-sourced signals are higher
   fidelity for `BD deal context` registry field updates.

Output: ranked list of competitors by mention count. Flag any competitor with
`objection`, `loss-context`, or `pricing-signal` types — these are the
highest-priority signals for Step 5 (Executive Relevance).

**Win/loss extraction:** When a signal is classified as `win-context` or `loss-context`:
- Extract: deal/company name (if mentioned), competitor(s), signal type (`eval-in-progress`,
  `potential-win`, `confirmed-win`, `confirmed-loss`, `no-decision`), key criteria quoted,
  whether your product was mentioned, Slack source (channel, date, author).
- Stage for win-loss-log append in Step 6b. Do NOT write to log yet — wait for Step 6b assembly.
- Always set status to `signal` (unconfirmed) — never classify as `confirmed-win` or
  `confirmed-loss` without explicit user instruction.

### 1d — Developer community web search

Runs after Step 1c. Only for:
- Competitors with `Status: active` in registry
- Any competitor that surfaced in Steps 1a–1c this cycle

For each qualifying competitor, run two WebSearch queries:

**Migration/comparison search:**
`"[competitor name]" developer "migrated" OR "switched" OR "alternative" OR "vs {YOUR_PRODUCT}" OR "moved from" after:AFTER_DATE`
Scope to: `site:reddit.com OR site:news.ycombinator.com OR site:dev.to`

**Pain point search:**
`"[competitor name]" problem OR issue OR limitation OR "doesn't support" developer after:AFTER_DATE`

For each result found, record:
- URL (post or comment link)
- Date of post/comment
- Platform (Reddit / HN / Dev.to)
- Quoted text (max 2 sentences)
- Classification: `developer-pain-point` / `competitive-comparison` / `migration-signal`

**Hallucination guard:** Same rules as Slack (§3 of persona doc). Quote directly. Do not infer sentiment.
If a post says "we're evaluating {COMPETITOR_1} vs {COMPETITOR_2}" — that is an evaluation signal,
not a loss signal.

**Output:** Add developer community signals as a separate sub-block in Step 3 snapshots under
"Developer community signals this cycle" (empty block if no results).

Do NOT file developer community signals as battlecard gaps unless the pain point maps to a
capability your product has already shipped. A developer complaint about a competitor's pricing
is that competitor's weakness, not your gap. A developer complaint about a capability your
product lacks IS a gap.

---

## Step 2 — Web Research (External Signals)

Goal: discover what competitors have shipped, announced, or changed their
positioning on in the lookback window.

**Which competitors to research this step:**
- Any competitor with a Slack signal in Step 1 (regardless of signal type).
- Any competitor with `Status: active` in the registry.
- For baseline run: all competitors in the registry.

For each in-scope competitor, execute the following:

### 2a — Blog / announcement search

Search for: `[competitor name] announcement OR blog OR release OR launch
after:AFTER_DATE`

Use `WebSearch` with a date-filtered query. Fetch the top 1-2 results using
`WebFetch`.

### 2b — Pricing page check (unconditional for all Active competitors)

For ALL competitors with `Status: active` in the registry, fetch their pricing
page URL from `REF-comp-intel-channels-v1.0.md §3`. Note any changes vs. the
registry's `Pricing signals` field.

This check runs every run regardless of whether Step 1 surfaced pricing signals.
Pricing pages are stable URLs with low fetch cost — unconditional monitoring
catches silent pricing changes that would otherwise be missed.

If a competitor has no known public pricing page, skip and note "pricing not
publicly available" in the snapshot.

### 2c — GitHub releases (for competitors with public SDKs only)

Use the GitHub MCP tools or check the GitHub releases page for the competitor
SDK repos listed in `REF-comp-intel-channels-v1.0.md §4b`.

Only if there were Slack signals for these competitors or they are `active` in
the registry.

### 2d — Hallucination guard for web research

For every piece of information sourced from the web:
- Record the URL and the publication/last-updated date.
- If the page does not have a clear date, note "date unclear" — do not assume
  the content is current.
- Do not infer that a feature is shipped based on a documentation page alone.
  A docs page may describe an unreleased feature. Only claim shipping if you
  see a blog post, changelog entry, or press release dated within the lookback
  window.

### 2e — Proactive blog/changelog scan (all modes)

Run for ALL competitors with `Status: active` in registry, regardless of whether Step 1 had
signals for them. This ensures product velocity changes are caught even when nobody posts
about them in Slack.

For each active competitor: `WebSearch` for
`"[competitor name]" release OR update OR announcement OR blog site:[competitor-blog-domain] after:AFTER_DATE`

Blog domains per competitor live in the channels file §3 (blog domain quick
reference table per mode).

If a result has a post dated within the lookback window AND contains product capability or
pricing language → fetch the full post with `WebFetch`, record URL + date, include in Step 3 snapshot.
If no posts dated in window → write "No blog activity found in [AFTER_DATE] to [today] window."
Do NOT include posts from outside the lookback window as "recent moves."

### 2f — Narrative shift detection

After homepage fetch (Step 2a), for each competitor fetched this run:
1. Extract: homepage primary headline + subheading + primary CTA text
2. Compare to registry `Current narrative` field for this competitor
3. If materially different OR if `Current narrative` is empty → flag narrative shift

**Classification rules:**
- Acquisition announced → type: `acquisition-reframe`
- New primary category claim (e.g., added "AI agents" to headline) → type: `expansion`
- Previous primary claim no longer on homepage → type: `pivot` or `consolidation`
- No change → type: `stable` (do NOT write to narrative-tracker.md for stable)

**If shift detected:**
- Write new row to `outputs/competitive/narrative-tracker.md`
- Update registry `Current narrative` and `Narrative changed` fields
- Add "**Narrative shift detected**" flag to this competitor's Step 3 snapshot

**Narrative conflict check (inline, during Step 2f):**
Scan new narrative text for the positioning terms your company is actively claiming
(list them in `REF-competitive-positioning-v1.0.md §1` — e.g., the headline phrases
of your current messaging arcs).
If any match → flag as `narrative-conflict` and note which of your narrative arcs is
being contested.

---

### 2g — Targeted social signal check (all modes)

Runs after Step 2f. This is NOT full social media monitoring — it enriches existing
signals with social proof data only when warranted.

**Trigger condition:** Run Step 2g ONLY if Steps 1-2 surfaced at least one notable
signal classified as: funding announcement, acquisition, major partnership, or
product launch (new product, not incremental update).

**For each notable signal:**
1. `WebSearch` for `"[competitor name]" "[signal keyword]" site:x.com OR site:twitter.com`
2. If a matching announcement post is found, record:
   - URL
   - Date
   - Quoted text (the tweet/post body, max 2 sentences)
   - Engagement metrics if visible (likes, retweets, views — approximate)
3. Include in signals cache under `## Social Signals (Step 2g)`

**If no notable signals were found in Steps 1-2:** Skip Step 2g entirely. Write
"Step 2g skipped — no notable signals to enrich."

**Social handle reference:** See `REF-comp-intel-channels-v1.0.md §5` for
X handles per competitor.

**Hallucination guard:** Same rules as web research (§2d). Quote directly. If the
tweet URL cannot be fetched, note "social signal not confirmed — manual check
recommended."

---

### Save signals cache

After completing Step 2, write a signals cache file silently. The cache must contain
everything Steps 3–6 need to run without any live data calls:
- All Slack signals from Step 1 (competitor, channel, date, author, quote, signal_type)
- All staged win/loss entries from Step 1c
- All web research findings from Step 2 (blog posts, pricing changes, GitHub releases)
- Narrative shift records from Step 2f (old narrative, new narrative, change type, arc conflict)
- `AFTER_DATE` and today's date

**Default mode:** `outputs/competitive/YYYY-MM-DD-comp-intel-signals.md`
**Other modes:** `outputs/competitive/YYYY-MM-DD-comp-intel-{mode}-signals.md`

**In full run mode:** Proceed immediately to Step 3.

**In collection-only mode:** After writing the signals cache, write a checkpoint to
`state/runs/comp-intel-YYYY-MM-DD-{run_mode}.yaml`:
```yaml
product: {product_line_slug}
skill_slug: comp-intel
run_date: "YYYY-MM-DD"
run_mode: "default"   # or the active mode name
after_date: "YYYY-MM-DD"
mode: collection
phase: 1-signals
artifacts:
  signals: "outputs/competitive/YYYY-MM-DD-comp-intel-signals.md"
```

**Wrapper note:** If you run this skill via a collection wrapper script, the checkpoint is
written automatically after a successful run. Only write it manually when running in a
direct chat session without the wrapper.

Then stop and print:

> "Collection complete — signals cached.
> - Signals: `outputs/competitive/YYYY-MM-DD-comp-intel-signals.md`
>
> Say **'resume comp intel'** (or the mode-specific variant) when ready."

---

## Step 3 — Positioning Snapshots

**Resume entry point:** If triggered by "resume comp intel" or "continue comp intel"
(any mode), begin here instead of Step 0. Do the following before Step 3:

1. Check for the most recent checkpoint at `state/runs/comp-intel-*.yaml` matching the
   resolved `RUN_MODE`. If found, read `phase` and `artifacts` to get the signals cache path.
   If no checkpoint found, glob `outputs/competitive/*-signals.md` for the most recent match.
2. Load the signals cache file. If not found, reply:
   *"No signals cache found. Run 'collect comp intel' first."* and stop.
3. Do not make any live Slack or web calls. Use only cached data from the signals file.

Goal: write a dated snapshot for each competitor that had new signals this cycle.

For each competitor with at least one signal from Step 1 or Step 2:

1. Write a new snapshot file at:
   `outputs/competitive/positioning/YYYY-MM-DD-[competitor-slug]-snapshot.md`

   Competitor slugs: lowercase hyphenated competitor names, defined per mode in
   the active registry.

2. Use the positioning snapshot format defined in
   `REF-comp-intel-persona-prd-v1.0.md §4`.

3. For the `Recent moves (this cycle)` section: only include things you
   sourced in Step 1 or Step 2 of this run. If no new moves: write "No new
   signals found this cycle. Last known state: [date from registry]."

4. Do not overwrite existing snapshot files. Each run creates new snapshot
   files with the current date.

**For baseline run:** Write snapshots for all competitors using the best
available sources. The `Recent moves` section should summarize the most
significant signals from the 120-day window, grouped by month if possible.

---

## Step 4 — Gap Analysis

Goal: identify where your company's messaging does not counter a competitor claim.

### 4a — Load your positioning

Read the positioning docs for the active `RUN_MODE`:
- Your product positioning / messaging doc for that product line ({POSITIONING_DOC} —
  wherever your approved product positioning lives)
- `references/REF-competitive-positioning-v1.0.md` (per-competitor counters)

Use the competitive positioning doc (§§2–5) to assess gap severity for each competitor claim:
- **STRONG** — you have a specific published counter in §§2–5 (no `[DRAFT]` tag)
- **WEAK** — Counter exists but marked `[DRAFT — NEEDS REVIEW]` or `[COUNTER NEEDED]`
- **MISSING** — No entry in §§2–5; file gap row in `battlecard-gaps.md` AND add `[COUNTER NEEDED]` note to the competitive positioning doc

### 4b — Cross-reference and identify gaps

For each competitor claim surfaced in Step 2 (web research):
1. Check `REF-competitive-positioning-v1.0.md` §§2–5 for a counter to this specific claim.
2. If counter exists and is not `[DRAFT]`: STRONG. Note "counter: strong" in the report.
3. If counter exists but marked `[DRAFT]` or `[COUNTER NEEDED]`: WEAK. Note
   "counter: needs work" in the report. Do NOT file a gap row.
4. If no counter entry exists: MISSING. File a gap row in `outputs/competitive/battlecard-gaps.md`.

### 4c — Write gap rows

Append new gap rows to `outputs/competitive/battlecard-gaps.md` using this
format:

```
| [ ] open | [Competitor] | [Their exact claim, in quotes if possible] | [Best available counter, or "missing"] | YYYY-MM-DD | — |
```

Dedup rule: Before appending, scan existing rows for the same competitor +
similar claim. If a match exists within 20% of the claim language, skip the
new row and note in the report: "Gap already tracked (filed YYYY-MM-DD)."

### 4d — Narrative conflict gap filing

For any competitor with a `narrative-conflict` flag from Step 2f:
File a gap row in `outputs/competitive/battlecard-gaps.md` with:
- Prefix `[narrative-conflict]` in the "Their Claim" column
- Their Claim: quote the exact narrative language that overlaps with your positioning arc
- Your Counter: check `REF-competitive-positioning-v1.0.md §1` for a published counter-narrative
- Note which of your narrative arcs is being contested

Narrative-conflict gaps are higher priority than capability gaps — they represent a competitor
actively fighting for the same positioning space, not just building competing features.
Flag narrative conflicts in Step 5 (executive relevance filter) when they map to {EXEC}'s
live concerns about brand clarity or category momentum.

---

## Step 5 — Executive Relevance Filter

Goal: reduce all signals to the 1–2 things {EXEC} most needs to know.

Apply the executive-lens rules from `REF-comp-intel-persona-prd-v1.0.md §2`. Map
each signal from Steps 1–4 to one of {EXEC}'s live concerns (maintain the concern
table in the persona file; example structure):

| {EXEC} concern | Signal types that map to it |
|---|---|
| Flagship product shipping timeline | Competitor ships an equivalent capability before you ship publicly |
| Emerging-category momentum | Competitor announces a feature in the category you are racing to own |
| Brand clarity | Competitor uses your naming/framing language — brand encroachment signals |
| Pricing defensibility | Competitor pricing signal undercutting your rate card |
| Frenemy posture | Any move by a partner-competitor that looks competitive rather than collaborative |
| Named watch competitors | Any product launch, funding, or pricing change from competitors {EXEC} tracks explicitly |

**Selection rule:** Pick the top 1–2 signals that:
(a) have the strongest source (BD-sourced > web-sourced; concrete > speculative)
(b) map to an {EXEC} concern most directly
(c) have the clearest recommended action

If no signal maps to any {EXEC} concern: write "No signals this cycle match {EXEC}'s
live concerns. No Slack draft warranted — registry updated."

---

## Step 6 — Output Assembly

### 6a — Write full report

**Default mode:** File: `outputs/competitive/YYYY-MM-DD-comp-intel-report.md`
**Other modes:** File: `outputs/competitive/YYYY-MM-DD-comp-intel-{mode}-report.md`

Use the report format defined in `REF-comp-intel-persona-prd-v1.0.md §4`.
Include:
- Top Signal This Cycle
- Competitor Activity section (active competitors only)
- Narrative Shifts (this cycle) — for each competitor where Step 2f detected a shift:
  `**[Competitor]:** [Old narrative] → [New narrative]`
  `Type: [change type]. Conflict with our positioning: [yes/no + which arc].`
  `Action: [what PMM should do]`
  (If no shifts detected: "No narrative shifts detected in [AFTER_DATE]–[today] window.")
- Battlecard Gaps Added This Cycle (include `[narrative-conflict]` tagged gaps from Step 4d)
- Executive Relevance (1–2 bullets — include narrative conflicts if they map to {EXEC}'s concerns)
- Executive Slack Draft (2–4 sentences, conclusion-first, per §2 of persona)
- Recommended PMM Actions (1–3)

### 6b — Update the registry and append win/loss log

**Registry update:**
Update the active mode's registry in-place following the
registry update rules in `REF-comp-intel-persona-prd-v1.0.md §6`:
- Update only changed fields.
- Bump `Last updated` for any row that changed.
- Update `Status` for all competitors based on this run's signal presence.
- Update `BD deal context` if a Slack signal provided new win/loss/objection context.
- Update `Current narrative` and `Narrative changed` if Step 2f detected a shift.

**Win/loss log append:**
For each staged win/loss entry from Step 1c:
- Append a new row to `outputs/competitive/win-loss-log.md`.
- Mode: the active `RUN_MODE`.
- Status: always `signal` (unconfirmed). Never write `confirmed-win` or
  `confirmed-loss` without explicit user instruction.
- Include note in the report: "X win/loss signal(s) appended to win-loss-log.md —
  review and update status to confirmed-win / confirmed-loss / rejected."

### 6c — Confirm outputs written

After writing all outputs, emit a brief confirmation listing:
- Report file path
- Snapshot files written (list)
- Gap rows appended (count — include `[narrative-conflict]` count separately)
- Registry fields updated (count)
- Narrative shifts detected (count)
- Narrative tracker rows appended (count)
- Win/loss log entries appended (count; all status=signal; requires review)
- Executive Slack Draft: present / not warranted

**Checkpoint (full run and resume):**

Write or update `state/runs/comp-intel-YYYY-MM-DD-{run_mode}.yaml`:
```yaml
product: {product_line_slug}
skill_slug: comp-intel
run_date: "YYYY-MM-DD"
run_mode: "default"   # or the active mode name
after_date: "YYYY-MM-DD"
mode: full-run        # or: resume
phase: 2-complete
artifacts:
  signals: "outputs/competitive/YYYY-MM-DD-comp-intel-signals.md"
  report: "outputs/competitive/YYYY-MM-DD-comp-intel-report.md"
```

**Wrapper note:** If you run this skill via a collection wrapper script, the checkpoint is
written automatically after a successful run. Only write it manually when running in a
direct chat session without the wrapper.

---

## Baseline Run (first run only — 120-day lookback)

Execute when triggered with "comp intel baseline run".

### Step A — Extended Slack scan (120-day)

Same as Step 1, but with `AFTER_DATE = today - 120 days`.
Goal: understand which competitors have been discussed most over the past 120
days, what contexts they came up in, and what the team has been concerned about.
Output: baseline internal-signal summary per competitor.

### Step B — Current-state web profiling

For each competitor in the registry:
- Fetch their current homepage, pricing page (if public), and most recent
  blog post or release note.
- Goal: establish where they stand today — not historical news.
- Write a positioning snapshot for all competitors in the registry.

### Step B2 — BD call signals extraction

Read the following files, if you maintain them. Scan each for competitor names,
comparisons, objections, and win/loss context. Extract any relevant BD deal
context per competitor and populate the `BD deal context` registry field.

1. `outputs/bd-call-signals/case-study-opportunities.md` — case study and win/loss signals
2. `outputs/bd-call-signals/competitor-landscape.md` — full competitor landscape from BD call transcripts; highest-fidelity source for deal-level competitive intelligence

Also read the most recent product-executive profile in `outputs/people/` (if
maintained). A product executive who synthesizes competitive intelligence into
product and positioning decisions unprompted will surface competitor comparisons
and gaps that are not in Slack keyword searches.

Then run extended keyword searches in BD channels over the 120-day window:

| Channel | Channel ID |
|---|---|
| `{BD_CHANNEL_1}` | {CHANNEL_ID} |
| `{BD_CHANNEL_2}` | {CHANNEL_ID} |
| `{BD_CHANNEL_3}` | (search by name) |
<!-- Fill in your BD-heavy channels -->

Use competitor keyword queries with `after:` set to the 120-day lookback date.

### Step C — Initial gap analysis

With Slack history (Step A) and web research (Step B), apply Step 4 logic.
Seed `outputs/competitive/battlecard-gaps.md` with known gaps from day one.

### Step D — Registry population

Fully populate all fields in the active registry from Steps A–C.
Replace `[PENDING baseline run]` entries with sourced content.

After baseline run completes, the skill shifts to 3-day cadence for all
subsequent runs.

---

## Error Handling

| Situation | Action |
|---|---|
| Slack search returns 0 results for a keyword | Note in output: "No Slack signals for [keyword] in window." Do not infer absence as confirmation of no activity. |
| Web fetch fails for a competitor URL | Note in output: "Could not fetch [URL] — manual check recommended." Continue with available data. |
| No web signals found for a competitor | Write "No new signals found this cycle" in that competitor's snapshot `Recent moves` section. Do not invent signals. |
| A claimed fact cannot be traced to a source | Omit it. Do not include unsourced claims. |
| No signals map to {EXEC}'s live concerns | Write "No signals this cycle match {EXEC}'s live concerns." Omit the Slack draft. |
| Registry file fails to load | Halt and report. Do not proceed with partial KB. |
