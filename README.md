# My Custom Copilot Agents

A collection of custom agents that work with both **GitHub Copilot** and **Claude Code**, plus a 6-stage feature pipeline that ties them together. The agents and pipeline are universal — no hardcoded paths, no framework assumptions — so they move cleanly from one user story (or project) to the next.

## What's Here

| Agent | Purpose |
|-------|---------|
| `architect-agent` | Plans code structure, files, interfaces, and data models before implementation |
| `docs-agent` | Generates and improves documentation without changing code |
| `reference-coder` | Documents code thoroughly, shows examples first, and explains the "why" |
| `review-agent` | Reviews code for bugs, edge cases, clarity, and maintainability |
| `test-agent` | Writes unit tests and test suites with strong coverage and readable descriptions |

## Installing in a Project

The agents and pipeline are designed to drop in unchanged. The only optional per-project file is `.claude/pipeline.json` for non-standard build/test commands.

### What goes where

| File / folder in this repo | Destination in target project | Tool |
|---|---|---|
| `.claude/agents/*.md` | `<project>/.claude/agents/` (project) **or** `~/.claude/agents/` (user) | Claude Code |
| `.claude/commands/*.md` | `<project>/.claude/commands/` (project) **or** `~/.claude/commands/` (user) | Claude Code |
| `agents/*.agent.md` | `<project>/.github/agents/` | GitHub Copilot |
| `PIPELINE.md` | `<project>/PIPELINE.md` | both |
| `.github/pull_request_template.md` | `<project>/.github/pull_request_template.md` | GitHub |

**Project vs user level:** Putting the Claude Code files under `~/.claude/` makes the agents and `/pipe-*` slash commands available in **every** project automatically — the right choice if you bounce between user stories often. Per-project install is the right choice when a repo wants its own pinned copy. Copilot has no user-level equivalent; copy per repo.

### Copy commands

Clone this repo somewhere, set `AGENTS_REPO` to that path, then run from inside the target project's root.

**Project-level — PowerShell (Windows):**
```powershell
$AGENTS_REPO = "C:\path\to\my-custom-agents"
robocopy "$AGENTS_REPO\.claude\agents"   ".\.claude\agents"   /E
robocopy "$AGENTS_REPO\.claude\commands" ".\.claude\commands" /E
robocopy "$AGENTS_REPO\agents"           ".\.github\agents"   *.agent.md
Copy-Item "$AGENTS_REPO\PIPELINE.md" ".\"
New-Item -ItemType Directory -Force -Path ".\.github" | Out-Null
Copy-Item "$AGENTS_REPO\.github\pull_request_template.md" ".\.github\"
```

**Project-level — bash (macOS/Linux):**
```bash
AGENTS_REPO="$HOME/path/to/my-custom-agents"
mkdir -p .claude/agents .claude/commands .github/agents
cp -r "$AGENTS_REPO"/.claude/agents/.   .claude/agents/
cp -r "$AGENTS_REPO"/.claude/commands/. .claude/commands/
cp    "$AGENTS_REPO"/agents/*.agent.md  .github/agents/
cp    "$AGENTS_REPO"/PIPELINE.md        ./
cp    "$AGENTS_REPO"/.github/pull_request_template.md .github/
```

**User-level (Claude Code only — applies to every project):**
```powershell
# PowerShell
robocopy "$AGENTS_REPO\.claude\agents"   "$HOME\.claude\agents"   /E
robocopy "$AGENTS_REPO\.claude\commands" "$HOME\.claude\commands" /E
```
```bash
# bash
mkdir -p ~/.claude/agents ~/.claude/commands
cp -r "$AGENTS_REPO"/.claude/agents/.   ~/.claude/agents/
cp -r "$AGENTS_REPO"/.claude/commands/. ~/.claude/commands/
```

`PIPELINE.md` and the PR template still go per-project regardless of scope.

### Configure (only if needed)

If the target project uses non-standard build/test commands (e.g., `pnpm`, `poetry run`, `make`), create `.claude/pipeline.json` in that project. See [.claude/pipeline.example.json](.claude/pipeline.example.json) for the schema. The slash commands auto-detect npm/pip/cargo/go projects without any config.

### Verify

- **Claude Code:** open the target project, run `/pipe` (the 6-stage matrix should print) and `/agents` (the five subagents should be listed).
- **Copilot:** open Copilot Chat — the five agents should appear in the dropdown.

### Caveats

- **Copying overwrites.** If the target project already has `.github/pull_request_template.md` or files with the same names in `.claude/agents/` or `.claude/commands/`, back them up before copying.
- **Bash permission prompts.** First time `/pipe-build`, `/pipe-test`, or `/pipe-fix` runs, Claude Code prompts to approve the build/test command. Pre-approve via `/permissions` if you want to skip future prompts (e.g., `Bash(npm test:*)`).
- **No project-specific edits to the agents themselves** — they reference no paths, frameworks, or codebases. If you find yourself editing an agent for a specific project, that's a smell; add a `pipeline.json` instead, or open an issue here.

## The Pipeline

The 6-step workflow below is formalized in [PIPELINE.md](PIPELINE.md), with explicit gates between stages and prompt templates for both tools. In Claude Code, run `/pipe` to see the stage matrix, then `/pipe-plan <feature>` to start — each `/pipe-*` slash command (defined in `.claude/commands/`) wraps the right subagent and prints the gate at the end. In Copilot, follow the prompt templates in [PIPELINE.md](PIPELINE.md).

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
