# Contributing

Thank you for helping improve this custom agents collection.

## Create a New Agent

1. Copy an existing `.agent.md` file or use the `templates/agent-template.md` starter.
2. Name your file `your-agent-name.agent.md`.
3. Add it to the repository root `agents/` folder or, for your own project, `.github/agents/`.

## Required Structure

Each agent file must begin with YAML frontmatter:

```yaml
---
name: your-agent-name
description: Brief description of what the agent does.
tools: ['read', 'search', 'edit']
model: gpt-4o-mini
---
```

- `name`: Unique agent identifier.
- `description`: One-line summary of purpose.
- `tools`: List only the tools the agent needs.
- `model`: The model the agent should use.

After the frontmatter, add plain-language instructions.

## Best Practices for Agent Instructions

- Be specific: tell the agent exactly what it should do.
- Use examples: show sample prompts or expected outputs.
- Set boundaries: explain what the agent should not do.
- Keep instructions organized with sections like `Core Instructions`, `Workflow`, and `Boundaries`.

Example:

```md
# Core Instructions
- Read the code and suggest improvements for clarity.

# Workflow
- First inspect the files.
- Then identify issues and suggest fixes.

# Boundaries
- Do not modify files directly unless asked.
```

## Test an Agent Before Sharing

1. Open the agent in GitHub Copilot Chat.
2. Run a realistic prompt that matches the agent's purpose.
3. Confirm the agent uses the intended tools and stays within its scope.
4. Fix wording if the agent is too broad or too vague.

## Submit Improvements

1. Fork the repository.
2. Create a new branch for your changes.
3. Add or update files.
4. Open a pull request back to `main`.
5. Describe your changes clearly in the PR description.
