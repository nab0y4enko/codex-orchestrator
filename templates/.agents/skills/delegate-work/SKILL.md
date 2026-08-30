---
name: delegate-work
description: Use whenever the parent agent is preparing implementation work for a subagent. Convert the task into a small, model-routed, testable worker package before delegation.
---

# Delegate Work

Before spawning an implementation worker, reject or split work that has unresolved architecture, unclear acceptance criteria, overlapping write scope, or no focused test plan.

## GOAL

State one precise outcome. If the sentence contains unrelated outcomes, split the package.

## MODEL

Name the selected model and reasoning effort:

- `luna-worker` (Luna medium) for small mechanical or low-risk bounded work.
- `luna-worker-high` (Luna high) for substantive bounded implementation.
- `terra-worker` (Terra high) only for difficult bounded implementation or debugging after package quality is confirmed.

Architecture, product decisions, integration, and final acceptance remain with the parent orchestrator.

## RATIONALE

Give one short task-specific reason why the selected model and effort are the cheapest reliable fit. Do not justify escalation only by task size or a failed attempt.

## CONTEXT

Include only the repository facts, decisions, and prior outcomes required for this package.

## SCOPE

List the exact files, components, behavior, and test surface owned by the worker.

## DO NOT TOUCH

Name relevant files, behavior, interfaces, and unrelated areas that must remain unchanged.

## CONTRACT

State interfaces, invariants, inputs, outputs, compatibility requirements, and architectural decisions the worker must preserve.

## TEST IMPACT

Identify:

1. behavior being changed;
2. affected boundaries and regression risks;
3. existing related tests;
4. exact tests to add or update;
5. applicable success, error, boundary, state-transition, compatibility, and integration cases;
6. tests considered but intentionally omitted when the reason is non-obvious;
7. any missed-test uncertainty the worker must investigate.

## DONE WHEN

List concrete, observable acceptance criteria for implementation and focused validation.

## VALIDATION

Provide exact focused test, lint, type, build, or smoke commands. By default, commands cover only the changed test case, file, module, package, service, and other directly affected surfaces.

Do not use a repository-wide command merely because it is convenient.

## FULL SUITE

Set one of:

- `not authorized` — default; the worker must not run the full test suite;
- `authorized by user: <request context>` — the user explicitly requested or approved it.

If the worker discovers that a full suite may be necessary, it must stop that validation step and return the reason to the parent orchestrator. The parent asks the user before any full-suite run.

## RETURN

Return only:

1. files changed;
2. concise implementation summary;
3. tests added or updated;
4. exact focused validation commands and exact results;
5. full-suite authorization and execution status;
6. newly discovered test gaps;
7. tests omitted and why;
8. assumptions;
9. remaining risks, questions, or blockers.

Workers do not perform final acceptance.
Workers are leaf executors: they must not spawn or delegate to another subagent. They return any need for additional expertise or work to the parent orchestrator—normally Sol, or the disclosed user-approved substitute.

## Pre-dispatch gate

Before spawning the worker, the parent orchestrator must announce the selected model, reasoning effort, exact named custom-agent profile, task, and short rationale. The dispatched profile must match `MODEL`; do not rely on an inherited worker model or effort.

If the selected profile is unavailable, disclose the actual available model and effort before implementation. Ask the user before substituting a route that materially changes expected quality or cost.

## Package quality gate

Reject and split or clarify the package if:

- it contains multiple loosely related outcomes;
- architecture, product behavior, contracts, or acceptance criteria are unresolved;
- the worker would need to rediscover the overall architecture;
- the worker would need to guess parent intent;
- the test impact analysis or focused validation is incomplete;
- it overlaps another active writing worker;
- the model tier is stronger than the package requires.

Pass the smallest sufficient context. Improve the package before escalating the model.
