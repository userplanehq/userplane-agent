# Agent Guidance

This repository contains agent skills for integrating Userplane screen recording SDK. Follow these conventions when contributing.

## Directory structure

```
skills/
  {skill-name}/
    SKILL.md              # Required: skill definition (single file)
    references/           # Optional: detailed reference material
      REFERENCE.md
```

## SKILL.md format

```markdown
---
name: userplane-{topic}
description: One sentence with trigger phrases. Used by agents to decide when to activate.
license: MIT
compatibility: framework version constraint (if applicable)
---

# Skill Title

Brief intro paragraph.

## When to use

Bulleted list of scenarios.

## Section content...

## Common patterns

Bulleted list of scenarios with which API/approach to use.
```

## Naming conventions

- Skill directories: `userplane-` prefix, kebab-case (e.g., `userplane-react`, `userplane-web-sdk`)
- SKILL.md: Always uppercase, exact filename
- No consecutive hyphens in names

## Best practices

- Keep each SKILL.md under 500 lines
- Use cross-references (`See the userplane-web-sdk skill`) instead of duplicating content
- Code examples should be minimal — show the pattern, not a full app
- Use progressive disclosure: put detailed reference data in `references/` subdirectory
- Frontmatter `description` should include trigger phrases agents will match on
