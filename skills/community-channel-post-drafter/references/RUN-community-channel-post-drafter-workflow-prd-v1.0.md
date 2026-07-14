# Community Channel Post Drafter — Workflow

**Version:** 1.0.0
**Skill:** [community-channel-post-drafter](../SKILL.md)

---

## Step 0 — Resolve the source

Source can arrive as:
1. A URL (X post, blog, forum post). Fetch via web tools when possible.
2. Pasted text (most common — X/Twitter doesn't fetch well, the user pastes).
3. A reference to a Slack post or internal doc.
4. A description of an event/announcement (the user wrote a sentence describing it).

If the URL is X/Twitter and fetch returns empty, ask the user to paste the post text. Don't proceed without source content.

---

## Step 1 — Resolve the channel list

Parse the trigger for channels mentioned. Match against supported set:
- TG / Telegram / community TG → Telegram
- Discord → Discord
- Reddit / subreddit → Reddit
- Secondary handle / foundation handle → Secondary brand X handle
- Main handle / {COMPANY_HANDLE} → Main brand X handle
- Amplification request / employee amplification / social growth request → Employee amplification request
- QT / quote tweet / personal QT → Personal-account QT

If the trigger is ambiguous ("all channels", "everywhere"), default to: **TG + Discord + employee amplification request**. Note the choice and offer to add more if the user wants.

---

## Step 2 — Apply legal flags

Load `../../memory/context/legal-flags.md`. Scan the source content for your company's compliance triggers. Typical rules (fill in your own):

| Trigger | Action |
|---------|--------|
| Regulated performance claims (e.g., yield/return figures) | Drop or flag `⚠ blocked — {LEGAL_REVIEWER} signoff pending`. Don't include in draft. |
| Headline business metrics (volume / usage / AUM-style numbers) | If from an already-public source (e.g., a {COMPANY_HANDLE} post), repeat is safe. If new, flag `⚠ verify with {METRICS_OWNER} before publishing`. |
| Legally sensitive phrasing (e.g., promises vs. commitments) | Use the approved softer phrasing from `legal-flags.md` |
| Competitor names | Omit unless announcement explicitly references them |
| Confidential figures listed in legal-flags.md | DROP entirely — confidentiality |
| Deprecated / renamed products | Use the approved current naming from `legal-flags.md` |

Track flags raised. Surface in the "Legal/brand flags" section of the output.

---

## Step 3 — Apply company voice + channel adaptations

Load `references/REF-channel-voices-v1.0.md`. For each channel in the list:

1. **Pull the headline / hook** from the source. Adapt for channel length + tone.
2. **Pull the key proof points** (metrics, partner names, technical claims). Verify each against legal flags.
3. **Apply the channel's format spec** (bullets vs prose, emoji policy, length cap).
4. **Apply the channel's tone spec** (community-native vs authority vs measured).
5. **Add CTA / link** appropriate for the channel:
   - X posts → link to source post or longer-form blog
   - TG/Discord/Reddit → link to source post or relevant company page
   - Amplification request → no CTA, but suggest 3 angles for employees
   - QT → no link (QT carries the source)

**Voice rules (apply to every channel):**
- No em dashes. Use colons, commas, semicolons, or separate sentences.
- Active voice. Subject does verb to object.
- Short sentences. Under 25 words preferred.
- No marketing-speak: avoid "revolutionary", "game-changing", "next-generation", "unlocking", "supercharging" (unless quoting a source that says it).
- Brand capitalization: follow your company's naming/capitalization rules (company name, product names, token/ticker style) from `legal-flags.md` or your style guide.
- Concrete > generic: a metric beats an adjective every time.

---

## Step 4 — Compose drafts

Compose each channel's draft using the format spec. Length budgets:

| Channel | Target length |
|---------|---------------|
| Telegram | 80-120 words |
| Discord | 80-150 words |
| Reddit | 150-300 words |
| Secondary handle X | ≤280 chars |
| Main handle X | ≤280 chars |
| Amplification request | 120-180 words |
| Personal QT | ≤280 chars |

Check each draft against:
- Source-fact alignment (every claim traces to source).
- Legal flags applied.
- Voice rules followed.
- Channel format applied.
- Length within budget.

---

## Step 5 — Output

Print to chat in this structure:

```markdown
## 📱 Telegram

```
{draft text}
```

## 💬 Discord

```
{draft text}
```

[... other channels in order ...]

---

**Legal/brand flags:** {list any items raised; if none, write "None for this content."}

**Posting notes:**
- {time-window suggestions per channel}
- {link CTAs to verify before posting}
- {cross-channel coordination notes — e.g., "post TG + Discord first, then amplification request 30 min later"}
```

Use code blocks (`` ``` ``) around each draft so the user can clean-copy with a single tap.

If the user asked to save it, write to `../../Resources/Channel-Drafts/YYYY-MM-DD-{slug}.md` and notify with the path.

---

## Step 6 — Optional: emit telemetry

If a telemetry emitter is wired:

```yaml
skill: community-channel-post-drafter
version: 1.0.0
source_type: url | paste | description
channels_drafted: [list]
legal_flags_raised: {count}
output_destination: chat | file
status: success | partial | failed
```

---

## Halt conditions

Halt and ask the user if:
- Source is empty or unfetchable AND no paste provided.
- Channel list is empty or unrecognized.
- A confidentiality flag fires that can't be auto-resolved (a restricted figure leaked into the source).
- Legal-flags.md fails to load.

---

## Anti-patterns to avoid

- **Don't restate the source word-for-word.** Adapt for the channel.
- **Don't use the same draft across channels.** Each channel has different voice/format.
- **Don't add hype words.** No "revolutionary", "game-changing", etc.
- **Don't add CTAs that aren't in the source.** Stay grounded.
- **Don't include hashtags unless the source uses them.** Brand X accounts don't lead with hashtags.
- **Don't tag people in TG/Discord** unless the user asks. Channel-wide announcements are usually @here at most, often nothing.

---

## Notes

- **Future enhancement:** auto-pull the source from your partner-updates alert channel when the trigger is "amplify the latest partner post".
- **Future enhancement:** add LinkedIn channel support (currently delegated to a dedicated LinkedIn ghostwriting skill).
