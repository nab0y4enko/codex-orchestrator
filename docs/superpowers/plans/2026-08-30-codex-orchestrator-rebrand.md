# Codex Orchestrator Rebrand Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Import the upstream Sol/Luna/Terra orchestration template into the user-owned repository and rebrand its project-facing identity to `codex-orchestrator`.

**Architecture:** Keep the repository as a documentation/configuration template. Import the upstream files into the empty destination, change only project-facing names and descriptions, and preserve the agent roles, filenames, model identifiers, delegation contract, and MIT license.

**Tech Stack:** Markdown, TOML, Git, shell-based structural validation.

**Spec:** `docs/superpowers/specs/2026-08-30-codex-orchestrator-rebrand-design.md`

## Global Constraints

- Preserve the Sol, Luna, and Terra model roles and all template behavior.
- Preserve the MIT license and its copyright notice.
- Keep the destination repository's existing `origin` remote; do not push or mutate GitHub repository settings.
- Do not alter model configuration values or orchestration instructions except where a repository/project name is user-facing.
- Treat old project-brand references as review failures except references required for attribution, provenance, or model role names.

### Task 1: Import the upstream project files

**Files:**
- Create: `LICENSE`
- Create: `README.md`
- Create: `examples/example-prompt.md`
- Create: `templates/AGENTS.md`
- Create: `templates/.agents/skills/delegate-work/SKILL.md`
- Create: `templates/.codex/agents/luna-worker.toml`
- Create: `templates/.codex/agents/terra-worker.toml`

**Interfaces:**
- Consumes: upstream repository `https://github.com/breko861-hash/sol-luna-codex-orchestrator.git`, branch `main`, source tip `cc2012f77a4989e914accab93a51728d98f4ddc`.
- Produces: the complete upstream file tree in the destination worktree, with no upstream remote added to the destination repository.

- [ ] **Step 1: Create a temporary shallow source clone outside the worktree**

Run:

```bash
source_dir=$(mktemp -d /tmp/codex-orchestrator-source.XXXXXX)
git clone --depth=1 --branch main https://github.com/breko861-hash/sol-luna-codex-orchestrator.git "$source_dir/repo"
printf '%s\\n' "$source_dir/repo" > /tmp/codex-orchestrator-source.path
printf '%s\\n' "$source_dir/repo"
```

Expected: clone succeeds and prints a temporary source path. Keep `/tmp/codex-orchestrator-source.path` until Task 1 is complete; the later snippets reload `source_dir` from it.

- [ ] **Step 2: Verify the source snapshot and destination state**

Run:

```bash
source_dir=$(dirname "$(cat /tmp/codex-orchestrator-source.path)")
git -C "$source_dir/repo" rev-parse HEAD
git -C "$source_dir/repo" ls-tree -r --name-only HEAD
git status --short --branch
```

Expected: source `HEAD` is `cc2012f77a4989e914accab93a51728d98f4ddc`; the source tree lists the seven expected project files; the destination only contains the already committed setup/spec files.

- [ ] **Step 3: Materialize the source files into the destination**

Run from the destination worktree:

```bash
source_dir=$(dirname "$(cat /tmp/codex-orchestrator-source.path)")
git -C "$source_dir/repo" archive --format=tar HEAD | tar -x -C "$PWD"
```

Expected: the seven source files are created and no `.git` directory is copied.

- [ ] **Step 4: Confirm the raw import before rebranding**

Run:

```bash
test -f LICENSE && test -f README.md
test -f examples/example-prompt.md
test -f templates/AGENTS.md
test -f templates/.agents/skills/delegate-work/SKILL.md
test -f templates/.codex/agents/luna-worker.toml
test -f templates/.codex/agents/terra-worker.toml
git diff --check
```

Expected: all checks succeed and the imported files are present without whitespace errors.

- [ ] **Step 5: Commit the raw import**

Run:

```bash
git add LICENSE README.md examples templates
git commit -m "chore: import orchestration template"
```

Expected: the upstream project files are committed without the rebrand changes.

### Task 2: Rebrand project-facing content

**Files:**
- Modify: `README.md`
- Modify: `examples/example-prompt.md`
- Modify: `templates/AGENTS.md` only if it contains project-brand copy after import
- Modify: `templates/.agents/skills/delegate-work/SKILL.md` only if it contains project-brand copy after import
- Do not modify: `LICENSE`, `templates/.codex/agents/luna-worker.toml`, `templates/.codex/agents/terra-worker.toml`

**Interfaces:**
- Consumes: imported source tree from Task 1.
- Produces: project-facing copy using `Codex Orchestrator` as the display name and `codex-orchestrator` as the repository/package name, while preserving Sol/Luna/Terra role names and behavior.

- [ ] **Step 1: Identify all old-brand occurrences outside provenance/spec documents**

Run:

```bash
rg -n -i --glob '!docs/superpowers/**' 'sol[ +_-]*luna|sol-luna-codex-orchestrator' .
```

Expected: occurrences are limited to project-facing copy that needs rebranding, plus any Sol/Luna role references that must remain because they describe the model roles.

- [ ] **Step 2: Apply the approved documentation rebrand**

Update project-facing text with these exact rules:

```text
Project title: Codex Orchestrator
Repository identifier: codex-orchestrator
Keep role names: Sol, Luna, Terra
Keep model identifiers: gpt-5.6-luna and gpt-5.6-terra
Keep filenames: luna-worker.toml and terra-worker.toml
```

Preserve the quick-start tree, routing table, workflow, example prompt, delegation contract, and philosophy. Do not change the license text or agent configuration values.

- [ ] **Step 3: Review the rebrand diff**

Run:

```bash
git diff -- README.md examples/example-prompt.md templates/AGENTS.md templates/.agents/skills/delegate-work/SKILL.md
git diff -- LICENSE templates/.codex/agents/luna-worker.toml templates/.codex/agents/terra-worker.toml
```

Expected: user-facing project identity changes are visible; the license and agent TOML files have no changes.

- [ ] **Step 4: Commit the rebrand**

Run:

```bash
git add README.md examples templates
git commit -m "docs: rebrand as codex orchestrator"
```

Expected: the rebranded project-facing content is committed.

### Task 3: Validate and commit the imported project

**Files:**
- Validate: all imported files and `docs/superpowers/specs/2026-08-30-codex-orchestrator-rebrand-design.md`
- Create: `docs/superpowers/plans/2026-08-30-codex-orchestrator-rebrand.md`

**Interfaces:**
- Consumes: rebranded project tree from Tasks 1–2.
- Produces: a clean, committed branch containing the imported and rebranded project.

- [ ] **Step 1: Run structural smoke validation**

Run:

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
    Path("templates/.codex/agents/terra-worker.toml"),
]
for path in required:
    assert path.is_file(), path

for path in required[-2:]:
    data = tomllib.loads(path.read_text())
    assert data["name"] in {"luna-worker", "terra-worker"}
    assert data["model_reasoning_effort"] == "high"

readme = Path("README.md").read_text()
assert "Codex Orchestrator" in readme
assert "Sol" in readme and "Luna" in readme and "Terra" in readme
PY
```

Expected: the script exits successfully.

- [ ] **Step 2: Scan for unresolved brand references and whitespace errors**

Run:

```bash
if rg -n -i --glob '!docs/superpowers/**' 'sol[ +_-]*luna codex orchestrator|sol-luna-codex-orchestrator' .; then
  exit 1
fi
git diff --check
```

Expected: `rg` finds no unresolved old project-brand reference outside the design/plan provenance documents, and `git diff --check` succeeds.

- [ ] **Step 3: Inspect final tree, remote, and diff summary**

Run:

```bash
find . -path './.git' -prune -o -type f -print | sort
git remote -v
git status --short
git diff --stat
```

Expected: the expected project files are present, `origin` points to `https://github.com/nab0y4enko/codex-orchestrator.git`, and only intended files are changed.

- [ ] **Step 4: Verify the committed state**

Run:

```bash
git status --short --branch
git show --stat --oneline --decorate HEAD
git diff HEAD^ HEAD --check
```

Expected: the worktree is clean, `HEAD` is the rebrand commit on `feat/codex-orchestrator-rebrand`, and the committed diff has no whitespace errors.
