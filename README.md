# openspec-diagrams

Mermaid diagram generation from OpenSpec change artifacts.

Reads your OpenSpec specs and design documents and generates visual diagrams:

- **From specs**: User journey maps, actor-goal overviews, entity relationships, status lifecycles
- **From design**: Component dependency graphs, sequence diagrams, state machines

## Installation

### Claude Code (plugin)

```sh
# From a git repo
claude plugin marketplace add <github-user>/openspec-diagrams
claude plugin install openspec-diagrams

# Or load locally for a session
claude --plugin-dir /path/to/openspec-diagrams
```

### Copilot / agents (via apm)

If published to a marketplace:
```sh
apm install openspec-diagrams@<marketplace-name>
```

### Manual copy

Copy the skill files into your project:
```sh
cp -r skills/diagrams <your-project>/.agents/skills/diagrams
```

Or for Claude Code specifically:
```sh
cp -r skills/diagrams <your-project>/.claude/skills/diagrams
```

## Usage

In your AI agent, invoke the skill:
```
/diagrams <change-name>
```

The skill will:
1. Read the specs and/or design from the named OpenSpec change
2. Generate `spec-diagrams.md` and/or `design-diagrams.md` in the change directory
3. Report which diagram sections were generated or omitted

## Requirements

- OpenSpec CLI (`brew install openspec`)
- An active OpenSpec change with specs and/or design artifacts

## Structure

```
openspec-diagrams/
  .claude-plugin/          # Claude Code plugin manifests
    plugin.json
    marketplace.json
  .apm/                    # apm distribution (Copilot/agents)
    skills/diagrams/
  skills/diagrams/         # Source skill files
    SKILL.md               # Skill definition
    spec-diagrams.template.md
    design-diagrams.template.md
  plugin.json              # apm plugin metadata
```

## License

MIT