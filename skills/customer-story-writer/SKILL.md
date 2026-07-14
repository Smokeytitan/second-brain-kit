---
name: customer-story-writer
description: "Draft publish-ready customer cards for your company's /customers page. Use whenever the user says 'draft customer story for [company]', 'write the [company] customer story', 'make a customer card for [company]', 'customer story [company]', 'next customer card is [company]', 'next up is [company] for the customer page', or any variant. Trigger even when the user doesn't say 'customer story' explicitly — if they're asking for a customer-page write-up about a real company using your product, this is the skill. Each invocation produces a single mini-blog (~400 words) that lives on the customer card; there is no separate long-form blog article. Strict no-invented-facts policy: every claim and quote traces to source material. NOT for social copy, community posts (community-channel-post-drafter), marketing briefs (marketing-brief), or press releases. Pairs with your brand-voice and brand-identity skills."
---

# Customer Story Writer

## What this skill produces

One thing: a customer card mini-blog for your company's /customers page. 400-1000 words (sweet spot 500-700). The card itself is the entire story; there is no separate long-form blog article.

The mini-blog uses a three-section Problem / Solution / Results structure. The file includes:

- Headline (action-oriented, names the customer, names what they do with {PRODUCT})
- 1-sentence subheading
- Opening customer quote (real, sourced)
- 2-3 hero metrics
- `### The problem` section: 1-2 paragraphs grounding the reader in what was broken, plus an inline customer quote
- `### The solution` section: 2-3 paragraphs on what the customer does with {PRODUCT}, with product capabilities (low costs, speed, reliability, ecosystem depth, etc.) folded INTO the narrative as supporting evidence (not as a standalone "Why {COMPANY}" feature block)
- `### The results` section: 1-2 paragraphs on outcomes and what's next, plus a closing customer quote
- About-the-company blurb (50-80 words)
- Metadata fields for the CMS (industry, company size, use case, region, features)
- A chat preview so the user can review without opening the file

Calibrated to best-in-class B2B customer story pages: a punchy ~440-word anchor for SMB-style stories and a ~1,400-word anchor for richer enterprise narratives. Most Tier 1 customers fit comfortably in 500-700 words.

Source of truth for the program lives in `Projects/Customer-Stories-Program.md` in your workspace. Read that file when invoked so the story aligns to the current Tier 1 / Tier 2 list and publishing schedule.

## Strategic frame (do not deviate)

The Customer Stories Program narrowed the filter. Apply it strictly:

1. **The customer must use {PRODUCT} itself.** Not adjacent product lines (those belong to their own separate customer-story programs). Not pure ecosystem partners. If the relationship is a partnership program, an indirect integration, or a sibling product, this skill is not the right fit. Flag and check with the user.

2. **{PRODUCT} must have solved a real problem for them.** The story must connect a specific customer pain point to a specific product capability and outcome. "We use {PRODUCT}" is not a story. "Acme Corp's multi-billion-dollar program runs on {PRODUCT} through a key integrator because the product delivers enterprise-grade reliability at costs that make the program economical" is a story.

3. **Customer + problem + outcome lead. Product features support.** Readers care that real companies with brand equity get real problems solved. They do not care about raw performance numbers in isolation. Product features fold INTO the Solution narrative as supporting evidence inside customer-shaped sentences. They never get their own section, header, or feature-list block.

4. **The thesis:** brand-equity customers solving real problems drives more inbound leads than product-metric posturing.

## Workflow

### Step 1: Confirm the customer fits the program

Before drafting:

- Open `Projects/Customer-Stories-Program.md`. Verify the customer is in the Tier 1 or Tier 2 list.
  - If yes: proceed.
  - If no: flag and ask the user whether to add them, route to a sibling program, or skip.
- Confirm the relationship is real {PRODUCT} usage (direct, production, meaningful integration). If unclear, ask.

### Step 2: Gather source material

Pull in this order. Do not invent details if a source returns empty.

1. **Customer interview transcripts.** Search your transcript folder (`{TRANSCRIPT_FOLDER_ID}` — your Drive folder of recorded customer interviews) for the company's transcript using the Google Drive search tool. This is the richest source of direct quotes.

2. **The interview-questions doc.** Doc id `{INTERVIEW_QUESTIONS_DOC_ID}` — your team's per-customer background doc, including relationship details. Always read this for context.

3. **CRM deal context.** If a CRM connector is available, pull deal records for the customer for commercial context.

4. **Web fetch for public info.** If the customer has a public press release, blog post, or filing relevant to the story, fetch it. URLs must be real and from a previous tool result.

If a source returns nothing, note the gap. Do not fabricate quotes, metrics, or facts to fill the gap. Flag "data pending" in those slots.

### Step 3: Lock the story spine

Before writing, identify and write down:

- **The specific pain.** What was painful before {PRODUCT}? Get concrete. This becomes the Problem section.
- **The solution.** What does the customer actually do with the product today? Get concrete. This is the lead of the Solution section.
- **The features that made it possible.** 3 to 5 product capabilities the customer actually leverages. These get folded INTO the Solution section as one dense supporting paragraph; they do not get their own section.
- **The outcome.** Real metrics and forward-looking direction. This becomes the Results section. Real numbers only; if none exist for a slot, write "data pending."
- **3 to 5 direct quotes.** Attributed to a real person at the company. Source the transcript first, then public materials. Plan placement: hero quote at the top, inline quote in Problem, optional inline quote in Solution, closing quote at the end of Results.

If you cannot articulate all five in one sentence each, the source material is too thin. Tell the user what is missing.

### Step 4: Draft the mini-blog

Use this exact skeleton (heading levels matter; the customer story title is `##` and the section headers are `###`):

```markdown
## [Headline: action verb + customer name + what they do with {PRODUCT}]

[1-sentence subheading: who they are and what they're doing with {PRODUCT}]

> "[Hero customer quote]"
>
> *[Name], [Title], [Company]*

| [Metric 1] | [Metric 2] | [Metric 3] |
|---|---|---|
| [Number] | [Number] | [Number] |

### The problem

[1-2 paragraphs. What was broken before. Make it concrete and quantified where possible.]

> "[Inline customer quote on the problem or stakes]"
>
> *[Name], [Company]*

### The solution

[Paragraph 1: what the customer actually does with {PRODUCT}. Customer is the subject of most sentences.]

[Paragraph 2: one dense paragraph that folds in the product features that mattered (low costs, speed, compatibility, ecosystem depth, etc.) as supporting evidence inside customer-shaped sentences. Do NOT make this a feature list or sub-section.]

### The results

[1-2 paragraphs on outcomes today and where the relationship is going next.]

> "[Closing customer quote that captures the broader pattern or strategic shift]"
>
> *[Name], [Company]*

### About [Company]

[50-80 word blurb: who they are, what they do, one sentence on scale or significance.]

### Metadata

| Field | Value |
|---|---|
| Company | ... |
| Industry | ... |
| Company size | SMB / Mid-market / Enterprise / Public sector |
| Use case | ... |
| Region | ... |
| {PRODUCT} features used | (the features cited in the Solution section) |
```

**Headline rules.** Action-oriented and specific. Customer name appears. Concrete verb (built, settles, moves, issues, scales, processes, powers, etc.). Plain language. The 6th-grader test: a 6th grader should understand what the customer does with {PRODUCT} from the headline alone. No abstract framing like "[Company]'s journey with {COMPANY}."

**Hero metric rules.** Real numbers only, sourced from the interview, the CRM, or public material. 2-3 metrics. Each labeled. If a real number isn't available, write "data pending."

**Word count target.** 400-1000 words for the body, with 500-700 as the sweet spot. Calibrated against best-in-class customer-story ranges (SMB-length anchors around 440 words, longer narrative customers stretching toward 1,000+). Length should match the story's substance: a punchy SMB-style story sits at the lower end; a richer enterprise narrative with multiple flows or pilot examples sits in the middle to upper end. Hard ceiling at 1,000.

### Step 5: About the company blurb

50-80 words. Sidebar copy. Plain English: who the company is, what they do, one sentence on scale or significance.

### Step 6: Metadata fields

Fill these for the CMS:

- Company name
- Industry
- Company size: SMB / Mid-market / Enterprise / Public sector
- Use case (e.g., "payments processing," "cross-border operations," "enterprise infrastructure," "consumer commerce")
- Region
- {PRODUCT} features used (same tags as cited in the Solution section)

### Step 7: Save and present

1. Save the markdown to `Projects/Customer-Stories/[company-slug]-customer-card.md` in your workspace. The slug is lowercased, hyphen-separated (e.g., `acme-corp-customer-card.md`). Create the `Customer-Stories` directory if it does not exist.

2. If the user asks for a Word doc to send for review (e.g., to {BRAND_LEAD}), also produce `[Company]-Customer-Card-For-[Reviewer].docx` in the same directory using pandoc:

   ```bash
   pandoc /path/to/[slug]-customer-card.md \
     -o "/path/to/[Company]-Customer-Card-For-[Reviewer].docx" \
     --from markdown --to docx --standalone
   ```

   When producing the docx for review, prepend a short cover section to the markdown (title, "Draft for review," 1-2 sentence framing) and a Notes-and-flags section at the end (any unresolved title attributions, metric verifications, or data-pending slots). Don't add these to the canonical markdown; just the reviewer's docx copy.

3. Surface in chat:
   - Headline + subheading
   - Hero quote
   - Hero metrics
   - Metadata block
   - Link to the saved markdown file (and the docx if produced)
   - Any flags (missing transcript, data pending, fact-gathering needed)

4. Stop. Do not generate social copy, Slack posts, marketing briefs, press releases, or tracker tasks. Each of those has its own skill.

## Hard rules

- **No invented facts.** Every claim traces to source material. If you cannot source it, omit it or flag "data pending."
- **No fake quotes.** Every direct quote comes from a customer interview transcript, a public interview, a press release, or another verifiable source. No reconstructed paraphrases dressed up as quotes.
- **No em dashes or en dashes anywhere.** Use colons, commas, semicolons, parentheses, plain hyphens, or separate sentences.
- **Company voice.** Pair with your brand-voice skill or style guide. Read its principles before drafting.
- **Headlines name the customer and include a concrete verb.** No abstract framing.
- **Product features fold into the Solution section.** Never get their own header or sub-section. Never appear as a bulleted feature list. They support customer-shaped sentences inside a single dense paragraph.
- **The 6th-grader test.** A 6th grader should understand the headline. Rewrite if not.

## Reference layouts

Primary model: **a three-section structure** (Problem / Solution / Results) with hero metrics and sidebar metadata, modeled on best-in-class B2B customer story pages. Study 2-3 customer-story pages you admire and note two anchors: a short punchy one (~440 words) and a long enterprise one (~1,400 words).

For action-verb headline patterns, look at customer-story pages that lead with what the customer does, not the vendor relationship. Borrow headline patterns; do not borrow over-sub-sectioned solution structures.

## What this skill does NOT do

- Social copy (X, LinkedIn, Reddit): use your social copywriting skill.
- Community posts (TG, Discord, Reddit content): use `community-channel-post-drafter`.
- Marketing briefs for {CONTENT_LEAD}: use `marketing-brief`.
- Press releases: use your press-release/content skill.
- Tracker task scaffolding: use `pmm-launch-scaffolder`.
- Posting anywhere (tracker, Slack, blog CMS): no. Just produces files and a chat preview.

## Related skills

- Your brand-voice skill: apply company voice. Read its principles before drafting.
- Your brand-identity skill: visual identity reference (not used here directly but useful context).
- The canonical project plan lives in `Projects/Customer-Stories-Program.md`.
- Your blog-writer skill: company blog voice and structure. Customer cards borrow from blog voice but are tighter and self-contained; consult its references if you are unsure about voice or formality calibration.

## Cross-References

- Project plan: `Projects/Customer-Stories-Program.md`
- Source corpus: the interview-questions doc (`{INTERVIEW_QUESTIONS_DOC_ID}`)
- Transcript folder: `{TRANSCRIPT_FOLDER_ID}`
