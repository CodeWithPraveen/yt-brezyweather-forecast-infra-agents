# BrezyWeather forecast infrastructure

Bicep for the forecast API's data storage, deployed to the resource group `rg-brezyweather-forecast-prod`. This file is the one set of instructions for every coding agent on the team.

## Rules

- Change only `main.bicep` and `main.prod.bicepparam`.
- Never run a command that changes Azure: no `az deployment group create`, and no `az group` or `az resource` commands. If a change needs a deploy, print the what-if command for a human and stop.
- Don't commit or push.

## Validate every change

Run both commands and fix every error before you finish:

```bash
az bicep lint --file main.bicep
az bicep build-params --file main.prod.bicepparam --stdout
```
