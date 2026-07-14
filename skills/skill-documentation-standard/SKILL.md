---
name: skill-documentation-standard
description: Documentation governance standard for AI skill files — naming conventions, AI-optimized formatting, few-shot requirements, and cross-referencing rules
---

# AI Skill Documentation Governance Standard (v1.0)

```yaml
doc_type: STD
normative: true
requires:
  - STD-doc-governance-prd-v1.0#section-1
```

**Status:** Adopted v1.0
**Owners:** {DOC_OWNER} — the team that owns your skill library
**Consumers:** Claude AI Agent, Engineering Team
**Change control:** ADR + PR Review

---

## 0. Overview
This standard defines how AI Skill documentation is formatted to ensure maximum comprehension by Large Language Models (LLMs) via the Model Context Protocol (MCP) or local file ingestion.

## 1. File Naming Conventions
All AI skill files must adhere to the global documentation prefix and versioning standards.
* `STD-*`: Core rules and constraints the AI must never break.
* `RUN-*`: Step-by-step Standard Operating Procedures (SOPs) the AI must execute.
* `DOC-*`: System catalog and Persona/Voice descriptions.
* `REF-*`: Reference knowledge (e.g., coding standards, API docs). Reference documents may omit the `-prd` suffix.
* `BP-*` (Companion `-impl`): Templates, few-shot examples, and expected output formats.

## 2. AI-Optimized Markdown Formatting
To ensure the AI strictly follows instructions without hallucinating, all files must use the following structural rules:

### 2.1 Explicit Sectioning
The AI context window relies on clear boundaries. Use standard Markdown headers (`##`) for human readability, but encapsulate exact templates or code formats inside XML-like tags (e.g., `<output_template>`, `<few_shot_examples>`) to prevent the AI from confusing instructions with output formats.

### 2.2 Direct Directives
Instructions must use imperative verbs (e.g., "Analyze the diff", "Generate a summary"). Avoid passive voice. Limit paragraphs to 3-4 sentences.

### 2.3 Required Header Block
Every file must include the standard governance header block containing `Status`, `Owners`, `Consumers`, and `Change control` to maintain auditability.

## 3. The "Few-Shot" Requirement
Any template document (`BP-*-impl-vX.Y.md`) must include at least one "Good" example and one "Bad" example of the desired output to explicitly anchor the AI's pattern matching.

## 4. Cross-Reference Requirement
Every document must include a `## Cross-References` section as the final section of the document. At minimum it must link to:

* `DOC-master-catalog-prd-v1.0.md` — the master registry of all documents.
* The skill-specific catalog (e.g., `DOC-<skill>-catalog-prd-v1.0.md`) if the document belongs to a skill.

`DOC-master-catalog-prd-v1.0.md` itself is exempt from this requirement.

## 5. Companion Documents (Implementation Guides)
For standards requiring detailed implementation guidance, create a companion document using the `-impl` suffix.

### 5.1 Naming Pattern
* Main document: `{PREFIX}-{slug}-prd-v{X}.{Y}.md`
* Companion guide: `{PREFIX}-{slug}-impl-v{X}.{Y}.md`

### 5.2 Purpose
* **Main document (`-prd`):** Requirements, policies, architecture, standards.
* **Companion guide (`-impl`):** Templates, code examples, step-by-step guides, reference tables.

### 5.3 Key Characteristics
* Same prefix and slug ensure association.
* Different suffix (`-prd` vs `-impl`) indicates purpose.
* Alphabetically adjacent in file listings.
* Versions can evolve independently.
* Both must be registered in the Master Catalog.

### 5.4 Cross-Referencing Requirements
* Main document MUST link to companion in a dedicated section (e.g., `## Implementation Guide`).
* Companion MUST reference main document in its `requires:` yaml block.
* Both MUST be registered in `DOC-master-catalog-prd-v*.md`.

### 5.5 When to Create a Companion
Create a companion document when any of the following apply:
* Implementation requires more than 500 lines of code examples or templates.
* Multiple working examples are needed.
* Step-by-step tutorials are required.
* Frequent implementation updates that should not trigger policy reviews.

### 5.6 Examples
* `STD-data-architecture-prd-v1.0.md` + `STD-data-architecture-impl-v1.0.md`
* `STD-scraper-prd-v1.0.md` + `STD-scraper-impl-v1.0.md` _(future)_
* `STD-api-architecture-prd-v1.0.md` + `STD-api-architecture-impl-v1.0.md` _(future)_

## Cross-References

- **Master Catalog:** [DOC-master-catalog-prd-v1.0.md](DOC-master-catalog-prd-v1.0.md)
