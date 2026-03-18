# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Claude Code skill plugin** (`xcode-mcp-workflow`) that teaches AI agents to prefer Xcode MCP tools over shell commands when working inside an open Xcode workspace or project. The plugin has no compiled code — it is purely documentation, metadata, and configuration.

## Repository Structure

```
.claude-plugin/
  plugin.json         # Plugin manifest (name, version, author, keywords)
  marketplace.json    # Self-hosted marketplace catalog entry
skills/
  xcode-mcp-workflow/
    SKILL.md          # Core skill: workflow rules, task map, fallback logic
    agents/
      openai.yaml     # Agent interface: trigger conditions, MCP tool dependencies
```

## No Build System

There is no build, lint, or test toolchain. Changes are validated manually by installing the plugin into Claude Code and invoking the skill against a live Xcode MCP server.

## Installation (for manual testing)

```bash
# Via local path
cc --plugin-dir /absolute/path/to/xcode-mcp-workflow

# Via self-hosted marketplace
/plugin marketplace add leepepe/xcode-mcp-workflow
```

## Key Authoring Notes

- **SKILL.md** is the source of truth for agent behavior. It defines: when to use the skill, the Xcode MCP task map, operating rules, fallback rules, and common mistakes.
- **openai.yaml** declares the MCP tool dependencies (`xcode` server required) and the implicit trigger condition. Keep `requires_mcp` in sync with actual tool names used in SKILL.md.
- **plugin.json** keywords affect discoverability in the marketplace — update them if the skill scope changes.
- The skill's fallback chain: try Xcode MCP first → suggest installing Xcode MCP if missing → fall back to shell tools only as last resort.
