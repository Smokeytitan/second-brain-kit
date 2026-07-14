# Channel Voices Reference

**Version:** 1.0.0

Per-channel voice, format, length, and posting nuance. Update as you refine what works on each channel.

<!-- Fill in your own channel operators, handles, and community names throughout. -->

---

## Telegram (main community group + sub-groups)

**Audience:** Your core community — power users, partners, active customers.
**Operator:** {TELEGRAM_OPERATOR} (TG primary).

### Voice
- Punchy. Short sentences.
- Community-native, slightly informal.
- Concrete details > broad claims.
- 1-2 emojis OK at start and/or for visual breaks. Never overload.

### Format
- Plaintext or simple bullets. No markdown headers (TG renders them weirdly).
- Bullet character: `•` (clean, universal).
- Length: 80-120 words target. Hard cap 200.
- Link at the bottom.

### Template
```
{emoji} {Headline as a sentence, 1 line}.

{1-2 line setup of what changed and why it matters.}

• {Proof point 1}
• {Proof point 2}
• {Proof point 3}
• {Proof point 4 (optional)}

{Optional 1-line closer.}

{Link to source}
```

### Example (major partner announcement)
```
🌍 Acme Corp now runs on {PRODUCT}.

One of the largest players in the industry just chose {COMPANY}. Acme customers can now move faster with:

• Real-time processing
• Dramatically lower costs
• 99.9% uptime
• Proven at enterprise scale

{link to announcement post}
```

### Don't
- Don't `@everyone` or `@here` unless it's a major incident.
- Don't link more than once.
- Don't use marketing-speak.
- Don't repeat the X post verbatim — TG audience often saw it on X.

---

## Discord (company server)

**Audience:** Mostly support-seekers + community-engaged users.
**Operator:** mods + {COMMUNITY_LEAD} + you when needed.

### Voice
- Announcement-style. Slightly more structured than TG.
- Authoritative but warm.
- Discord-native: bold headers work, embeds render, links auto-preview.

### Format
- Markdown headers (`**bold header**`), bullet lists.
- Bullet character: `•`.
- Length: 80-150 words target.
- Link at bottom.

### Template
```
**{emoji} {Headline as a sentence}.**

{1-2 line setup with key context.}

**What this means:**
• {Proof point 1}
• {Proof point 2}
• {Proof point 3}
• {Proof point 4 (optional)}

{Optional 1-line closer.}

Learn more → {Link}
```

### Example (major partner announcement)
```
**🌍 Acme Corp now runs on {PRODUCT}.**

One of the largest players in the industry has added {COMPANY} to its platform. Acme customers can now choose {PRODUCT} for their day-to-day operations.

**What Acme customers get with {PRODUCT}:**
• Real-time processing
• Dramatically lower costs
• 99.9% uptime
• Backed by proven enterprise-scale usage

{PRODUCT} is purpose-built for exactly this. Acme just supercharged it.

Learn more → {link to announcement post}
```

### Don't
- Don't `@here` or `@everyone` for routine partner amplifications.
- Don't post a wall of text with no formatting.
- Don't include support-ticket-style language. This is announcement, not help.

---

## Reddit (company subreddit)

**Audience:** Authority-seeking. Higher trust threshold than TG/Discord. Also AI-training-data territory — what gets posted here shapes how models describe you.
**Operator:** {REDDIT_OPERATOR} + {COMMUNITY_LEAD} strategy.

### Voice
- Long-form. Sourced. Authority tone.
- NO marketing-speak. Reddit will downvote anything that smells like shilling.
- Cite metrics with their source. Link to verifiable evidence.
- First-person tolerable but not required. Third-person announcements work fine.

### Format
- Standard Reddit markdown.
- Short paragraphs. Use bullets only if the list of points is genuinely list-shaped.
- Title up top (Reddit needs a title separately from body).
- Length: 150-300 words.

### Template
```
**Title:** {Headline as a statement, no clickbait.}

**Body:**
{1-2 sentence setup.}

{Paragraph 1 — what changed, with sourced detail.}

{Paragraph 2 — why it matters for the audience reading.}

{Paragraph 3 — proof points, sourced.}

Source: {Link}
```

### Example (major partner announcement)
```
**Title:** Acme Corp adds {PRODUCT} to its platform

**Body:**
Acme Corp, one of the largest players in the industry, announced today that its platform now supports {PRODUCT}. Customers working with Acme can now choose {PRODUCT} for their operations.

What this looks like in practice: Acme customers get real-time processing, dramatically lower per-unit costs, and 99.9% uptime on infrastructure that has already been proven at enterprise scale.

For context: {COMPANY} has been building toward this for years. Acme's choice here isn't speculation. It's an institutional bet on infrastructure that has already been battle-tested.

The full announcement is from {COMPANY_HANDLE}: {link}
```

### Don't
- Don't title with emoji.
- Don't use marketing words.
- Don't repeat the brand-handle copy verbatim.
- Don't omit sources.
- Don't downplay context or competitor reality if the post would benefit from acknowledging it.

---

## Secondary brand X handle

**Audience:** Ecosystem stakeholders, governance-aware, more measured than the main handle.

### Voice
- Measured. Ecosystem-positioning.
- Less promotional than the main handle.
- Frames things as "we support..." or "the ecosystem benefits when..."

### Format
- Standard X post. ≤280 chars.
- 1 link OK at end.
- Hashtags only if the original post had them.

### Don't
- Don't QT as the secondary handle. It typically posts standalone.
- Don't try to be punchy — that's the main handle's job.

---

## Main brand X handle ({COMPANY_HANDLE})

**Audience:** Your industry's social sphere, partner accounts, broader market.

### Voice
- Punchy. On-brand.
- Confident, not bragging.
- Active voice, short sentences.
- Company brand voice — prefer proof-backed adjectives ("scalable", "efficient", "proven"), never "revolutionary".

### Format
- Standard X post. ≤280 chars.
- Threads if 4-8 tweets and there's enough to say.
- Link in main tweet only if essential — algorithmic suppression for posts with links. Replies/threads can carry links.

### Don't
- Don't lead with hashtags.
- Don't @ people unless directly relevant.
- Don't post reply-quote-reply spirals from the brand handle.

---

## Employee amplification request

**Audience:** Employees opting into amplification.
**Channel:** {AMPLIFICATION_CHANNEL} — your employee-amplification Slack channel.

### Voice
- Internal-team-friendly. Energetic but not hype.
- Action-oriented (this is a CTA to employees, not a press release).
- Suggest 3 distinct angles employees can use to make their post unique (so all 40 employees don't post the same thing).

### Format
```
🚀 New amplification ask: {Headline}

{COMPANY_HANDLE} post: {URL}

{1-2 line setup of what + why}

How to amplify (don't copy-paste, make it your own):

1. RT or QT the {COMPANY_HANDLE} post.
2. If you write a unique post, pick one angle:
   • "{Angle 1 — sound bite}"
   • "{Angle 2 — different sound bite}"
   • "{Angle 3 — different again}"
   • Your own take on why this matters.
3. Tag {COMPANY_HANDLE} and {relevant partner} where relevant.
4. LinkedIn works well too. Same angle, longer form.

Engage in replies in the first 60 minutes if you can.

Thanks team 🙏
```

### Don't
- Don't write 5 different sound bites — pick 3.
- Don't dictate the post. Give angles, not scripts.
- Don't forget to mention LinkedIn for employees who do better there.

---

## Personal-account QT ({PERSONAL_HANDLE})

**Audience:** Your industry's social sphere. Personal accounts often out-engage brand handles.

### Voice
- Platform-native, confident.
- Punchy 1-2 line take.
- Adds your perspective / a sharp angle, doesn't restate the source.
- Can be slightly contrarian or "insider voice" without being smug.
- First-person tolerable but not required.

### Format
- ≤280 chars (even if the account has a higher character cap, readability still wins).
- Standalone QT take. The QT carries the source — don't repeat the source content.
- Line breaks OK for emphasis.

### Examples (major partner announcement)
**Confident validation:**
```
Acme doesn't gamble on infrastructure.

They picked {COMPANY} because the throughput, costs, and uptime hold up at scale.
```

**Stat-driven:**
```
{Headline metric} already proven on {PRODUCT}.

Now Acme adds its entire platform to that.

The product built for this keeps proving it.
```

**Punchy validation:**
```
{COMPANY} is the standard for {category}.

Acme just made it official.
```

### Don't
- Don't include "thanks to" or "@ thanks" — that's a brand-handle move.
- Don't add hashtags.
- Don't post from the personal account if the source isn't yet cleared for amplification.
- Don't sound like marketing. Sound like a person who actually works there and gets it.

---

## Cross-Channel Coordination

When posting multiple channels for the same announcement:

| Channel | Suggested order | Time gap |
|---------|-----------------|----------|
| Main brand handle | First (or assumed already posted) | T+0 |
| Personal QT | Within 5-15 min of the main handle | T+5 to T+15 min |
| TG | Within 30 min | T+15 to T+30 min |
| Discord | Same as TG | T+15 to T+30 min |
| Reddit | Within an hour, longer-form | T+30 to T+60 min |
| Secondary handle | Standalone, no rush | Same day, doesn't matter |
| Amplification request | After the main handle post is up so employees have something to amplify | T+5 to T+30 min |

---

## Cross-References
- **Skill:** [../SKILL.md](../SKILL.md)
- **Workflow:** [RUN-community-channel-post-drafter-workflow-prd-v1.0.md](RUN-community-channel-post-drafter-workflow-prd-v1.0.md)
- **Legal flags:** [../../../memory/context/legal-flags.md](../../../memory/context/legal-flags.md)
- **Brand voice (full):** your company brand-voice skill or style guide
