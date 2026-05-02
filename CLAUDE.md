# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository is a collection of **agent skills** — reusable prompt-based commands that extend coding-agent capabilities. Skills are installed by users into their Claude Code (or compatible agent) environment via:

```bash
gh skill install rmuraix/skills --agent claude-code
# or
npx skills add rmuraix/skills
```

There is no build system, package manager, or test runner for the repository itself. Development consists of authoring and editing Markdown files.

## Skill File Structure

Each skill lives at `skills/<skill-name>/SKILL.md` and must begin with YAML frontmatter:

```markdown
---
name: skill-name
description: One-sentence trigger description used by the agent to decide when to invoke the skill. Should specify trigger conditions explicitly.
license: MIT
---

# Skill Title

...skill content...
```

- `name`: matches the directory name and the `/skill-name` slash command users invoke
- `description`: critical — the agent reads this to decide when to auto-trigger the skill; be explicit about trigger conditions

## Evals (Optional)

Skills may include `skills/<skill-name>/evals/evals.json` to define automated evaluation cases:

```json
{
  "skill_name": "skill-name",
  "evals": [
    {
      "id": 0,
      "prompt": "User prompt that should trigger the skill",
      "expected_output": "What a correct response looks like",
      "files": [],
      "assertions": [
        {
          "id": "assertion-id",
          "description": "What this checks",
          "type": "contains" | "not_primary_recommendation",
          "value": "string to check for"
        }
      ]
    }
  ]
}
```

Not all skills require evals; add them when the correct behavior is easy to specify and verify programmatically.

## Adding a New Skill

1. Create `skills/<skill-name>/SKILL.md` following the frontmatter structure above.
2. Write the skill body in English as clear, actionable instructions for an agent — not documentation for humans.
3. Optionally add `skills/<skill-name>/evals/evals.json`.
4. All content in this repository is expected to be written in English.
