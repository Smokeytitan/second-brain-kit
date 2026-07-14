---
name: release-radar
description: >-
  Set up and run a personalized weekly Release Radar for any role and any product. Use this
  when someone says 'set up a release radar', 'build me a release radar', 'product release radar',
  'I need to track releases for my product', 'help me stay on top of what is shipping',
  'release tracking', or 'onboard me to the release radar'. The skill first interviews the person to
  learn their role, the product or area they own, and where release signal lives (recurring meetings
  and their notes, chat channels, project trackers, code repos, changelogs), then verifies those
  sources, writes a tailored workflow spec, schedules a recurring brief, and produces the first one.
  Do not use for a one-off 'what shipped this week' question; just answer that directly.
---

# Release Radar (interview-driven setup)

Turn "I can never keep up with what my product and eng teams are shipping" into a standing, automated weekly brief. This skill interviews the person, wires up their real sources, and triangulates them so nothing slips, especially releases that live outside the official tracker.

## Core principle
No single source is complete. The official release tracker (ClickUp, Jira, Linear) almost always lags reality. The radar's job is to triangulate: pick the most complete "primary truth" source (often a recurring product or eng meeting's running notes), then cross-check it against the tracker, chat channels, code repos, and changelogs, and flag whatever is missing, mislabeled, or slipping.

## When to run
- First time for a person: run the full interview (Phase 1), then build (Phases 2 to 4).
- Returning: if a spec already exists for this person, skip the interview and run the weekly brief (Phase 4), or ask what changed and update the spec.

## Phase 1: Interview
Style: one question at a time, conversational, voice-dump answers welcome (dictation friendly). Target about 10 minutes. Offer an example with each question. Accept "not sure" and infer a sensible default. Summarize the captured config at the end and confirm before building.

Ask in order, adapting follow-ups to the answers:
1. Role and remit. "What is your role, and what are you accountable for?" (PMM for a platform; PM for a SaaS app; DevRel; founder.) This sets the lens.
2. The product or area. "Which product, product line, or area do you need a release radar for?" Get specifics.
3. What counts as a release. "What kinds of things do you need to know about?" (shipped features, version releases, API changes, deprecations, infra or performance changes, incidents, roadmap shifts.) This defines the net.
4. The meetings. "Which recurring meetings cover this, and where do their notes live?" (product weekly, eng sync, steering call.) Capture meeting name plus notes location (Gemini or Google Doc, Notion, Confluence). Ask which single one is the most complete "source of truth."
5. Chat. "Which Slack, Teams, or Discord channels carry release signal?" Capture channel names and IDs, including the eng-heavy channels, not just the marketing ones.
6. Trackers. "What project tracker holds the release or roadmap items?" (ClickUp list, Jira project, Linear team.) Capture IDs or URLs. Ask directly whether it is complete or known to lag.
7. Code. "Which code repos should I watch for releases, tags, and merged PRs?" (GitHub or GitLab org and repos.) Optional.
8. Changelogs and other. "Any product changelogs, status pages, or docs sites to watch?" Optional.
9. Triage lens. "When you see a change, what three questions decide whether it matters to you?" Offer a default per role (see Triage lens below).
10. Comms tiering. "What warrants a big announcement, versus a quiet note, versus no comms at all?" Capture their tiers and who gatekeeps what gets comms.
11. Cadence and delivery. "What day and time should the brief land, and where?" (For example Monday 9am, before the staff meeting.) Delivery options: a file in their notes system, a chat DM, email, or a live dashboard.
12. Guardrails. "Any legal or compliance rules I should apply?" (claims needing review, competitor-naming rules) and "any writing preferences?" (for example, some people ban em dashes and en dashes).

## Phase 2: Verify sources
Before building, confirm each named source is reachable and identify the primary truth source:
- Search chat for the exact channels; capture IDs.
- Resolve tracker lists or projects to IDs; sample recent items to confirm access and gauge how complete the tracker is.
- Locate meeting-notes docs (search the notes system by meeting name plus "notes"); confirm the person can actually open them. If a key meeting's notes are not shared with them, tell them to get added and record it as pending.
- Verify repos exist and pull their latest releases and tags.
- If a needed connector is missing (GitHub, Jira, a chat tool), suggest connecting it before relying on that source.
Note gaps explicitly rather than guessing.

## Phase 3: Build
1. Write the person's workflow spec to their notes system (for example `agents/scheduled/release-radar-<name>.md`, or the team's equivalent), filled in from the interview: a sources table with IDs, the primary-truth designation, the method, their triage lens, their comms tiering, the output format, delivery, and guardrails.
2. Create a recurring scheduled task at their chosen day and time. The scheduled prompt must be fully self-contained (each run starts fresh): list every source with IDs, the output path, the delivery target, and the rules. Point it at the spec as the source of truth.
3. Produce the first dated brief now and deliver it (file plus their chosen ping), so they see value immediately.
4. Tell them to click Run now once to pre-approve connectors, and how to adjust cadence or sources later.

## Output format (the brief)
Keep it skimmable. Sections, in order:
- Header: date, window covered, sources pulled, which meetings it preps.
- Needs your attention: items in the next ~14 days that need a decision or action, plus reconciliation issues.
- Upcoming releases (next ~30 days): a table with columns Release, Date, Type and comms tier, In tracker?, Owned or covered?, Triage.
- Surfaced outside the tracker: the triangulation payoff, things found in meetings, chat, code, or changelogs that the official tracker is missing or mislabeling.
- On the roadmap (undated).
- Recently shipped (last ~2 weeks).
- Coverage check: which upcoming releases have an owner or a marketing/enablement task, and which do not.
- Reconciliation and data hygiene: tracker versus reality mismatches, stale dates, wrong labels.
- Sources, with links.

## Triage lens
Apply the person's three questions to every item. Default archetypes if they did not specify:
- Platform or infra PMM: dev impact; cost or speed; competitor gap.
- SaaS PM: customer impact; revenue or retention; support load.
- DevRel: breaking changes; docs and sample impact; community sentiment.

## Comms tiering
Classify each upcoming release by the person's tiers. Default:
- Major milestone: full announcement.
- Minor release: changelog or forum note.
- Security or private release: no public comms until released; log it internally.
Respect the org's gatekeeping (who decides what gets comms).

## Rules (apply to every brief)
- Use exact calendar dates. Convert tracker and code timestamps (Unix milliseconds, ISO) to specific dates. Never use a vague range ("early July", "Q3") when a real date exists. Distinguish the RELEASE date from any DELIVERABLE due dates, and flag when deliverables lag a slipped release.
- Verify before flagging anything "missing": confirm via the actual source first.
- Apply the person's guardrails: flag claims that need legal or compliance review, and do not publish them. Honor writing preferences (for example, if the person bans em dashes and en dashes, use commas, colons, parentheses, or plain hyphens instead).
- Read and brief only by default: do not post public content and do not modify trackers. It may DM or notify the person.

## Worked example (illustrative)
A PMM at Acme Corp runs this for the Acme platform's core-service upgrades:
- Primary truth: the Product Weekly's running "monthly commit" notes doc, which is more complete than the eng release calendar in the tracker.
- Cross-checks: the tracker's release-calendar list ({RELEASE_CALENDAR_LIST_ID} — your project tracker's release list), the marketing releases list, three eng and product Slack channels, the recurring steering-call notes, and the product's GitHub repos.
- Triage lens: dev impact; cost or speed; competitor gap.
- Comms tiering: major milestone equals splash; minor equals forum acknowledgment; private security equals no comms.
- Cadence and delivery: a dated brief to `Resources/Release-Radar/` plus a Slack DM, Mondays at 9am.
- Spec: `agents/scheduled/release-radar.md`.

## Changelog
- v1: interview-driven, any-role and any-product release radar.
