---
name: xcode-mcp-workflow
description: Use when working in an Xcode project and an Xcode MCP server is available, especially for browsing the project navigator, reading or editing project files, building the active workspace, running Swift or XCTest targets, inspecting build diagnostics, rendering SwiftUI previews, or executing Swift snippets in file context.
---

# Xcode MCP Workflow

## Overview

Prefer Xcode MCP tools over shell commands when the task is scoped to an open Xcode workspace or project. Use shell fallback only when the Xcode MCP server is unavailable, no matching workspace is open, or the required action is outside the Xcode MCP surface.

## Workflow

1. Start with `XcodeListWindows` to confirm that a suitable Xcode window is open and to get the `tabIdentifier`.
2. Stay inside the Xcode toolchain for project-scoped work once a `tabIdentifier` exists.
3. Use shell tools only for work outside the Xcode project, unsupported actions, or situations where no suitable Xcode workspace is available.

## Task Map

| Task | Prefer | Notes |
| --- | --- | --- |
| Find the active workspace, project, or scheme context | `XcodeListWindows` | Always do this first. |
| Browse groups and files from the project navigator | `XcodeLS`, `XcodeGlob` | Prefer project organization over raw filesystem paths. |
| Search source inside the Xcode project | `XcodeGrep` | Prefer this over shell search for project files. |
| Read a project file | `XcodeRead` | Use project-relative paths returned by Xcode tools. |
| Edit a project file | `XcodeUpdate`, `XcodeWrite` | Keep project edits in the Xcode toolchain when possible. |
| Move or rename files | `XcodeMV` | Preserves project organization. |
| Create folders or groups | `XcodeMakeDir` | Updates project structure. |
| Remove files or groups | `XcodeRM` | Use only when deletion is explicitly required. |
| Build the active project or scheme | `BuildProject` | Use before guessing about compiler errors. |
| Inspect build failures | `GetBuildLog`, `XcodeListNavigatorIssues` | Read diagnostics before editing. |
| Refresh file diagnostics after an edit | `XcodeRefreshCodeIssuesInFile` | Useful during focused iteration. |
| Discover tests | `GetTestList` | Use before targeted test runs when identifiers are unknown. |
| Run one or a few tests | `RunSomeTests` | Prefer targeted runs during iteration. |
| Run the full test suite | `RunAllTests` | Use for broader verification. |
| Render SwiftUI previews | `RenderPreview` | Render before describing UI output. |
| Execute Swift in file context | `ExecuteSnippet` | Use for quick runtime checks or inspections. |
| Search Apple framework documentation | `DocumentationSearch` | Use for framework and API questions tied to Xcode work. |

## Operating Rules

- Get a `tabIdentifier` before using Xcode tools that require it.
- Prefer Xcode project paths over filesystem paths for Xcode MCP actions.
- For build or test failures, inspect logs and diagnostics before changing code.
- For preview requests, run `RenderPreview` before claiming what the UI looks like.
- For test requests, call `GetTestList` first when the exact test identifier is not already known.
- If the task spans files outside the Xcode project, say that clearly and use shell tools only for that part.

## Fallback Rules

- If `XcodeListWindows` shows no suitable workspace, state that Xcode MCP cannot operate on the project and fall back to shell tools.
- If a needed action is not covered by the available Xcode MCP tools, explain the gap briefly and use the smallest shell fallback that solves it.
- Do not switch to shell out of habit for reads, searches, builds, tests, previews, or diagnostics that Xcode MCP already supports.

## Common Mistakes

- Using `rg`, `cat`, filesystem edits, or generic shell commands for project files before checking whether Xcode MCP can do the same job.
- Running broad shell builds when `BuildProject` and Xcode diagnostics would provide better context.
- Guessing test identifiers instead of calling `GetTestList`.
- Describing a SwiftUI preview without rendering it first.
