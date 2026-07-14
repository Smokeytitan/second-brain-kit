```yaml
doc_type: REF
normative: false
requires: []
```

# Status Update — Changelog

---

## v1.3.0

**Built-in Priorities, built-in AI Items Shipped from the tracker, and cursor-driven auto-mode windows.**

- Added a "Current Priorities" header block sourced from the team's ClickUp Priorities doc via the `collect_clickup_priorities` collector (no URL needed; auto-runs when `CLICKUP_API_TOKEN` is configured).
- Added an "AI Items Shipped" subsection sourced from `state/work/tasks/*.yaml` with `status=done` in the window, separate from the main Accomplished list so AI/system work stays visible alongside primary-discipline execution.
- Changed default window resolution to auto-mode via `state/status-update/last-run.json`: the collector picks up from the cursor, falls back to 7 days if missing or malformed, and writes the cursor on successful auto-mode runs. Manual `--start-date`/`--end-date` still work and do not touch the cursor.
- Consolidated collector invocation to a single call per run; Drive and ClickUp list URLs are passed as optional enrichment on the same call.
- Rewired authority split: ClickUp is canonical for primary-discipline work; `state/work/` is canonical for AI/system shipped items.
- Bumped all three output format templates (Detailed / Executive / Slack) to include Current Priorities and AI Items Shipped. Empty sections are omitted.

## v1.1.0

**Optional Drive notes and ClickUp enrichment.**

- Added repo-native enrichment guidance for optional Google Doc weekly notes and ClickUp list/view URLs
- Defined `scripts/utilities/status_update_sources.py` as the read-only collector for Drive and ClickUp evidence
- Expanded synthesis rules so Slack, Gmail, GitHub, Drive notes, and ClickUp are treated as co-equal evidence streams
- Added conflict-resolution rules for deduping overlapping items across sources

## v1.0.0

**Initial governance-complete release.**

- Added `version: 1.0.0` to `SKILL.md`
- Registered the skill as a standalone status/accomplishment summarizer
- Defined the current three-source workflow: Slack, Gmail, and GitHub
- Defined the three required outputs: detailed report, executive summary, and Slack-ready update
