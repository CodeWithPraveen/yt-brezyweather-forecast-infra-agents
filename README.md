# BrezyWeather Forecast Infrastructure: Three Agents

Demo repository for the Code with Praveen video "Claude Code vs. Copilot vs. Codex for DevOps". The forecast storage Bicep with one `AGENTS.md` that Claude Code, GitHub Copilot CLI, and Codex CLI all read, and no tool-specific config. `comparison.md` holds the results.

## Requirements

- Claude Code 2.1.302 or later, GitHub Copilot CLI 1.0.87 or later, Codex CLI 0.170.0 or later, each signed in
- A GitHub Copilot plan for Copilot CLI (every plan includes it, Copilot Free too)
- Azure CLI 2.89.0 or later with Bicep; no Azure subscription is used
- Tested on macOS, 19 Nov 2026 (Claude Code 2.1.328, Copilot CLI 1.0.115, Codex CLI 0.170.0). `main.bicep` fails lint on purpose: its storage API version is older than the linter's 730-day window

## Setup

```bash
git clone https://github.com/CodeWithPraveen/yt-brezyweather-forecast-infra-agents && cd yt-brezyweather-forecast-infra-agents
az bicep lint --file main.bicep        # fails on use-recent-api-versions: the starting point
claude                                 # or: copilot, or: codex; then paste the prompt below
git checkout -- . && git clean -fd     # reset before the next agent
```

The prompt, the same for every agent:

```text
Turn on blob soft delete for the forecast storage account, with the retention in days as a parameter: 7 by default and 30 in prod. Validate the change the way AGENTS.md says. Don't deploy anything.
```

Each agent acts with your local CLIs and their sign-ins. Don't run this repo's prompt with an Azure login that can change production.

## Tear down

Nothing to delete. Remove the folder when you're done.
