---
name: "retro"
description: "Use after a coding session or a session Retro hook request to suggest evidence-backed improvements to navigation, automated checks, coding standards, steering files, tooling, and agent workflows; require approval before changes."
argument-hint: "Review the current session or specify a prior session and approved improvement scope."
user-invocable: true
disable-model-invocation: false
---

# Retro

Improve the coding agent's environment based on observed friction, so future sessions go better. Fix the setup, not the code from this session. Proposals first: invocation, including a hook request, never authorizes edits, installations, tracker writes, publications, or new automation. Obtain explicit approval for the exact scope before applying changes.

This skill does not enforce tool permissions. Follow the current agent's restrictions and project instructions; do not bypass a read-only role. Do not run commands or recursively invoke Retro during a suggestions-only hook pass.

## Evidence and production standard

- Default to the current session. If the user names another session, read its primary sources: session logs on this machine, `task/<id>/` (findings, plan, summary), `docs/todo/handover-*.md`, and git history. Do not claim unseen telemetry.
- Separate repeated friction from one-off bad luck or task complexity. One occurrence is rarely a finding; say how many times you saw it.
- Protect secrets, customer data, and private transcripts. Keep repository facts in the repository and global preferences general.
- Improve correctness, strong typing, explicit errors, testability, reliability, navigation, and feedback speed without weakening quality gates.
- Never claim production readiness, coverage, completed tests, or changes based on a proposed improvement.

## Procedure

1. Read the session evidence, then the repo's existing scripts (`package.json` or build-tool `lint`/`check`/`test` targets), CI workflows, pre-commit hooks, standards, and steering files before proposing anything new.
2. Look for candidates in these categories, only where evidence supports them:
   - **Navigation**: repeated searches, hidden dependencies between files, a missing pointer to a doc or symbol. _Use when_ the agent took a long time to find something.
   - **Automated checks**: a lint, type, test, or filesystem check that would have caught a mistake the agent made. An existing check that is unwired, skipped, or silently failing is the finding, not a reason to build a new one. A repo with no guardrail at all (no pre-commit hook and no CI job running lint, typecheck, or tests) is itself a finding. _Use when_ the agent made a mistake a check could have caught, or the repo has no guardrail.
   - **Coding standards**: classify the violation first. A **mechanical** rule (a fixed pattern, a banned API, an import shape, a file-location rule) gets a deterministic check in the repo's own linter, a pre-commit hook, or a CI job, whichever is cheapest; prefer building the check over writing the rule. Reserve `CODING_STANDARDS.md` (or the repo's review guidance) for genuine **judgment calls** no check could replace. _Use when_ review missed a mistake.
   - **Steering**: always-loaded instructions (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, global files) that belong in coding standards, a check, or an on-demand doc instead; duplicates and conflicts. _Use when_ a steering file is large, in the repo or the user's global scope.
   - **No-ops**: instructions that don't change the agent's behaviour and only cost tokens. _Use when_ steering files are large and unwieldy.
   - **Tool economy**: expensive or repeated calls, oversized output, needless delegation, token-inefficient custom CLIs or MCP servers, a missing bounded query helper. _Use when_ the agent made an expensive tool call.
   - **Information access**: missing safe, read-only evidence, such as teed dev-server logs or read-only access to a third-party service, without requesting broad permissions or exposing private data. _Use when_ a crucial piece of information was not available to the agent.
3. Propose at most three changes, ranked by severity, then confidence, then cost. For each give the observed friction and how often, root cause, smallest improvement, location and scope, benefit, risk, verification, and rollback.
4. Search for prior art before recommending a new script, dependency, standard, doc, or skill. Do not apply a repository-specific lesson globally or prescribe a fixed tracker project or model.
5. Ask through an available structured question tool for approval before applying changes. An automatic pass may simply report suggestions and No changes applied; it need not interrupt the user with an approval question.
6. After approval, edit only authorized files using existing patterns. Verify with existing checks appropriate to the change. Include positive, negative, and regression scenarios; behavioural hooks also need no-op, invalid-input, failure, recursion, and concurrency checks.
7. If validation requires unavailable tools or exceeds the current agent's permissions, state the exact remaining checks as Not run. Do not silently broaden permissions or install dependencies.

## Output and automatic use

Give a compact table (see Reference) with evidence, proposed improvement and location, benefit, risk, verification, rollback, and approval status. Say No actionable environment improvements found when warranted. Finish with No changes applied unless approved changes actually occurred.

If you have set up a completion hook that requests a retro (none ships with this skill), it asks for one pass at the first eligible completion in a session, not at chat closure. If that turn has no substantive engineering evidence yet, explicitly defer and recommend `/retro` at task completion. Do not claim the hook guarantees a final-session review, successful skill discovery, or another agent invocation.

## Reference

### Implementation vs review

Work goes through two stages. The implementation agent carries the most context pressure: it explores, writes code, and debugs. The review agent receives a diff, so it needs no exploration and rarely writes code. Put coding standards on the review side, not in the implementation agent's always-loaded context.

### Where a fix belongs

- `CLAUDE.md` / `AGENTS.md` / `.github/copilot-instructions.md`: loaded into every session. Use them sparingly, mostly for **navigation pointers** (paths only).
- `CODING_STANDARDS.md` or review guidance: read during review, not implementation. Judgment calls only; if it grows past about 1,000 lines, split it into docs and point to them.
- `docs/` (`adr/`, `runbook/`, `architecture/`, `research/`): reference files that other files point to. Look for an existing doc before writing a new one.
- Scripts, linters, hooks, CI: anything mechanical.
- Skills: on-demand instructions or user-invoked commands. Only the name and description sit in context until used.

### Proposal table

At most three rows, ranked:

| # | Seen | Friction (evidence) | Smallest fix | Where | Benefit | Risk / cost | Verify | Rollback | Status |
|---|---|---|---|---|---|---|---|---|---|

Example:

| # | Seen | Friction (evidence) | Smallest fix | Where | Benefit | Risk / cost | Verify | Rollback | Status |
|---|---|---|---|---|---|---|---|---|---|
| 1 · high | 3× | full test run, 4m12s, flaky without `--runInBand` | `test:auth` script with `--runInBand` | package.json | auth tests in seconds | low | `npm run test:auth` | remove the script | proposed |
| 2 · medium | 3× | 6 greps for the auth config | one path line under the context map | CLAUDE.md | no search | low | next session finds it without grep | delete the line | proposed |
| 3 · low | 2× | lint fixed by hand | pre-commit lint hook | .husky/ | lint caught before commit | slower commits | commit with a lint error is blocked | remove the hook | proposed |

No changes applied.

Then ask which items to apply (for example "1, 2" or "none"). After applying, report each item as applied, skipped, or failed, with the verification result or Not run.

---

Adapted from Matt Pocock's [retro skill](https://github.com/mattpocock/skills/blob/main/skills/engineering/retro/SKILL.md) (MIT License, © 2026 Matt Pocock).
