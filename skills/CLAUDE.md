# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## CLAUDE usage scope

- CLAUDE should maintain the README.md
- CLAUDE should focus on review skills not writing them
- CALUDE can provide small suggestion but should not write or rewrite entire skills

## Repository Purpose

This is a Claude Code skills repository. Skills are custom capabilities that extend Claude Code's functionality through structured markdown files with frontmatter metadata and workflow instructions.

## Architecture

### Skill Structure

Each skill is organized in its own directory with a `SKILL.md` file containing:

1. **Frontmatter** (YAML):
   - `name`: Skill identifier (lowercase, hyphenated)
   - `description`: When and how to invoke the skill
   - `license`: License type
   - `metadata.version`: Version number

2. **Workflow**: Step-by-step instructions for skill execution
   - MCP tool integrations (e.g., `mcp-obsidian`)
   - User interaction patterns
   - Content formatting requirements

### Skill Development Pattern

Skills follow this pattern:

- Gather context (via MCP tools or user input)
- Present options to user for confirmation
- Execute action based on confirmed choices
- Confirm completion with details

Example: `make-note` skill analyzes existing vault tags → suggests relevant tags → waits for user confirmation → creates note with confirmed tags.

## Skill Guidelines

### Frontmatter Description

Write descriptions that clearly indicate:

- Trigger phrases (e.g., "Use when the user asks to create a note")
- Primary function
- Key workflow steps or unique features

### User Interaction

- Present options as numbered lists for easy selection
- Always wait for explicit user confirmation before executing actions
- Confirm completion with relevant details (paths, applied settings, etc.)

### MCP Integration

Skills leverage MCP servers for external integrations:

- Reference MCP tools by server and tool name
- Document expected return formats
- Handle tool responses appropriately

### Content Formatting

When skills generate structured content:

- Use YAML frontmatter for metadata
- Follow consistent date formats (DD-MM-YYYY HH:MM)
- Include appropriate markdown structure (headings, lists, links)
- Only include optional sections when relevant
