# Community Channel Post Drafter — Changelog

## v1.0.0

**Initial release.** Packaged from live drafting work for a major partner announcement.

**Origin:** The user asked for TG, Discord, employee amplification, and personal-QT versions of a single brand-handle announcement, one at a time. Five interactions to produce four drafts. This skill produces all four (or any subset) from one invocation.

**Patterns surfaced from the live run:**
- Each channel needs distinct voice + format + length, NOT just length truncation.
- Stat-driven proof points work everywhere when sourced.
- The same set of legal flags applies to every channel (centralizing on `legal-flags.md` works).
- Cross-channel timing matters as much as channel-specific copy.

**Channels supported in v1.0:**
- Telegram (main community group)
- Discord (company server)
- Reddit (company subreddit)
- Secondary brand X handle
- Main brand X handle
- Employee amplification request ({AMPLIFICATION_CHANNEL})
- Personal-account QT

**Voice references for each channel** captured in `REF-channel-voices-v1.0.md` with concrete examples from the reference run.

**Hallucination guards baked in:** every claim must trace to source. Defaults to no marketing-speak. No competitor names unless source includes them.

**Legal guards** wired to `memory/context/legal-flags.md` — regulated performance claims blocked, headline-metric verification flag, confidential-figure blocks, deprecated-product naming rules.

**Open items for v1.1:**
- LinkedIn channel support (currently delegated to a dedicated LinkedIn ghostwriting skill).
- Auto-pull source from the partner-updates alert channel when trigger is "amplify the latest partner post".
- Wire to telemetry emitter once installed.
- Train on more example runs after 5+ campaigns.
- Possible enhancement: parameterize the amplification-request angle bank by topic so the agent picks angles matched to the announcement type.
