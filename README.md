# agent-actions

Reusable GitHub Actions workflow for running AI agents (currently Claude Code) across repos. Define the agent configuration once here, call it from any repo with a thin workflow file.

## Quick Start

1. Add the `ANTHROPIC_API_KEY` secret to your repo (or org-level)
2. Copy `caller-template.yml` to `.github/workflows/agent-actions.yml` in your repo
3. Mention `@claude` in an issue comment or PR review, or add the `claude` label to an issue

## How It Works

This repo contains a [reusable workflow](https://docs.github.com/en/actions/sharing-automations/reusing-workflows) that wraps [`anthropics/claude-code-action`](https://github.com/anthropics/claude-code-action). Your repos call this workflow instead of configuring the action directly — so agent configuration lives in one place.

```
your-repo/.github/workflows/agent-actions.yml  (thin caller, ~15 lines)
    └── calls → dwmkerr/agent-actions/.github/workflows/claude.yml  (all config)
                    └── runs → anthropics/claude-code-action@v1
```

## Triggers

The agent responds to:

- **`@claude` in comments** — issue comments, PR review comments, PR reviews
- **`claude` label** — added to an issue
- **`@claude` in issue body** — when an issue is opened

## Configuration

The caller workflow can override defaults:

| Input | Default | Description |
|-------|---------|-------------|
| `trigger_phrase` | `@claude` | Phrase that triggers the agent |
| `timeout_minutes` | `30` | Max runtime per invocation |
| `allowed_users` | `dwmkerr` | Comma-separated GitHub usernames |

## Examples

- Create an issue with the `claude` label: [example issue](https://github.com/dwmkerr/agent-actions/issues/1)
- Comment `@claude` on a PR: [example PR](https://github.com/dwmkerr/agent-actions/pull/2)

## Requirements

- `ANTHROPIC_API_KEY` secret in each repo (or set at org level)
