# xcode-mcp-workflow

`xcode-mcp-workflow` is a single-skill Claude Code plugin that teaches the agent to stay inside the Xcode MCP toolchain whenever work is happening inside an open Xcode workspace.

It improves behavior for tasks like project navigation, project-scoped search, file reads and edits, builds, diagnostics, targeted tests, SwiftUI preview rendering, and Swift snippet execution.

## Why this plugin exists

Without a focused workflow skill, agents often fall back to shell habits such as raw filesystem search, `cat`, `rg`, or generic build commands even when an Xcode MCP server is available and can provide better context. This plugin makes the MCP-first workflow explicit and discoverable.

## What gets installed

- One skill: `xcode-mcp-workflow`
- UI metadata in `agents/openai.yaml` for skill discovery and invocation
- Claude plugin metadata in `.claude-plugin/plugin.json`
- A self-hosted marketplace catalog in `.claude-plugin/marketplace.json`

## Installation

### Claude Code via self-hosted marketplace

```bash
/plugin marketplace add leepepe/xcode-mcp-workflow
/plugin install xcode-mcp-workflow@xcode-mcp-workflow
```

### Claude Code via local path

```bash
cc --plugin-dir /absolute/path/to/xcode-mcp-workflow
```

## Searchability

The plugin manifest and skill metadata include search terms for:

- Xcode MCP
- Swift and SwiftUI
- iOS and macOS projects
- Builds, tests, diagnostics, previews, and snippet execution

That improves discovery in plugin marketplaces and in-product skill search.

## Requirements

- Claude Code with plugin support
- An available Xcode MCP server
- An open Xcode workspace or project when invoking the workflow

## Repository layout

```text
xcode-mcp-workflow/
├── .claude-plugin/
│   ├── marketplace.json
│   └── plugin.json
├── skills/
│   └── xcode-mcp-workflow/
│       ├── agents/
│       │   └── openai.yaml
│       └── SKILL.md
├── LICENSE
└── README.md
```

## License

MIT
