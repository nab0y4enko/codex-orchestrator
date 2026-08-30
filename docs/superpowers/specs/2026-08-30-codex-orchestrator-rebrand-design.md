# Codex Orchestrator Rebrand and Routing Design

## Goal

Create an independently branded copy of the `sol-luna-codex-orchestrator` source in the user-owned `codex-orchestrator` repository, and strengthen it into a cost-aware orchestration template with explicit model selection, test planning, validation scope, escalation, and final-acceptance rules.

## Scope

- Import the source repository's complete project files from its `main` branch.
- Brand the project-facing documentation as `Codex Orchestrator` / `codex-orchestrator`.
- Preserve the Sol, Luna, and Terra role architecture while refining how work is routed between them.
- Add separate Luna medium and Luna high worker profiles so effort can match task complexity.
- Require a short model-selection explanation before implementation starts.
- Require change-specific test planning and a missed-test analysis.
- Run only tests related to the planned change by default.
- Run the full test suite only when the user explicitly requests it. If the agent believes a full-suite run is necessary, it must explain why and ask first.
- Replace the example with a realistic prompt containing requirements, constraints, acceptance criteria, and focused validation expectations.
- Preserve the MIT license and its copyright notice.
- Keep the destination repository's existing `origin` remote; do not push or mutate GitHub repository settings.

The source snapshot is the upstream `main` tip observed on 2026-08-30 (`cc2012f77a4989e914accab93a51728d98f4ddc`). The destination starts as an empty repository, so the imported content will become a fresh destination history rather than retaining the upstream repository's commit history.

## Design Principles

### Quality ownership

Sol owns requirements, architecture, decomposition, test-plan review, integration, and final acceptance. Implementation workers may validate their own work but cannot accept the overall result. Architectural or product ambiguity always returns to Sol rather than being delegated to a stronger implementation worker.

### Cost-aware routing

Select models by uncertainty, risk, and task shape rather than file count alone:

| Work | Default model and effort | Escalation |
|---|---|---|
| Requirements, architecture, decomposition, test-plan review | Sol medium | Sol high for ambiguity, security-sensitive design, migrations, public interfaces, or difficult review |
| Small mechanical implementation, documentation, and focused test updates | `luna-worker` (Luna medium) | `luna-worker-high` when implementation requires nontrivial bounded reasoning |
| Normal substantive bounded implementation | `luna-worker-high` (Luna high) | `terra-worker` when debugging, exploration, or cross-cutting implementation exceeds Luna's reliable scope |
| Difficult bounded implementation or debugging | `terra-worker` (Terra high) | Return to Sol when the unresolved issue is architectural or product-level |
| Integration and final acceptance | Sol medium | Sol high when failure impact or review complexity is high |
| Exceptional quality-first work | No automatic default | Sol xhigh only with a concrete reason |

The template must not automatically select the strongest model or reasoning level. A higher tier is justified only by task complexity, risk, failed bounded attempts, or review requirements.

This follows current official OpenAI guidance: Sol is the flagship choice for complex reasoning and coding, Terra balances intelligence and cost, Luna targets cost-sensitive high-volume work, and medium reasoning is the balanced starting point while high or xhigh should be selected when they provide a meaningful quality gain:

- https://developers.openai.com/api/docs/models
- https://developers.openai.com/api/docs/guides/latest-model

The repository will describe relative roles rather than hard-code prices, because prices and availability can change.

The parent session should use `gpt-5.6-sol` at medium effort by default. Dispatch must use the exact named worker profile rather than inherited model settings. If the active parent or selected profile is unavailable, the orchestrator discloses the actual model and effort before implementation and asks before a substitution that materially changes expected quality or cost.

### Delegation economics

Delegation has context and coordination overhead. Sol may perform trivial, tightly bounded work directly when creating and reviewing a worker package would cost more than the task. Non-trivial implementation should be delegated when the architecture and acceptance criteria are resolved.

Parallel workers are allowed only for genuinely independent packages with non-overlapping write scopes. Sequential dependencies and shared files remain sequential.

### Escalation ladder

1. Confirm the work package is clear, bounded, and testable.
2. Route small or mechanical work to Luna medium.
3. Route substantive bounded implementation to Luna high.
4. If a Luna attempt fails, first inspect whether the package was ambiguous, too broad, or missing context.
5. Tighten or split an inadequate package before changing models.
6. Use Terra high only when the package is sound and the remaining difficulty is bounded implementation, debugging, exploration, or cross-cutting reasoning.
7. Return architecture, product, contract, or acceptance ambiguity to Sol.
8. Reserve stronger Sol reasoning for genuinely difficult planning or review, not to compensate for poor decomposition.

## Model Plan Announcement

Before implementation begins, the orchestrator must briefly state which model is selected for each planned implementation or review stage and why. A concise form is sufficient:

> Model plan: parent `gpt-5.6-sol` at medium for planning and final review; `luna-worker-high` (`gpt-5.6-luna`, high) for the bounded implementation; `terra-worker` (`gpt-5.6-terra`, high) only if the approved package requires difficult debugging.

The announcement must:

- precede implementation or worker dispatch;
- name the model and effort for each active stage or task;
- give a short task-specific reason;
- identify conditional fallbacks without implying they have already been selected;
- be updated before dispatch if routing changes materially.

Repeated announcements are unnecessary when several packages use the same already-announced route.

## Test Planning Policy

### Plan tests from the change surface

Every implementation plan and worker package must identify:

- behavior being added, changed, or fixed;
- affected interfaces, boundaries, and regression risks;
- existing tests relevant to those behaviors;
- exact tests to add or update;
- exact focused test commands to run;
- tests considered but intentionally omitted, with a reason when omission is non-obvious.

The plan must audit for missed tests before implementation. Relevant categories include success behavior, regression protection, error behavior, boundaries, state transitions, compatibility, and integration points. Only categories that apply to the change need tests.

### Focused validation by default

By default, agents run only tests related to the planned change. Focused validation may include:

- the changed test file or test case;
- the directly affected package, module, or service tests;
- targeted linting, type checks, builds, or smoke tests required to validate the changed surface.

Agents must not silently expand focused validation into a full repository test suite.

### Full-suite approval boundary

A full test suite may run only when the user explicitly requests or approves it.

An agent may recommend a full-suite run when focused validation cannot provide reasonable confidence, including changes to shared infrastructure, global configuration, build systems, core interfaces, schemas or migrations, broad dependency upgrades, or unexplained failures outside the focused surface. The agent must explain the specific reason and ask before running it.

If approval is not available, the agent completes focused validation and reports the remaining risk without claiming full-suite coverage.

### Final missed-test audit

Before final acceptance, Sol reviews:

- whether every changed behavior has proportionate test coverage;
- whether worker-discovered risks require additional focused tests;
- whether any relevant test was omitted;
- whether the exact planned focused commands ran and passed;
- whether full-suite coverage was requested, approved, run, or intentionally not run.

Workers must report test additions, exact commands, exact results, omissions, and remaining risks. Worker self-validation is evidence for Sol, not final acceptance.

## Architecture and Files

The project remains a configuration-and-documentation template:

- `README.md` explains the product identity, setup, routing matrix, escalation policy, model-plan announcement, testing policy, workflow, included files, and philosophy.
- `templates/AGENTS.md` is the authoritative installed policy for model routing, delegation, test planning, focused validation, full-suite approval, review, and final acceptance.
- `templates/.agents/skills/delegate-work/SKILL.md` defines the worker assignment contract, including model rationale and test-impact fields.
- `templates/.codex/agents/luna-worker.toml` defines the Luna medium profile for routine bounded implementation.
- `templates/.codex/agents/luna-worker-high.toml` defines the Luna high profile for substantive bounded implementation.
- `templates/.codex/agents/terra-worker.toml` defines the Terra high fallback for difficult bounded work.
- `examples/example-prompt.md` demonstrates a realistic feature request with requirements, constraints, acceptance criteria, focused tests, and full-suite approval behavior.
- `LICENSE` preserves the upstream MIT terms and attribution.

## Worker Package Contract

Each delegated package must contain:

- `GOAL`: one precise outcome;
- `MODEL`: selected model and reasoning effort;
- `RATIONALE`: why the model fits this package;
- `CONTEXT`: only the information required for the work;
- `SCOPE`: files, components, and behavior owned by the worker;
- `DO NOT TOUCH`: boundaries that remain unchanged;
- `CONTRACT`: interfaces and invariants to preserve;
- `TEST IMPACT`: existing tests, changed behavior, regression risks, and missed-test analysis;
- `DONE WHEN`: concrete acceptance criteria;
- `VALIDATION`: exact focused tests and checks to run;
- `FULL SUITE`: whether the user explicitly authorized it; default is `not authorized`;
- `RETURN`: changed files, summary, tests added or updated, exact commands and results, omitted tests, assumptions, risks, and blockers.

A package must be rejected or split when architecture is unresolved, acceptance criteria are unclear, write scopes overlap, the test plan is missing, or the worker would need to guess.

Workers are leaf executors and may not spawn or delegate to another subagent. Any need for additional expertise or work returns to Sol. Before dispatch, Sol announces the exact named profile and confirms it matches the package's `MODEL` field.

## Example Design

The example feature remains passwordless login but becomes concrete:

- add email magic-link login;
- preserve the current email provider and password login;
- define expiration, one-time use, invalid-token behavior, and successful session creation;
- require focused tests for creation, expiration, one-time use, invalid tokens, successful login, and regression protection for password login where those behaviors are affected;
- prohibit a full-suite run unless the user asks for it or approves an agent recommendation.

The example teaches users to state product constraints and acceptance criteria without forcing them to repeat the repository's persistent orchestration policy.

## Data Flow

1. Sol reads the request and relevant repository context.
2. Sol resolves product and architecture ambiguity.
3. Sol maps changed behavior, risk, and relevant tests.
4. Sol creates bounded packages and chooses model/effort per package.
5. Sol announces the model plan before implementation.
6. Independent, non-overlapping packages may run in parallel; all others run sequentially.
7. Luna medium handles small/mechanical work; Luna high handles substantive bounded work.
8. Terra high handles only difficult bounded work after package quality is confirmed.
9. Workers run focused validation and return structured evidence.
10. Sol reviews diffs, test evidence, and missed-test risk.
11. Sol requests user approval before any full-suite run that was not already requested.
12. Sol performs final acceptance.

## Error Handling and Safety

- Do not overwrite unrelated destination content; the destination is verified empty except for its worktree ignore setup and design/plan documents.
- Do not remove or rewrite the MIT license.
- Do not push to GitHub automatically.
- Do not delegate unresolved architecture or product decisions.
- Do not let workers silently broaden scope or validation.
- Do not run the full test suite without explicit user authorization.
- Do not escalate model strength before checking decomposition and package quality.
- Treat remaining old project-brand references as review failures unless required for attribution or provenance.

## Validation

1. Check the full worktree for unresolved old-brand references outside provenance documents.
2. Run `git diff --check`.
3. Confirm the expected file tree and inspect the final diff.
4. Parse all worker TOML files and assert their names, models, and reasoning efforts.
5. Assert that the installed `AGENTS.md`, delegation skill, and worker configs contain the focused-test and full-suite approval rules.
6. Assert that the delegation skill contains every required package field.
7. Assert that the README contains the routing matrix, model-plan announcement, test strategy, escalation ladder, and new profile.
8. Assert that the example contains requirements, acceptance criteria, focused tests, and the full-suite approval boundary.
9. Run only this focused structural validation because the repository has no executable application test suite.
