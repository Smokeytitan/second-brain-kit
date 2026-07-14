```yaml
doc_type: RUN
normative: true
requires:
  - BP-sales-one-pager-output-template-v1.0
  - REF-sales-one-pager-product-positioning-v1.0
  - REF-sales-one-pager-reviewer-v1.0
```

# Sales One-Pager Workflow (v1.0)

**Status:** Active v1.0
**Owner:** {PMM_LEAD}
**Consumers:** Claude AI Agent
**Change control:** PR Review

---

## 0. Overview

This runbook fills a sales one-pager template from source material. It is a template-fill
workflow. Output is strictly constrained by the template and the provided inputs.

Do not research, synthesize beyond the source material, or invent facts. Missing inputs are
flagged with `[Missing]`.

---

## Step 0 — Accept Input

Accept the source material from the user. Valid inputs include:
- Prospect name and context
- Product docs, PRDs, or plan files
- Call notes, meeting notes, Slack threads
- Existing briefs, decks, or positioning docs
- Raw notes, brainstorm docs, or any combination

Required minimum:
- **Prospect name** (or "General" for reusable material)
- **Product context** (which product(s) from the BD taxonomy)

If no input is provided, ask: "Who is the prospect, which product, and paste any notes or
docs you want me to work from."

---

## Step 1 — Load Context (Silent)

Load all reference files silently. Do not output during this step.

1. Read `BP-sales-one-pager-output-template-v1.0.md` — template variants, section order,
   word limits, and writing rules.
2. Read `REF-sales-one-pager-product-positioning-v1.0.md` — per-product positioning,
   differentiators, audience, and competitive context.

---

## Step 2 — Analyze (Silent)

Work through the source material silently. Do not output during this step.

### 2a. Identify the product

Map the input to one or more products from your BD taxonomy (defined in
`REF-sales-one-pager-product-positioning-v1.0.md`):
- **P1:** {PRODUCT_1} — [short descriptor]
- **P2:** {PRODUCT_2} — [short descriptor]
- **P3:** {PRODUCT_3} — [short descriptor]
- **P4:** {PRODUCT_4} — [short descriptor]
- **P5:** {PRODUCT_5} — [full-stack / bundle offering]

If the input names a specific product, use it. If ambiguous, infer from context. If truly
unclear, ask the user.

### 2b. Identify the prospect context

Extract from input:
- Prospect name and industry
- Use case or problem they are solving
- Region or market if mentioned
- Any specific requirements, objections, or decision criteria

### 2c. Select template variant

Choose based on the input:
- **Variant A (Prospect-Specific):** Use when the input focuses on one product for one
  named prospect. This is the default.
- **Variant B (Comparison):** Use when the input compares two or more products or positions
  one product against named competitors.

If the user explicitly requests a variant, use that. Otherwise, default to Variant A.

### 2d. Identify key differentiators

From the product positioning reference, select the 3-4 differentiators most relevant to
this prospect's use case and industry.

### 2e. Identify missing fields

For each template section, check whether the source material provides enough to fill it.
If a required field cannot be filled from source material, mark it `[Missing]` in the output.

---

## Step 3 — Fill Template

Fill every section of the selected template variant in exact section order, following all
rules from `BP-sales-one-pager-output-template-v1.0.md`.

### Prospect-specific rules (Variant A)

- Personalize the hero copy to the prospect's use case and industry
- Frame "What You Get" sections around the prospect's stated needs
- Write "Why [Product] Fits [Prospect]" bullets that address their specific situation
- Include implementation timeline only if source material supports it
- Use the prospect's language and framing where possible

### Comparison rules (Variant B)

- Keep the comparison table factual and balanced
- Lead with your product's strengths without misrepresenting competitors
- Frame positioning around the prospect's decision criteria
- Use "Best Fit For" to help the reader self-select

### Writing rules (enforced throughout)

- Use only facts from the source material and the product positioning reference
- Do not invent metrics, dates, claims, customers, or pricing
- Write in plain language. Use active voice.
- Never use em dashes.
- Lead with the bottom line.
- Avoid fluff, jargon, and repetition.
- Respect every section word limit.
- Use approved product names from your style guide, never legacy or internal names.

---

## Step 3.5 — Internal Review

Run the draft through the reviewer defined in `REF-sales-one-pager-reviewer-v1.0.md`
and capture the verdict and top 3 must-fix issues.

**Do not auto-rewrite the draft.** No verdict triggers silent edits. The reviewer
returns findings; the user decides what to apply.

- **Green:** Proceed to Step 4 with the review header and no must-fix list.
- **Yellow:** Proceed to Step 4. Include the top 3 must-fix issues in the header.
- **Red:** Proceed to Step 4. Include the full must-fix list in the header. Do not
  re-run the review or rewrite the draft.

The draft surfaced in Step 4 is the exact draft produced in Step 3, unchanged.

---

## Step 4 — Return Output

Return the review header followed by the unmodified Step 3 draft.

Review header format:
```
**Review:** [Green / Yellow / Red] — [one sentence on overall quality]
**Must-fix:**
- [issue 1, with section reference]
- [issue 2, with section reference]
- [issue 3, with section reference]
---
```

If the verdict is Green, omit the `**Must-fix:**` block. The header is then just the
verdict line plus the separator.

Then the full one-pager with no other preamble or commentary.

- No preamble, no commentary, no "here's your one-pager" intro.
- No task lists, project management steps, or execution checklists.
- One one-pager per prospect-product combination.
- Include the required footer disclaimer.

---

## Edit Handling

If the user requests edits after the one-pager is returned:
- Preserve the template structure unless explicitly asked to change it.
- Apply the edit to the specific section(s) mentioned.
- Return the full updated one-pager, not just the changed section.

---

## Error Handling

| Situation | Action |
|---|---|
| No source material provided | Ask for prospect, product, and input material. Do not proceed until received. |
| Source material too vague to fill any section | Fill what you can, mark the rest `[Missing]`. |
| Product unclear from input | Ask the user to confirm which product (P1-P5). |
| Multiple prospects in one input | Create one one-pager per prospect. |
| User asks for research or external data | Decline. This skill uses source material and the positioning reference only. |
| User asks to add sections not in template | Decline unless explicitly instructed to modify the template. |
