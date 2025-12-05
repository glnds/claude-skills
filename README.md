# Claude Config

Personal Claude Code configuration: MCP server templates, user instructions, and custom skills.

## Repository Contents

### MCP Configuration Template

The `mcp.json` file provides a starting template for MCP server configuration. Copy to your
project's `.mcp.json` and customize as needed.

**Included servers:**

| Server | Purpose |
|--------|---------|
| chrome-devtools | Browser automation via Chrome DevTools |
| playwright | Web testing and automation |
| context7 | Code snippet and documentation lookups |
| serena | IDE assistant integration |
| awslabs.aws-documentation-mcp-server | AWS documentation search and lookup |
| iam-policy-autopilot | IAM policy generation assistance |
| awslabs.aws-api-mcp-server | AWS CLI command execution |
| awslabs.aws-iac-mcp-server | Infrastructure-as-code assistance |
| awslabs.aws-serverless-mcp-server | Serverless AWS service guidance |
| awslabs.dynamodb-mcp-server | DynamoDB schema and query assistance |

### Project Memory

Tools that give AI coding agents persistent memory about your project:

- [Beads](https://github.com/steveyegge/beads): Git-backed distributed issue tracker for AI
  agents. Maintains task dependencies and work state across sessions.

### User Memory

The `user_memory/CLAUDE.md` file contains user-level instructions that apply across all projects.
Copy to `~/.claude/CLAUDE.md` for global application.

**Key directives:**

- Conciseness over grammar in all interactions
- Clarify before implementing (never assume)
- Test-driven development (red-green-refactor)
- GitHub issue integration
- Markdown linting with markdownlint-cli2

### Skills

Skills extend Claude Code with structured workflows. Each skill contains frontmatter metadata,
step-by-step instructions, and MCP integrations.

#### make-note (v1.2)

Creates Obsidian notes with intelligent tag suggestions from existing vault patterns.

**Trigger:** "create a note", "make a note", "save to Obsidian"

#### sparring (v1.0)

Critical thinking partner for technical concepts. Researches Obsidian notes for context,
identifies gaps and assumptions, provides constructive challenge.

**Trigger:** "let's spar on...", "challenge my thinking on..."

#### text-enhancer (v1.0)

Enhances professional/technical text with grammar, clarity, and factual verification while
preserving authentic style.

**Trigger:** "enhance [text]", "polish [text]"

#### verbalized-sampling (v1.0)

Prompt engineering technique to overcome LLM mode collapse by generating multiple responses
with probability distributions.

**Trigger:** "use verbalized sampling", "show multiple responses with probabilities"

#### zellij-config (v1.3)

Zellij terminal multiplexer configuration: layouts, themes, keybindings, plugins.
Config location: `~/dotfiles/.config/zellij/`

**Trigger:** "configure zellij", "create a zellij layout"

## Repository Structure

```text
claude-config/
├── mcp.json               # MCP server configuration template
├── CLAUDE.md              # Repo-level Claude instructions
├── README.md              # This file
├── user_memory/
│   └── CLAUDE.md          # User-level instructions (copy to ~/.claude/)
└── skills/
    └── [skill-name]/
        ├── SKILL.md       # Skill definition
        └── references/    # Optional supporting docs
```

## License

MIT
