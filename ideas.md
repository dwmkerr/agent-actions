# Ideas

## Agent Dashboard

Build an interface to see all agent actions in progress across repos. Options:

- **GitHub Actions dashboard** — already shows running workflows, but scattered across repos. A custom view using `gh run list` across repos could aggregate this.
- **Simple web dashboard** — poll the GitHub API for workflow runs matching `agent-actions` across all dwmkerr repos. Show status, duration, which issue/PR triggered it, and link to the conversation.
- **Slack/Discord notifications** — post to a channel when an agent starts, finishes, or fails. Lightweight alternative to a full dashboard.
- **GitHub Projects board** — auto-add issues/PRs with active agent runs to a project board for tracking.

## Future Improvements

- **MCP server configuration** — add centralized MCP config that gets injected into the agent environment.
- **Custom instructions** — pass a shared `CLAUDE.md` or system prompt from this repo to all agent runs.
- **Multi-agent support** — add workflows for other agents beyond Claude Code (e.g. Codex, Gemini).
- **Per-repo overrides** — allow repos to provide a local config file that merges with the central defaults.
- **Sync script** — `gh`-based script to deploy/update the caller workflow and secrets across all target repos.
- **Cost tracking** — log token usage per run and aggregate across repos.
- **Custom GitHub App identity** — register `@dwmkerr-agent` as a GitHub App for proper @mention support and its own avatar.
