---
name: xcode-mcp-workflow
description: Use when working inside an open Xcode workspace or project and an Xcode MCP server is available. Prefer over shell commands for any project-scoped task — navigation, file edits, builds, diagnostics, tests, previews, or Swift snippet execution.
---

# Xcode MCP Workflow

## Overview

Prefer Xcode MCP tools over shell commands when the task is scoped to an open Xcode workspace or project. Use shell fallback only when the Xcode MCP server is unavailable, no matching workspace is open, or the required action is outside the Xcode MCP surface.

## When to Use

**Use this skill when:**
- An Xcode MCP server is connected and a workspace or project is open in Xcode.
- The task involves navigating, reading, editing, building, testing, or debugging Swift/SwiftUI/Objective-C source inside that project.
- You are about to reach for `cat`, `rg`, `find`, or a shell build command for something that is scoped to the Xcode project.

**Do not use this skill when:**
- No Xcode window is open or `XcodeListWindows` returns no suitable workspace.
- The task is entirely outside the Xcode project (e.g., shell scripts, CI config, non-Xcode files).
- The required action has no equivalent in the Xcode MCP tool surface.

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
- `XcodeGrep` searches only the Xcode project navigator tree. If the project uses Swift Packages whose sources live outside the `.xcodeproj` structure (e.g. a local `Packages/` directory), try `XcodeGrep` first and fall back to `Grep` with the repo root if it returns no results.

## Common Mistakes

| Mistake | Reality |
|---------|---------|
| Going straight to `Grep`/`rg` for Swift source search | Try `XcodeGrep` first; fall back to `Grep` only if the file lives in a resolved Swift Package outside the navigator tree. |
| Using `Read`/`cat` to read a project file whose path is already known | `XcodeRead` is still preferred — it operates on project-relative paths and keeps work inside the Xcode toolchain. |
| Running `xcodebuild` in the shell | Use `BuildProject`; it triggers the Xcode build pipeline and returns structured diagnostics. |
| Guessing test identifiers | Call `GetTestList` first; test identifiers change when tests are added or renamed. |
| Describing a SwiftUI preview without rendering | Run `RenderPreview` before making any claim about what the UI looks like. |
