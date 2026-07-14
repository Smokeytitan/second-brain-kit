```yaml
doc_type: REF
normative: true
requires: []
```

# Source Priority

**Status:** Active v1.0
**Owner:** {PMM_LEAD}
**Consumers:** Claude AI Agent
**Change control:** PR Review

---

Use this file to decide which inputs win when source materials conflict.

## Rule
Use the highest-priority source that directly answers the section.
Do not merge conflicting claims.
Do not invent missing facts.
If a required fact is missing or conflicts across sources, write `[Missing]`.

## Default Source Order
1. Final PRD or approved product spec
2. Approved launch plan or campaign brief
3. PM, PMM, or GTM strategy doc approved for the launch
4. Approved messaging or positioning doc
5. Tiering / launch framework
6. Customer, partner, or sales notes tied to the launch
7. Meeting notes, brainstorm docs, or Slack summaries

## What Each Source Owns

### 1) Final PRD or approved product spec
Use for:
- What is shipping
- Launch scope
- Feature details
- Availability
- Requirements and constraints

Do not use for:
- Final messaging
- Marketing claims without proof

### 2) Approved launch plan or campaign brief
Use for:
- Launch summary
- Launch type
- Timing
- Major channels
- Named audiences
- Success metrics

### 3) PM, PMM, or GTM strategy doc approved for the launch
Use for:
- Market context
- Audience framing
- Problem statement
- Strategic rationale

### 4) Approved messaging or positioning doc
Use for:
- Topline message
- Support points
- Proof points
- CTA language
- Terminology

### 5) Tiering / launch framework
Use for:
- Tier classification
- Expected level of effort
- Typical deliverables
- Channel range
- Research depth

Do not use for:
- Product facts
- Launch-specific claims
- Audience specifics unless explicitly stated

### 6) Customer, partner, or sales notes tied to the launch
Use for:
- Real pain points
- Customer language
- Use cases
- Objections

Treat as directional unless confirmed by a higher-priority source.

### 7) Meeting notes, brainstorm docs, or Slack summaries
Use for:
- Open questions
- Working assumptions
- Early ideas

Do not use as final truth if a higher-priority source exists.

## Conflict Rules
- Product facts: PRD wins.
- Launch timing: approved launch plan wins.
- Messaging: approved messaging doc wins.
- Tier: tiering framework wins unless leadership explicitly overrides it.
- Customer examples: approved customer notes or approved brief wins.
- Metrics and claims: only use if the source states them clearly.

## Missing Info Rules
- If launch type is unclear, write `[Missing]`.
- If target audience is implied but not named, write `[Missing]`.
- If proof points are not sourced, leave them out.
- If CTA is not explicit, write `[Missing]`.

## Tier Override Guidance
Use the same source order for every launch, but adjust how much evidence the brief needs by tier.

### Tier 1
Expect new research, stronger proof, and more formal source docs.
Do not rely on assumptions if a core section affects business impact.

### Tier 2
Use approved launch docs, existing ICP, and existing messaging where possible.
Add new support only when the source set has a real gap.

### Tier 3
Work from existing docs and approved assumptions.
Keep the brief short.
Do not create new strategy unless explicitly asked.

## Output Rule
- Return one clean brief only.
- Do not expose source conflicts unless they block accuracy.
- Launch names and titles must reflect user outcomes, not internal feature names or technical mechanisms.
- If they block accuracy, mark the field `[Missing]`.
