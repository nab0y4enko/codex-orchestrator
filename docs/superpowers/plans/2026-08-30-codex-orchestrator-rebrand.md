# Codex Orchestrator Rebrand and Routing Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Import the upstream orchestration template, rebrand it as `codex-orchestrator`, and enforce cost-aware model routing plus focused-test governance.

**Architecture:** Keep the repository lightweight and policy-driven. The installed `AGENTS.md` owns orchestration decisions, the delegation skill defines worker contracts, TOML profiles encode model/effort tiers, and focused structural validation verifies the template as a coherent unit.

**Tech Stack:** Markdown, TOML, Git, Python 3 standard library.

**Spec:** `docs/superpowers/specs/2026-08-30-codex-orchestrator-rebrand-design.md`

## Global Constraints

- Preserve the upstream MIT license and attribution.
- Keep `origin` pointed at `https://github.com/nab0y4enko/codex-orchestrator.git`.
- Use Sol for architecture and acceptance, Luna medium/high for bounded work, and Terra high only for difficult bounded work.
- Announce model and effort choices before implementation or dispatch.
- Use `gpt-5.6-sol` medium for the parent by default and dispatch workers through the exact named profile.
- Keep workers as leaf executors; only Sol may dispatch additional subagents.
- Plan and audit tests from the changed behavior and risk surface.
- Run only related tests and checks by default.
- Never run a full test suite unless the user explicitly requests or approves it.
- Do not push until focused validation and final review pass.

## Test Impact and Missed-Test Analysis

This repository contains documentation and agent configuration rather than executable application code. The changed behavior is the installed orchestration policy.

Relevant focused validation:

- all required files exist;
- all TOML worker profiles parse;
- Luna medium, Luna high, and Terra high profiles use the expected models and reasoning efforts;
- `AGENTS.md` includes model-plan announcements, routing, test-impact planning, focused validation, full-suite approval, escalation, parallelism, and final acceptance;
- the delegation skill includes every required package and return field;
- worker profiles prohibit unauthorized full-suite runs and require focused validation evidence;
- README branding, routing, testing policy, escalation, workflow, and included-file list are consistent;
- the example contains concrete requirements, acceptance criteria, focused tests, and the full-suite boundary;
- old project branding is removed outside provenance documents;
- the license remains byte-for-byte identical to upstream;
- the Git remote remains the user-owned repository.

No executable application tests, runtime behavior, or package-specific test suites exist. A repository-wide application test suite is therefore not applicable and was not requested. The focused structural validator plus `git diff --check` covers the complete changed surface.

### Task 1: Establish RED structural validation

**Files:**
- Inspect: current worktree before project import
- Test: focused inline Python structural validator

**Interfaces:**
- Consumes: approved specification.
- Produces: evidence that the validator fails because the required imported/rebranded files do not yet exist.

- [ ] **Step 1: Run the focused validator before implementation**

Run the final structural validator defined in Task 4 against the current worktree.

Expected: FAIL on a missing required file such as `README.md`. The failure must be caused by the absent implementation, not a syntax error in the validator.

### Task 2: Import the upstream source snapshot

**Files:**
- Create: `LICENSE`
- Create: `README.md`
- Create: `examples/example-prompt.md`
- Create: `templates/AGENTS.md`
- Create: `templates/.agents/skills/delegate-work/SKILL.md`
- Create: `templates/.codex/agents/luna-worker.toml`
- Create: `templates/.codex/agents/terra-worker.toml`

**Interfaces:**
- Consumes: `https://github.com/breko861-hash/sol-luna-codex-orchestrator.git` at `cc2012f77a4989e914accab93a51728d98f4ddc`.
- Produces: a clean source snapshot without upstream Git metadata or remotes.

- [ ] **Step 1: Clone and verify the upstream snapshot in a temporary directory**

```bash
source_root=$(mktemp -d /tmp/codex-orchestrator-source.XXXXXX)
git clone --depth=1 --branch main https://github.com/breko861-hash/sol-luna-codex-orchestrator.git "$source_root/repo"
test "$(git -C "$source_root/repo" rev-parse HEAD)" = "cc2012f77a4989e914accab93a51728d98f4ddc"
```

Expected: clone and commit assertion succeed.

- [ ] **Step 2: Materialize the source tree**

```bash
git -C "$source_root/repo" archive --format=tar HEAD | tar -x -C "$PWD"
```

Expected: the seven upstream files appear and no upstream `.git` metadata is copied.

- [ ] **Step 3: Verify the raw import and license**

```bash
test -f LICENSE
test -f README.md
test -f examples/example-prompt.md
test -f templates/AGENTS.md
test -f templates/.agents/skills/delegate-work/SKILL.md
test -f templates/.codex/agents/luna-worker.toml
test -f templates/.codex/agents/terra-worker.toml
cmp LICENSE "$source_root/repo/LICENSE"
git diff --check
```

Expected: all checks pass.

### Task 3: Implement the rebrand and orchestration policy

**Files:**
- Modify: `README.md`
- Modify: `examples/example-prompt.md`
- Modify: `templates/AGENTS.md`
- Modify: `templates/.agents/skills/delegate-work/SKILL.md`
- Modify: `templates/.codex/agents/luna-worker.toml`
- Create: `templates/.codex/agents/luna-worker-high.toml`
- Modify: `templates/.codex/agents/terra-worker.toml`
- Preserve: `LICENSE`

**Interfaces:**
- Consumes: imported source and approved specification.
- Produces: an installable `Codex Orchestrator` template with deterministic routing, test planning, validation boundaries, and worker evidence.

- [ ] **Step 1: Rebrand and expand README**

Add:

- `Codex Orchestrator` identity;
- Luna medium/high and Terra high setup tree;
- cost-aware routing matrix;
- model-plan announcement example;
- focused-test and full-suite approval policy;
- escalation ladder;
- revised workflow;
- improved example summary;
- official OpenAI model-selection links.

- [ ] **Step 2: Make AGENTS.md authoritative**

Encode:

- Sol quality ownership and delegation economics;
- routing criteria for Luna medium, Luna high, Terra high, and stronger Sol review;
- model-plan announcement before implementation;
- exact worker package fields;
- change-surface and missed-test analysis;
- focused validation default;
- full-suite user authorization boundary;
- parallelism and non-overlapping write scopes;
- escalation ladder;
- worker evidence and Sol final acceptance.

- [ ] **Step 3: Strengthen delegation and worker profiles**

Update the delegation skill with exact `GOAL`, `MODEL`, `RATIONALE`, `CONTEXT`, `SCOPE`, `DO NOT TOUCH`, `CONTRACT`, `TEST IMPACT`, `DONE WHEN`, `VALIDATION`, `FULL SUITE`, and `RETURN` sections.

Configure:

```text
luna-worker:      gpt-5.6-luna / medium
luna-worker-high: gpt-5.6-luna / high
terra-worker:     gpt-5.6-terra / high
```

All workers must run only package-scoped validation, refuse unauthorized full-suite runs, report missed-test findings, and avoid final acceptance.

- [ ] **Step 4: Replace the example**

Use a magic-link login request with:

- product requirements;
- preserved password login and email provider;
- expiration, one-time use, invalid-token, and successful-login behavior;
- focused related-test expectations;
- explicit full-suite approval boundary;
- measurable done conditions.

### Task 4: Validate, review, and commit

**Files:**
- Validate: every imported or modified project file
- Preserve: design and implementation-plan documents

**Interfaces:**
- Consumes: completed template.
- Produces: verified commits ready to merge into `main`.

- [ ] **Step 1: Run focused structural validation**

```bash
python3 - <<'PY'
from pathlib import Path
import tomllib

required = [
    Path("LICENSE"),
    Path("README.md"),
    Path("examples/example-prompt.md"),
    Path("templates/AGENTS.md"),
    Path("templates/.agents/skills/delegate-work/SKILL.md"),
    Path("templates/.codex/agents/luna-worker.toml"),
    Path("templates/.codex/agents/luna-worker-high.toml"),
    Path("templates/.codex/agents/terra-worker.toml"),
]
for path in required:
    assert path.is_file(), f"missing {path}"

profiles = {
    "luna-worker.toml": ("luna-worker", "gpt-5.6-luna", "medium"),
    "luna-worker-high.toml": ("luna-worker-high", "gpt-5.6-luna", "high"),
    "terra-worker.toml": ("terra-worker", "gpt-5.6-terra", "high"),
}
for filename, expected in profiles.items():
    data = tomllib.loads((Path("templates/.codex/agents") / filename).read_text())
    actual = (data["name"], data["model"], data["model_reasoning_effort"])
    assert actual == expected, (filename, actual, expected)
    instructions = data["developer_instructions"].lower()
    assert "focused" in instructions
    assert "full suite" in instructions
    assert "final acceptance" in instructions
    assert "do not spawn or delegate" in instructions

agents = Path("templates/AGENTS.md").read_text().lower()
for phrase in [
    "model plan", "test impact", "focused validation", "full test suite",
    "explicitly requests", "missed-test", "luna medium", "luna high",
    "terra high", "final acceptance",
    "gpt-5.6-sol", "exact named custom-agent profile", "ran and passed",
]:
    assert phrase in agents, phrase

skill = Path("templates/.agents/skills/delegate-work/SKILL.md").read_text()
for field in [
    "## GOAL", "## MODEL", "## RATIONALE", "## CONTEXT", "## SCOPE",
    "## DO NOT TOUCH", "## CONTRACT", "## TEST IMPACT", "## DONE WHEN",
    "## VALIDATION", "## FULL SUITE", "## RETURN",
    "## Pre-dispatch gate",
]:
    assert field in skill, field

readme = Path("README.md").read_text().lower()
for phrase in [
    "# codex orchestrator", "luna-worker-high.toml", "model plan",
    "focused tests", "full test suite", "escalation",
]:
    assert phrase in readme, phrase

example = Path("examples/example-prompt.md").read_text().lower()
for phrase in [
    "requirements", "acceptance criteria", "focused tests",
    "full test suite", "magic link",
]:
    assert phrase in example, phrase
PY
```

Expected: PASS with exit code 0.

- [ ] **Step 2: Run focused repository checks**

```bash
if rg -n -i --glob '!docs/superpowers/**' 'sol[ +]luna codex orchestrator|sol-luna-codex-orchestrator' .; then
  exit 1
fi
git diff --check
git remote get-url origin
```

Expected: no stale brand matches; whitespace check passes; remote URL is `https://github.com/nab0y4enko/codex-orchestrator.git`.

- [ ] **Step 3: Perform missed-test and requirements audit**

Confirm every specification requirement maps to an assertion or direct diff inspection. Confirm no full application suite exists or was requested. Review the complete branch diff from `main`.

- [ ] **Step 4: Commit implementation**

```bash
git add LICENSE README.md examples templates docs/superpowers
git commit -m "feat: add cost-aware codex orchestration"
```

Expected: repository hooks pass and the implementation commit is created.

### Task 5: Integrate and push main

**Files:**
- Merge: `feat/codex-orchestrator-rebrand` into `main`
- Push: `origin/main`

**Interfaces:**
- Consumes: verified feature branch.
- Produces: user-owned GitHub repository with the complete project on `main`.

- [ ] **Step 1: Verify the committed feature branch**

Run the focused structural validator and repository checks again against committed `HEAD`.

- [ ] **Step 2: Merge into main**

From the primary checkout:

```bash
git checkout main
git merge --ff-only feat/codex-orchestrator-rebrand
```

Expected: fast-forward merge succeeds.

- [ ] **Step 3: Verify and push**

Run focused validation on merged `main`, then:

```bash
git push -u origin main
```

Expected: `origin/main` is updated without force-pushing.
