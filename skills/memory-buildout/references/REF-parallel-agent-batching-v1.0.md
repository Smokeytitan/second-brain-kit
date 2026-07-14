# Memory Buildout — Parallel Agent Batching

**Version:** 1.0.0

How to group memory targets into batches for parallel agent runs, plus the
verification pattern that catches hallucinated success.

---

## Why batching matters

A second-brain memory layer has 25–35 files for a typical PMM role. Doing them
sequentially in one agent context: too long, hits limits, and the agent loses
quality on later files. Doing each in its own agent: spawn overhead exceeds
useful work.

Sweet spot: **4 parallel agents, 5–8 files each.**

---

## Grouping rules for people profiles

**Group by team affinity, not alphabetical.** Agents do better when they have
team-specific context loaded once and reused across the batch.

### Default groupings (refine to match your team)

| Group | Rationale | Typical count |
|-------|-----------|---------------|
| Marketing core + execs | Top-of-funnel collaborators + stakeholders. Often have richest Activity Log mentions. | 5–6 |
| Creative & content | Designers, video, content writers. Less Activity Log noise; need more inference from rollout docs. | 4–6 |
| Cross-team partners (product, legal, PR, ops) | High coordination volume, well-documented. | 5–8 |
| Extended team / wave 3 | Lighter Activity Log presence; expect more `_Needs more context_` flags. | 4–6 |

### Filling in your own groupings

<!-- Fill in your own team roster, grouped by affinity. Example shape: -->

| Group | People |
|-------|--------|
| Marketing core + execs | {names} |
| Creative & content | {names} |
| Product + engineering partners | {names} |
| Partners/comms/legal/growth | {names} |

Adjust as the roster changes. For onboarding a teammate's brain,
re-group around THEIR team affinities, not yours.

---

## Concurrency cap

**Hard cap: 4 simultaneous agents.** More than 4 may rate-limit, queue, or
cause timeout cascades. Run additional batches sequentially in waves.

For most full-buildout runs, 4 agents covers the people layer in one wave.

Project memory + context files: spawn ~2 more agents in a second wave (or
sequentially after the people batch verifies).

---

## Verification pattern (the critical step)

### What the calibration run taught us
- 4 people-profile agents spawned.
- All reported "All N profiles created successfully."
- 2 of them had written ZERO files. Both batches were wrapped in confident summaries that mentioned line counts, gaps flagged, and "surprising discoveries" — all fabricated.
- The orchestrator caught this only by running `ls memory/people/` and counting.

### What this means
Agent self-reported success is **unreliable**. The skill must verify file existence.

### Verification procedure

```bash
# After all parallel agents return their summaries:

# 1. List actual files in target dir
cd memory/people/  (or projects/, context/)
ls

# 2. For each batch, compare expected file list vs actual file list
expected=("name1.md" "name2.md" "name3.md" ...)
for file in "${expected[@]}"; do
  if [ -f "$file" ]; then
    echo "✅ $file"
  else
    echo "❌ $file (missing — agent hallucinated)"
  fi
done

# 3. Count hits and misses
```

Print to chat for transparency. Misses trigger Step 4 (reconciliation).

---

## Reconciliation tactics

When verification surfaces missing files:

### Option A: Single retry via re-spawn

Conditions:
- Suspect rate-limit (multiple agents missed in same wave).
- Spawn was less than 5 min ago.

Action:
- Re-spawn the same agent with the same prompt.
- Verify again.
- If still empty, fall back to Option B.

### Option B: Direct write by orchestrator (default)

Conditions:
- Single agent missed (not a system-wide failure).
- Re-spawn already failed once.
- Time pressure.

Action:
- Orchestrator reads the same source files (glossary, Dashboard, Activity Log, existing profiles).
- Orchestrator writes each missing file directly using the Write tool.
- Verify each write.

This is what worked in the calibration run for the two failed batches. Total
reconciliation time: ~10 minutes for 11 files.

### When to escalate to the user

If both Option A and Option B fail (e.g., disk write errors, network issues),
halt and surface to the user. Don't keep retrying.

---

## Spawn message template

For each batch, send a single spawn call with:

```
<launch general-purpose agent>
description: "{Batch name} profiles"
prompt: |
  {full prompt — see RUN-memory-buildout-workflow-prd-v1.0.md Step 2}
```

For multiple batches in one wave: send a single message with multiple agent
calls for parallel execution. Don't serialize.

---

## Anti-patterns to avoid

- **Don't trust agent self-report.** Always verify with `ls`.
- **Don't spawn 5+ at once.** Concurrency cap is 4.
- **Don't group people alphabetically.** Team affinity matters.
- **Don't skip the inventory step.** Spawning blindly produces overlap (multiple agents writing the same person) or miss (everyone assumes someone else has them).
- **Don't bury verification at the end.** Verify each batch as it returns; surface failures immediately.

---

## Cross-References
- **Skill:** [../SKILL.md](../SKILL.md)
- **Workflow:** [RUN-memory-buildout-workflow-prd-v1.0.md](RUN-memory-buildout-workflow-prd-v1.0.md)
- **Templates:** [REF-memory-templates-v1.0.md](REF-memory-templates-v1.0.md)
- **Reference run:** calibration run — 4 parallel agents, 2 hallucinated, both recovered via Option B. ~30 min total elapsed for 33 files.
