# agent-actions

[![Agent Actions - Running](https://img.shields.io/badge/Agent_Actions-Running-green?logo=github-actions&logoColor=white)](https://github.com/dwmkerr/agent-actions/actions/workflows/agent-actions.yml)

Reusable GitHub Actions workflow for running AI agents (currently Claude Code) across repos. Define the agent configuration once here, call it from any repo with a thin workflow file.

## Quick Start

1. Add the `ANTHROPIC_API_KEY` secret to your repo (or org-level)
2. Copy `caller-template.yml` to `.github/workflows/agent-actions.yml` in your repo
3. Mention `@claude` in an issue comment or PR review, or add the `claude` label to an issue

## Badge

Add this badge to your README to show that agent actions are configured:

```markdown
[![Agent Actions - Running](https://img.shields.io/badge/Agent_Actions-Running-green?logo=github-actions&logoColor=white)](https://github.com/{owner}/{repo}/actions/workflows/agent-actions.yml)
```

Replace `{owner}/{repo}` with your repository path.

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

## Important: Agent Identity

By default, the agent operates using the automatic `GITHUB_TOKEN` — which means commits and API calls appear as the **github-actions bot**, scoped to the triggering repo. It does _not_ run as your personal account, but it does have the permissions granted in the workflow (`contents: write`, `pull-requests: write`, `issues: write`).

To use a specific identity (e.g., a dedicated bot account), pass a Personal Access Token or GitHub App token as `github_token`, and set `bot_name`/`bot_id` to match:

```yaml
- uses: anthropics/claude-code-action@v1
  with:
    github_token: ${{ secrets.MY_BOT_PAT }}
    bot_name: "my-bot"
    bot_id: "12345678"
```

To limit scope, reduce the `permissions` block in the reusable workflow or use a fine-grained PAT with minimal permissions.

## What the Agent Can Do

- **Comment on issues and PRs** — respond to questions, explain code, suggest fixes
- **Read and review pull requests** — analyze diffs, leave review comments, approve/request changes
- **Push commits** — make code changes directly on branches

See the [claude-code-action docs](https://github.com/anthropics/claude-code-action) for the full list of capabilities and configuration options.

## Examples

- Create an issue with the `claude` label: [example issue](https://github.com/dwmkerr/agent-actions/issues/1)
- Comment `@claude` on a PR: [demo PR](https://github.com/dwmkerr/agent-actions/pull/2) — try commenting `@claude describe the changes in this PR`

## Requirements

- `ANTHROPIC_API_KEY` secret in each repo (or set at org level)
