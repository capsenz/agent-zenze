# Agent Zenze 🛠️

A collection of skills and configurations for AI agents (Claude Code, Pi).

## Skills

- [add-reference](skills/add-reference/SKILL.md): Scrape or scaffold design styles.
- [use-reference](skills/use-reference/SKILL.md): Load design principles and snippets into context.

## Installation

The recommended way is using the `skills` CLI:

### add-reference
```bash
npx skills add capsenz/agent-zenze@skills/add-reference -g -y
```

### use-reference
```bash
npx skills add capsenz/agent-zenze@skills/use-reference -g -y
```

## Setup

### Configuration

Set `DESIGN_REFERENCES_DIR` in your shell profile (e.g., `~/.zshrc` or `~/.bashrc`):

```bash
export DESIGN_REFERENCES_DIR="/Users/yourname/path/to/design-references"
```

### Symlinking

Link these to your agent configuration directories:

**Claude Code**
```bash
ln -s "$(pwd)/skills/add-reference" ~/.claude/skills/
ln -s "$(pwd)/skills/use-reference" ~/.claude/skills/
```

**Pi Agent**
```bash
ln -s "$(pwd)/skills/add-reference" ~/.agents/skills/
ln -s "$(pwd)/skills/use-reference" ~/.agents/skills/
```

## Usage

- `/add-reference <URL|name>`: Create new reference.
- `/use-reference`: List all available references.
- `/use-reference <name>`: Load specific reference into context.
