---
name: customer-frame-agent
description: "Two jobs in one skill — translates any product feature into a customer-outcome explanation that anyone can understand (Mode 2), and reframes any existing content from we-perspective to you-perspective and from capability-language to outcome-language (Mode 1). Use when a product/eng team ships something and marketing needs to understand what it means for customers, or when reviewing any tweet, blog, headline, deck slide, ad, email, or landing page before it ships. Triggers on: 'explain the benefit for customers', 'translate this for buyers', 'what does this mean for [customer segment]', 'what's the customer angle', 'explain this in customer terms', 'reframe this', 'make this customer-focused', 'this sounds too internal', 'rewrite from the customer's perspective', 'frame check', 'customer check'."
owner: "{BRAND_OWNER} — your brand or messaging lead"
version: 0.1
status: Production
---

# Customer Frame Agent

The single rule: **everything you say or publish must be about what the customer can do for their users, anchored in the customer's core outcome.** Not what you built. Not what you shipped. Not what's new in v1.5.

First, define your **core outcome** — the thing customers actually buy your product for, stated as a verb phrase. For a payments company it's "money moves." For a devtools company it's "code ships." For a security company it's "threats get stopped." Every claim in every piece of content should ladder back to it. Wherever this skill says {CORE_OUTCOME}, substitute yours.

This skill exists because teams have a consistent failure mode: they describe **capabilities** (what we offer) instead of **solutions** (what customers can do). And they describe **infrastructure** (systems, architectures, internal concepts) instead of **outcomes** (the customer's job gets done).

The best companies in every category lead with the customer: "Give your customers a global dollar account." "Power online payments." "Connect a user's financial accounts in seconds." The failure mode defaults to: "{PRODUCT} v1.5 ships composable atomic {INTERNAL_JARGON}." This skill exists to close that gap, every time.

---

## Two modes

This skill does two distinct jobs. Pick the right one based on what's in front of you.

### Mode 2 — Customer Translation (start here when product ships something new)
**Input:** a raw product feature, engineering announcement, technical capability, or spec.
**Output:** a structured explanation of what it means for customers, in plain language a non-specialist can follow.
**Use when:** a product or eng team announces something and marketing needs to understand the customer benefits before writing any content.

### Mode 1 — Frame Check / Reframe (run before anything ships)
**Input:** existing draft content (tweet, blog, headline, deck slide, ad, email, landing page).
**Output:** verdict (PASS / NEEDS REFRAME / BLOCK), specific violations of the two laws, and a rewritten version.
**Use when:** any product-related content is about to go out. This is the pre-flight check.

### How they connect
Mode 2 is the **foundation**. Mode 1 is the **gate**. The natural flow:

```
[product/eng input]
        ↓
   Mode 2 (Translate)  →  customer-benefit primer
        ↓
   [marketing writes content from the primer]
        ↓
   Mode 1 (Frame Check)  →  verdict + reframe
        ↓
   ship
```

Run Mode 2 once per feature. Run Mode 1 on every piece of content that comes out of it.

---

## Mode 2 — Customer Translation

The "explain the benefits for customers" mode. Marketing's job, before writing any content about a new product feature, is to understand what it actually means for customers. This mode produces that primer.

### When to use Mode 2

- Product ships a new feature, upgrade, or capability and marketing needs to write about it.
- An engineering announcement (spec, version bump, protocol change, API update) lands and the team has to translate it for a non-technical buyer.
- Sales asks "how do I explain this to [buyer persona]?"
- Anyone says "explain the benefit for customers" or "what's the customer angle here?"

### How to brief Mode 2

Provide:
1. **The raw input.** A blog draft, an engineering note, a spec description, a PR, a chat post, a wiki doc, or just a paragraph describing what shipped. Plain text is fine. The skill does not need polished input.
2. **(Optional) The buyer persona.** If known, name who you're explaining it for (your key customer segments). If unspecified, default to "customers broadly" with relevant sub-personas called out.
3. **(Optional) The strategic context.** If the feature unlocks a specific deal, partner type, or competitive position, mention it. The skill will close with a strategic implication and this context sharpens it.
4. **(Optional) Known pipeline accounts.** If you know specific accounts already in conversation for this feature, name them. The skill will fold them into the "Who this is most useful for" section flagged as pipeline/sensitive. If you don't provide them, the skill will infer fits from public information — and those inferences need sales/BD verification before any external use.

### The Mode 2 output template

Return this structure. Each section is named so it can be skimmed.

```
## What this is, in plain customer terms
[1-2 sentences. Map the feature to a concept the buyer already speaks. Lead
with the analogy the buyer already uses, not the new term you invented.]

## The technical reality (briefly)
[1-2 sentences. What's actually happening, in plain language. No internal
jargon. Use comparisons to systems the reader already knows.]

## What model it maps to or unlocks
[1-2 sentences. What existing concept in the buyer's world does this
replicate, replace, or unlock? This is the bridge from "what we shipped" to
"why the buyer should care."]

## Specific benefits for customers

### [Benefit 1 — short, named]
[2-4 sentences. State the benefit. Include a concrete example, comparison, or
number that makes it real.]

### [Benefit 2 — short, named]
[Same structure. 3-6 benefits total is the right range. If you have fewer,
the feature might be too small for a Mode 2 run. If you have more, you're
listing capabilities not benefits — consolidate.]

### [Benefit N — short, named]
[Same structure. The last benefit, if there's a strategic edge or moat,
should usually be that one — and labeled as such.]

## Who this is most useful for

[3-5 customer segments most likely to need this feature. For each segment:

- The segment, with a one-line description of who they are and why they
  care about this specific feature.
- 2-4 specific named companies or accounts that fit. Include real company
  names — don't name a category without examples.
- The wedge — the specific reason this feature unlocks this segment.

Distinguish three tiers when relevant:
- **Confirmed public fits** — companies whose use case is publicly
  visible. Safe to name in external content.
- **Inferred fits** — real companies in the right segment with publicly
  visible products that match the wedge, but whose evaluation status is
  unconfirmed. Useful as targeting hypotheses; require sales/BD
  verification before going into target lists, ad campaigns, or
  one-pagers.
- **Pipeline / strategic** — accounts in active conversation, partner
  pilots, or prospect lists. Internal-only. Flag these clearly so
  marketing knows not to use them in public copy.

Be specific. A one-word category is not a segment. "Mid-market retailers
running their own loyalty apps (with 2-4 named examples)" is a segment.

**Standing rule:** Mode 2 identifies segment fit. Only the sales/BD team can
confirm pipeline status. Any named account in Mode 2 output that isn't a
confirmed public fit needs verification before it hits a target list, an ad
campaign, an outbound email, or any external collateral. Mode 2 output is
hypothesis, not approval to ship.]

## Strategic implication
[1-2 sentences. Why this matters at the deal level — what does it unlock,
what makes it defensible, who needs it most. This is the line a sales person
or exec will quote.]

## Marketing-ready hooks
[3-5 short candidate headlines, taglines, or first lines that marketing can
build content around. Each one passes the two laws — customer is the
subject, an outcome verb is in it, no version numbers or internal jargon.]
```

### Worked example: the calibration target

This is what Mode 2 should produce. The example below is for a fictional feature — "payment holds" shipped by a fictional payments-infrastructure company — but the shape is the target regardless of your industry. Substitute your own domain.

> **What this is, in plain customer terms.** For card issuers and payment companies, this replicates the auth/capture model their entire stack is built around — which is the unlock.
>
> **The technical reality.** Today, transfers on our rails are atomic: funds move or they don't. Card rails work on a two-step flow (authorize a hold, then capture later for the actual amount), and every piece of merchant tooling, fraud logic, dispute handling, and accounting assumes that model. Without holds, you can't run a credit card on these rails.
>
> **The specific benefits:**
>
> *Risk and fraud windows.* Issuers need time between authorization and settlement to run fraud checks, confirm fulfillment, or wait for a merchant to capture. A hold gives them that window without the user being able to double-spend the locked funds.
>
> *Variable settlement.* Hold $200 at a hotel, settle $187. Hold $50 at a restaurant, settle $62 with tip. Card rails do this every day. Atomic transfers can't.
>
> *Auto-expiry matches existing behavior.* Card authorizations drop off after ~7 days if not captured. Same model here, so back-offices don't need new reconciliation logic.
>
> *Non-custodial is the real strategic edge.* Funds stay in the user's account, just locked. The issuer isn't taking custody, which sidesteps a huge chunk of licensing exposure. They get card-like control without becoming a custodian.
>
> *Better UX than card today.* Users can see exactly who placed the hold, with metadata, and cancel holds themselves. Try doing that with a legacy pre-auth.
>
> **Strategic implication.** This is the missing primitive for card issuance on our rails. If we land a card issuer deal, this is the feature that makes it possible — and it's defensible because most competitors don't have it.

Notice what makes this work:
- Every benefit names a concept the buyer already understands (auth/capture, fraud windows, variable settlement, licensing, pre-auth).
- Every benefit includes a concrete example or number ($200/$187, ~7 days, "Try doing that with a legacy pre-auth").
- The strategic implication closes with a deal-level argument and a competitive moat.
- A marketer reading this can write a tweet, a one-pager, a sales deck slide, or a blog from it. It's the foundation, not the finished content.

### Mode 2 quality bar

A Mode 2 output passes if:
- A non-specialist could read it once and understand what shipped and why a customer would care.
- Every named benefit maps to an existing concept in the buyer's world.
- Each benefit has a concrete example, number, or comparison.
- "Who this is most useful for" lists 3-5 segments with named companies, not generic categories.
- The strategic implication closes with a deal-level or competitive argument.
- The marketer reading it could write a tweet from any single benefit and have it pass Mode 1.
- The marketer reading it could pick a target account and write a named-account email or one-pager from it.

A Mode 2 output fails if:
- It reads like a feature dump with bullets and no comparisons.
- Internal jargon shows up without translation.
- The benefits are abstract ("faster," "cheaper," "better") instead of specific ("hold $200 at a hotel, settle $187").
- The "Who this is for" section is segments without named companies, or named companies without the wedge that makes them fit.
- The strategic implication is missing or generic ("this matters because the market is big").
- A marketer couldn't generate finished content from it without doing more research.

### Mode 2 anti-patterns

- **Don't list features, list benefits.** "Approval steps are eliminated" is a feature. "Your users sign once, not twice — every time" is a benefit.
- **Don't explain the technology.** Explain the customer concept the technology enables. Buyers know their own domain. They don't know your internal primitives.
- **Don't bury the strategic argument.** Lead toward it. The reader should finish Mode 2 with a clear sense of what deal this unlocks or what competitive position this defends.
- **Don't dilute with too many benefits.** 3-6 is the range. More than that and they blur. Pick the ones that map cleanest to concepts the buyer already has.

---

## The two laws

Both modes apply these laws. Mode 2 builds the foundation that satisfies them. Mode 1 enforces them on finished content.

### Law 1 — Customer-Solution Law
Lead with what the customer can DO (for their users, business, or operations), not what you OFFER.

Bad: *"{COMPANY} provides a vertically integrated, open infrastructure platform."*
Good: *"Do {CORE_OUTCOME} for your customers globally — on rails you don't have to build."*

**The pronoun test:** count uses of "we / our / {COMPANY} / [product name]" vs. "you / your / your customers." If we-words outweigh you-words in the lede, the piece fails.

**The solutions test** (codified from real buyer feedback on best-in-class competitors): *"They've focused on solutions they enable rather than capabilities they have. They make it totally about you. What you can do. Not what they offer."* If a reader of your headline learns what *we* did instead of what *they* can now do, the piece fails.

### Law 2 — Customer-Outcome Law
Every claim ladders back to {CORE_OUTCOME}. Faster, cheaper, more global, more compliant, more reliable, lower abandonment — always stated as the customer's outcome.

Bad: *"1.5-second processing latency."*
Good: *"{CORE_OUTCOME} happens in 1.5 seconds. The incumbents can't say that."*

**The outcome-verb test:** is there an outcome verb (the verbs of your {CORE_OUTCOME} domain) in the first two lines? If not, the piece fails.

**The relevance test** (codified from exec feedback): *"'We are now faster' is irrelevant. 'We can handle even more of your workload — by the way, look at how much is already flowing through' is much more relevant."* Every infrastructure claim must be paired with a customer consequence.

---

## Mode 1 — Frame Check / Reframe

The pre-flight check on finished content. Run on any tweet, blog, headline, deck slide, ad, email, or landing page before it ships.

### When to use Mode 1

- Reviewing any draft content before it publishes.
- Translating an engineering announcement into customer-facing copy.
- Reframing existing content that "sounds too internal" or "doesn't say anything about the customer."
- Pre-flight check on any product launch comms.
- Any time someone says "make this more customer-focused" or "is this on-message?"

If a piece of content touches the customer's outcome at all, default to running Mode 1 on it.

### The diagnostic checklist

Run any piece through these questions before publishing. Any "no" means reframe.

1. **Does the headline name what their customer or user gets, or what we built?**
2. **Is there a customer outcome ({CORE_OUTCOME} happens, costs less, takes less time) in the lede?**
3. **Could you swap {COMPANY} for any competitor and the copy still work?** If yes → too generic, add company-specific proof.
4. **Does it speak to the buyer's actual KPI?** Conversion rate, unit cost, turnaround time, geographic reach, regulatory cover, time-to-integrate.
5. **If a CFO read only the headline, would they get what their company can DO with this?**
6. **Does the post still make sense if you removed every product name?** If yes → it's about outcomes. If no → it's about us.
7. **Is the customer the subject of at least the first sentence?** If we / {COMPANY} / a product name is the subject, reframe.

---

## Translation rules

These are the moves that take a piece from capability-shape to customer-shape.

### 1. "We" → "You"
- "We support 100+ countries" → "Reach customers in 100+ countries"
- "We've built a certified compliance stack" → "Launch in 48 states without standing up a license stack"

### 2. Capability nouns → customer verbs
- "Cross-platform orchestration" → "Run your workload across platforms"
- "Embedded infrastructure" → "Offer the feature without building the plumbing"
- "Programmable settlement layer" → "Close the loop in seconds, anywhere your customer is"

### 3. Product names → outcomes (in headlines)
- "{PRODUCT} v1.5 ships approval-less flows" → "Your users now sign once, not twice"
- "{PRODUCT} adds 12 new regions" → "Serve 12 more regions, with no new compliance lift"
- "{PRODUCT} updates the SDK" → "Embed the feature in your app without owning the risk"

### 4. Technical metrics → customer metrics
- "1.5s processing latency" → "The whole job completes in 1.5 seconds — incumbents can't say that"
- "200+ supported integrations" → "Reach whatever stack your customer already runs"
- "99.9% uptime" → "Keep operating 24/7. Including the weekends legacy systems sit out."

### 5. Product diagrams → outcome flows
Show the customer's job entering somewhere, moving through, landing somewhere. Don't show your system architecture.

If a diagram has a box labeled with your product name in the center and arrows pointing in and out — that's an architecture diagram, not a customer diagram. Reshape so the boxes trace the customer's job from start to finish, with your product as the rail underneath. Show what happens to the customer's work, not how your software is organized.

### 6. Version numbers → customer capability shifts
- "{PRODUCT} v1.5 ships..." → "More one-click flows just went live..."
- "Spec 87 introduces fixed pricing..." → "You can now pay in the currency you already account in."

Version numbers are for changelogs. Customers do not care.

---

## Before/After examples (the most important section)

These are the recurring patterns this skill exists to fix. All examples use a fictional company ("Acme") — substitute your own product and domain, keeping the shape.

### Feature launch

**Before (what shipped):**
> New to Acme v1.5: Composable actions are completed in one request.

**After:**
> A platform paying creators across 100 countries. One request.
> A merchant closing out the day's orders on their chosen stack. One request.
> An ops team moving work across regions, converting formats as it goes. One request.

Why it passes: customer is the subject of every line, outcome verb in every sentence, three buyer ICPs named, no product version number in the lede.

### Performance improvement

**Before:**
> Acme processing latency will drop from 2s to 1.5s with capacity gains from the Q3 infrastructure upgrade.

**After:**
> Incumbents acknowledge a request in milliseconds. The actual job takes days to finish.
> Acme now starts AND finishes the job in the same 1.5 seconds.

Why it passes: anchored to a system the buyer already knows, customer outcome is the headline, infrastructure detail is the supporting fact.

### Compliance/coverage expansion

**Before:**
> Acme holds operating licenses in 48 US states, providing regulated infrastructure.

**After:**
> Launch across 48 states. Acme holds the licenses. You ship the product.

Why it passes: customer verb leads, capability is the proof point, last line is the control statement.

### Embedded infrastructure product

**Before:**
> Acme offers embedded infrastructure with built-in verification, managed custody, and cost abstraction.

**After:**
> Offer the feature to your users without becoming a compliance company. Verification, custody, and costs — handled.

Why it passes: customer can name the verb, the risk they avoid, and the things they don't have to build.

### Positioning line

**Before:**
> The Acme Platform is an open, integrated, programmable infrastructure stack.

**After:**
> Do {CORE_OUTCOME} for your customers globally. 100+ countries. Any format. Any stack. One integration. Your brand. Your economics.

Why it passes: every line is what the customer can do or has. Acme is invisible. The proof points are specific. The control statements ("Your brand. Your economics.") put the customer in charge.

### Governance/spec change

**Before:**
> Spec 87 authorizes the business side to pursue deals for generalized fixed pricing, allowing fee payments in fiat currency.

**After:**
> You can now pay fees in dollars. No volatile estimates. No special balance to maintain. Just an invoice, in the currency you already account in.

Why it passes: customer verb leads, customer KPI (predictable cost) anchors, internal jargon gone.

---

## Reference patterns from companies doing this well

Study the market leaders in any infrastructure category — their homepages follow the same shapes. Use these abstracted patterns as templates when reshaping copy.

### Pattern A — the full stack pitch
> "Give your customers [the end-user outcome]. [Reach metric]. [Capability proof]. Your brand. Your economics. One integration."

The shape: **second-person verb → customer-of-customer outcome → proof points → control statements ("your brand, your economics") → simplification ("one integration")**. Every line is about what the customer can do or has. The company itself never appears as the subject.

### Pattern B — the category claim
> "[The customer's job] is easier with [Company]. Power your business with one platform."

The shape: **outcome → simplification → control**. The company shows up only as the supporting agent.

### Pattern C — the speed claim
> "[Do the customer's job] in seconds. Powering the apps you love."

The shape: **action verb → customer noun → speed metric → ecosystem proof**. The company is the verb modifier, not the subject.

### Pattern D — the empowerment claim
> "Build the unique customer experiences you envision."

The shape: **second-person verb → customer-of-customer → empowerment**.

### Pattern E — the reach claim
> "Send [the thing] to your users in 50+ countries. Automate it for marketplaces, platforms, and creator economies."

The shape: **action verb → customer-of-customer → reach metric → use case stack**.

### What they all have in common
- The company name shows up at the end, not the beginning.
- The customer's verb is the subject of the sentence.
- The company is the enabler, not the hero.
- Proof points are specific (numbers, geographies, integrations).
- Control statements ("your brand," "your economics," "your team") put the customer in charge.

---

## Anti-patterns (block these on sight)

| Anti-pattern | Why it fails | Fix |
|---|---|---|
| Version numbers in headlines ("{PRODUCT} v1.5 ships...") | Tells the customer the post is for someone else | Lead with the customer-facing capability shift |
| "We're excited to announce..." | Nobody is excited except us | Cut entirely. Lead with the customer outcome. |
| Product name as headline subject ("{PRODUCT} now does X") | We are the hero | "You can now do X with {PRODUCT}" |
| Capability lists without customer verbs | Customer doesn't know what to DO with the list | Pair every capability with a customer verb |
| Internal jargon before translation | Buyer doesn't know what it means and shouldn't have to | Translate to plain customer language |
| System architecture diagrams | Shows our org, not the customer's journey | Show the outcome flow, not box-and-arrow |
| Product names in CTAs ("Try {PRODUCT}") | Brand instead of action | "[Do the outcome] in one click" |
| "Powered by {COMPANY}" as a headline | We are still the subject | "Reach 100+ countries with one integration" |
| Adjective stacks ("fast, secure, scalable") | Generic, swappable for any competitor | Replace with specific proof points |

---

## Mode 1 output format

When invoked on a piece of content, return:

**Verdict**: PASS / NEEDS REFRAME / BLOCK
- PASS: ships as-is
- NEEDS REFRAME: ships after specific rewrites
- BLOCK: pull and start over — fundamental we-perspective or no-customer-outcome

**Diagnostic**: 2-4 specific violations, each tied to one of the two laws or a translation rule. Be concrete. Don't say "this is too internal" — say "the headline names a product version, not a customer outcome (Law 1, Translation Rule 6)."

**Reframe**: a rewritten version that passes the diagnostic. If the original is multi-section (a thread, a blog, a landing page), reframe each section.

**Notes**: any tradeoffs the rewrite makes (e.g., "moved version number to body, kept proof point," or "compressed three feature bullets into one customer verb").

If a Mode 1 review BLOCKS because the writer clearly doesn't understand the customer angle (the content has no idea what the feature means for customers), recommend running **Mode 2 first** to build the foundation. Then re-write from the Mode 2 primer and re-run Mode 1.

If invoked during content generation rather than review, apply the laws and rules as you write — and return the customer-solution, customer-outcome version directly, not a draft to be reviewed.

---

## Integration with other skills

This skill is the **frame** layer. Order of operations:

1. **Your messaging skill** — provides the substance (positioning, proof points, competitive context, canonical language)
2. **This skill** — applies the customer-solution + customer-outcome FRAME
3. **Your brand-voice skill** — applies the final tone, rhythm, and style polish

Use sequence:

- **For new content:** messaging skill → write → run through this skill → brand-voice skill → ship
- **For existing content:** run through this skill first to diagnose; then re-run through the messaging and brand-voice skills for any rewrites
- **For competitor or market response:** run this skill first to make sure the response leads with what *our* customers gain, not how *we* compare

When this skill conflicts with another:
- Conflicts with the brand-voice skill on tone → brand-voice wins on style
- Conflicts with the messaging skill on substance → messaging wins on facts
- This skill wins on FRAME and PERSPECTIVE in all cases. If a piece is "on-voice and on-message" but written from we-perspective, it still fails this skill.

---

## Mode 1 quality bar

A piece passes Mode 1 if:
- The headline names a customer outcome, not a product feature
- The first sentence is about what the customer can do, not what we built
- An outcome verb appears in the first two lines
- Every claim has a specific proof point (number, license, name, volume, country)
- A CFO understands the headline without translation
- The post would still make sense if you removed every product name

A piece fails Mode 1 if:
- Version numbers or internal jargon appear before any customer verb
- The first sentence subject is "we / {COMPANY} / [product name]"
- The customer is invisible
- The outcome is invisible
- The headline could belong to a competitor word-for-word

---

## Sharing this skill with the team

This skill is built to be shared across the marketing org so anyone can take a product input and:
1. Understand how it impacts customers (Mode 2)
2. Lead the narrative with that customer angle (Mode 2's marketing-ready hooks → Mode 1's reframe)
3. Orient every output around the customer (the two laws, enforced by Mode 1)

### Quick-start for a teammate using this skill for the first time

**Scenario A — Product just shipped something.**
1. Paste the raw input (blog draft, eng announcement, spec, chat post) into Claude with the prompt: *"Run Mode 2 of customer-frame-agent on this — explain the benefits for customers."*
2. Get back a primer with named benefits and marketing-ready hooks.
3. Pick the hook that fits the channel (tweet, blog, deck slide, ad).
4. Write the content using the primer as your foundation.
5. Run Mode 1 on the draft before publishing.

**Scenario B — Reviewing a draft before it ships.**
1. Paste the draft into Claude with the prompt: *"Run Mode 1 of customer-frame-agent on this — frame check."*
2. Get back a verdict (PASS / NEEDS REFRAME / BLOCK) with specific violations and a rewritten version.
3. Decide whether to ship the reframe, edit further, or run Mode 2 to better understand the customer angle.

**Scenario C — Sales or BD asks "how do I explain this to [buyer persona]?"**
Run Mode 2 with the buyer persona specified. The output is the briefing.

### The two questions every teammate should ask before publishing

1. *Does the first line tell a buyer what they can now do?*
2. *Does it tell them how {CORE_OUTCOME} gets better because of it?*

If either is "no," reframe — or run the skill to do it for you.

---

## The North Star

If a buyer reads only the first line of any piece of content, they should know:
1. **What their company can now do for their customers.**
2. **How {CORE_OUTCOME} gets better because of it.**

If they don't know both of those things from the first line, the content fails this skill. Reframe and re-ship.
