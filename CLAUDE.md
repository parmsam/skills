# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

A personal collection of Claude Skills for using my open-source projects — primarily Quarto extensions. Skills are structured markdown files that teach Claude how to help users author content with these extensions. There is no application code to build or deploy — the primary artifacts are Markdown files consumed by Claude's skill system.

## Directory Structure

```
<category>/
  README.md                   # Category-level notes (optional)
  <skill-name>/
    SKILL.md                  # Required: skill definition with YAML frontmatter
    references/               # Optional: supplementary docs loaded on demand
      *.md
    scripts/                  # Optional: shell or R helpers
    templates/                # Optional: file templates
```

Do **not** create a `README.md` inside individual skill directories — documentation about a skill's design goes in the category README.

## SKILL.md Format

Every skill requires YAML frontmatter:

```yaml
---
name: your-skill-name        # kebab-case, matches directory name
description: >               # Claude reads this to decide when to activate the skill
  Clear description of what this skill does and when to trigger it.
  Keep under 100 tokens.
---
```

The body is instructions written **for Claude** to help users author content with the extension — installation, syntax, configuration options, and usage tips.

## Skill Categories

| Category | Purpose |
|----------|---------|
| `quarto-extensions/` | Skills for using parmsam's Quarto extensions (quiz, flashcards, quizdown) |

Add new category directories as needed.

## Registering a New Skill

After creating the skill directory, add it to `.claude-plugin/marketplace.json`:

```json
{
  "name": "quarto-extensions",
  "skills": [
    "./quarto-extensions/your-new-skill"
  ]
}
```

## Key Conventions

- **Progressive disclosure**: Put large reference content in `references/*.md` and instruct Claude to read those files only when needed. This keeps `SKILL.md` within token limits.
- **Token limits**: Warn when `SKILL.md` exceeds 5,000 tokens / 500 lines, or when the skill `description` exceeds 100 tokens.
- **Testing**: Install locally with `cp -r <category>/<skill> ~/.config/claude-code/skills/` and verify Claude activates it in Claude Code.
