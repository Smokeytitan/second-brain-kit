```yaml
doc_type: RUN
normative: true
requires:
  - BP-linkedin-post-template-v1.0
  - REF-[person-slug]-voice-profile-v1.0
```

# LinkedIn Ghostwriter Workflow (v1.0)

**Status:** Active v1.0
**Owner:** {SKILL_OWNER} — the person on your team who maintains this skill
**Consumers:** Claude AI Agent
**Change control:** PR Review

---

## 0. Overview

This runbook drafts a LinkedIn post in the authentic voice of a named person. It combines
their voice profile (the primary authority on how they sound) with their people-intelligence
profile (supplementary context on priorities and current work) and applies company-level
content standards from the BP template.

---

## Step 0 — Accept Input

Accept three inputs from the user:

1. **Person name** — who the post is written as (required)
2. **Topic or source material** — what the post is about. Valid inputs:
   - Blog post URL or article
   - PRD, launch doc, announcement
   - Talking points or bullet notes
   - Slack thread summary
   - A simple topic description ("the {PRODUCT} launch", "our latest usage milestone")
3. **Post type** (optional) — if specified, use it. Otherwise infer from source material
   per the post type definitions in `BP-linkedin-post-template-v1.0.md`.

If person name is missing, ask: "Who should I write this post as?"
If topic/source is missing, ask: "What should the post be about? Paste source material or describe the topic."

---

## Step 1 — Load Context (Silent)

Do not output during this step.

### 1a. Load person profile

Search `outputs/people/` for the most recent file matching the person's name.
Pattern: `*-[firstname]-[lastname]-profile*.md`

If multiple files exist (e.g., 14-day and 90-day profiles), prefer the longest lookback
window for deeper voice understanding.

If no profile exists, stop and tell the user:
"No people-intelligence profile found for [name] in outputs/people/. Run the people-intelligence
skill first to generate one."

### 1b. Load voice profile

Search `skills/linkedin-ghostwriter/references/` for:
`REF-[firstname]-[lastname]-voice-profile-v*.md`

If no voice profile exists, stop and tell the user:
"No voice profile found for [name]. Create one at
skills/linkedin-ghostwriter/references/REF-[firstname]-[lastname]-voice-profile-v1.0.md
(start from REF-voice-profile-template-v1.0.md)
with voice characteristics, vocabulary, LinkedIn patterns, what-not-to-say table, and at least 2 real LinkedIn posts."

### 1c. Load template

Read `BP-linkedin-post-template-v1.0.md` for platform constraints and content standards.

---

## Step 2 — Load Voice Model (Silent)

Do not output during this step. The voice profile is the primary authority.
The people-intelligence profile provides supplementary context.

### From the voice profile, load:

- **Voice Characteristics:** register, sentence length, energy, directness, specificity
- **Tone Register by Platform:** use the LinkedIn row
- **Vocabulary & Phrasing:** phrases they use, framing patterns
- **LinkedIn-Specific Patterns:** structure, opening, closing, emoji, hashtags, tagging
- **What Matters Right Now:** use to weight emphasis correctly
- **What Not to Say table:** hard guardrails for this person
- **Sample Posts:** the ground truth for voice matching

### From the people-intelligence profile, supplement with:

- **Relationship to topic:** how does this topic connect to what they're working through?
- **Additional vocabulary:** any quoted phrases not yet captured in the voice profile
- **Current priorities:** if the voice profile's "What Matters" section is stale, the
  people-intelligence profile is more recent

---

## Step 3 — Draft

Write the post by layering three sources, in this priority order:

1. **Voice profile** — how the person sounds (overrides everything)
2. **BP template** — platform constraints and content standards (the quality floor)
3. **Source material** — what the post is about (the facts)

### Drafting process:

1. Select structure and post type per voice profile patterns. Fall back to BP post type
   defaults only if the voice profile has no observed pattern for this type.
2. Apply the person's opening, closing, emoji, hashtag, and tagging patterns from their
   voice profile. Fall back to BP person-deferred defaults only where the voice profile
   is silent.
3. Draft the post body using the person's vocabulary, sentence length, and framing patterns.
4. Apply BP content standards: terminology, confidence rules, writing mechanics, banned
   words. These are non-negotiable regardless of person.
5. Stay within BP length constraints.

### Hallucination guards:

- Every factual claim must trace to the provided source material or the person's known
  positions from their profile.
- If a claim can't be sourced, mark it `[needs source]` inline.
- Do not fabricate metrics, quotes, dates, customer names, or performance predictions.
- If the topic has no supporting source material, flag it: `[No source — needs input]`.
- If the person's voice samples don't cover the tone register the draft requires, flag it.

---

## Step 4 — Voice Check

Review the draft against the voice profile's sample posts. Answer internally:

1. **Corporate test:** Could this be mistaken for a comms team post? If yes, rewrite with
   more of the person's natural vocabulary and sentence patterns.
2. **Sentence length test:** Does average sentence length match the voice profile's range?
3. **Energy test:** Does the draft's energy match the person's LinkedIn tone register?
4. **What Not to Say test:** Does the draft contain any phrase from the person's What Not
   to Say table or any BP banned word?
5. **Specificity test:** Does it sound like THIS person, or could any executive have
   written it? If generic, rewrite.

If the draft fails any check, revise before outputting.

---

## Step 5 — Output

Present the final draft in this format:

### LinkedIn Post Draft — [Person Name]

```
[The post text, ready to copy-paste into LinkedIn]
```

**Post type:** [announcement | milestone | perspective | welcome]
**Word count:** [N]
**Source material used:** [Brief description of what was used]

**Flags:**
- [Any unsourced claims marked with [needs source]]
- [Any missing context that would strengthen the post]
- [Link placement recommendation per BP link algorithm rule]

If there are no flags, write: "No flags — draft is fully sourced."

---

## Error Handling

| Condition | Action |
|---|---|
| No profile found for person | Stop. Tell user to run people-intelligence skill first. |
| No voice profile found | Stop. Tell user to create voice profile REF file from the template. |
| Source material is too thin to draft a full post | Draft what you can, flag gaps with `[needs source]`. |
| Person's LinkedIn voice contradicts their Slack voice | Defer to voice profile's LinkedIn tone register. |
| User asks for a topic the person has no known position on | Draft neutrally from source material, flag: "No known position from [person] on this topic." |
