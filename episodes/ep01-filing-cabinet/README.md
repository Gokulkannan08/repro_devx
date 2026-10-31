# Ep. 01 — My AI forgets everything. So I gave it a filing cabinet.

*AI Setup, Ep. 01*

Every new agent session starts from zero. This episode moves the context into the
repo, where the agent can read it: `docs/` for shared knowledge, a local
`task/<id>/` folder per task, and a paths-only map in `CLAUDE.md`.

## The prompt

[`SETUP_PROMPT.md`](SETUP_PROMPT.md) — copy everything below the line, paste it
into your agent at the root of your repo. It shows a plan before writing anything.

Works with Claude Code, Copilot, Codex, and Cursor.

## What it creates

| Path | Holds |
| --- | --- |
| `docs/adr/` | one decision per file, immutable once accepted |
| `docs/architecture/` | system overview, components, data flow |
| `docs/research/` | spikes and investigations |
| `docs/runbook/` | build, test, lint, deploy commands |
| `docs/todo/` | handover notes between sessions |
| `task/<id>/` | local-only working notes: findings → plan → summary |
| `CLAUDE.md` | the paths-only context map (mirrored to `AGENTS.md`, `.github/copilot-instructions.md`) |

`docs/` is committed. `task/` is excluded locally via `.git/info/exclude`, not
`.gitignore`, so it never reaches the shared repo.

## Links

- Write-up: https://gokulkannank.substack.com/
