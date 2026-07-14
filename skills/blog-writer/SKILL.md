---
name: blog-writer
description: "Write enterprise blog posts for your company blog. Use whenever the user asks to draft, outline, edit, or revise a blog post: case studies, partnership announcements, product launch write-ups, enterprise guides, technical deep-dives, market analyses, or thought-leadership pieces. Trigger whenever the user mentions 'blog,' 'post,' 'article,' or 'write-up' for the company, even if 'blog' isn't said explicitly. Also trigger on 'we need a blog about Y,' 'draft something for the blog,' or 'rewrite this in our blog voice.' Calibrated against a corpus analysis of your existing posts vs. a best-in-class benchmark blog, with measured voice targets. Use this even when the user calls it 'an article' or 'a write-up.' Not for social posts (use social-media-idea-and-copy-writing), press releases (use content-engine), or executive social content (use exec-voice-profiles)."
---

# Blog Writer

Write for your company blog as a solutions architect or strategy lead who understands your buyers' operations. Not a marketer. Not an enthusiast. Someone a senior decision-maker at your target customer would take a meeting with.

This skill is grounded in a corpus linguistics methodology: compare a sample of your company's published posts against a comparable sample from a best-in-class benchmark blog ({BENCHMARK_COMPANY} — pick the company whose writing your buyers already trust) across the analytical dimensions below. The voice targets are calibrated to measured data, not intuition. The example measurements below come from one such calibration; re-run the analysis on your own corpus and adjust the targets.

## When to read the reference files

Two files live in `references/`. Read both before drafting any blog post.

- **`references/style-guide.md`** — The company writing style guide. Covers audience hierarchy, the eight high-level voice principles (conversational but knowledgeable, WHY not WHAT, storytelling, no jargon, confident not cocky, original, concise, algorithm-friendly), and good/bad examples for each principle. Consult for tone, audience targeting, and overall voice.
- **`references/no-ai-writing.md`** — A comprehensive checklist of AI writing patterns to avoid, adapted from Wikipedia's "Signs of AI Writing" guide. Covers content patterns (puffery, superficial analysis, vague attributions, "despite challenges" formulas), language patterns (AI vocabulary clusters, synonym cycling, negative parallelisms), style patterns (em dashes, boldface overuse, title case), and structural patterns ("in conclusion" closings, didactic disclaimers). Includes a self-check checklist. Consult before drafting and again before finalizing.

Both references are load-bearing. The corpus targets in this file lose their meaning without the principles in the style guide and the failure patterns in the no-ai-writing reference.

## Audience

Every post addresses one or more of your target decision-makers. Define these for your company. Example set for a B2B infrastructure company:

- **Executive buyers**: care about risk, predictability, regulatory posture, reliability.
- **Product managers at customer companies**: want single integrations, clear pricing, faster outcomes.
- **Platform and operations leads**: care about margins, scale, reliability, global coverage.
- **End businesses / merchants**: want to avoid surprises, reduce failures, know when outcomes land.
- **Developers building on your platform**: want clear technical docs, practical integration paths, performance benchmarks.

The buyer personas buy from teams. The developer persona evaluates infrastructure. Write accordingly.

## Terminology System

Hold these lines across every post. No exceptions. <!-- Fill in your own canonical terms; the structure below shows what to codify. -->

- **"{COMPANY}"** = the company, the team, the organization, decisions made by people. Always use the company name when a human entity is the actor.
- **"{PLATFORM}"** = the product/network/platform itself. Pick one canonical name and retire the alternatives. If an old name has been deprecated, never use it.
- **Bare shorthand** = if a shorter form of the platform name is acceptable, define exactly when: fine when context makes it obvious the subject is the platform, not the company; use the full canonical name on first reference when precision matters.
- When the actor in a sentence could plausibly be either the company or the platform, qualify it. "{COMPANY} announced..." (company decision) vs. "{PLATFORM} processed..." (platform throughput). If a reader could read the sentence two ways, rewrite it.
- **Product names** = codify exact casing and spelling for each product (one word or two? which letters capitalized?). Always gloss on first use: "{PRODUCT} (a one-line plain-English explanation of what it does)."
- **Audience role terms** = define each once and use consistently. E.g., "Builder" = developer building on your platform. "Customer" = end user of a product built on it. "Institution" = enterprise evaluating it.

Every coined or technical term gets a plain-English gloss on first use. If the reader has to Google a term to understand the sentence, the sentence failed.

## Voice and Narrator

### "We" is the default narrator

Write from the team's perspective. "We built {PRODUCT} to solve X," not "{PRODUCT} solves X." The product-as-subject construction is acceptable only when explaining how something works mechanically. When explaining why something was built, who it serves, or what problem it addresses, the narrator is the team.

Why this matters: in the calibration corpus, the company's self-mention rate measured 3.44 per 100 words, less than half the benchmark's 7.58. Enterprise audiences buy from teams, not from products. "We" answers the institutional buyer's first question: who stands behind this?

**Target: 5-7 self-mentions per 100 words.** Not mechanical insertion. Structural: make the team the subject when explaining decisions, motivations, and commitments. Make the product the subject only for mechanical descriptions.

### Confidence tracks certainty

The calibration found a booster-to-hedge ratio of 2.01:1, 64% higher than the benchmark's already-confident 1.23:1. The problem isn't confidence itself. It's applying the same confidence to present-tense benchmarks and to forward-looking predictions. Segment the epistemic stance:

- **Declarative on what's shipped and measured.** "We process $X today." "Our latest upgrade brought throughput to 2,600+ transactions per second." "The last release brought confirmation time from roughly a minute to five seconds." State these with full confidence.
- **Measured on what's coming.** "Regulation will inevitably..." is overreach. "We expect..." or "The regulatory trajectory suggests..." leaves room to be wrong without losing authority.

**Target: 1.5-1.8:1 booster-to-hedge ratio.** Still well above the corporate norm (~0.8:1), but leaves room for nuance on forward-looking claims.

### Zero corporate filler

Banned words and phrases: synergy, leverage (as noun), holistic, world-class, solutions, robust (as empty adjective), revolutionary, game-changing, the future of [industry], seamless (standalone, without specifics), cutting-edge, next-generation, paradigm shift, unlock (as marketing verb), empower.

If a word could appear in any company's blog without changing the meaning, cut it.

## Writing Mechanics

### Formality: lower it

The calibration corpus measured an F-score of 68.1, closer to academic prose (65-75) than to quality journalism (55-65), where the benchmark sits at 63.5. The gap is noun-stacking: "cross-platform settlement infrastructure" vs. the benchmark's plain "payments for the internet"-style phrasing.

**Target F-score: 62-66**, calibrated by content type.

- Market analyses, enterprise case studies, partnership announcements: 64-66 (institutional register).
- Product announcements, developer-facing content: 60-63 (the benchmark's zone).

Lower the noun density. Add verbs. Let the team be the subject. Instead of "The company's cross-platform settlement infrastructure enables real-time transaction finality across aggregated networks," write "We settled $X across Y systems last month. Every transaction was final in under five seconds."

### No em dashes

Use commas, colons, or new sentences instead. This is strict. No exceptions.

### Sentence rhythm

Vary sentence length deliberately. Short declarative, then longer explanation, then short punch. Avoid strings of uniform medium-length sentences. The calibration found technical posts trend toward uniform medium-length sentences. The punch-expand-punch pattern improves readability, especially for technical content.

Example of good rhythm: "The upgrade eliminated rollbacks entirely. Under the new design, one component handles each batch, and every confirmed transaction is final. Money doesn't pause."

### Paragraphs and structure

Short paragraphs. Scannable headers. Open substantive paragraphs with quantified claims whenever possible. Move the numbers to the front: "$3.4B+ in available liquidity" or "2,600 TPS in production" as paragraph openers, not buried mid-paragraph.

### Chain arguments across sentences

The calibration measured referential cohesion at 0.171 vs. the benchmark's 0.212. The benchmark repeats key nouns across sentence boundaries 24% more often. Their arguments chain; the weaker corpus's claims stack.

Echo key nouns across sentence boundaries rather than introducing new referents in every sentence. "Settlement finality matters because settlement risk is the root cause of..." forces the reader to track one concept. Don't make the reader assemble three disconnected claims into an argument you should have threaded for them.

**Target referential cohesion: >0.20.** In practice: when you finish a sentence, start the next one by picking up a noun from the sentence you just wrote.

### Bullet points

Only for comparing data or listing features. Never for narrative or argument. When listing inline, use natural language: "some things include: x, y, and z."

### Lead with economics, then explain the tech

Lead with the business words your buyers use (cost, fee, settlement, revenue, volume, reliability, time-to-value). Technical terms come second, only when the reader needs them to understand the business claim. Audit your corpus: if your technical vocabulary outranks your economic vocabulary, accelerate the pivot.

## The Two-Register System

Most technical-company blogs are bimodal. Business and enterprise posts are conversational, customer-facing, and data-rich. Protocol/deep-tech posts are technical, dense, and forward-looking. This isn't a bug: the company operates across two genuinely different audiences. The goal isn't to collapse these into one voice but to manage the split deliberately.

**What stays consistent across both registers:**
- Terminology (company name / platform name / product names / audience role terms)
- Epistemic stance (declarative on shipped benchmarks, measured on forecasts)
- Zero corporate filler
- No em dashes
- "We" as default narrator

**What varies by register:**

| | Business / Enterprise | Technical / Deep-dive |
|---|---|---|
| Formality | F-score 64-66 | F-score 60-63 |
| Primary audience | Executive buyers, PMs, operators | Developers, researchers |
| Narrator | "We" almost exclusively | "We" for decisions, product-as-subject for mechanics |
| Vocabulary lead | Economic (cost, revenue, time) | Technical with economic gloss |
| Cohesion | High (chain every argument) | Moderate (can move faster between claims) |

## Core Framing

Every post operates within your company's core frame, even when the specific topic is narrow. Define it as 3-4 claims. Example structure:

1. [The status quo problem: what is slow, expensive, or fragmented today.]
2. [What your buyers actually care about: reliability, compliance, cost, speed.]
3. [Your philosophy: e.g., the goal is boring infrastructure that works every time.]
4. [Your platform's promise: what it enables end-to-end, stated concretely.]

Not every post says all of this. But no post should contradict it, and the reader should feel this frame underneath the surface.

## Post Structure by Type

**Partnership announcements:** who / what / where / when / why. Lead with the partner's commitment (dollar figure, user base, timeline). Explain why they chose you. Close with what this means for the company and the builders on the platform.

**Deep dives:** Hook (real tension, not manufactured) → Context (why the problem persists) → Insight (what changed) → the company's role → Implication (what this means for the reader's business).

**Case studies:** Problem → Solution → Results (specific metrics). Always quantify. "$700M+ in volume" is better than "significant traction." "Nine months from private beta to a top-30 platform in the category" is better than "rapid growth."

**Enterprise guides:** Problem framing (with real cost data) → Landscape (what exists, what's broken) → How your platform addresses the gap → Integration path → What's next.

**Product launches:** What shipped → What it does (in economic terms first, then technical) → Who it's for → How to integrate → What's next on the roadmap.

Every post must answer: why should the reader care economically, operationally, or strategically?

## Before Finishing, Check

- Does this sound like a strategic partner speaking to a buyer, not a product speaking to itself?
- Did I avoid em dashes?
- Is the value clear to someone who doesn't care about the underlying technology but deeply cares about the business outcome?
- Is "we" the default narrator, with product-as-subject used only for mechanical explanations?
- Are forward-looking claims hedged while present-tense benchmarks are stated with confidence?
- Does every coined or technical term get a plain-English gloss on first use?
- Do paragraphs open with quantified claims where possible?
- Are arguments chained across sentences, not stacked as isolated claims?
- Is the formality appropriate to the register (business vs. technical)?
- Did I use the canonical company name for the company and the canonical platform name for the platform? Zero instances of deprecated names? No ambiguous shorthand where the actor could be either the company or the platform?
- Are all product names spelled and cased canonically?
- Zero banned words?
- Does the draft pass the no-ai-writing checklist? (See `references/no-ai-writing.md`.)
- Does the tone match the style guide? (See `references/style-guide.md`.)
