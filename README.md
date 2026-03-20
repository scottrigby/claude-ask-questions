# ask-questions

A Claude Code plugin that instructs Claude to always use the `AskUserQuestion` tool when it needs to ask the user something, rather than asking in plain response text.

## Why

When Claude asks questions in plain text, any hooks listening for `AskUserQuestion` (e.g. notification hooks that alert you when Claude is waiting for input) never fire — because no tool call was made. By enforcing structured tool use, every question reliably triggers downstream hooks.

This makes Claude sessions predictable for automation: you can depend on `AskUserQuestion` always being the channel through which Claude asks for input.

## How it works

A `UserPromptSubmit` hook injects an `additionalContext` instruction into every prompt before Claude processes it. The instruction tells Claude to use `AskUserQuestion` (and to load it via `ToolSearch` first if it is deferred).

## Usage

Install from GitHub:

```bash
claude plugin install github:scottrigby/claude-ask-questions
```

This adds it to your user settings (`~/.claude/settings.json`). To scope it to a single project instead, install with `--scope project`.

## Workaround status

This plugin exists because Claude Code has no `UserInputRequired` hook event — there is currently no reliable built-in way to detect when Claude is waiting for user input. Enforcing `AskUserQuestion` via `UserPromptSubmit` is a workaround that makes question detection predictable.

Track [anthropics/claude-code#10168](https://github.com/anthropics/claude-code/issues/10168): once a `UserInputRequired` hook is available, notification hooks can listen for it directly and this plugin will no longer be needed.

## Disabling

Uninstall with `claude plugin uninstall claude-ask-questions`.
