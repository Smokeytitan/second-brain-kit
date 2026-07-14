# Memory Buildout — File Templates

**Version:** 1.0.0

The exact output format for each memory file type. Model these on the 2–3
richest profiles already in your second brain; the templates below are the
proven shapes.

---

## People profile template

File: `memory/people/{slug}.md` (slug = first-name-last-name in lowercase, e.g., `jane-doe.md`)

Length: ~30–50 lines.

```markdown
# {Full Name}

**Also known as:** {nicknames, comma-separated}
**Role:** {role title}
**Team:** {team / reports to}
**Tenure:** {if known}
**Chat handle/ID:** {if known, else omit line}
**Relationship to the user:** {peer / report / manager / cross-functional partner / external collaborator / exec}
**Onboarding wave:** {Wave 1/2/3 — only include if applicable}

## Communication
- {style observations: direct/structured/casual/etc.}
- {channel preferences: project tracker first / chat DM / public channel / etc.}
- {cadence with the user: weekly 1:1 / async only / event-driven}

## Context
- {what they own — 2-3 lines on their scope}
- {recent collaborations with the user — pull 3-5 from ACTIVITY-LOG with dates}
- {anything the user needs to know before drafting to them}

## Recent interactions
- {YYYY-MM-DD}: {dated interaction from Activity Log}
- {YYYY-MM-DD}: {another}
{3-5 total}

## Key things to remember
- {nuances, sensitivities, preferences}
- {do's and don'ts specific to this person}
```

**Honesty rule:** If source context is thin (< 3 Activity Log mentions), include only what's verifiable + flag gaps:
```
_Needs more context from the user on direct working cadence._
```

---

## Project memory template

File: `memory/projects/{slug}.md` (slug = lowercase project name with hyphens, e.g., `flagship-launch.md`)

Length: ~50–100 lines.

```markdown
# {Project Name} — Deep Context

**Status:** {current — match Dashboard / Projects/ file}
**Owner:** {the user} {with collaborators if shared}
**Started:** {date or rough timeframe}
**Key partners:** {names, comma-separated}

## What this project is
{1-2 paragraph narrative: what it is, why it exists. The "why" matters more
than the "what" — this is deep context, not status. Status lives in
Projects/{name}.md.}

## Origin
{How it came to be — leadership directive, competitor pressure, team capacity issue, inherited from prior owner. Capture decisions made BEFORE the project formally started.}

## Key Decisions Timeline
{Chronological list of material decisions with dates + context. Pull from Activity Log + memory/decisions.md.}
- **YYYY-MM-DD:** {decision} — {context}.
- **YYYY-MM-DD:** {decision} — {context}.
{...}

## Key Partners + Roles
| Person | Role in this project |
|--------|----------------------|
| **{Name}** | {role} |

## Critical Context Not Captured Elsewhere
{Anything the user knows in their head but isn't documented yet — sensitivities,
risks, stakeholder nuances, confidentiality flags.}

## Open Questions / Watchlist
{Things the user is watching for — pending decisions, expected events, risks.}

## Cross-References
- **Projects file (running):** [../../Projects/{Name}.md](../../Projects/{Name}.md)
- **Decisions log:** [../decisions.md](../decisions.md)
- **Activity Log:** [../../ACTIVITY-LOG.md](../../ACTIVITY-LOG.md)
- **Related projects:** {list as relevant}
- **Key people:** [../people/{slug}.md](../people/{slug}.md), {...}
```

---

## Context file template

File: `memory/context/{topic}.md`. No fixed slug rule — match the topic (e.g., `meetings.md`, `stakeholder-map.md`, `legal-flags.md`, `company.md`).

Length: variable — 50–200 lines depending on scope.

```markdown
# {Topic Title}

**Purpose:** {1 sentence — what this file is for and when to consult it}.
**Last updated:** YYYY-MM-DD
**Owner:** {the user} {plus authority if applicable, e.g., "with {LEGAL_REVIEWER} as legal authority"}
**Maintenance:** {when to update — periodic / triggered / append-only}

---

{Body — structure depends on topic. Common patterns:}

## {Section} — table-driven
| {Col 1} | {Col 2} | ... |
|---------|---------|-----|
| {row} | {row} | ... |

## {Section} — list-driven
- {item}
- {item}

## Cross-References
- **{Related file}:** [path](path)
- {...}
```

### Concrete context-file types that work well

**`meetings.md`** — Recurring meetings + cadences. Sections: weekly cadences, 1:1s, working groups, ad-hoc relevant meetings. Per-meeting fields: when, attendees, doc link, owner, the user's role, prep pattern.

**`stakeholder-map.md`** — Approval matrix + escalation paths + reporting lines. Tables: decision-type → primary-approver → secondary → notes. Plus narrative escalation paths per domain.

**`legal-flags.md`** — Compliance + brand triggers. Sections: critical (always {LEGAL_REVIEWER} review), confidentiality (never disclose), competitive/brand ({CMO} review), content process rules, writing rules, escalation path.

**`company.md`** — Company background, org structure.

---

## Cross-Reference rules

Every memory file should link to:
- **Dashboard.md** (current state)
- **memory/decisions.md** (strategic context)
- **Activity Log** (historical record)
- **Related people files** (for projects: who's involved; for context: who has authority)
- **Related project files** (for people: what they're working on; for context: what it applies to)

Use relative paths from each file's location:
- People file → `../decisions.md`, `../../Dashboard.md`, `../people/{related}.md`
- Project file → `../decisions.md`, `../../Dashboard.md`, `../people/{slug}.md`
- Context file → `../decisions.md`, `../../Dashboard.md`, `../people/{slug}.md`

---

## Style rules

- **No em dashes** in any output. Use colons, commas, semicolons, or separate sentences.
- **Concise and direct.** No fluff. No marketing-speak.
- **Honest gaps.** Use `_Needs more context from the user on {topic}._` for unknowns. Don't fabricate.
- **Active voice.** Subject does verb to object.
- **Date all dated events.** YYYY-MM-DD format, no exceptions.
- **Cross-link generously.** Better to over-link than make the user search.

---

## Cross-References
- **Skill:** [../SKILL.md](../SKILL.md)
- **Workflow:** [RUN-memory-buildout-workflow-prd-v1.0.md](RUN-memory-buildout-workflow-prd-v1.0.md)
- **Batching rules:** [REF-parallel-agent-batching-v1.0.md](REF-parallel-agent-batching-v1.0.md)
- **Reference files:** your richest existing `memory/people/`, `memory/projects/`, and `memory/context/` files serve as the live baselines
