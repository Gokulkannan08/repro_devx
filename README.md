# repro_devx

Prompts and skills that give your AI coding agent a memory it can read.

Works with Claude Code, Copilot, Codex, and Cursor. Everything here is plain
markdown, and nothing changes your repo until you've seen a plan or approved it.

## Episodes

| Episode | Title | Use |
| --- | --- | --- |
| Ep. 01 | My AI forgets everything. So I gave it a filing cabinet. | [`episodes/ep01-filing-cabinet/`](episodes/ep01-filing-cabinet/) |
| Ep. 02 | I typed "hi". It cost 18k tokens. | write-up only |
| Ep. 03 | 3 hours in, my AI undid my own fix. | [`skills/handover/`](skills/handover/SKILL.md) |
| Ep. 04 | Same mistake. Third time this week. | [`skills/retro/`](skills/retro/SKILL.md) |

## Ep. 01 — the context system

Sets up `docs/` (ADRs, architecture, research, runbooks, and handover notes in
`docs/todo/`), a personal `task/<id>/` folder per task (findings, then plan,
then summary), and a paths-only context map in `CLAUDE.md`, `AGENTS.md`, and
`.github/copilot-instructions.md`.

**Use it:** open
[`episodes/ep01-filing-cabinet/SETUP_PROMPT.md`](episodes/ep01-filing-cabinet/SETUP_PROMPT.md),
copy everything below the line, and paste it into your agent at the repo root.

## Ep. 02 — the context window

No prompt for this one. It explains what's in the context window before you
type, and why to keep it lean: cut unused MCP servers, keep `CLAUDE.md` to
paths, let skills load lazily, and hand over before it fills.

## Ep. 03 — `/handover`

Ends a session on purpose instead of letting it auto-compact. It reads the task
folder and the git diff, writes one note to `docs/todo/handover-NNN-<slug>.md`
(done · next · decisions · gotchas · don't touch), and asks you to review it.
Then you start a fresh session pointed at that note.

Only you can start it (`disable-model-invocation: true`). It never commits or
pushes.

## Ep. 04 — `/retro`

Reviews the session for repeated friction and proposes at most three setup
fixes, ranked, each with evidence, risk, verification, and rollback. Proposals
first: it changes nothing until you approve specific items.

## Installing the skills

**Claude Code:** copy a skill folder into your project's `.claude/skills/`
(or `~/.claude/skills/` for every project), then run `/handover` or `/retro`.

```
.claude/skills/
├── handover/SKILL.md
└── retro/SKILL.md
```

**Other agents:** keep the files anywhere in the repo and ask the agent to
follow them, e.g. "Follow skills/handover/SKILL.md".

## Layout

```
episodes/
└── ep01-filing-cabinet/
    ├── README.md         # what the episode covers
    └── SETUP_PROMPT.md   # the copy-paste prompt
skills/
├── handover/SKILL.md     # Ep. 03
└── retro/SKILL.md        # Ep. 04
```

Prompts live in one folder per episode, named `epNN-<slug>`. Reusable skills
live in `skills/<name>/`.

Write-up: https://gokulkannank.substack.com/
