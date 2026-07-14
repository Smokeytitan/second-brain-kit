# Agents

Specs for the recurring, automated side of the system. Three kinds:

- **scheduled/** — workflows that run on a cron cadence (weekly release radar, daily digests, channel monitors). Each spec defines owner, cadence, sources, method, output format, and rules. The matching skill (e.g. `skills/release-radar/`, `skills/slack-monitor-scaffolder/`) does the setup; the spec here is the contract for what each run must do.
- **specialists/** — trigger-driven agents invoked by name for a job (drafters, reviewers, scrapers). Most of these live as skills; keep a roster here of what exists and when to use which.
- **workflows/** — multi-skill pipelines (e.g. launch content: brief → drafts → legal pass → scaffold tasks). Document the order and handoffs.

## Building your roster

Don't guess what agents you need — interview for it. Run `skills/interview-agent/` on yourself: it captures how you actually work, then synthesizes a prioritized agent roster and scheduled-task specs. Re-run it quarterly; kill agents that stopped earning their keep.

## Spec template (scheduled)

See `scheduled/_template.md`. The non-negotiables learned the hard way:

1. **Name every source with a stable ID** (doc ID, list ID, channel ID) — "check the release calendar" rots; `{LIST_ID}` doesn't.
2. **State which source is the primary truth** when sources disagree.
3. **Define the output format exactly** (sections, in order) so runs are comparable week to week.
4. **Verify file-writes actually happened** — agents report success optimistically.
5. **Deliver to where you already look** (a DM, a dated file in Resources/), not a new place.
