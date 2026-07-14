# Launch Content Multi-Drafter — Changelog

## v1.0.0

**Initial release.** Built the day after `community-channel-post-drafter` shipped, as the orchestrator that wraps it for full product/proposal launches.

**Origin:** From a workflow interview, the user described their content production flow: "a PRD from the product lead turns into 6 parallel outputs: Reddit, Telegram, Discord, secondary handle, main handle, marketing brief for the content lead." The interview synthesis sharpened this into the Launch Content Multi-Drafter spec — single-input → 6 parallel outputs with optional project-tracker scaffolding and legal pass.

**Key architectural choices:**
- **Orchestrator pattern.** Calls `community-channel-post-drafter` for outputs 1-5, calls `marketing-brief` for output 6, calls `guidance-review` for legal pass, optionally calls `pmm-launch-scaffolder` for the project tracker.
- **Tier-aware sizing.** T1/T2/T3 launch tiers drive output depth. Most launches are T2.
- **PMM process discipline.** Outputs are DRAFTS — the user reviews, {MARKETING_LEAD} + {CONTENT_LEAD} sign off the marketing brief, then channel posts ship.
- **File-based output.** All 6 outputs save to `Resources/Launch-Content/{slug}-YYYY-MM-DD/` for handoff + archival.

**Sub-skill dependencies (graceful fallback for each):**
- `community-channel-post-drafter` (for outputs 1-5) → fallback: inline drafting using channel-voices reference.
- `marketing-brief` (for output 6) → fallback: inline SCQR template.
- `guidance-review` (for legal pass) → fallback: manual scan against `legal-flags.md`.
- `pmm-launch-scaffolder` (for the tracker) → graceful skip with note.

**Optional add-ons:**
- Employee amplification request
- Personal-account QT
- Press release angle for {PR_LEAD}
- Creator program brief for {CREATOR_PROGRAM_LEAD}

**Legal guards** wired to `memory/context/legal-flags.md` — regulated performance claims blocked, headline-metric verification, confidential-figure blocks, approved product naming.

**Open items for v1.1:**
- Wire to telemetry emitter once installed.
- Auto-detection of new proposal forum posts to suggest a run.
- Integration with `slack-monitor-scaffolder` for "PRD ready" signal monitoring on product working-group channels.
- Once scheduled agents run, a morning brief should flag launches hitting today and remind to run this skill.
- Run on the next real launch when it ships — first real production run for this skill.

**Known limitations:**
- Doesn't currently support LinkedIn output (use a dedicated LinkedIn ghostwriting skill separately).
- No retry logic on sub-skill failures (will note in legal-pass-notes if a sub-skill flaked).
- Doesn't auto-generate founder/exec voice — still requires the user to invoke exec voice skills separately for exec posts.
