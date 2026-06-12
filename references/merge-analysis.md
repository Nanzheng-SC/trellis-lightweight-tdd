# Merge Analysis: Trellis + Lightweight TDD

## Sources Reviewed

Superpowers:

- `skills/test-driven-development/SKILL.md`
- `skills/verification-before-completion/SKILL.md`
- `skills/executing-plans/SKILL.md`
- `skills/writing-plans/SKILL.md`
- `skills/finishing-a-development-branch/SKILL.md`

Trellis:

- `README.md`
- `AGENTS.md`
- `CLAUDE.md`
- `packages/cli/src/templates/trellis/workflow.md`
- `packages/cli/src/templates/codex/skills/before-dev/SKILL.md`
- `packages/cli/src/templates/codex/skills/check/SKILL.md`
- `packages/cli/src/templates/codex/skills/update-spec/SKILL.md`
- `packages/cli/src/templates/codex/skills/finish-work/SKILL.md`
- `packages/cli/src/templates/codex/skills/improve-ut/SKILL.md`
- `packages/cli/src/templates/common/bundled-skills/trellis-spec-bootstrap/SKILL.md`

## Directly Reused Ideas

From Trellis:

1. Specs live under `.trellis/spec/` and should be read before implementation.
2. Tasks live under `.trellis/tasks/` and hold PRDs, research, context lists, and status.
3. The durable workflow is Plan -> Implement -> Verify -> Finish.
4. `before-dev` means discover applicable spec layers and read their concrete guideline files.
5. `check` means inspect changed files, map them to specs, run lint/type-check/tests, and fix violations.
6. `update-spec` captures reusable project knowledge after implementation or debugging.
7. `finish-work` is for archiving and journaling after code changes are already handled.

From Superpowers:

1. Red-green-refactor is useful for behavior changes.
2. A regression test should reproduce a bug before the fix when practical.
3. Tests should fail for the expected reason before implementation.
4. Minimal implementation reduces speculative work.
5. Completion claims need fresh verification evidence.

## Rewritten Or Lightened

1. Superpowers' "always TDD" rule is rewritten as "TDD when it provides useful confidence."
2. The strict "delete code written before tests" rule is removed; it is too destructive for shared Codex/API environments.
3. Trellis' default "create task for every implementation" is softened for lightweight tasks.
4. Trellis' default subagent dispatch is converted to an inline-first Codex default unless parallel work is genuinely useful.
5. Superpowers' plan-writing style is reduced to durable Trellis PRDs only for multi-step or unclear work.
6. Finish behavior is kept as a pattern, but automatic commits, branch cleanup, PR creation, and pushes require user intent or local workflow authorization.

## Removed From The Merged Skill

1. Default brainstorming for small changes.
2. Default writing-plans for small changes.
3. Default git worktree creation.
4. Default branch creation.
5. Default subagent-driven development.
6. Default commits of specs/plans/code.
7. Mandatory large test infrastructure setup in projects without tests.
8. Any destructive cleanup outside approved temporary clone directories.

## Added As Trellis Supplements

1. Bug-fix TDD lane: reproduce -> failing regression test if feasible -> minimal fix -> verification.
2. Feature TDD lane: smallest behavior test -> minimal implementation -> edge tests where useful -> refactor.
3. No-test fallback: smoke test, script, or manual verification plus explicit gap reporting.
4. Lightweight task gate so small changes avoid heavy workflow by default.
5. Codex defaults: inspect files, use executing-plans or direct implementation, verify locally, avoid subagent assumptions.
6. One-question clarification cap for lightweight tasks.
7. Explicit high-risk confirmation list.

## Not Suitable For Current Codex Default

1. Forcing worktrees for implementation plans.
2. Assuming all platforms support Trellis subagent hooks.
3. Assuming Trellis commands exist everywhere.
4. Pushing or opening PRs as part of finishing without explicit user request.
5. Rigid "no production code without failing test" rules for docs, style, small config, generated code, or no-test projects.

## Second-Pass Enhancement

The first merged version was intentionally lightweight. The second pass adds a mode router so the same skill can also intentionally reproduce native-feeling workflows for large projects:

1. **Lightweight Hybrid** remains the default for small fixes and local patches.
2. **Trellis Native** restores PRD, task, research, context, spec, check, update-spec, and finish behavior for large project work.
3. **Superpowers Native** restores strict red-green-refactor, plan discipline, and verification-before-completion when explicitly requested.

This lets the skill act like a gearbox instead of a single fixed workflow.

## Final Design Choice

The final skill keeps Trellis as the organizing memory layer and borrows Superpowers as a verification discipline. The merged rule is:

Use the lightest workflow that still produces evidence. Scale up to Trellis-native tasks, specs, PRDs, and finish flow when the work needs durable coordination. Scale into Superpowers-native strict TDD when the user explicitly wants maximum test discipline. Scale down to direct implementation and local checks when the task is small.
