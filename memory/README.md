# Memory system

Two-tier memory that makes Claude a real collaborator instead of a stranger every session.

- **Tier 1 — CLAUDE.md** (vault root): working memory. Who you are, active projects, preferences. Claude reads it every session.
- **Tier 2 — this directory**: the knowledge base. Claude reads specific files on demand ("who is X?", "what did we decide about Y?", drafting content that must pass legal flags).

## Layout

| Path | What lives there | When Claude reads it |
|------|------------------|----------------------|
| `glossary.md` | Acronyms, internal terms, nicknames, codenames, writing preferences, channel map | Decoding any shorthand |
| `decisions.md` | Running log of decisions, grouped by topic, dated | "Why do we do it this way?" |
| `people/` | One file per key collaborator | Drafting messages to them, meeting prep |
| `projects/` | Deep context per project (richer than vault Projects/ files) | Any work on that project |
| `areas/` | Trackers for ongoing responsibilities | Recurring-task work |
| `context/company.md` | Org structure, tools, processes, constraints | Onboarding-level questions |
| `context/legal-flags.md` | Content rules: what triggers review, what's confidential | EVERY piece of external content |
| `context/stakeholder-map.md` | Who cares about what, influence and escalation paths | Cross-team work |
| `context/meetings.md` | Recurring meetings, cadence, who runs them, where notes live | Meeting prep, scheduling |

## Rules

1. Copy `_template.md` in each folder to create new entries; keep the section headers.
2. Date-stamp entries; newest first.
3. When a fact changes (re-org, tool switch, decision reversed), update the file — stale memory is worse than no memory.
4. Use the `memory-buildout` skill to generate profiles in bulk from existing context, and `consolidate-memory` to dedupe and compact over time.
5. Cross-reference between files with relative links instead of duplicating content.
