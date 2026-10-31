# Ep. 01 — My AI forgets everything. So I gave it a filing cabinet.

*AI Setup, Ep. 01 · the setup prompt from the episode.*

Paste everything below the line into Claude Code, Copilot Chat (agent mode), Codex, or Cursor, at the root of your repo.

---

You are setting up a lightweight, agent-agnostic context system in this repository. Goal: any AI coding agent can start a new session, read a short map, and resume work without me re-explaining. Keep every file short. Paths over prose. Save tokens.

## Rules
- Inspect the repo first (tree, package manager, existing docs, existing CLAUDE.md / AGENTS.md / .github/copilot-instructions.md).
- Never overwrite an existing file. If one exists, propose a merge and wait for my approval.
- Show me the full plan (files to create or change) before writing anything.
- Never put secrets, customer names, or ticket IDs into committed docs.

## 1. Shared docs (committed)
Create `docs/` with:
- `docs/adr/`: `README.md` (what an ADR is, when to write one, rules: one decision per file, `NNNN-short-slug.md`, immutable once Accepted, supersede instead of rewrite, plus an index table) and `0000-template.md` (Title, Status, Date, Context, Decision, Consequences).
- `docs/architecture/README.md`: system overview stub with sections for components, data flow, and boundaries. Fill it with what you can infer from the repo; mark guesses with `TODO(verify)`.
- `docs/research/README.md`: one line: "Spikes and investigations. One file per topic."
- `docs/runbook/README.md`: a table of runbooks (Runbook | Use it when…). Include only ones that apply to this repo (e.g. local-setup, build-test-lint, deployment, troubleshooting). Create stubs only for local-setup and build-test-lint, using real commands from the repo.
- `docs/todo/`: an empty folder with `.gitkeep`. Handover notes go here as `handover-NNN-slug.md`.

## 2. Personal task folders (NOT committed)
- Create `task/` at the repo root and add `task/` to `.git/info/exclude` (local-only, not .gitignore).
- Convention, one folder per task: `task/<TASK-ID>/` containing
  - `<TASK-ID>-findings.md`: what is true (code read, constraints, risks)
  - `<TASK-ID>-plan.md`: what we will do (steps, files, tests)
  - `<TASK-ID>-summary.md`: what we did (changes, verification, follow-ups)
- Create `task/EXAMPLE-001/` with the three files as short templates.

## 3. The context map (paths only)
Create `CLAUDE.md` (or merge into the existing one) with a section of at most 15 lines:

```md
## Context map
- docs/adr/            # why: read before architectural changes
- docs/architecture/   # how it fits
- docs/research/       # prior investigations
- docs/runbook/        # commands: build, test, deploy
- docs/todo/           # handover notes
- task/<id>/           # my working notes: findings -> plan -> summary

## Workflow
1. New task: create task/<id>/, write findings before code.
2. Write plan, wait for my approval, then implement.
3. Finish: write summary with how it was verified.
4. Never claim done without running tests or lint from docs/runbook/.
```

Mirror the same section into `AGENTS.md` and `.github/copilot-instructions.md` (create them if missing). Same lines and same paths, so every agent reads the same map.

## 4. Finish
- Print the final tree of everything you created.
- List anything marked `TODO(verify)` for me to check.
- Suggest a first ADR from a decision you can see in the repo (do not write it).
