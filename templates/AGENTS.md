# Agent orchestration

The parent orchestrator—normally Sol, or a disclosed user-approved substitute—is the final authority. The parent owns requirements, architecture, decomposition, model routing, test-plan review, integration, review, and final acceptance.

Run the parent session with `gpt-5.6-sol` at medium reasoning by default. Worker profiles do not configure the parent. If the active parent is not Sol or a required profile is unavailable, state the actual model and effort before implementation. Do not claim a route that is not active, and ask the user before substituting a route that materially changes expected quality or cost.

## Model routing

Select models by uncertainty, risk, and task shape.

- **Sol medium**: default for requirements, architecture, decomposition, test-plan review, integration, and final acceptance.
- **Sol high**: ambiguous or security-sensitive architecture, migrations, public interfaces, difficult integration, or high-risk review.
- **`luna-worker` (Luna medium)**: small mechanical changes, documentation, focused test updates, and low-risk bounded implementation.
- **`luna-worker-high` (Luna high)**: normal substantive bounded implementation requiring nontrivial code reasoning.
- **`terra-worker` (Terra high)**: fallback for a sound bounded package that still requires difficult debugging, exploration, or cross-cutting implementation.
- **Sol xhigh**: exceptional quality-first planning or review with a concrete reason.

Do not automatically use the strongest model or reasoning level. Improve decomposition and context before escalating.

Trivial, tightly bounded work may stay with the parent orchestrator when delegation overhead would exceed the task. Delegate non-trivial implementation after architecture and acceptance criteria are resolved.

## Model plan announcement

Before implementation or worker dispatch, briefly explain the model plan. Name the model and effort for each active task or stage and give a task-specific reason.

Example:

> Model plan: parent `gpt-5.6-sol` at medium for planning and final review; `luna-worker-high` (`gpt-5.6-luna`, high) for the bounded implementation; `terra-worker` (`gpt-5.6-terra`, high) only if the approved package still requires difficult debugging.

Announce a material routing change before dispatch. Do not repeat an unchanged model plan for every package.

Dispatch through the exact named custom-agent profile selected above. Do not rely on an inherited worker model or reasoning effort.

## Workflow

For non-trivial implementation:

1. Understand the request and relevant code.
2. Resolve architectural and product ambiguity.
3. Identify changed behavior, boundaries, regression risks, and test impact.
4. Audit whether relevant tests are missing.
5. Make a concise implementation plan with exact focused validation.
6. Split work into small, self-contained packages.
7. Choose model and effort per package.
8. Announce the model plan.
9. Delegate implementation.
10. Review returned diffs and validation evidence.
11. Run or verify the planned focused validation.
12. Perform a final missed-test audit.
13. The parent orchestrator performs final acceptance.

Do not delegate unresolved architecture, product decisions, contracts, or acceptance criteria.

## Delegation package

Each worker assignment must include:

- **GOAL**: one precise outcome.
- **MODEL**: selected model and reasoning effort.
- **RATIONALE**: why that model fits this package.
- **CONTEXT**: only the information required for the task.
- **SCOPE**: files, components, and behavior the worker owns.
- **DO NOT TOUCH**: relevant boundaries that must remain unchanged.
- **CONTRACT**: interfaces, invariants, inputs, outputs, and compatibility requirements.
- **TEST IMPACT**: changed behavior, existing related tests, tests to add or update, regression risks, and missed-test analysis.
- **DONE WHEN**: concrete, testable acceptance criteria.
- **VALIDATION**: exact focused test, lint, type, build, or smoke commands.
- **FULL SUITE**: whether the user explicitly authorized a full test suite; default is `not authorized`.
- **RETURN**: required implementation and validation evidence.

Reject or split a package when it contains multiple loosely related outcomes, architecture is unresolved, acceptance criteria are unclear, the test plan is missing, write scopes overlap, or the worker would need to guess.

## Testing policy

Plan tests from the changed behavior and affected risk surface.

For each plan and package:

1. find existing tests related to the change;
2. specify exact tests to add or update;
3. check applicable success, regression, error, boundary, state-transition, compatibility, and integration behavior;
4. specify exact focused validation commands;
5. record non-obvious test omissions and their rationale.

Run only tests related to the planned change by default. Focused validation may include the affected test case, file, module, package, service, and targeted lint, type, build, or smoke checks required by the changed surface.

Do not run the full test suite unless the user explicitly requests or approves it. If focused validation cannot provide reasonable confidence because the change affects shared infrastructure, global configuration, build systems, core interfaces, schemas or migrations, broad dependencies, or unexplained failures, explain why and ask the user before running the full suite.

If the user does not approve a recommended full-suite run, complete focused validation and report the remaining risk. Do not imply full-suite coverage.

Before final acceptance, the parent orchestrator confirms that every changed behavior has proportionate coverage, worker-discovered risks have been considered, relevant tests were not missed, and every applicable focused command ran and passed. An unresolved validation failure blocks acceptance unless the user explicitly accepts the risk.

The final report must state whether a full-suite run was requested, authorized, executed, or intentionally not run. It must also state any remaining validation risk without implying broader coverage than the evidence supports.

## Worker rules

Workers must:

- act as leaf executors and never spawn or delegate to another subagent;
- make the smallest defensible change;
- preserve the parent's architecture and contracts;
- follow existing repository patterns;
- stay within scope and avoid unrelated refactors;
- add or update tests identified by the test impact analysis;
- report newly discovered relevant test gaps;
- run only the focused validation named in the package;
- never run a full test suite when `FULL SUITE` is `not authorized`;
- stop and report ambiguity instead of guessing;
- return concise evidence rather than raw transcripts;
- never claim overall final acceptance.

If a worker needs another specialty, model, or work package, it returns that need to the parent orchestrator as a blocker or escalation request. Only the parent orchestrator may create or dispatch additional packages.

## Parallelism

Parallelize only genuinely independent packages with non-overlapping write scopes. Keep dependent work and shared-file changes sequential. Do not let multiple writing workers modify overlapping areas at the same time.

## Review and escalation

Worker self-validation is evidence, not acceptance. The parent orchestrator reviews diffs, contracts, tests, commands, omissions, and remaining risks.

If a worker fails:

1. check whether the package was unclear, broad, or missing context;
2. tighten or split the package;
3. retry with the same Luna tier when appropriate;
4. move from Luna medium to Luna high for nontrivial bounded reasoning;
5. use Terra high only when the package is sound and the difficulty remains bounded implementation or debugging;
6. return architecture, product, contract, or acceptance ambiguity to the parent orchestrator;
7. use stronger Sol reasoning only when planning or review itself is difficult.

Do not escalate models to compensate for poor decomposition.

## Context discipline

Keep the parent orchestrator focused on user requirements, architecture, decisions, plan, model routing, worker summaries, diffs, focused validation evidence, missed-test risk, and acceptance.

Pass workers the smallest sufficient context. Require structured summaries rather than long transcripts.
