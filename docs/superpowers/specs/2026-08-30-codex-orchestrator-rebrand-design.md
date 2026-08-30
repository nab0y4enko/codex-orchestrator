# Codex Orchestrator Rebrand Design

## Goal

Create an independently branded copy of the `sol-luna-codex-orchestrator` source in the user-owned `codex-orchestrator` repository.

## Scope

- Import the source repository's complete project files from its `main` branch.
- Brand the project-facing documentation as `Codex Orchestrator` / `codex-orchestrator`.
- Preserve the Sol, Luna, and Terra model roles and all template behavior.
- Preserve the MIT license and its copyright notice.
- Keep the destination repository's existing `origin` remote; do not push or mutate GitHub repository settings.

The source snapshot is the upstream `main` tip observed on 2026-08-30 (`cc2012f77a4989e914accab93a51728d98d4fddc`). The destination starts as an empty repository, so the imported content will become a fresh destination history rather than retaining the upstream repository's commit history.

## Architecture and Files

The project remains a configuration-and-documentation template:

- `README.md` explains the product identity, setup, routing, workflow, included files, and philosophy.
- `templates/AGENTS.md` supplies project-level orchestration instructions.
- `templates/.agents/skills/delegate-work/SKILL.md` defines the worker assignment contract.
- `templates/.codex/agents/luna-worker.toml` and `terra-worker.toml` define the bounded worker configurations.
- `examples/example-prompt.md` demonstrates the intended post-install prompt style.
- `LICENSE` preserves the upstream MIT terms and attribution.

Only project-facing branding and descriptive copy change. Agent names, filenames, model identifiers, routing semantics, delegation contract, and example behavior remain stable.

## Data Flow and Behavior

The installed template continues to route work from Sol to Luna, optionally Terra, and back to Sol for review and acceptance. The rebrand changes how the repository is described and identified, not how that routing operates. The destination's Git remote remains `https://github.com/nab0y4enko/codex-orchestrator.git`.

## Error Handling and Safety

- Do not overwrite unrelated destination content; the destination is verified empty except for its worktree ignore setup.
- Do not remove or rewrite the MIT license.
- Do not alter model configuration values or orchestration instructions except where a repository/project name is user-facing.
- Do not push to GitHub automatically.
- Treat any remaining old project-brand references as a review failure unless they are required for attribution or describe the model role names.

## Validation

1. Check the full worktree for unresolved old-brand references.
2. Run `git diff --check`.
3. Confirm the expected file tree and inspect the final diff.
4. Since this is a documentation/configuration template with no executable test suite, run a focused structural smoke check: required files exist, TOML files parse, and the delegation skill/config files contain their expected role identifiers.

