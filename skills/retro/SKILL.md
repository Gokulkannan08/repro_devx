---
name: "retro"
description: "Use after a coding session or a session Retro hook request to suggest evidence-backed improvements to navigation, automated checks, coding standards, steering files, tooling, and agent workflows; require approval before changes."
argument-hint: "Review the current session or specify a prior session and approved improvement scope."
user-invocable: true
disable-model-invocation: false
---

# Retro

Improve the coding environment based on observed friction. Proposals first: invocation, including a hook request, never authorizes edits, installations, tracker writes, publications, or new automation. Obtain explicit approval for the exact scope before applying changes.

This skill does not enforce tool permissions. Follow the current agent's restrictions and project instructions; do not bypass a read-only role. Do not run commands or recursively invoke Retro during a suggestions-only hook pass.

## Evidence and production standard

- Default to the current session. Inspect prior logs/history only when requested or needed and available; do not claim unseen telemetry.
- Separate root causes from one-off task complexity. Protect secrets, customer data, and private transcripts; keep repository facts local and global preferences general.
- Improve correctness, strong typing, explicit errors, testability, reliability, navigation, and feedback speed without weakening quality gates.
- Never claim production readiness, coverage, completed tests, or changes based on a proposed improvement.

## Procedure

1. Read session evidence and relevant existing scripts, CI/check configuration, standards, and steering before proposing new tooling.
2. Assess the following only where evidence supports them:
   - Navigation: repeated searches, hidden relationships, missing symbol/document pointers.
   - Automated checks: missing guardrails or existing lint/type/test commands that are unwired, skipped, or failing silently.
   - Standards: a mechanical rule belongs in the cheapest appropriate existing deterministic checker; genuine judgment belongs in review guidance. Do not add verbose always-on prose for what a check can enforce.
   - Steering: duplicate, conflicting, ineffective, or overlarge instructions; prefer concise pointers and scoped on-demand references.
   - Tool economy: expensive/repeated calls, oversized output, needless delegation, or missing bounded query helpers.
   - Information access: missing safe read-only evidence/logs, without requesting broad permissions or exposing private data.
3. Propose at most three changes, ranked by impact/severity, confidence, and cost. For each give the concrete observed friction, root cause, smallest improvement, correct location/scope, benefit, risk, verification, and rollback.
4. Search existing prior art before recommending a new script, dependency, standard, or skill. Do not apply a repository-specific lesson globally or prescribe a fixed Jira project/model.
5. Ask through an available structured question tool for approval before applying changes. An automatic pass may simply report suggestions and No changes applied; it need not interrupt the user with an approval question.
6. After approval, edit only authorized files using existing patterns and relevant customization guidance. Verify with existing checks appropriate to the change. Include positive/negative/regression scenarios; behavioral hooks also need no-op, invalid-input, failure, recursion, and concurrency checks.
7. If validation requires unavailable tools or exceeds the current agent's permissions, state the exact remaining checks as Not run. Do not silently broaden permissions or install dependencies.

## Output and automatic use

Give a compact table with evidence/friction, proposed improvement/location, benefit, risk/cost, verification, and approval status. Say No actionable environment improvements found when warranted. Finish with No changes applied unless approved changes actually occurred.

The global completion hook requests one pass at the first eligible completion in a session, not at chat closure. If that turn has no substantive engineering evidence yet, explicitly defer and recommend `/retro` at task completion. Do not claim the hook guarantees a final-session review, successful skill discovery, or another agent invocation.

## Reference

<!-- TODO: the photo cut off here (line 44+). Paste the rest of the original Reference section. -->
