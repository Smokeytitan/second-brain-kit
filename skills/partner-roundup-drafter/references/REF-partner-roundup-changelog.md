# Partner Roundup Drafter — Changelog

## v1.0.0

**Initial release.** First specialist agent built from an interview of the user's manual workflow.

**Inspired by:**
- The user's manual weekly flow (~1 hour weekly of cross-source scanning).
- The team's established published post format (`■` bullets).
- The user's existing roundup file structure in `Resources/Digests/`.
- The `comp-intel` skill structure (multi-source aggregation, dedup rules, hallucination guards).

**ROI claim:** ~52 hours/year recovered.

**Sources defined:**
- The partner-scanner Slack channel + owner-DM fallback
- Cross-channel Slack scan (5 channels)
- 2 public metrics dashboards (flagship product + company north-star)

**Dedup rules:**
- Cross-week (compare to last week's file)
- Same-week (collapse duplicate beats)
- Cross-source (scanner vs cross-channel)

**Legal guard wired** to `memory/context/legal-flags.md` — yield claims blocked, TVL/AUM flagged for verification, competitor names dropped, confidential figures dropped, approved product-naming enforced.

**Format:** Long-form single post (Version A) by default, thread (Version B) optional.

**Output destination:** `Resources/Partner-Roundups/YYYY-MM-DD-partner-roundup.md`.

**Open items for v1.1:**
- Wire to run-telemetry once installed.
- Add scheduled task (Friday morning) once a scheduling platform decision is made.
- Audit which Slack channels in cross-channel scan yield high-quality signals after 3+ runs; drop low-yield from scan list.
- Possible enhancement: webhook directly from the scanner bot rather than Slack search.
- Possible enhancement: auto-fetch the partner-scanner spreadsheet for shortlist updates.
