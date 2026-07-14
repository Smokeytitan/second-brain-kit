---
name: memory-buildout
version: 1.0.0
description: >
  This skill should be used when the user says "build out memory", "build memory
  for {name}", "refresh memory", "memory buildout", "generate people profiles",
  "generate project memory", "build memory layer", "onboard {name} to second
  brain memory", or any variant asking to populate or refresh the
  `memory/people/`, `memory/projects/`, or `memory/context/` directories.
  Spawns parallel agents to generate per-file outputs from existing source
  context (glossary, Dashboard, Activity Log, CLAUDE.md, existing profiles).
  CRITICAL: includes mandatory file-existence verification step because agents
  hallucinate completion reports — a confirmed failure mode from calibration
  runs.
---

## Role

You are an orchestrator for parallel agent runs that build out memory layer
files in the user's PARA-based second brain. Memory buildout has three flavors:

1. **People profiles** — `memory/people/{name}.md` files. One per teammate.
2. **Project memory** — `memory/projects/{slug}.md` files. Deep context per project (vs the running-state files in `Projects/`).
3. **Context files** — `memory/context/*.md` files. Cross-cutting reference: meetings, stakeholder map, legal flags, company background.

You don't write the files yourself by default. You spawn parallel sub-agents,
verify they actually produced the files, and re-run or fall back to direct
writes when an agent hallucinates completion.

You replace ad-hoc memory-file creation. ROI: ~3 hours of memory work compresses to ~30 min of orchestration + verification.

---

## Trigger

Activate on any variant of:

- "build out memory"
- "memory buildout"
- "build memory layer"
- "generate people profiles"
- "generate project memory"
- "refresh memory"
- "onboard {name} to memory" / "build memory for {name}"
- "audit memory" (gap-finding mode)

**Mode detection:**
- **Full buildout (default):** trigger doesn't specify a person/project → generate all missing people + projects + context files based on glossary + Dashboard + Activity Log.
- **Targeted person:** trigger contains a name → just `memory/people/{name}.md` for that person.
- **Targeted project:** trigger contains a project name → just `memory/projects/{slug}.md` for that project.
- **Refresh / audit mode:** trigger contains "refresh" or "audit" → identify stale or incomplete files, surface gap list, generate fixes for the worst gaps.

---

## Pre-Run Preparation

Before spawning anything, take inventory:

1. List existing files in `memory/people/`, `memory/projects/`, `memory/context/`.
2. Cross-reference against the glossary (`memory/glossary.md`) to find missing people.
3. Cross-reference against `CLAUDE.md` Active Projects + `Dashboard.md` Active Projects to find missing project memory files.
4. Check for `_Needs more context from the user_` flags in existing profiles — those are the audit targets.

Print to chat:
> "Memory inventory: {N} people profiles exist, {M} missing per glossary. {N} project memory files exist, {M} missing per Dashboard. {N} context files exist, {M} planned-but-not-built. Will spawn {agent count} parallel agents to fill gaps. Proceed?"

Wait for confirmation before spawning. (User may want to scope down.)

---

## Knowledge Base

| Priority | File | What it provides |
|---|---|---|
| 1 | `references/RUN-memory-buildout-workflow-prd-v1.0.md` | Step-by-step orchestration including the mandatory verification step |
| 2 | `references/REF-memory-templates-v1.0.md` | Output formats for person / project / context files |
| 3 | `references/REF-parallel-agent-batching-v1.0.md` | How to group people into batches; agent count rules; verification pattern |
| 4 | `../../memory/glossary.md` | Source of truth for who's on the team + nicknames |
| 5 | `../../CLAUDE.md` | Active projects list + the user's profile context |
| 6 | `../../Dashboard.md` | Current state for fresh project memory |
| 7 | `../../ACTIVITY-LOG.md` | Historical context for people interactions and project decisions |
| 8 | 2–3 existing files in `../../memory/people/` (your richest profiles) | Format templates |

---

## Execution

Follow `references/RUN-memory-buildout-workflow-prd-v1.0.md` exactly. The
workflow has three phases:

1. **Inventory + plan** (Steps 0–1): figure out what's missing, group into batches.
2. **Parallel spawn** (Step 2): launch general-purpose agents with explicit prompts.
3. **VERIFY + reconcile** (Steps 3–5): confirm files actually exist, re-run or
   write directly when agents hallucinated success.

**Step 3 (verify) is non-negotiable.** In a calibration run of this skill,
agents reported "All 6 profiles created successfully" while writing zero files.
Skipping verification ships an empty memory layer.

---

## Scope Constraints

- **In scope:** Generating `memory/people/`, `memory/projects/`, `memory/context/` files. Updating cross-references (CLAUDE.md memory index, master catalog).
- **Out of scope:** Writing to `Projects/` running files (those are user-owned). Writing to `Dashboard.md` (also user-owned). Modifying decisions in `memory/decisions.md` without explicit ask.
- **Honesty rule:** When source context is thin for a person/project, write what's verifiable and flag gaps as `_Needs more context from the user_`. Never fabricate interactions or attribute claims that aren't in the Activity Log or glossary.
- **No em dashes** in any output (the user's writing rule — adjust to taste).
- **Concise file format:** ~30–50 lines per person profile, ~50–100 lines per project memory file. Don't bloat.

---

## Output

### Files written
- `memory/people/{name}.md` — one per missing person, format per template.
- `memory/projects/{slug}.md` — one per missing project, format per template.
- `memory/context/{topic}.md` — one per missing context file (meetings, stakeholder-map, legal-flags, etc.).

### Cross-reference updates
- Update CLAUDE.md Memory Index table if new files added.
- Update master catalog to reflect new files.

### Notification
After verification:
```
✅ Memory buildout complete.

People profiles: {N} new, {M} skipped (already exist or no source context).
Project memory: {N} new, {M} skipped.
Context files: {N} new.

Verification: all {total} files written + readable.

Files needing your attention (flagged for thin context):
- {file path} — {what's missing}
- {file path} — {what's missing}

Re-run "audit memory" in 30 days to refresh stale entries.
```

---

## Failure Modes (and how this skill handles them)

### Agents hallucinating completion
**Symptom:** Sub-agent reports "All N files created successfully" but ls shows none of them.
**Mitigation:** Verification step (Step 3 of the workflow) lists actual files. Mismatch triggers either re-spawn (one retry) or direct-write fallback.
**Origin:** In the calibration run, 2 of 6 agents failed this way. Both batches were re-done by the orchestrator writing files directly.

### Source context too thin
**Symptom:** Activity Log has zero mentions of the person; glossary has only role.
**Mitigation:** Generate a skeleton profile with `_Needs more context from the user_` flag, surface in final notification. Never fabricate.

### Conflicting facts across sources
**Symptom:** Glossary spells a teammate's name one way, a rollout doc spells it another.
**Mitigation:** Use the more recent source (newer file mod time wins). Flag the conflict for the user.

### Parallel spawn rate-limited
**Symptom:** Spawning 6+ agents in one message times out or queues.
**Mitigation:** Cap concurrent spawns at 4. Larger jobs run in two waves of 4.

---

## Modes in detail

### Full buildout (default)
For onboarding a NEW second brain or first-pass memory layer. Default to 4 parallel agents. Group people by team affinity (not alphabetical) so agents can share team-specific context.

### Targeted person/project
Skip parallelization — direct write. Faster for one-off updates.

### Refresh / audit mode
1. Read every existing memory file.
2. For each, score freshness (last update date), completeness (no `_Needs more context_` flags), and relevance (still in active set).
3. Surface a punch list: stale, incomplete, orphaned.
4. Ask the user which to fix; spawn agents only for confirmed targets.

---

## Changelog
See `references/REF-memory-buildout-changelog.md`.
