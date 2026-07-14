# Memory Buildout — Changelog

## v1.0.0

**Initial release.** Packaged from a live memory buildout run on a working
second brain.

**Origin:** The second brain had 3 people profiles, 2 project memory files,
and 1 context file when the agent-roster work started. The memory layer was
thin and didn't support the scheduled + on-demand agents being planned.
Spawned 4 parallel agents to generate 26 people profiles, 5 project memory
files, and 2 context files in ~30 minutes elapsed time.

**What we learned (baked into the skill):**

1. **Group by team affinity.** Alphabetical groupings produced disjointed
   results. Marketing core + execs → one agent. Creative & content team → one
   agent. Etc. Each agent loaded team-specific context once and reused across
   their batch.

2. **Cap concurrency at 4.** Tried 5–6 in early planning; queued and lost
   parallelism. 4 is the sweet spot.

3. **Agents hallucinate completion.** TWO of the agents reported "All N
   profiles created successfully" while writing zero files. Both batches were
   detected by the orchestrator running `ls memory/people/` and counting.
   This is the #1 lesson — the verification step is non-negotiable.

4. **Direct-write fallback works.** Orchestrator wrote 11 missing files
   directly in ~10 min. Faster than retry loops.

5. **Honest gaps > fabrication.** Several profiles had thin Activity Log
   evidence. The agents that flagged `_Needs more context from the user_`
   produced more useful output than agents that filled in interactions to
   make the profile feel complete.

**Patterns surfaced for future hardening:**
- Source-context conflict resolution (e.g., a teammate's name spelled
  differently across docs).
- Extended-team profiles will always be thinner — set expectations.
- Activity Log grep is the highest-yield single input for richness.

**Open items for v1.1:**
- Wire to run telemetry once a telemetry hook is installed.
- Add chat DM history as an additional source signal (when the chat connector
  is stable).
- Add calendar attendee history for "people the user actually meets with
  weekly".
- Audit mode polish: surface stale profiles automatically rather than waiting
  for ad-hoc invocation.
- Possible enhancement: integrate with a feedback-review skill so corrections
  from the user on profiles auto-promote to format guidance.

**Future use cases:**
- Teammate onboarding (smaller scope, ~10 files).
- Additional teammate brains as the rollout expands.
- Quarterly refresh of the user's own memory (catch stale profiles, archive
  departed team members, add new joiners).
