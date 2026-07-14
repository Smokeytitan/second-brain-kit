# Launch Content Multi-Drafter — Workflow

**Version:** 1.0.0
**Skill:** [launch-content-multi-drafter](../SKILL.md)

The four-phase orchestration: ingest → draft → legal pass → optional scaffold.

---

## Step 0 — Setup

1. Resolve launch slug from trigger (e.g., "draft launch comms for pricing update" → slug `pricing-update`).
2. Resolve source: PRD URL, proposal forum post URL, paste, or `Projects/{slug}.md` reference.
3. Confirm output scope (6 default, or subset).
4. Create output folder: `../../Resources/Launch-Content/{launch-slug}-YYYY-MM-DD/`.

---

## Step 1 — Source ingestion + context loading

### 1a. Pull the source
- If URL: web fetch. If X/Twitter, ask the user to paste (X blocks scrape).
- If paste: capture as-is.
- If `Projects/{slug}.md`: load the running file.
- Save raw source to `{output-folder}/{slug}-source.md`.

### 1b. Load launch context
- Check `../../memory/projects/{slug}.md` for deep context.
- Check `../../memory/decisions.md` for strategic decisions touching this launch.
- Check `../../memory/context/legal-flags.md` for compliance triggers specific to this launch.

### 1c. Identify launch tier (per `REF-launch-content-output-spec-v1.0.md`)
- **T1 (Critical):** full GTM, all 6 channels mandatory, longer drafts, founder/exec voice optional.
- **T2 (High):** all 6 channels, standard sizing.
- **T3 (Standard):** smaller drafts, may skip the secondary handle, marketing brief is shorter.

Default to T2 if ambiguous. Note tier choice in `{slug}-legal-pass-notes.md`.

### 1d. Extract canonical facts
From the source, pull:
- Headline / hook (1 sentence)
- Key proof points (3-5 bullets)
- Numerical claims (each tagged: public-already / new / sensitive)
- Partner names / handles
- CTA links / next steps
- Audience hint (enterprise / power user / general / niche segment)

Save extraction to `{output-folder}/.cache/{slug}-canonical-facts.json`.

---

## Step 2 — Parallel drafting

Spawn or call sub-skills in parallel where possible. Outputs 1-5 (community channels + social handles) all use the same source extraction and channel-specific voice rules.

### 2a. Outputs 1-5 — Community + social channels
**If `community-channel-post-drafter` is installed:** call it with `{source, channels: [reddit, telegram, discord, secondary-handle, main-handle]}`. It produces all 5.

**If not installed:** draft each inline using `../../skills/community-channel-post-drafter/references/REF-channel-voices-v1.0.md` directly.

Save each output:
- `{output-folder}/{slug}-reddit.md`
- `{output-folder}/{slug}-telegram.md`
- `{output-folder}/{slug}-discord.md`
- `{output-folder}/{slug}-secondary-x.md`
- `{output-folder}/{slug}-main-x.md`

### 2b. Output 6 — Marketing brief for {CONTENT_LEAD}

**If `marketing-brief` skill is installed:** call it with the source + extracted canonical facts. It produces the brief in your team's standard template format.

**If not installed:** draft inline using SCQR structure:
- **Situation:** what's happening, market context
- **Complication:** what made this needed (problem statement)
- **Question:** what's the strategic question this launch answers
- **Resolution:** the launch + what it delivers

Plus standard brief sections:
- Audience
- Key messages (3 pillars)
- Channels + cadence
- Approval chain ({MARKETING_LEAD} → {CONTENT_LEAD} → ship)
- Open questions for stakeholder review

Save to: `{output-folder}/{slug}-marketing-brief.md`

---

## Step 3 — Legal pass

For each of the 6 outputs, run guidance-review:

**If `guidance-review` skill is installed:** call it with each output + `memory/context/legal-flags.md` as the guide doc.

**If not installed:** manually scan each output for the trigger list in `legal-flags.md`, e.g.:
- Regulated performance claims (yield/return figures) → flag, do not include
- Headline business metrics (volume/usage/AUM-style numbers) → tag with verification status
- Legally sensitive phrasing → confirm the approved softer wording
- Competitor names → omit unless explicitly in source
- Confidential figures listed in legal-flags.md → DROP entirely
- Deprecated / renamed products → use the approved current naming

Save findings to `{output-folder}/{slug}-legal-pass-notes.md`. Format:

```markdown
# Legal Pass Notes — {launch-slug}

**Date:** YYYY-MM-DD
**Launch tier:** T1 / T2 / T3
**Reviewer:** guidance-review skill (or manual)

## Flags raised

| Output | Flag | Action taken |
|--------|------|--------------|
| reddit | Performance claim mentioned in source | Removed from draft. Replaced with neutral placeholder. Surface to {LEGAL_REVIEWER}. |
| {output} | {flag} | {action} |

## Items needing the user's attention before publishing

- {action item}
- {action item}

## All clear

(if no flags raised: "All 6 outputs cleared legal pass. Safe to ship pending user + {MARKETING_LEAD} + {CONTENT_LEAD} review.")
```

---

## Step 4 — Optional: project-tracker scaffold

If the user said "and scaffold the tracker" or similar:

**If `pmm-launch-scaffolder` is installed:** call it with the launch name + tier. It creates the tracker parent task + all standard subtasks (assignees, dates, tags).

**If not installed:** skip with note: "Tracker scaffolding skipped — `pmm-launch-scaffolder` not installed. Run manually in your project tracker or install the skill."

---

## Step 5 — Output package + notify

Print to chat (concise summary + key flags):

```
✅ Launch content package drafted for {launch-slug}.

Output folder: Resources/Launch-Content/{launch-slug}-YYYY-MM-DD/

Drafts:
1. Reddit ({N} words) → {slug}-reddit.md
2. Telegram ({N} words) → {slug}-telegram.md
3. Discord ({N} words) → {slug}-discord.md
4. Secondary X ({N} chars) → {slug}-secondary-x.md
5. Main X ({N} chars) → {slug}-main-x.md
6. Marketing brief ({N} words) → {slug}-marketing-brief.md

Legal pass:
- {summary of flags or "All clear"}

Tracker scaffold:
- {created N parent task + N subtasks | skipped}

Next steps:
1. Send marketing brief to {MARKETING_LEAD} + {CONTENT_LEAD} for sign-off (per PMM process).
2. Wait for sign-off before publishing channel posts.
3. After sign-off, publish in this order: main handle → personal QT (if requested) → TG + Discord → Reddit → secondary handle.
```

---

## Step 6 — Optional telemetry

If a telemetry emitter is wired:

```yaml
skill: launch-content-multi-drafter
version: 1.0.0
launch_slug: {slug}
launch_tier: T1 | T2 | T3
source_type: prd | proposal-forum-post | internal-doc | paste
outputs_drafted: {count}
sub_skills_used: [community-channel-post-drafter, marketing-brief, guidance-review, pmm-launch-scaffolder]
legal_flags_raised: {count}
tracker_scaffolded: bool
status: success | partial | failed
```

---

## Halt conditions

Halt and notify the user if:
- **Source is empty or unfetchable** AND no paste provided.
- **Launch slug is ambiguous.**
- **`memory/context/legal-flags.md` fails to load.**
- **All 6 outputs fail legal pass** (likely a confidentiality leak in source).
- **Output folder write fails.**

---

## Anti-patterns to avoid

- **Don't re-draft the marketing brief if the source already includes one.** Capture and refine, don't replace.
- **Don't skip the legal pass.** Even "clean" launches sometimes have a buried performance claim or competitor mention.
- **Don't publish-skip the PMM process.** Marketing brief → {MARKETING_LEAD}/{CONTENT_LEAD} → channels. The skill outputs DRAFTS, not published copy.
- **Don't tier-up casually.** T1 is rare — flagship-product launch level. Most launches are T2 or T3.
- **Don't include @everyone or @here in TG/Discord drafts.** Channel-wide pings are reserved for incidents.

---

## Notes

- **Future enhancement:** auto-trigger when a new proposal forum post is detected. Watch your governance/proposal forum.
- **Future enhancement:** integrate with `slack-monitor-scaffolder` to monitor your product working-group channels for "PRD ready" signals from {PRODUCT_LEAD}.
- **Future enhancement:** when scheduled agents are wired, a morning brief can flag "this launch hits today, run launch-content-multi-drafter" reminders.
