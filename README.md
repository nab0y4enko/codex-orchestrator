# Codex Orchestrator

A lightweight, cost-aware Codex multi-agent template that keeps architecture and final acceptance with Sol, routes bounded implementation to Luna, and reserves Terra for difficult bounded work.

> Sol resolves the problem and accepts the result. Luna implements bounded work. Terra handles bounded work that genuinely needs stronger independent reasoning.

## Quick start

Copy the contents of `templates/` into the root of your Codex project.

```text
your-repo/
├── AGENTS.md
├── .agents/
│   └── skills/
│       └── delegate-work/
│           └── SKILL.md
└── .codex/
    └── agents/
        ├── luna-worker.toml
        ├── luna-worker-high.toml
        └── terra-worker.toml
```

If your project already has an `AGENTS.md`, merge the orchestration policy with the project-specific instructions instead of replacing them.

Run the parent Codex session with `gpt-5.6-sol` at medium reasoning by default. The files under `.codex/agents/` configure workers; they do not change the parent session model. If Sol or a named worker profile is unavailable, disclose the actual model and effort before implementation and ask before substituting a route that materially changes expected quality or cost.

## Model routing

Choose models by uncertainty, risk, and task shape—not by file count or a reflex to use the strongest model.

| Work | Default | Escalate when |
|---|---|---|
| Requirements, architecture, decomposition, test-plan review | Sol medium | Sol high for ambiguity, security-sensitive design, migrations, public interfaces, or difficult review |
| Small mechanical changes, documentation, and focused test updates | `luna-worker` (Luna medium) | `luna-worker-high` when bounded implementation needs nontrivial reasoning |
| Normal substantive bounded implementation | `luna-worker-high` (Luna high) | `terra-worker` when a sound package still requires difficult debugging, exploration, or cross-cutting reasoning |
| Difficult bounded implementation or debugging | `terra-worker` (Terra high) | Return to the parent orchestrator when the unresolved issue is architectural or product-level |
| Integration and final acceptance | Sol medium | Sol high when review complexity or failure impact is high |
| Exceptional quality-first work | No automatic default | Sol xhigh only with a concrete reason |

Do not automatically use the strongest model or reasoning level. First improve task decomposition and context. Escalate only when the remaining difficulty, risk, or review requirement justifies it.

Current OpenAI guidance describes Sol as the flagship model for complex reasoning and coding, Terra as the intelligence/cost balance, and Luna as the cost-sensitive high-volume choice. It recommends medium reasoning as a balanced baseline and higher effort when the workload benefits from it:

- [OpenAI model catalog](https://developers.openai.com/api/docs/models)
- [OpenAI model guidance](https://developers.openai.com/api/docs/guides/latest-model)

## Model plan before implementation

Before implementation or worker dispatch, briefly explain the selected model and effort for each active stage.

> Model plan: parent `gpt-5.6-sol` at medium for planning and final review because the architecture is already understood; `luna-worker-high` (`gpt-5.6-luna`, high) for the bounded implementation because it requires nontrivial code reasoning; `terra-worker` (`gpt-5.6-terra`, high) only if difficult bounded debugging remains after the package is verified.

Update the model plan before dispatch if routing changes materially. Repeating it for every package is unnecessary when the route is unchanged.

Dispatch through the exact named custom-agent profile from the routing table. Do not rely on an inherited worker model or effort.

## Testing policy

Plan tests from the requested change, affected boundaries, and regression risks.

For every implementation plan and work package:

1. identify the behavior being changed;
2. locate existing related tests;
3. specify tests to add or update;
4. audit success paths, regressions, errors, boundaries, state transitions, compatibility, and integration points when relevant;
5. name exact focused tests and checks to run;
6. record intentionally omitted tests when the reason is not obvious.

Run only focused tests related to the planned change by default. This can include directly affected module or package tests and targeted lint, type, build, or smoke checks.

Do not run the full test suite unless the user explicitly requests or approves it. If shared infrastructure, global configuration, migrations, core interfaces, broad dependency changes, or unexplained failures make a full run necessary, explain the reason and ask first. Without approval, run focused validation and report the remaining risk.

Before final acceptance, the parent orchestrator performs a missed-test audit and confirms every applicable focused check ran and passed. An unresolved failure blocks acceptance unless the user explicitly accepts the risk. The final report states whether a full-suite run was requested, authorized, executed, or intentionally not run, and names any remaining risk.

## Workflow

```text
Sol
 ↓
understand requirements, architecture, risks, and test impact
 ↓
split work into clear, non-overlapping packages
 ↓
choose model and effort; announce the model plan
 ↓
Luna medium / Luna high
 ↓
bounded implementation + focused validation
 ↓
Terra high only for difficult bounded work
 ↓
Sol
 ↓
diff review + missed-test audit + final acceptance
```

Parallelize only genuinely independent packages with non-overlapping write scopes. Keep dependent or shared-file work sequential.

## Escalation

1. Verify that the package has one clear outcome, resolved architecture, complete context, exact acceptance criteria, and a focused test plan.
2. Use Luna medium for small mechanical work and Luna high for substantive bounded implementation.
3. If Luna struggles, first tighten or split the package.
4. Use Terra high only when the package is sound and the remaining problem is difficult bounded implementation or debugging.
5. Return architectural, product, contract, or acceptance ambiguity to the parent orchestrator.
6. Use stronger Sol reasoning only when planning or review itself is genuinely difficult.

## Worker package

Every delegation includes:

- `GOAL`
- `MODEL`
- `RATIONALE`
- `CONTEXT`
- `SCOPE`
- `DO NOT TOUCH`
- `CONTRACT`
- `TEST IMPACT`
- `DONE WHEN`
- `VALIDATION`
- `FULL SUITE`
- `RETURN`

The installed delegation skill defines each field and the required worker evidence.

## Example

The included example requests passwordless login with explicit requirements, compatibility constraints, acceptance criteria, focused tests, and the full-suite approval boundary. Persistent orchestration policy handles model routing, decomposition, test-gap review, and final acceptance.

See `examples/example-prompt.md`.

## Included files

- `templates/AGENTS.md`
- `templates/.agents/skills/delegate-work/SKILL.md`
- `templates/.codex/agents/luna-worker.toml`
- `templates/.codex/agents/luna-worker-high.toml`
- `templates/.codex/agents/terra-worker.toml`
- `examples/example-prompt.md`

## Philosophy

1. Resolve ambiguity before delegation.
2. Select the cheapest model and effort that reliably fits the task.
3. Keep work packages small, complete, and independently testable.
4. Plan tests from changed behavior instead of defaulting to broad suites.
5. Let workers validate their implementation, but keep final acceptance with the parent orchestrator—normally Sol.
6. Improve decomposition before escalating model strength.
7. Parallelize only independent, non-overlapping work.
