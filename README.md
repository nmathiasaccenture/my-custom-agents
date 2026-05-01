# My Custom Copilot Agents

A collection of custom agents for GitHub Copilot.

## What's Here

| Agent | Purpose |
|-------|---------|
| `architect-agent` | Plans code structure, files, interfaces, and data models before implementation |
| `docs-agent` | Generates and improves documentation without changing code |
| `reference-coder` | Documents code thoroughly, shows examples first, and explains the "why" |
| `review-agent` | Reviews code for bugs, edge cases, clarity, and maintainability |
| `test-agent` | Writes unit tests and test suites with strong coverage and readable descriptions |

## Quick Start

1. Copy any `.agent.md` file into your project's `.github/agents/` folder.
2. Open GitHub Copilot Chat in VS Code.
3. Select the agent from the dropdown menu.
4. Follow the workflow below to use all agents together for a new feature.

## The Agent Workflow

Use these agents together in a repeatable 6-step process:

1. **architect-agent** — Ask it to plan files, interfaces, and data models for a feature.
2. **reference-coder** — Ask it to build the code following the plan with full documentation.
3. **test-agent** — Ask it to write unit tests covering edge cases.
4. **review-agent** — Ask it to review the code for issues and clarity.
5. **reference-coder** again — Ask it to fix issues from the review.
6. **docs-agent** — Ask it to update the README with the new feature.

### Example: Discount Code System

Use this example prompts at each step.

1. **architect-agent**
   - Prompt: "Plan a discount code system with files, interfaces, and data models. Include a discount code entity, validation rules, and checkout integration."
2. **reference-coder**
   - Prompt: "Build the discount code system using the plan from architect-agent. Add full documentation and examples for each function."
3. **test-agent**
   - Prompt: "Write unit tests for the discount code system. Cover valid codes, expired codes, invalid codes, and edge cases."
4. **review-agent**
   - Prompt: "Review the discount code implementation and tests for bugs, clarity, and maintainability."
5. **reference-coder** (again)
   - Prompt: "Fix the issues found by review-agent in the discount code system. Keep documentation and examples updated."
6. **docs-agent**
   - Prompt: "Update the README to describe the new discount code feature, how to use it, and any configuration details."

### Example: Login Feature

1. **architect-agent**
   - Prompt: "Plan a login feature with the required files, authentication interface, user session model, and error handling."
2. **reference-coder**
   - Prompt: "Implement the login feature from the architect-agent plan with full documentation and examples."
3. **test-agent**
   - Prompt: "Write unit tests for login success, invalid credentials, locked accounts, and input validation."
4. **review-agent**
   - Prompt: "Review the login code and tests for security issues, clarity, and edge case handling."
5. **reference-coder**
   - Prompt: "Apply review-agent feedback to the login implementation and keep docs up to date."
6. **docs-agent**
   - Prompt: "Update the README with the login feature overview, usage examples, and any configuration notes."

## Agent Matrix

| Agent | Purpose | Tools | When to Use |
|-------|---------|-------|--------------|
| `architect-agent` | Plan structure, files, interfaces, and data models | `read`, `search` | Before writing any implementation code |
| `reference-coder` | Implement code with documentation and examples | `read`, `search`, `edit` | After planning and when writing code |
| `test-agent` | Write unit tests for normal and edge cases | `read`, `search`, `edit` | After implementation and before review |
| `review-agent` | Review code for bugs, readability, and maintainability | `read`, `search` | After tests are in place and before fixes |
| `docs-agent` | Generate or update documentation and README content | `read`, `search`, `edit` | After implementation and fixes, to document the feature |

## Creating Your Own Agents

Create a new `.agent.md` file with:
- YAML frontmatter (`name`, `description`, `tools`)
- Plain English instructions

The agents folder structure should be: `.github/agents/your-agent-name.agent.md`

## Requirements

- GitHub Copilot subscription (Free, Pro, or Enterprise)
- VS Code with Copilot extension installed

## License

Feel free to use and modify these agents for your own projects.
