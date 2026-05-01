# My Custom Copilot Agents

A collection of custom agents for GitHub Copilot.

## What's Here

| Agent | Purpose |
|-------|---------|
| eference-coder | Documents everything, shows examples first, writes for future developers |
| docs-agent | Generates and improves documentation without changing code |
| 	est-agent | Writes unit tests covering edge cases and normal paths |

## How to Use

1. Copy any .agent.md file into your project's .github/agents/ folder
2. Open GitHub Copilot Chat in VS Code
3. Select the agent from the dropdown menu
4. Start coding — the agent will follow its instructions automatically

## Creating Your Own Agents

Create a new .agent.md file with:
- YAML frontmatter (name, description, tools)
- Plain English instructions

The agents folder structure should be: .github/agents/your-agent-name.agent.md

## Requirements

- GitHub Copilot subscription (Free, Pro, or Enterprise)
- VS Code with Copilot extension installed

## License

Feel free to use and modify these agents for your own projects.
