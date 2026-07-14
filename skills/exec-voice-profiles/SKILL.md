---
name: exec-voice-profiles
description: "Codify and write in the voice of your company's executives. Use this skill whenever the user asks to write a post, tweet, LinkedIn post, thread, quote tweet, or any content that a named executive will publish. Also trigger on 'write this as our CEO', 'draft the CEO post', 'founder post', 'in [executive]'s voice', or any request for executive social content. The skill has two jobs: (1) a methodology for building a voice profile for any executive from their real posts, and (2) fillable profile templates that capture the voice-pattern categories needed to write convincingly as that person. Use it alongside your social media skill when the output is a social post for an executive account."
---

# Executive Voice Profiles

Executive content only works when it sounds like the specific human, not the brand account with a headshot. This skill gives you a repeatable method for codifying any executive's voice, plus a fillable profile template. Build one profile per executive, keep each in its own file, and load the relevant profile before drafting anything attributed to that person.

## Part 1: How to Build a Voice Profile

1. **Collect the corpus.** Gather 20-50 real writing samples from the executive: X posts, LinkedIn posts, long-form articles, internal broadcast messages, launch communications. More recent samples weigh more. Note which platform each came from.
2. **Categorize by register.** Separate samples by platform and by type (announcement, reaction, thought leadership, community post). Most executives have distinct registers per platform.
3. **Extract patterns, not vibes.** For each category in the template below, find at least 2-3 concrete examples in the corpus before writing the pattern down. If you can't find evidence, leave the section blank rather than guessing.
4. **Codify the negatives.** The "What they do NOT do" section is the most load-bearing part of the profile. It's easier to sound like someone by avoiding their anti-patterns than by imitating their tics.
5. **Fill in the template.** One file per executive: `references/{exec-slug}-voice-profile.md`.
6. **Validate against held-out samples.** Draft a test post, place it next to 3 real posts, and check whether it stands out. Revise the profile until it doesn't.
7. **Refresh quarterly.** Voices drift with company strategy. Re-run the analysis when the executive's focus or the company narrative shifts.

## Part 2: The Voice Profile Template

Copy this structure for each executive. Every section header is a required analysis category; the bracketed guidance describes what to capture.

```markdown
# {EXECUTIVE_NAME} Voice - {TITLE}, {COMPANY}

## Who {EXECUTIVE_NAME} Is

[2-3 paragraphs: role, background, and how that background shapes their
communication. What is their core thesis or mission language? What kind of
authority do they project - data-driven operator, visionary founder,
technical builder? How emotionally expressive are they relative to a
typical executive?]

## Voice Characteristics

### Tone
[One paragraph. E.g., "authoritative but measured" vs. "visionary and warm."
Include a comparison that makes it concrete: "senior partner presenting
findings," "founder writing to their community."]

### Sentence Structure
- [Typical sentence length and rhythm - e.g., medium declaratives with short
  punchy follow-ups, or a mix of one-liners and longer explanatory passages]
- [First-person usage: "I" vs. "we" vs. letting the product/data be the subject]
- [How arguments build: thesis -> evidence -> conclusion? data -> emotion?]
- [Any letter-like or signed conventions in long-form]

### Signature Patterns
- [How they open: bold thesis? striking data point? personal observation?]
- [Recurring declarative closers or catchphrases they actually use]
- [How they handle numbers: proof points vs. emotional hooks]
- [How they reference values, philosophy, or personal conviction]
- [How they do competitive positioning: implicit data comparisons? never
  naming competitors? observational, not attacking?]
- [Quote-tweet behavior: one sharp observation vs. added context]
- [Emoji and formatting habits, precisely - "zero emoji" or "chart emoji and
  checkmarks, occasionally celebratory"]

### What {EXECUTIVE_NAME} Does
- [How they frame products - structural gaps vs. features? mission-tied?]
- [Who they center: the company, the community, holders/customers, builders]
- [Thread length preferences, formatting habits]
- [How they connect individual announcements to the macro vision]
- [How they celebrate wins - detached vs. genuinely enthusiastic]

### What {EXECUTIVE_NAME} Does NOT Do
- [Banned openers - e.g., "excited to announce," "thrilled to share"]
- [Emoji/hashtag/slang limits]
- [Superlatives and hype language they never use]
- [Hedging patterns they avoid, or bravado they avoid]
- [Legal/compliance lines they never cross - e.g., no financial advice, no
  guaranteed outcomes, no forward-looking promises]
- [Anything else that would instantly break the illusion]

## Platform-Specific Patterns

### X (Twitter)
- [Single tweets vs. threads; typical thread length]
- [Text-first vs. image use]
- [Link placement strategy - e.g., link in reply, not main tweet]
- [Register shift vs. LinkedIn - punchier? more casual? more playful?]

### LinkedIn
- [Structure: thesis -> argument -> conclusion? personal narrative?]
- [Narrator: "I" vs. "we"]
- [Closing conventions: link, CTA style, warmth level]
- [Hashtag/emoji rules; line-break habits]
- [Company-mention cap - e.g., max 2 mentions so it reads as insight,
  not marketing]

## Product Launch Voice

When writing about a new product or feature, {EXECUTIVE_NAME}'s pattern is:

1. **[Opening move]** - [e.g., open with the gap/problem backed by data, or
   open with a compelling number that creates immediate impact]
2. **[Framing move]** - [e.g., name the structural cause, or add personal
   conviction: "that gap has been bothering me"]
3. **[Product introduction]** - [factual and specific vs. willing to get
   technical]
4. **[Proof/stakes move]** - [e.g., anchor with live proof points, or connect
   to what it means for customers/holders/the ecosystem]
5. **[Close]** - [e.g., forward-looking ecosystem impact, or warm principled
   vision]

Example pattern: "[Opening]. [Framing]. [Product specifics]. [Proof or
stakes]. [Close]."

## Example Outputs

[Paste 3-5 REAL posts from the executive here, one per format you'll need:
single tweet, thread, quote tweet, LinkedIn post, long-form closer. These are
the ground truth for voice matching. Label each with format and what makes it
representative. Never invent samples - if you don't have a real one for a
format, note the gap.]

## Checklist Before Finalizing

Before submitting content written in {EXECUTIVE_NAME}'s voice, verify:
- [ ] No banned openers or corporate filler
- [ ] Opens the way they actually open (data, thesis, or personal claim)
- [ ] Narrator pronoun matches their habit ("I" vs. "we")
- [ ] Emoji, hashtag, and formatting rules respected
- [ ] Closes the way they actually close
- [ ] Any claims are backed by specific, sourced data points
- [ ] No compliance-risk language (no financial advice, no guaranteed outcomes)
- [ ] Tone matches the platform register from their profile
- [ ] The post could NOT have come from the brand account - it has a
      perspective only this person would voice
```

## Part 3: Archetype Calibration Notes

Most executive voices cluster into recognizable archetypes. Knowing which one you're profiling helps you look for the right patterns. Two common poles:

**The measured operator (often a CEO with a legal, finance, or operator background):**
- Authority comes from clarity, not volume. States what is true and why it matters.
- Prefers "we" or lets the data be the subject; rarely "I."
- Zero emoji, zero exclamation marks, zero superlatives, no hashtags.
- Competitive positioning is implicit and data-driven; never names competitors to dunk.
- Short declaratives; closes with a landing statement, not a CTA.
- Launch pattern: problem with data -> structural cause -> product as the answer -> live proof points -> ecosystem implication.

**The expressive visionary (often a founder):**
- Personally invested and shows it. Comfortable being directly bullish, backed by specifics.
- Comfortable with "I" and direct address to the community ("Dear ecosystem...").
- Leads with data but lands on emotion or vision; uses numbers as emotional hooks.
- References personal philosophy and values naturally; may sign long-form personally.
- More liberal with emoji and playful framing than the operator, but still no hedging and no negativity toward competitors.
- Launch pattern: compelling data -> personal conviction -> product specifics -> what it means for customers/community -> warm principled close.

Real executives sit somewhere between these poles. The profile's job is to locate them precisely, with evidence.

## Universal Guardrails (apply to every profile)

- Never fabricate quotes, metrics, or positions. Every factual claim traces to source material.
- No "excited to announce" unless the corpus proves the executive genuinely uses it.
- No financial advice, yield promises, or guaranteed-outcome language, regardless of voice.
- If the profile lacks samples for the register a draft requires, flag it rather than improvising.
- Executive voice must differ from brand voice. Run the test: remove the name and photo - if the post could have come from the brand account, it's not personal enough.
