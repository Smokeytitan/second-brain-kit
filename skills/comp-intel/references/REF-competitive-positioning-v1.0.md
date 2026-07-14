```yaml
doc_type: REF
normative: false
requires: []
```

# Competitive Positioning — Counter-Claims (v1.0)

**Status:** Template — seed from your first comp-intel run
**Owner:** {SKILL_OWNER} — a human maintains counter-claims manually
**Consumers:** Claude AI Agent (comp-intel skill, Step 4a gap analysis)
**Change control:** The human owner maintains counter-claims manually. The comp-intel run
  reads this file and flags `[COUNTER NEEDED]` when a competitor claim has no entry. The run
  never overwrites existing counters without explicit instruction.

---

## 0. How to Use This File

**AI reads this file during Step 4a gap analysis.** For each competitor claim found in Step 2
web research, check whether this file contains a counter:

- **STRONG** — you have a specific published counter in §§2–5 for this claim (no `[DRAFT]` tag)
- **WEAK** — Counter exists but marked `[DRAFT — NEEDS REVIEW]` or `[COUNTER NEEDED]`
- **MISSING** — No entry in §§2–5 for this competitor or claim; file gap row in battlecard-gaps.md

When a new competitor claim is found with no entry here:
1. File the gap row as usual
2. Add a `[COUNTER NEEDED]` note to the relevant §§2–5 section

**Do NOT overwrite any existing counter without explicit instruction from the owner.**

Cross-references:
- Product positioning (your messaging arcs): {POSITIONING_DOC} — wherever your approved positioning lives
- Persona (buyer groups): {PERSONA_DOC} — your buyer-segment doc
- Competitor registry: the registry file for this mode

---

## 1. Your Competitive Claims (what you CAN say today)

These are published, verifiable claims from your product positioning doc. Use as evidence
when assessing whether a counter exists. Group them by messaging arc, and mark
content holds explicitly:

### Arc 1 — {ARC_1_NAME} (shipped)
- [claim 1 — capability + ship date]
- [claim 2]

### Arc 2 — {ARC_2_NAME} (content hold — upcoming)
- [claim — target date]
- **Content hold:** Do not pre-announce. Do not cite as published capability. Note as "upcoming" in internal contexts only.

### Arc 3 — {ARC_3_NAME} (mixed — some shipped, some content hold)
- [shipped claim + date]
- [feature-flagged or contract-pending claim; **content hold** + the unblocking condition]

### Not yet claimable
- [capability the exec has prioritized but that is not shipped or announced]
- [known evaluation friction points, e.g., "no published pricing page"]

Also list here the positioning TERMS you are actively claiming (the arc headline
phrases) — Step 2f scans competitor narratives for these terms to detect
narrative conflicts.

---

## 2. Versus {COMPETITOR_A}

**Their primary claim:**
[Their headline positioning, in their language, with scale proof points they cite
and any acquirer/backer framing.]

**You win on:**
- [Architecture/model-level differentiator: the structural difference, not a feature checkbox]
- [Enforcement/control differentiator: where your guarantees live vs where theirs live]
- [Unique capability with no competitor equivalent — state it precisely]
- [Full-stack / fewer-vendors argument, if applicable]
- [Integration path to an adjacent product line, if applicable]

**You concede:**
- [Capability they have that you lack. `[COUNTER NEEDED — contingent on X shipping]`]
- [Pricing transparency or other evaluation-friction concession. `[COUNTER NEEDED — requires a decision]`]
- [Scale/install-base comparisons you should not engage on — note the reframe instead]
- [Certifications to verify before countering]

**Reframe:**
[2-4 sentences. Do not fight their strongest story head-on. Name the sharper
contrast that favors your model, and the buyer situation where it matters.]

**Killer question:**
"[The one question a seller can ask a prospect that exposes the difference.]"

**Key BD signals:**
- [dated, sourced deal signals involving this competitor]
- [installed-base / migration-source estimates with their provenance]

---

## 3. Versus {COMPETITOR_B}

[Same structure: Their primary claim / You win on / You concede / Reframe /
Killer question / Key BD signals.]

---

## 4. Versus {COMPETITOR_C}

`[DRAFT — NEEDS REVIEW]`

[Same structure. Keep the draft tag until a human validates the counters.]

---

## 5. Versus {COMPETITOR_D}

`[DRAFT — NEEDS REVIEW]`

[Same structure.]

---

## 6. Monitor Competitors (no active counter needed yet)

### {MONITOR_COMPETITOR_1}
[2-3 sentences: who they are, why they are only monitored.]
**Counter trigger:** File counter if they appear in a BD deal signal or ship a major feature.

### {MONITOR_COMPETITOR_2}
[Same.]
**Counter trigger:** File counter if they appear in an enterprise deal where you compete,
or if they ship into your priority category.

---

## Changelog

### v1.0
- Initial template. Seed from your first comp-intel run: all counters start
  `[DRAFT — NEEDS REVIEW]`; the human owner removes the tag as counters are
  confirmed. Track known `[COUNTER NEEDED]` items (unshipped capabilities,
  pricing-page decisions, compliance credentials to verify) at the bottom of
  each section.
