```yaml
doc_type: BP
normative: true
requires:
  - REF-sales-one-pager-product-positioning-v1.0
```

# Sales One-Pager Output Template (v1.0)

**Status:** Active v1.0
**Owner:** {PMM_LEAD}
**Consumers:** Claude AI Agent
**Change control:** PR Review

---

## Global Writing Rules

### Non-negotiables
- Never use em dashes.
- Use active voice.
- Lead with the bottom line.
- Use plain language.
- Avoid jargon, buzzwords, and hype.
- Do not invent facts, metrics, dates, customers, claims, or pricing.
- Keep names, links, and dates exactly as provided.
- Soften risky claims unless the source supports certainty.
- Use approved product names from your style guide, never legacy or internal names.

### Tone
- Professional
- Pragmatic
- Direct
- Slightly positive

### Format rules
- Follow the section order exactly.
- Stay within each section word limit.
- Use parallel structure in bullets.
- If information is missing, write `[Missing]` instead of guessing.
- Only use spacing to divide sections.

---

## Variant A — Prospect-Specific One-Pager

Use when the input focuses on one product for one named prospect. This is the default.

### Header

```
# [Product Name]
```

*Draft for BD use. Legal review required before external distribution.*

### Section 1 — One-Line Summary

**Word limit:** 20 words max.

Subheadline under the product name. Tells the prospect what this product does for them specifically. Write from the prospect's perspective, naming what they gain.

### Section 2 — Product Definition

**Character limit:** 154 characters max.

Framed as a question: `**What is [Product Name]?**` on its own line, followed by a definition sentence. The definition must be plain, literal, and free of marketing framing. 154 characters is a hard cap, not a target.

Example format:
```
**What is [Product Name]?**
Transfers that protect sensitive data while meeting enterprise compliance standards, so operations teams can move work reliably at scale.
```

### Section 3 — Audience

Two parts, in order.

**Part A — Audience summary.** 40 words max. Who at the prospect organization should read this and what decision it supports.

**Part B — Use-case lines.** 2-4 lines. Each line follows the format:

```
USE_CASE_NAME - one line about the outcome this audience gets
```

- `USE_CASE_NAME` is the audience segment or use case in SMALL CAPS or ALL CAPS (e.g., `OPERATIONS TEAMS`, `TREASURY`, `GLOBAL PAYROLL`).
- Separator is ` - ` (hyphen surrounded by spaces). Never an em dash.
- The outcome line is **68 characters max**, including the separator and use-case name. Hard cap.
- Each line is a standalone outcome statement, not a feature description.

### Section 4 — Hero Copy

**Word limit:** 80 words max.

The bold product name as a heading, then 3-4 sentences that:
1. State the value proposition in the prospect's context
2. Explain what changes for the prospect's users or operations
3. Connect the product to the prospect's business model

### Section 5 — How It Works / What You Get

**Heading resolution rule:**
- Use **"How it works"** when the product is a single feature or capability and the blocks below describe mechanics. Default for single-product one-pagers.
- Use **"What you get"** when the product is a bundle or stack and the blocks describe deliverable components.

If ambiguous, default to "How it works."

2-4 blocks. Each block:
- **Heading:** What the user or prospect sees (not the technical mechanism)
- **Body:** 50 words max per block
- **Focus:** Frame around the prospect's stated needs

Include `[Review]` tags on any claims that need legal or product confirmation.

### Section 6 — Implementation Timeline

**Optional.** Include only if the source material supports specific phases.

Format: `**Weeks N-M:** description of phase.`

Keep to 3-4 phases max.

### Section 7 — Product Layers

**Optional.** 2-4 product layers, each with:
- **Heading:** Layer name (user-facing framing)
- **Body:** 30 words max
- **Bullets:** 1-2 supporting details

Use when the product has distinct functional layers worth explaining separately.

### Section 8 — Why [Product] Fits [Prospect]

3-4 subsections, each with:
- **Heading:** The fit dimension (e.g., "White-Labeled by Design")
- **Body:** 30 words max

Connect product capabilities to the prospect's specific situation.

### Section 9 — CTA

**Word limit:** 20 words max.

Name and email of the BD contact. Format: `Email [Name] at [email]`

### Section 10 — Required Footer Disclaimer

Always include your company's approved disclaimer. Template (replace with your legal team's approved wording):

```
*Certain compliance controls, certifications, and program features are entity-specific
and may be subject to change as the product suite evolves. Regulated services are
provided by {LICENSED_ENTITY}, a licensed entity. {COMPANY} provides the underlying
technology. Regulatory status, certifications, and program features vary by entity
and jurisdiction.*
```

---

## Variant B — Product Comparison One-Pager

Use when the input compares two or more products or positions one product against named
competitors.

### Header

```
# [Comparison Title]
```

### Section 1 — One-Line Summary

**Word limit:** 20 words max.

One sentence positioning the primary product.

### Section 2 — Why [Company Product]

3 bullets, each with:
- **Bold lead:** The differentiator in 5 words
- **Body:** 15 words max explaining the advantage

### Section 3 — At A Glance

Comparison table with columns for each product/competitor being compared.

Rows (adjust to fit):
- Product architecture
- Control model
- Operating model
- End-user experience
- Commercial model

Keep each cell to 15 words max.

### Section 4 — Recommended Positioning

**Word limit:** 60 words max.

Frame the key decision criterion and explain why your product wins on that
criterion. Do not misrepresent competitors.

### Section 5 — Best Fit For

4-5 bullets describing the ideal prospect profile for your product.
Each bullet 12 words max.

### Section 6 — Practical Differentiators

4-5 bullets listing concrete technical or business differentiators.
Each bullet 10 words max.

### Section 7 — Short Close

**Word limit:** 30 words max.

One sentence that reframes the value proposition as a closing statement.
