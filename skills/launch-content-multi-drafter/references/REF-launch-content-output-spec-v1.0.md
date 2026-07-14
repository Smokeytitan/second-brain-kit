# Launch Content Output Spec

**Version:** 1.0.0

Per-output structure, voice, and tier-of-launch sizing rules.

---

## Launch tiers

Tier classification determines depth, channel coverage, and approval chain rigor. Align with your company's product tiering framework if one exists.

### T1 — Critical
Full GTM. All 6 channels mandatory plus add-ons (employee amplification request, personal QT, press release coordination with {PR_LEAD}, possibly founder/exec posts).

**Examples:** flagship product launches, major brand launches, major platform upgrades, fundamental product pivots.

**Sizing:**
- Reddit: 250-300 words, multiple paragraphs, sourced metrics
- TG: 100-120 words
- Discord: 120-150 words with embed-ready formatting
- Secondary handle: full ≤280 chars
- Main handle: full ≤280 chars
- Marketing brief: 800-1200 words, full SCQR + 3 messaging pillars + content calendar + sign-off chain

**Approval chain:** {MARKETING_LEAD} (strategy) → {CONTENT_LEAD} (long-form direction) → {LEGAL_REVIEWER} (legal) → {CEO}/{FOUNDER} (if exec voice) → ship.

### T2 — High
All 6 channels, standard sizing. Most product features and partner co-launches.

**Examples:** pricing-change launches, mid-size partner integrations, meaningful feature launches.

**Sizing:**
- Reddit: 150-250 words
- TG: 80-120 words
- Discord: 80-150 words
- Secondary handle: ≤280 chars
- Main handle: ≤280 chars
- Marketing brief: 500-800 words

**Approval chain:** {MARKETING_LEAD} → {CONTENT_LEAD} → ship ({LEGAL_REVIEWER} only if legal flags fire).

### T3 — Standard
Smaller drafts. May skip the secondary handle. Often partner amplifications or minor updates.

**Examples:** Partner amplifications, minor platform updates, single-feature shipments.

**Sizing:**
- Reddit: 100-150 words (or skip if not sub-relevant)
- TG: 60-100 words
- Discord: 60-100 words
- Secondary handle: SKIP (default)
- Main handle: ≤280 chars
- Marketing brief: 300-500 words (or skip if no blog planned)

**Approval chain:** {MARKETING_LEAD} (lighter touch) → ship.

---

## Per-output structure

### 1. Reddit (company subreddit)

**Title:** Headline as a statement, no clickbait, no emoji.

**Body structure:**
1. 1-2 sentence setup (what changed)
2. Paragraph: what this looks like in practice (proof points, sourced)
3. Paragraph: why it matters / context
4. Paragraph (T1/T2): broader trajectory / ecosystem position
5. Source link

**Voice:** Authority, sourced, no marketing-speak. First-person tolerable for exec accounts.

### 2. Telegram (main community group)

**Structure:**
1. Emoji + headline as a sentence (1 line)
2. 1-2 line setup
3. Bulleted proof points (3-4 bullets, `•` character)
4. Optional 1-line closer
5. Link

**Voice:** Punchy, community-native, slightly informal. 1-2 emojis OK.

### 3. Discord (company server)

**Structure:**
1. `**emoji + headline**` (markdown bold)
2. 1-2 line setup
3. `**What this means:**` or `**What [partner] gets:**` header
4. Bulleted proof points (3-4 bullets, `•` character)
5. Optional 1-line closer
6. `Learn more → {link}`

**Voice:** Announcement-style, structured, warmer than Reddit. No `@here`/`@everyone` for routine launches.

### 4. Secondary brand X handle ({SECONDARY_HANDLE})

**Structure:**
- Single tweet, ≤280 chars.
- Lead with ecosystem-positioning frame (e.g., "The ecosystem benefits when...").
- Cite the partner/source if applicable.
- Link at end.

**Voice:** Measured. Less promotional than the main handle. Frames things in stewardship language.

### 5. Main brand X handle ({COMPANY_HANDLE})

**Structure:**
- Single tweet, ≤280 chars.
- T1: thread of 4-8 if there's enough to say.
- Active voice, short sentences.
- Lead with the strongest stat or partner name.

**Voice:** Punchy, on-brand. Company brand voice — proof-backed adjectives ("scalable", "efficient", "proven"), not "revolutionary" or "game-changing".

### 6. Marketing brief (for {CONTENT_LEAD})

**Structure (full SCQR):**

```markdown
# Marketing Brief — {Launch Name}

**Tier:** T1 / T2 / T3
**Date:** YYYY-MM-DD
**Owner:** {your name}
**Approval chain:** {MARKETING_LEAD} → {CONTENT_LEAD} → {LEGAL_REVIEWER if legal} → ship
**Source:** {PRD link / proposal forum link / paste reference}

---

## Situation
{1-2 paragraphs on the market context, what's true today, who's affected.}

## Complication
{What problem made this launch needed. The "why now."}

## Question
{The strategic question this launch answers, framed as the audience would ask it.}

## Resolution
{The launch itself. What it delivers. How it works at a high level.}

---

## Audience
- **Primary:** {who this is for}
- **Secondary:** {if applicable}

## 3 Messaging Pillars
1. **{Pillar 1}** — {1-line proof}
2. **{Pillar 2}** — {1-line proof}
3. **{Pillar 3}** — {1-line proof}

## Channels + Cadence
| Channel | Cadence | Notes |
|---------|---------|-------|
| Blog ({CONTENT_LEAD} writes from this brief) | Day 0 | SEO-optimized |
| {COMPANY_HANDLE} | Day 0 | Pinned for 24 hr |
| {SECONDARY_HANDLE} | Day 0 | Standalone |
| TG + Discord + Reddit | Day 0 + 30-60 min after main handle | |
| Creator program ({CREATOR_PROGRAM_LEAD}) | Day 0 + amplification | |
| Employee amplification | Day 0 + 30 min after main handle | |

## Open Questions for Stakeholder Review
- {Question 1 for {MARKETING_LEAD} / {CONTENT_LEAD} / {LEGAL_REVIEWER}}
- {Question 2}

## Legal Flags Cleared
- Regulated performance claims: {status}
- Headline business metrics: {status}
- Sensitive phrasing: {status}
- Confidentiality: {status}
- Competitor naming: {status}

## Distribution-Ready Drafts
See sibling files in this folder:
- {launch-slug}-reddit.md
- {launch-slug}-telegram.md
- {launch-slug}-discord.md
- {launch-slug}-secondary-x.md
- {launch-slug}-main-x.md
```

**Voice:** Internal, structured, decision-ready. This is the doc {MARKETING_LEAD} and {CONTENT_LEAD} read. No marketing-speak. Sourced.

---

## Optional add-ons

### A. Employee amplification request
If the user says "and an amplification request", produce per the `community-channel-post-drafter` amplification template.

### B. Personal-account QT
If the user says "and a QT" or "and a personal post", produce 2-3 options per the `community-channel-post-drafter` QT template.

### C. Press release angle for {PR_LEAD}
If the user says "and a press release angle", produce a 100-150 word PR brief framed for the trade/enterprise media your PR lead targets.

### D. Creator program brief for {CREATOR_PROGRAM_LEAD}
If the user says "and a creator brief", produce an 80-120 word amplification brief sized for external creators (different from the internal-team amplification request).

---

## Cross-References
- **Skill:** [../SKILL.md](../SKILL.md)
- **Workflow:** [RUN-launch-content-multi-drafter-workflow-prd-v1.0.md](RUN-launch-content-multi-drafter-workflow-prd-v1.0.md)
- **Channel voices (reused from sister skill):** [../../community-channel-post-drafter/references/REF-channel-voices-v1.0.md](../../community-channel-post-drafter/references/REF-channel-voices-v1.0.md)
- **Marketing brief template:** [../../marketing-brief/SKILL.md](../../marketing-brief/SKILL.md)
- **Legal flags:** [../../../memory/context/legal-flags.md](../../../memory/context/legal-flags.md)
- **Product tiering:** your company's product tiering reference, if one exists
