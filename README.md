# openspec-diagrams

Mermaid diagram generation from OpenSpec change artifacts.

Reads your OpenSpec specs and design documents and generates visual diagrams:

- **From specs**: User journey maps, actor-goal overviews, entity relationships, status lifecycles
- **From design**: Component dependency graphs, sequence diagrams, state machines

## Installation

### Claude Code (plugin from GitHub)

```sh
# Register as a marketplace (project-scoped)
claude plugin marketplace add aklikic/openspec-diagrams-plugin --scope project

# Install the plugin
claude plugin install openspec-diagrams --scope project
```

Or load locally for a single session:
```sh
claude --plugin-dir /path/to/openspec-diagrams-plugin
```

### Copilot / agents (manual from git)

Clone and copy the skill files into your project:
```sh
git clone https://github.com/aklikic/openspec-diagrams-plugin.git
cp -r openspec-diagrams-plugin/skills/diagrams <your-project>/.agents/skills/diagrams
```

For GitHub Copilot specifically:
```sh
cp -r openspec-diagrams-plugin/skills/diagrams <your-project>/.github/copilot/skills/diagrams
```

### Manual copy (any tool)

If you already have the repo locally:
```sh
cp -r skills/diagrams <your-project>/.agents/skills/diagrams
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
openspec-diagrams-plugin/
  .claude-plugin/              # Claude Code plugin manifests
    plugin.json
    marketplace.json
  .apm/skills/diagrams/        # apm-compatible distribution
  skills/diagrams/             # Source skill files
    SKILL.md                   # Skill definition
    spec-diagrams.template.md
    design-diagrams.template.md
  plugin.json                  # Plugin metadata
```

## License

MIT