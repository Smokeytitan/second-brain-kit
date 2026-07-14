# Memory Buildout — Workflow

**Version:** 1.0.0
**Skill:** [memory-buildout](../SKILL.md)

The orchestration flow for parallel-agent memory file generation, with
mandatory verification.

---

## Step 0 — Inventory

List existing files:

```bash
ls memory/people/
ls memory/projects/
ls memory/context/
```

Cross-reference against:
- **`memory/glossary.md`** — full team list (with nicknames). Anyone in the glossary without a `memory/people/{slug}.md` is a target.
- **`CLAUDE.md`** Active Projects table — any project without `memory/projects/{slug}.md` is a target.
- **`Dashboard.md`** Active Projects + Active Areas — supplementary list.
- **Memory Index in CLAUDE.md** — any file marked "to be built" is a context-file target.

Build the gap list. Print to chat:

```
Memory inventory:
- People: {N} exist, {M} missing → [list of missing names]
- Projects: {N} exist, {M} missing → [list]
- Context: {N} exist, {M} planned → [list]

Will spawn {agent_count} parallel agents to fill gaps. Proceed?
```

Wait for confirmation. The user may want to scope down.

---

## Step 1 — Batch the work

Group missing files into batches that fit one agent's context window
efficiently. Use [REF-parallel-agent-batching-v1.0.md](REF-parallel-agent-batching-v1.0.md) for the rules.

**Default groupings** (refine based on your glossary):
- Marketing core + execs (5–6 people)
- Creative & content (4–6 people)
- Product + engineering partners (5–6 people)
- Partners/comms/legal/growth (6–8 people)

Cap at 4 agents per spawn wave. If gap list exceeds 4 batches, run sequential
waves.

For project memory + context files: typically one agent each (smaller volume,
each file needs more depth).

Save the batch plan to `.cache/{date}-memory-buildout-plan.json` with each
agent's assigned scope so verification can compare expected vs actual.

---

## Step 2 — Spawn parallel agents

For each batch, spawn a `general-purpose` agent with this prompt structure:

```
You are generating people profiles for {USER_NAME}'s second brain
({USER_ROLE} at {COMPANY}).

**Your scope:** Create {N} markdown files for these people in
`{SECOND_BRAIN_PATH}/memory/people/`:

1. `{slug-1}.md` — {one-line role context}
2. `{slug-2}.md` — {one-line role context}
{...}

**Required inputs (read in order):**
- CLAUDE.md (the user's profile)
- memory/glossary.md (roles + channels)
- 2 existing files in memory/people/ (format templates)
- Dashboard.md (current context)
- ACTIVITY-LOG.md (grep each name for interactions)

**Output format per file** (~30-50 lines, match existing profile style):
{paste template from REF-memory-templates-v1.0.md}

**Writing rules:**
- No em dashes. Use colons, commas, semicolons, or separate sentences.
- Concise and direct.
- Pull actual evidence from the Activity Log and Dashboard. Don't invent interactions.
- If you can't find enough context, write what you know and flag missing info as `_Needs more context from the user_`. Do not fabricate.
- Cross-reference other profiles when relevant.

Create all {N} files. **CRITICAL:** Use the Write tool to actually write each
file to disk. Confirm with Read or Bash that each file exists before reporting.
Do not report success unless every file is verifiable.

Report a short summary (under 100 words) when done: which files you created,
any gaps you flagged, and any surprising discoveries from the Activity Log.
```

For project memory and context-file agents: similar structure but with the
appropriate template from REF-memory-templates-v1.0.md.

Send all spawn calls in a single message (parallel execution). Capture the
agent IDs in the cache file for traceability.

---

## Step 3 — VERIFY (mandatory, non-negotiable)

**This step exists because agents hallucinate completion.** In the
calibration run of this skill, 2 of 6 agents reported "All N files created
successfully" while writing zero files. Skipping verification ships an empty
memory layer.

For each batch:

```bash
# List actual files in the target directory
ls memory/people/  (or memory/projects/, memory/context/)

# Cross-reference against expected files from the cache plan
```

Compare expected vs. actual:
- **Match:** mark batch as `verified: true`.
- **Partial:** some files written, some missing. Mark `verified: partial` and list missing.
- **Empty:** zero files written. Mark `verified: false` (full hallucination).

**Print to chat:**
```
Verification results:
- Batch A (Marketing core + execs): ✅ 6/6 files verified
- Batch B (Creative & content): ❌ 0/6 files verified — agent hallucinated success
- Batch C (Product + eng partners): ✅ 6/6 files verified
- Batch D (Partners/comms/legal/growth): ✅ 8/8 files verified
- Project memory batch: ❌ 0/5 files verified — agent hallucinated success
- Context files batch: ✅ 2/2 files verified
```

---

## Step 4 — Reconcile failures

For each batch with `verified: partial` or `verified: false`:

**Option A — single retry via re-spawn** (use ONLY if rate-limit was suspected):
- Re-spawn the same agent with the same prompt.
- Verify again.
- If still fails, fall back to Option B.

**Option B — direct write by orchestrator** (default for hallucination failures):
- Read the same source files the agent should have read.
- Write each file directly using the Write tool.
- Verify after each write that the file exists.

The calibration run used Option B for both failed batches. Total time to
reconcile: ~10 min direct work vs. unbounded retry loops.

After reconciliation, re-run Step 3 verification.

---

## Step 5 — Update cross-references

Once all batches verified:

1. **CLAUDE.md Memory Index table** — update entries for new files. Mark previously "to be built" entries as built (with the build date).
2. **Master catalog document** (if you keep one) — refresh the Memory section to reflect new files.
3. **Memory file cross-references** — each new file should link back to: Dashboard, Activity Log, decisions log, and any related profiles or projects.

---

## Step 6 — Notify

```
✅ Memory buildout complete.

People profiles: {N} new (verified), {M} skipped (already exist or no source context).
Project memory: {N} new (verified), {M} skipped.
Context files: {N} new (verified).

Total files written: {total}.
Hallucination failures recovered: {count} (orchestrator wrote directly).

Files needing the user's attention (flagged for thin context):
- {file path} — {what's missing}
- {file path} — {what's missing}

Cross-references updated:
- CLAUDE.md Memory Index → {N entries}
- Master catalog → {N entries}

Re-run "audit memory" in ~30 days to refresh stale entries.
```

---

## Step 7 — Emit run telemetry (if a telemetry hook is wired)

```yaml
skill: memory-buildout
version: 1.0.0
mode: full | targeted | audit
date: YYYY-MM-DD
batches_planned: {N}
agents_spawned: {N}
verification_passes: {N}
verification_failures_recovered: {N}
files_written_total: {N}
flagged_thin_context: {count}
status: success | partial | failed
```

---

## Halt Conditions

Halt and notify the user if:
- **Glossary file unreadable** (Step 0): can't determine targets.
- **All agents fail in a batch** AND Option B direct-write also fails (e.g., disk write errors).
- **Source context completely empty** for >50% of targets (means the buildout will be all-skeletons; not worth running).
- **User cancellation** at the Step 0 confirmation prompt.

---

## Notes

- **Always read the existing format examples first.** Your 2–3 richest existing profiles set the bar. Don't reinvent the format per run.
- **Activity Log grep is the highest-yield input.** Most "rich" profile content comes from grepping the person's name in the Activity Log and pulling 3–5 recent interactions.
- **Be honest about gaps.** A 20-line skeleton with `_Needs more context from the user_` flags is more useful than a 50-line fabrication.
- **Future enhancement:** auto-generate from chat DMs / calendar attendees as additional source signals once those connectors are stable.
