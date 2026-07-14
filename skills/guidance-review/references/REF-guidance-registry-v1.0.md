---
doc_type: REF
title: Guidance Registry
version: 1.0
description: >
  Maps trigger keywords to guidance documents and defines the category taxonomy
  used for checklist output. The skill scans guidance/*/GUIDE.md for convention-based
  discovery; this registry provides metadata, keyword aliases, and category structure.
---

# Guidance Registry

<!-- Fill in your own guides. One example entry is provided; replace with your
company's actual guidance documents. -->

```yaml
guides:
  - slug: product-legal
    display_name: "{PRODUCT} Legal Compliance"
    trigger_keywords: ["legal", "product-legal", "compliance"]
    source_doc: "guidance/product-legal/GUIDE.md"
    owner: "Legal / {PMM_LEAD}"
    last_reviewed: "YYYY-MM-DD"
    categories:
      - slug: entity-claims
        display_name: "Entity & Regulatory Claims"
      - slug: approved-language
        display_name: "Approved Language (DOs)"
      - slug: prohibited-language
        display_name: "Prohibited Language (DON'Ts)"
      - slug: disclaimers
        display_name: "Required Disclaimers"
      - slug: security-claims
        display_name: "Security & Certification Claims"
      - slug: slides-bd
        display_name: "Slides & BD Guidelines"
```

## Adding a New Guide

1. Create a new folder: `guidance/{slug}/`
2. Add a `GUIDE.md` following the standard format (see `guidance/product-legal/GUIDE.md` as the template):
   - YAML frontmatter with `guide_slug`, `guide_name`, `version`, `source`, `last_reviewed`
   - `## Category: {name}` sections containing `### Rule: {name}` entries
   - Each rule has: **Check**, **Approved** (optional), **Prohibited**, **Severity** (HIGH/MEDIUM/LOW)
3. Add an entry to this registry with `slug`, `display_name`, `trigger_keywords`, `source_doc`, `owner`, `last_reviewed`, and `categories`
4. The skill auto-discovers the new guide on next invocation
