# Redo Knowledge Base — Page Templates

## Standard Metadata Header (ALL pages)

Every page must start with this metadata block:

```
**Owner:** [name]
**Last Updated:** [YYYY-MM-DD]
**Status:** Draft | Active | Stale
**Related Pages:** [links to related KB pages]
```

Status definitions:
- **Draft** = Work in progress, may be incomplete or unverified
- **Active** = Current, reviewed, good to use
- **Stale** = May be outdated, needs review

---

## Product Cloud Template

Used for pages under **Company & Products**: Reverse Logistics Cloud, Shipping Cloud, Conversion Cloud, Marketing Cloud, Finance Cloud.

```markdown
**Owner:** 
**Last Updated:** 
**Status:** Draft
**Related Pages:** 

---

# Features
> Document features, capabilities, and pricing model

---

# Competitive Positioning
> Competitors, differentiation, win/loss patterns

---

# ICP Definitions & Scoring
> Ideal customer profiles with scoring criteria

---

# Target Segments
> Revenue tiers, verticals, platform requirements

---

# Disqualification Criteria
> When to walk away
```

---

## Generic Page Template

Used for all other pages (GTM, CX, Systems, Finance, Reference).

```markdown
**Owner:** 
**Last Updated:** 
**Status:** Draft
**Related Pages:** 

---

# Overview
> Purpose and scope of this page

---

# Content
> Main content goes here

---

# Key Takeaways
> Summary points for quick reference
```

---

## Section Index Template

Used for section root pages (e.g., 🏢 Company & Products, 🎯 GTM).

```markdown
**Purpose:** [what this section covers]
**Owner:** [name]
**Last Updated:** [YYYY-MM-DD]
**Status:** Draft
**Related Pages:** [cross-references to other sections]

---

# How to Use This Section
[Brief instructions specific to this section's content pattern]

---

# [Child pages listed below]
```

---

## Changelog Entry Format

When making significant updates, add an entry to Reference > Changelog:

```
| [YYYY-MM-DD] | [What changed] | [Who / Claude] |
```
