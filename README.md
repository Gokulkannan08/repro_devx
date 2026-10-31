# repro_devx

Prompts that give your AI coding agent a memory it can read.

Works with Claude Code, Copilot, Codex, and Cursor. Each episode is one
copy-paste prompt that shows you a plan before writing anything.

## Episodes

| Episode | Title | Prompt |
| --- | --- | --- |
| Ep. 01 | My AI forgets everything. So I gave it a filing cabinet. | [`episodes/ep01-filing-cabinet/`](episodes/ep01-filing-cabinet/) |
| Ep. 02 | `/handover` + `/retro` | *planned* |

## Ep. 01 — the context system

Sets up `docs/` (ADRs, architecture, research, runbooks, handovers), a personal
`task/<id>/` folder per task (findings, then plan, then summary), and a
paths-only context map in `CLAUDE.md`, `AGENTS.md`, and
`.github/copilot-instructions.md`.

**Use it:** open
[`episodes/ep01-filing-cabinet/SETUP_PROMPT.md`](episodes/ep01-filing-cabinet/SETUP_PROMPT.md),
copy everything below the line, and paste it into your agent at the repo root.

## Layout

```
episodes/
└── ep01-filing-cabinet/
    ├── README.md         # what the episode covers
    └── SETUP_PROMPT.md   # the copy-paste prompt
```

One folder per episode, named `epNN-<slug>`.

Write-up: https://gokulkannank.substack.com/
