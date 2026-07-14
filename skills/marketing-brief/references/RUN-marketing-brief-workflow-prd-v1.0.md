```yaml
doc_type: RUN
normative: true
requires:
  - BP-marketing-brief-template-impl-v1.0
  - REF-marketing-brief-source-priority-v1.0
  - REF-marketing-brief-launch-tiers-v1.0
```

# Marketing Brief Workflow (v1.0)

**Status:** Active v1.0
**Owner:** {PMM_LEAD}
**Consumers:** Claude AI Agent
**Change control:** PR Review

---

## 0. Overview

This runbook fills the marketing brief template from source material. It is a template-fill
workflow. Output is strictly constrained by the template and the provided inputs.

Do not research, synthesize beyond the source material, or invent facts. Missing inputs are
flagged with `[Missing]`.

---

## Step 0 -- Accept Input

Accept the source material from the user. Valid inputs include:
- PRDs or product specs
- Launch plans or campaign briefs
- Strategy docs, messaging docs, positioning docs
- Customer notes, partner notes, sales notes
- Meeting notes, brainstorm docs, Slack summaries
- Any combination of the above

If no input is provided, ask: "Paste the PRD, launch doc, or notes you want me to brief."

**Multi-launch detection:** Scan the input for multiple distinct launches. If detected,
inform the user: "I see N separate launches in this input. I'll create one brief per launch."
Process each launch as a separate brief using the same workflow.

---

## Step 1 -- Load Context (Silent)

Load all reference files silently. Do not output during this step.

1. Read `BP-marketing-brief-template-impl-v1.0.md` -- the template with section order, word
   limits, and global writing rules.
2. Read `REF-marketing-brief-source-priority-v1.0.md` -- the source conflict resolution rules.
3. Read `REF-marketing-brief-launch-tiers-v1.0.md` -- the tier classification framework.

---

## Step 2 -- Analyze

Work through the source material silently. Do not output during this step.

### 2a. Classify the launch tier

Using `REF-marketing-brief-launch-tiers-v1.0.md`, determine the tier:
- **Tier 1:** Market-expanding alliances. High complexity, long timeline, quantitative research needed.
- **Tier 2:** Brand awareness and ecosystem growth. Moderate complexity, may need new supporting docs.
- **Tier 3:** Brand visibility and credibility. Low complexity, short turnaround, work from existing docs.

If the tier is ambiguous, default to Tier 2. If the source explicitly states a tier, use it.

### 2b. Resolve source conflicts

Apply the rules in `REF-marketing-brief-source-priority-v1.0.md`:
- Product facts: PRD wins.
- Launch timing: approved launch plan wins.
- Messaging: approved messaging doc wins.
- Tier: tiering framework wins unless leadership explicitly overrides.
- Customer examples: approved customer notes or approved brief wins.
- Metrics and claims: only use if the source states them clearly.

Do not merge conflicting claims. Use the highest-priority source that directly answers
each section.

### 2c. Identify missing fields

For each template section, check whether the source material provides enough to fill it.
If a required field cannot be filled from source material, mark it `[Missing]` in the output.

---

## Step 3 -- Fill Template

Fill every section of the template in exact section order, following all rules from
`BP-marketing-brief-template-impl-v1.0.md`.

### Section-by-section rules

**Section 1 -- Brief Info:**
- Each field 8 words max.
- Launch name describes the user outcome, not the mechanism. 6 words max.
- Do not add strategy or messaging here.

**Section 2 -- Launch Summary:**
- 50 words max.
- 2 to 3 sentences answering: what changes for users, who is affected, what enables it.

**Section 3 -- Audience and Problem:**
- Primary and secondary audience: 5 words max each.
- Core problem: 18 words max.
- Why now: 18 words max.
- State one core problem. Keep each line distinct.

**Section 4 -- Launch Scope and Value:**
- Scope: 20 words max.
- Customer value: 18 words max.
- Proof points: 10 words max each. Use facts only.
- Focus on this launch only.

**Section 5 -- Messaging:**
- Topline message: 15 words max.
- Support points: 10 words max each.
- CTA: 8 words max.
- Do not restate proof points word for word.

**Section 6 -- Distribution:**
- Keep high level. Name channels, not tasks.
- Put detailed execution in your project tracker.

**Section 7 -- Success:**
- Goals: 10 words max each.
- Use measurable outcomes.
- 1 to 3 goals tied to this launch.

### Writing rules (enforced throughout)

- Follow the template section order exactly.
- Respect every section word limit.
- Use only facts from the source material.
- Do not invent metrics, dates, claims, customers, positioning, or proof points.
- Write in plain language. Use active voice.
- Never use em dashes.
- Lead with the bottom line.
- Avoid fluff, jargon, and repetition.
- Use approved product names from your style guide, never legacy or internal names.

---

## Step 4 -- Return Output

Return the final brief only.

- No preamble, no commentary, no "here's your brief" intro.
- No task lists, project management steps, or execution checklists.
- One brief per launch. If multiple launches, return them sequentially with a clear separator.

---

## Edit Handling

If the user requests edits after the brief is returned:
- Preserve the template structure unless explicitly asked to change it.
- Apply the edit to the specific section(s) mentioned.
- Return the full updated brief, not just the changed section.

---

## Error Handling

| Situation | Action |
|---|---|
| No source material provided | Ask for input. Do not proceed until received. |
| Source material is too vague to fill any section | Fill what you can, mark the rest `[Missing]`. |
| Source material conflicts across documents | Apply source priority rules. Use the winner. Do not merge. |
| Multiple launches in one input | Split and create one brief per launch. |
| User asks to add sections not in template | Decline unless explicitly instructed to modify the template. |
| User asks for research or external data | Decline. This skill uses source material only. |
