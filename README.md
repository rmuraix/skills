# Skills

A collection of custom skills for [Claude Code](https://claude.ai/code).

## About

This repository contains Agent skills — reusable prompt-based commands that extend Claude Code's capabilities. Skills let you define specialized behaviors, automate workflows, and invoke complex tasks with a simple `/skill-name` command.

## Usage

### Installing skills

To use skills from this repository, add the path to your Claude Code configuration:

```bash
claude config add skillsDirectory /path/to/skills
```

Or clone this repository and reference it:

```bash
git clone https://github.com/rmuraix/skills ~/.claude/skills
```

### Invoking a skill

Skills are invoked using the `/` prefix in a Claude Code session:

```
/skill-name [optional arguments]
```

### Creating a new skill

A skill is a Markdown file placed in the skills directory. Create a new file `my-skill.md`:

```markdown
---
description: A short description of what this skill does
---

Your skill prompt here. Describe what Claude should do when this skill is invoked.
```

## Contributing

Your contribution is always welcome. Please read [Contributing Guide](https://github.com/rmuraix/.github/blob/main/.github/CONTRIBUTING.md).
