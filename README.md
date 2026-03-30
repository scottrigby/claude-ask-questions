# claude-ask-questions

A Claude Code plugin that instructs Claude to use the `AskUserQuestion` tool whenever it needs to ask the user something, instead of embedding questions in plain response text.

## Why

When Claude asks questions inline in its response, the conversation flow breaks — you have to read through prose to find the question, and there's no structured way to respond. The `AskUserQuestion` tool creates a proper interactive prompt, keeping questions distinct from explanations and actions.

This plugin injects that instruction automatically on every prompt via a `UserPromptSubmit` hook, so you never have to think about it.

## Requirements

- [Claude Code](https://claude.ai/code)
- [`jq`](https://jqlang.org/) must be installed and available in your `PATH`

## Installation

Add this repo as a marketplace, then install the plugin:

```bash
/plugin marketplace add scottrigby/claude-ask-questions
/plugin install claude-ask-questions
```

## How it works

A `UserPromptSubmit` hook fires on every prompt and uses `jq` to inject an `additionalContext` instruction into the request. The instruction tells Claude to:

1. Use `AskUserQuestion` for any question it needs to ask
2. If `AskUserQuestion` is a deferred tool (not yet loaded), first call `ToolSearch` to fetch its schema, then call it

No configuration needed — it works automatically after installation.

## License

MIT
