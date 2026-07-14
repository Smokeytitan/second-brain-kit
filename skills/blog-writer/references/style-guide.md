# Company Style Guide

Your company's writing voice, distilled for blog work. Read alongside the corpus-calibrated rules in `SKILL.md`. The eight principles below apply to everything the company writes; the blog work adds quantitative targets on top. Examples use a fictional company ("Acme") — substitute your own products and metrics.

---

## Audience Hierarchy

<!-- Fill in your own audiences, ranked by weight. The example below is for a B2B
infrastructure company. -->

Your blog speaks to multiple audiences in roughly this order of weight:

1. **Executive and institutional buyers** — care about risk, predictability, regulatory posture, reliability. Read for proof, not vibe.
2. **Product managers at customer companies** — want single integrations, clear pricing, faster outcomes. Read for capability.
3. **Platform operators** — care about margins, scale, reliability, coverage. Read for unit economics.
4. **End businesses / merchants** — want to avoid surprises, reduce failures, know when outcomes land. Read for outcomes.
5. **Developers building on your platform** — want clear technical docs, practical integration paths, performance benchmarks. Read for substance.

The buyer personas buy from teams. The developer persona evaluates infrastructure. Adjust register accordingly.

---

## The Core Voice

The company sounds like **a brilliant friend who knows the subject cold** — not a press release, not a whitepaper, not a hype account. The voice is alive, active, direct, and confident without being smug. It treats readers as intelligent adults who have better things to do than wade through jargon.

The single most important question to ask before writing any line of copy: **"Why should the reader care?"** Answer that first. Structure follows.

---

## The Eight Principles

### 1. Give readers a reason to be here

Respect people's time. Every piece of content — a tweet, a blog intro, a product page header — needs to earn its keep. Be compelling, insightful, and honest. If you can't quickly articulate why the reader should care, rewrite.

**Bad:** "Acme is pleased to share an update on recent developments in our platform."
**Good:** "Settlement still takes three days at most banks. On Acme, it takes five seconds."

### 2. Conversational but knowledgeable

The voice is warm, not corporate. It knows what it's talking about but doesn't perform expertise. Write like you're explaining something to a smart person at dinner, not presenting at a conference. Short sentences. Active verbs. A little personality.

**Bad:** "The implementation of adversarial validation constitutes a fundamental advancement in cross-system security architecture."
**Good:** "Adversarial validation assumes every connected system could be malicious. We built it that way on purpose: it's easier to add trust later than to add safety."

### 3. Lead with WHY, not WHAT

Don't describe what something *is* — tell readers what it *does for them*. "Fast confirmation helps users onboard without waiting" beats "The platform achieves 5-second confirmation." Lead with the value, back it up with the fact.

**Bad:** "The platform achieves 2,600 TPS following the latest upgrade."
**Good:** "Merchants don't wait anymore. The upgrade brought the platform to 2,600 TPS, which means payouts clear during the transaction, not after it."

### 4. Tell a story

Hook → vibrancy → evidence → why the reader should care. The best content reads like journalism, not marketing. Give it a frame, a tension, a reason to keep reading.

A good blog opening sets up a real tension: a problem that costs money, a regulatory shift that just landed, a number that doesn't add up. The reader should know within two paragraphs why this post exists and what it's going to tell them.

### 5. No jargon

Say what you mean. If a term needs explaining, either explain it cleanly or replace it. "Adversarial validation — the mechanism that keeps cross-system transactions safe without requiring trust" is fine. "Atomic synchronous composability" on its own is not.

Apply the **CFO test**: would a sharp executive at a traditional firm in your buyers' industry understand this without a glossary? If the answer is no, either explain the term inline or pick a different one.

### 6. Confident, not cocky

Project authority. We know what we're doing and we're going to execute. Confidence comes from specificity and evidence — not superlatives.

**Banned-word list (hard ban):**
revolutionary, game-changing, game-changer, paradigm shift, unprecedented, groundbreaking, transformative (as a standalone claim), "the future of X," unleashing, redefining, next-generation (without explaining what that means).

**Prefer instead:** scalable, efficient, proven, fast, reliable, practical. Name the specific improvement. "12x faster confirmation" beats "game-changing performance" every time.

**Bad:** "Acme's groundbreaking technology is revolutionizing global payments."
**Good:** "$2.5T in volume has moved across the platform. The infrastructure has been boring on purpose."

### 7. Be original

No received phrases. No "unleashing the power of." No "in today's fast-paced world." No "[technology] enables trustless X." Find the specific angle, the unusual metaphor, the concrete image. Write what only your company can write.

Identify your company's unusual vantage point — the combination of capabilities or experience no competitor shares — and use it. Posts should sound like they could come from no one else.

### 8. Be concise (internet writing rules apply)

Make copy scannable. Short paragraphs. Headers. Enough white space. Cut every word that isn't doing work. This matters doubly for anything going on a website, social channel, or email — people skim before they read.

For blog posts specifically: short paragraphs beat long ones. A blog post should read fast even when it's saying something heavy.

---

## Tone Shifts by Audience

The core voice doesn't change, but emphasis does:

| Audience | Tone shift |
|----------|------------|
| CTOs / institutional execs | More formal, evidence-heavy. Lead with outcomes, business value, and reliability. Skip insider shorthand. |
| Founders / product teams at customers | Efficiency-focused. Lead with speed, cost, and developer experience. Use product names confidently. |
| Technical builders | More technical vocabulary is fine. Product and protocol names without glossing in every sentence (still gloss on first use). |
| General / retail / first-time users | Warmest tone. Maximum plain language. Analogies welcome. No assumed knowledge. |
| LLMs / AI agents (AEO/SEO) | Clean, structured, unambiguous. Educational and parseable. |

For blog work specifically, calibrate the register to the post type:
- Enterprise case studies, market analyses, partnership announcements → F-score 64-66 (institutional register).
- Product announcements, developer-facing posts → F-score 60-63 (benchmark zone).

---

## Words and Terms (Quick Reference)

<!-- Fill in your own canonical terms and deprecated terms. -->

**Always use:**
- "{COMPANY}" (the company) and "{PLATFORM}" (the platform). Define when bare shorthand is acceptable.
- "{TOKEN_OR_ASSET}" styled canonically (e.g., no "$" prefix in running prose, if applicable)
- Canonical names for your platform/product suite and each capability within it
- Product names with the company prefix on first reference in a section, if that's your convention

**Avoid externally:**
- {DEPRECATED_NAME_1} (use {REPLACEMENT_1})
- {DEPRECATED_NAME_2} (use {REPLACEMENT_2})
- Internal code names that never shipped as public brands
- Stacking superlatives: revolutionary, game-changing, unprecedented, groundbreaking

---

## Quick Voice Checklist

Run before submitting any blog post:

- Does the first sentence earn the reader's attention?
- Is the value (the WHY) stated before the feature (the WHAT)?
- Are there any jargon terms that a smart industry outsider wouldn't immediately understand?
- Is there anything that sounds like hype or could be seen as cocky?
- Does it use any clichés or received phrases?
- Is it scannable — headers, short paragraphs, no walls of text?
- Are company, platform, and product names used correctly and consistently throughout?
- Zero em dashes?
- Zero banned superlatives?

---

## Working with your visual identity skill

For any blog post that ships with a hero image, callout cards, or other design treatment, pair this voice with your brand's visual identity skill or guidelines. Write the post first using these voice rules; then apply visual identity at the design stage.
