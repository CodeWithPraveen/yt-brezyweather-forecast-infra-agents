# Three agents in one infra repo

Claude Code, GitHub Copilot CLI, and OpenAI Codex CLI on their documented defaults, as of 19 Nov 2026. Defaults change often; check each tool's docs before you rely on a row.

## What each runs without asking

| | Claude Code | Copilot CLI | Codex CLI |
|---|---|---|---|
| Interactive default | Auto mode: a classifier reviews each action | Manual: asks before every write and every command that isn't read-only | Auto preset in a git repo once you trust the folder: workspace-write sandbox, asks before leaving it |
| Sandbox | Off unless you run `/sandbox` | Off (experimental `/sandbox`) | On: no network, `.git` read-only |
| Non-interactive | `claude -p` starts in Manual and denies anything that would prompt | `copilot -p` needs tool grants such as `--allow-tool` | `codex exec` never asks: workspace-write in a trusted repo, read-only in an untrusted one |
| Narrowest grant for the validator | `Bash(az bicep lint *)` and `Bash(az bicep build-params *)` | `shell(az:*)`, which allows every `az` command: only `git` and `gh` match a subcommand | Can't run `az` inside the sandbox; approve it outside, or run the validator as its own step |
| Block a command | Deny rules in `.claude/settings.json`; they win in every mode | `--deny-tool`, for one session; it wins over every allow | A `forbidden` prefix rule in `.codex/rules/`; experimental, trusted projects only |
| Reads `AGENTS.md` | Yes, when there's no `CLAUDE.md` or `CLAUDE.local.md` | Yes, combined with `.github/copilot-instructions.md`, `CLAUDE.md`, and others | Yes, from the repo root down to your folder |

## Which agent for which job

| Job | Pick | Why |
|---|---|---|
| A change where the agent should run your validators and fix its own mistakes, in a repo you trust | Claude Code | Its default ran lint, fixed the error, and stopped short of deploying, with no prompts. Tighten it with project allow and deny rules. |
| A change in an unfamiliar or production repo where you want to approve each command | Copilot CLI | Its default asks before every write and every command that isn't read-only. Answer Yes each time: the second option stops asking for that tool in this repo. |
| Reviewing a repo or PR you didn't write, fenced off from your machine and network | Codex CLI | Its sandbox is on by default. Expect to approve cloud CLIs such as `az`, which write under your home folder, outside it, or validate with the standalone Bicep CLI, which runs inside the workspace-write sandbox when `DOTNET_BUNDLE_EXTRACT_BASE_DIR` points at a temp folder. |
| A CI step | Any of the three | Grant only the commands the step needs, or run the validator as its own step and give the agent its output. A Copilot `shell(az:*)` grant covers every `az` command, so run that step with no Azure login. |
