# Partner Roundup — Format Reference

**Version:** 1.0.0

Output format spec for the partner roundup. Modeled on your team's established
published post style + the established file structure. Update when the format
evolves.

---

## File structure (output to `Resources/Partner-Roundups/YYYY-MM-DD-partner-roundup.md`)

```markdown
# {COMPANY_HANDLE} Weekly Ecosystem Roundup — {Month D – Month D, Year}

**Sources:** {list cited sources with snapshot dates}
**Destination:** {COMPANY_HANDLE} on X
**Angle this week:** {1-line angle, see workflow §5a}

---

## Flagship product snapshot (from {PRODUCT_DASHBOARD_URL}, {snapshot date})

**⚠ Legal flag per legal-flags.md:** TVL and AUM figures need {METRICS_VERIFIER} verification before publishing. Yield/APY stays out entirely until {LEGAL_REVIEWER} signs off.

| Metric | Value | Dashboard freshness |
|--------|-------|---------------------|
| Product supply | {N} | {N}h |
| TVL | ${N}M | {N}h |
| Holders | {N} | {N}h |
| Market share of category | {N}% | {N}h |
| Exchange/rate metric | {N.NNNN} | {N}h |
| Category total (network-wide) | {N}B | {N}h |
| Days since launch | {N} | — |

**Narrative:** {1-2 sentences synthesizing what these numbers say.}

---

## Network snapshot (from {NORTHSTAR_DASHBOARD_URL}, {snapshot date})

### Use these (positive direction)

| Metric | Value | Freshness | Note |
|--------|-------|-----------|------|
| Headline supply metric | **${N}B** | {age} | {prior + delta} |
| Supply MoM change | **+{N}%** | {age} | |
| Transfer volume (all-time) | **${N}T** | {age} | |
| Transfer volume (30d) | ${N}B | {age} | |
| Trading volume (30d) | **${N}B** | {age} | |
| Transactions (all-time) | {N}B | {age} | |

### Handle with care (negative direction)

| Metric | Value | Freshness | Why careful |
|--------|-------|-----------|-------------|
| Transactions (30d) | {N}M | {age} | {reason} |
| % txn growth (30d) | **−{N}%** | {age} | Don't cite growth |
| Unique users (30d) | {N}M | {age} | |
| % user growth (30d) | **−{N}%** | {age} | Don't cite growth |
| TVL | ${N}B | {age} | Per legal-flags.md still needs verification |
| TVL % change (30d) | **−{N}%** | {age} | Skip |

### Strategic takeaway

{2-3 sentences on what to lead with this week. Be concrete: "Lead with the supply metric. Skip raw network activity."}

**Payload line for the post:** "{Punchy 1-liner that could become the closing.}"

---

## Deduped partner items ({N} stories, from {raw source count})

{If dedup occurred, briefly explain the collapse logic.}

1. **@PartnerHandle** — {beat in 1 sentence}. {source link}
2. **@PartnerHandle** — {beat in 1 sentence}. {source link}
{...}

### Cross-check from {SOCIAL_FEEDS_CHANNEL}

{Additional flagged items not in the partner scanner. If empty, write "None this week — the scanner caught everything."}

- **@SourceHandle ({date}, {flagger})** — {beat}. {standalone or RT-only}.

**Recommendation:** {Whether to include in main roundup or reply-quote separately.}

---

## Version A (FINAL) — Long-form single post

Modeled on the established published format: `■` bullets, short `@handle [verb] on/for {COMPANY}` lines, company/product milestones at the end, closing line.

```
■ @PartnerA {verb} {what} on {COMPANY}
■ @PartnerB {verb} {what} on {COMPANY}{optional with: detail}
■ @PartnerC {verb} {what} on {COMPANY}
■ @PartnerD {verb} {what} on {COMPANY}
■ @PartnerE {verb} {what} on {COMPANY}
■ @PartnerF reports {metric} on {COMPANY}
■ {FLAGSHIP_PRODUCT} {milestone}, {N} days post-launch
■ {COMPANY} {headline metric} tops ${N}, up {N}% this month

{Closing line — punchy, in your voice. Examples below.}
```

---

## Optional Version B — Thread (only if 8+ items justify)

Groups items into 3 thematic clusters. Hook tweet must earn distribution on its own.

```
1/ {N} partner moves on {COMPANY} this week. {Hook — 1 sentence.} ↓

2/ {Theme cluster 1}:

■ ...
■ ...

3/ {Theme cluster 2}:

■ ...
■ ...

4/ By the numbers:

■ ...
■ ...

5/ Institutional & coverage:

■ ...
■ ...

6/ {Closing line.}
```

---

## Alternate opening hooks (swap-in for Version A line 1, if needed)

- "Seven days. {N} partner moves. One stack."
- "Another week of builders shipping on {COMPANY}:"
- "What the {COMPANY} stack did this week:"
- "{COMPANY}'s ecosystem, week of {Month D}:"
- "{N} partner moves on {COMPANY} this week."
- "This week in the {COMPANY} ecosystem:"

---

## Algorithm notes for whoever posts

- **[Algo] No links in main tweet.** If you want to link the original source or a recap blog, drop it as a reply — 10–17x more reach on the primary tweet.
- **[Algo] Reply engagement matters most (+75 signal weight).** Whoever runs the {COMPANY_HANDLE} reply queue should be online in the first 60 min after this goes up, especially to reply to partner accounts when they engage.
- **[Algo] Text-only outperforms video on X.** Don't force an image on this one unless it's a strong infographic.
- **[Algo] Best posting windows:** Tue/Wed/Thu, 9 AM–3 PM in your audience's core timezone. If it's already Friday when posting, aim for Monday 8 AM instead.
- **[Algo] Cap at ~280 chars in the main post line.** A premium account has no hard cap, but readability still wins. Long-form bullets work because each line is short.

---

## Voice rules

- **No em dashes.** Use colons, commas, semicolons, or separate sentences.
- **Active voice.** Subject does verb to object.
- **Concrete detail per bullet.** Generic "ships on {COMPANY}" is weak. "Distributes $470K+ in zero-fee subsidies" is strong.
- **Partner handles always with @.** Verify handle exists before final.
- **No marketing-speak.** Avoid "revolutionizing," "game-changing," "next-generation," "unlocking," etc.
- **Closing line in your voice:** punchy, declarative, ecosystem-confident. Examples:
  - "The rails are live. Every week, more ships."
  - "The rails compound."
  - "More builders. More volume. Same {COMPANY}."
  - "Every week, the stack gets more useful."

---

## Changelog
See [REF-partner-roundup-changelog.md](REF-partner-roundup-changelog.md).
