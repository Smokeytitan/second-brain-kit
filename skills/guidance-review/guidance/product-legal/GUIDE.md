---
guide_slug: product-legal
guide_name: "{PRODUCT} Legal Compliance"
version: 1.0
source: "Internal legal team — Compliance-Forward Marketing Guidelines"
last_reviewed: YYYY-MM-DD
---

# {PRODUCT} Legal Compliance Guide

<!-- Example guide for a product that combines regulated and unregulated components,
where a licensed partner ({LICENSED_ENTITY}) performs the regulated activities and
{COMPANY} provides the underlying technology. Replace the specifics with your own
legal team's guidance; keep the rule format (Check / Approved / Prohibited / Severity). -->

## Category: Entity & Regulatory Claims

### Rule: {PRODUCT} is infrastructure, not a regulated product
- **Check:** Copy does not describe {PRODUCT} as a single regulated product, financial service, or licensed entity. {PRODUCT} is a vertically integrated stack combining regulated and unregulated components.
- **Approved:** "Infrastructure" / "Vertically integrated stack" / "Technology rails + regulated access" / "Enables regulated services"
- **Prohibited:** "Regulated platform" / "Licensed provider" / "Regulated product"
- **Severity:** HIGH

### Rule: Regulatory status depends on implementation
- **Check:** Copy does not state that {PRODUCT} has a fixed regulatory status. Regulatory characterization depends on the specific implementation and role allocation — some configurations may not constitute regulated activity at all.
- **Approved:** "Regulatory characterization depends on the specific implementation and role allocation" / "Infrastructure alone is not automatically regulated — but integrated operations may be"
- **Prohibited:** Any blanket claim that {PRODUCT} is or is not regulated without context
- **Severity:** HIGH

### Rule: {COMPANY} is not a bank
- **Check:** Copy does not state or imply {COMPANY} operates as a bank or depository institution. {COMPANY} provides infrastructure and tooling. Where regulated fiat access is required, licensed entities perform that function.
- **Approved:** "{COMPANY} provides the underlying infrastructure and technology"
- **Prohibited:** "{COMPANY} processes payments" / "{COMPANY}'s bank" / "{COMPANY}'s regulated payment network" / implying {COMPANY} is a depository institution
- **Severity:** HIGH

### Rule: {COMPANY} is not a money transmitter
- **Check:** Copy does not state or imply {COMPANY} is a money transmitter. {COMPANY} provides non-custodial infrastructure and software. Money transmission activities (accepting/transmitting value, controlling customer funds, operating custodial services) are performed by licensed partners such as {LICENSED_ENTITY}. NOTE: update this rule if the corporate structure changes.
- **Approved:** "{COMPANY} provides the underlying infrastructure and technology" / "Regulated services are provided by {LICENSED_ENTITY}"
- **Prohibited:** "{COMPANY} transmits money" / "{COMPANY} accepts and transmits funds" / implying {COMPANY} controls customer funds
- **Severity:** HIGH

### Rule: {LICENSED_ENTITY} is the regulated entity
- **Check:** When discussing regulated services within {PRODUCT}, copy correctly attributes these to {LICENSED_ENTITY}, not {COMPANY}. {LICENSED_ENTITY} operates as the licensed entity; {COMPANY} provides the technology.
- **Approved:** "{LICENSED_ENTITY} operates the regulated services as a licensed entity" / "Regulated services are provided by {LICENSED_ENTITY}"
- **Prohibited:** Attributing {LICENSED_ENTITY}'s licenses, compliance program, or regulated activities to {COMPANY}
- **Severity:** HIGH

### Rule: {PRODUCT} changes the mechanism, not regulatory obligations
- **Check:** Copy does not imply that using {PRODUCT} eliminates regulatory obligations. {PRODUCT} changes how the work gets done, not whether regulation applies. Regulatory obligations attach to custody, fiat touchpoints, customer onboarding, control of funds, and representations made to users.
- **Approved:** "{PRODUCT} changes how the work gets done, not whether regulation applies"
- **Prohibited:** "No compliance needed" / "Regulation-free" / "Bypass regulatory requirements"
- **Severity:** HIGH

---

## Category: Approved Language (DOs)

### Rule: Approved {PRODUCT} descriptors
- **Check:** When describing {PRODUCT}, copy uses approved framing language.
- **Approved:** "Infrastructure" / "Vertically integrated stack" / "Technology rails + regulated access" / "Enables regulated services"
- **Prohibited:** Any framing that positions {PRODUCT} as a single regulated entity or financial product
- **Severity:** MEDIUM

### Rule: Approved compliance framing
- **Check:** Copy correctly frames compliance obligations — regulated activities are performed by licensed entities, compliance obligations remain in place, and role allocation determines regulatory exposure.
- **Approved:** "Regulated activities are performed by licensed entities" / "Compliance obligations remain in place" / "Role allocation determines regulatory exposure"
- **Prohibited:** Implying compliance is optional or eliminated
- **Severity:** HIGH

### Rule: Approved risk language
- **Check:** When discussing redundancy or concentration risk, copy uses qualified language about design intent rather than absolute guarantees.
- **Approved:** "Designed to reduce risk" / "Designed to reduce concentration and disruption risk" / "{PRODUCT} is designed with redundancy in mind" / "Designed using multiple providers, modular partners, and separation of concerns"
- **Prohibited:** "Eliminates risk" / "Always uninterrupted" / "Fully provider-agnostic" / "Guaranteed uninterrupted access" / "Zero compliance risk"
- **Severity:** HIGH

### Rule: Approved {LICENSED_ENTITY} claims
- **Check:** Claims about {LICENSED_ENTITY}'s regulatory status are specific and accurate — its actual licenses, actual years of operation, actual registrations, and actual geographic coverage. State geographic limits explicitly.
- **Approved:** "{LICENSED_ENTITY} has been licensed and regulated for [N] years" / "{LICENSED_ENTITY} operates under [named regulator] oversight" / "{LICENSED_ENTITY} holds [named registration]" / "Services are currently available only within [jurisdiction]"
- **Prohibited:** Overstating {LICENSED_ENTITY}'s geographic coverage or implying global licensing
- **Severity:** MEDIUM

---

## Category: Prohibited Language (DON'Ts)

### Rule: Banned phrases
- **Check:** Copy does not contain any of the following banned phrases or close variants.
- **Prohibited:** "Unregulated [service]" / "Bypass [incumbents]" / "No compliance needed" / "Replace [incumbent system]" / "No intermediaries" (if regulated partners exist) / "Fully decentralized stack" / "Zero compliance risk" / "Guaranteed uninterrupted access" / "Fully provider-agnostic"
- **Severity:** HIGH

### Rule: Banned implications — guarantees and equivalence
- **Check:** Copy does not imply the product is equivalent to an insured or guaranteed alternative, that returns are guaranteed or risk-free, or that cross-border activity is regulation-free.
- **Approved:** Qualified language; explicit risk disclosures
- **Prohibited:** Implying insurance or guarantees that don't exist / "risk-free yield" / "regulation-free cross-border" / false equivalence with insured products
- **Severity:** HIGH

### Rule: Banned conflations
- **Check:** Copy does not conflate the infrastructure layer with regulated service provision, and does not conflate non-custodial software with custodial asset control (unless the product actually is custodial).
- **Approved:** Clear distinction between infrastructure layer and regulated service layer
- **Prohibited:** Describing {COMPANY}'s infrastructure as a regulated service / describing non-custodial software as custodial
- **Severity:** HIGH

### Rule: Banned assumptions about licensing scope
- **Check:** Copy does not assume one jurisdiction's licensing covers global operations or that one jurisdiction's classification applies globally.
- **Approved:** "Services are currently available only within [jurisdiction]" / jurisdiction-specific qualifiers
- **Prohibited:** Implying global coverage from single-jurisdiction licenses / "Licensed worldwide" / "Globally regulated"
- **Severity:** HIGH

---

## Category: Required Disclaimers

### Rule: Regulated-services footnote disclaimer
- **Check:** Marketing materials about the regulated components must include the following footnote disclaimer (exact or substantively equivalent wording).
- **Required text:** "Regulated services are provided by {LICENSED_ENTITY}, a licensed entity. {COMPANY} provides the underlying technology. Regulatory status, certifications, and program features vary by entity and jurisdiction."
- **Severity:** MEDIUM

### Rule: {PRODUCT} page disclaimer
- **Check:** {PRODUCT}-focused pages or materials must include the following disclaimer (exact or substantively equivalent wording).
- **Required text:** "Certain compliance controls, certifications, and program features are entity-specific and may be subject to change as the product suite evolves."
- **Severity:** MEDIUM

---

## Category: Security & Certification Claims

### Rule: SOC 2 qualification
- **Check:** SOC 2 claims are properly qualified per entity — state the actual certification stage for each entity (e.g., Type I certified, Type II in progress, examination underway). Do not use "SOC 2 compliant" or "SOC 2 certified" without qualification.
- **Approved:** "SOC 2 examination in progress" / "SOC 2 Type I certified" / "SOC 2 Type II in progress" (matched to the correct entity)
- **Prohibited:** "SOC 2 compliant" (unqualified) / "SOC 2 certified" (for an entity that isn't) / "SOC 2 certified infrastructure" (over-scoped)
- **Severity:** HIGH

### Rule: ISO 27001 scoping
- **Check:** ISO 27001 claims are scoped correctly — name the certified entity and the certified scope. Planned scope expansions are not yet certified.
- **Approved:** "{COMPANY} maintains ISO 27001 certification for its information security program" / "{COMPANY} is ISO/IEC 27001:2022 certified [for the certified scope]"
- **Prohibited:** Claiming certification for scopes where expansion is planned but not complete
- **Severity:** HIGH

### Rule: No absolute security guarantees
- **Check:** Copy uses "designed to reduce" language rather than absolute guarantees about security, uptime, or risk elimination.
- **Approved:** "Designed to reduce concentration and disruption risk" / "Designed to reduce risk"
- **Prohibited:** "Eliminates risk" / "Zero risk" / "Guaranteed uptime" / "Always uninterrupted"
- **Severity:** HIGH

---

## Category: Slides & BD Guidelines

### Rule: No unapproved logos
- **Check:** Presentation materials do not display logos of partners unless contractually approved for use.
- **Prohibited:** Partner logos without contractual approval
- **Severity:** MEDIUM

### Rule: No metrics without legal sign-off
- **Check:** Presentation materials do not show compliance metrics (report percentages, alert volumes, etc.) without legal sign-off.
- **Prohibited:** Compliance KPIs or alert metrics without legal approval
- **Severity:** HIGH

### Rule: No over-claiming autonomy in regulated contexts
- **Check:** Slides and BD materials do not use "fully decentralized" or "fully autonomous" framing when describing {PRODUCT} in a regulated-services context.
- **Approved:** "Vertically integrated stack" / "Technology rails + regulated access"
- **Prohibited:** "Fully decentralized" (in a regulated-services context)
- **Severity:** HIGH

### Rule: Anchor claims to evidence
- **Check:** Slides use diagrams (flows, controls, escalation paths) and anchor claims to specific licenses, audits, certifications, and operating history rather than making unsubstantiated assertions.
- **Approved:** Claims backed by specific license names, audit types, certification standards, or years of operation
- **Prohibited:** Vague or unsubstantiated compliance claims without evidence anchors
- **Severity:** MEDIUM
