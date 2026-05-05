# Page Template

> Use this for new knowledge pages in your Knowledge Repo. Copy, fill in, and commit.

```markdown
---
title: "Your Page Title"
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: learning  # learning | methodology | domain | decision
tags: [tag1, tag2]
related:
  - "[Related Page 1](../path/to/page1.md)"
  - "[Related Page 2](../path/to/page2.md)"
---

# Your Page Title

> One-sentence summary of what this page answers.

---

## Core Concept

Explain the concept in 2-3 paragraphs. Assume the reader has context but not detail.

## Data Verification

How was this tested against real data? What was the result?

```python
# Include actual verification code if applicable
```

## Key Insight

The one thing someone should take away from this page.

## Limitations

What does this NOT cover? What assumptions might break?

---

## Cross-References

- [Related Page 1](../path/to/page1.md) — how it connects
- [Related Page 2](../path/to/page2.md) — how it connects
```

---

## Rules

1. **Every page must have YAML frontmatter** — title, date, type, tags
2. **One page = one question answered** — if you need two questions, write two pages
3. **Always cross-reference** — link to related pages in `related` and at the bottom
4. **Verification is not optional** — theory without data test stays in draft
5. **Keep it under 500 lines** — if longer, split it

---

> **Also see:** [ADR Template](adr-template.md)
