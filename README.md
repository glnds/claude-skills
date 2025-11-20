# Claude Skills

Custom skills that extend Claude Code's functionality through structured workflows and integrations.

## What are Skills?

Skills are capabilities defined in markdown files that Claude Code can invoke
to perform specialized tasks. Each skill contains:

- **Frontmatter metadata**: Name, description, version, license
- **Workflow instructions**: Step-by-step execution logic
- **Integration patterns**: MCP server tools, user interactions

## Available Skills

### make-note

**Version:** 1.2
**License:** MIT

Creates structured notes in Obsidian with intelligent tag suggestions based on existing vault patterns.

**Invocation:** "create a note", "make a note", "save content to Obsidian"

**Key Features:**

- Analyzes existing vault to identify common tag patterns
- Suggests 3-5 relevant tags based on note content
- Waits for user confirmation before creating note
- Creates notes in Resources folder with proper frontmatter
- Supports hierarchical tags (e.g., `ai/code-agent`, `dpg/strategy`)

**MCP Integration:** `mcp-obsidian` (tag analysis, note creation)

---

### sparring

**Version:** 1.0
**License:** MIT

Critical thinking partner for technical concepts and strategy development. Researches your Obsidian notes to understand context, identifies gaps and assumptions, then provides constructive challenge.

**Invocation:** "let's spar on...", "help me think through...", "challenge my thinking on..."

**Key Features:**

- Searches Obsidian vault for relevant past thinking and decisions
- Consults AWS documentation for technical accuracy (when applicable)
- Identifies gaps, questions assumptions, surfaces contradictions
- Connects ideas across your knowledge base
- Provides direct, honest feedback matching your communication style

**MCP Integration:** `mcp-obsidian` (context research), `aws-documentation` (technical validation)

---

### text-enhancer

**Version:** 1.0
**License:** MIT

Enhances professional and technical text with grammar correction, clarity improvements, and factual verification while preserving your authentic style.

**Invocation:** "enhance [text]", "polish [text]"

**Key Features:**

- Corrects grammar and improves vocabulary
- Enhances clarity and flow while maintaining conciseness
- Verifies technical accuracy (especially AWS services)
- Preserves authentic voice and style
- Never uses em dashes (prefers colons for natural flow)
- Searches Obsidian for related context and consistency

**MCP Integration:** `mcp-obsidian` (related content), `aws-documentation` (technical verification)

---

### verbalized-sampling

**Version:** 1.0
**License:** MIT

Prompt engineering technique to overcome mode collapse in LLM responses by generating multiple answers with associated probabilities.

**Invocation:** "use verbalized sampling", "show me multiple responses with probabilities"

**Key Features:**

- Generates 5-10 alternative responses with probability distributions
- Explores beyond top-ranked answers to discover unexpected solutions
- Supports filtering by probability ranges (high, mid, low, tail distribution)
- Useful for creative tasks, brainstorming, and complex decision-making
- Training-free technique compatible with all major LLMs

**MCP Integration:** None (pure prompting technique)

---

### zellij-config

**Version:** 1.3
**License:** MIT

Comprehensive Zellij terminal multiplexer configuration management. Handles config.kdl files, layouts, themes, keybindings, plugins, and workflow automation.

**Invocation:** "configure zellij", "create a zellij layout", "customize zellij theme"

**Key Features:**

- Creates and modifies config.kdl files
- Builds layouts with command panes, tabs, and templates
- Customizes themes and keybindings
- Sets up plugins and workspace automation
- Works with configuration in `~/dotfiles/.config/zellij/`
- Includes bundled reference documentation for KDL syntax

**MCP Integration:** None (file manipulation only)

## Repository Structure

```text
claude-skills/
├── CLAUDE.md              # Guidance for Claude Code
├── README.md              # This file
├── LICENSE
├── user_memory/           # User-specific Claude instructions
│   └── CLAUDE.md
└── skills/
    └── [skill-name]/
        ├── SKILL.md       # Skill definition with frontmatter and workflow
        └── references/    # Optional: Supporting documentation
            └── *.md
```

## Creating Skills

Each skill requires:

1. Directory named after skill (lowercase, hyphenated)
2. `SKILL.md` with YAML frontmatter and workflow
3. Clear trigger phrases in description
4. User confirmation points for actions
5. MCP tool integration (if external systems involved)

See [CLAUDE.md](CLAUDE.md) for detailed guidelines.

## License

MIT
