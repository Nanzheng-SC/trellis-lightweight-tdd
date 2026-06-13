---
name: trellis-lightweight-tdd
description: Use for agentic coding workflow control with Trellis/TDD. Trigger on "light模式"/"ight模式" for lightweight inline work, "tn模式"/"TN模式" for Trellis Native, "sn模式"/"SN模式" for Superpowers Native, or when a coding task mentions Trellis, TDD, tests, regression, verification, specs, tasks, or finish-work.
license: MIT
---

# Trellis Lightweight TDD

Use this skill as a workflow router for coding tasks that need both project memory and test discipline.

It supports three experiences:

1. **Lightweight Hybrid**: small changes get direct implementation, minimal TDD, and local verification.
2. **Trellis Native**: large project work follows Trellis tasks, specs, PRD, research, implement, check, update-spec, and finish flow.
3. **Superpowers Native**: strict red-green-refactor, evidence-first verification, and plan execution discipline when the user explicitly wants the Superpowers feel.

Default to the lightest mode that still protects correctness. Escalate when the task's risk or the user's wording asks for it.

Read [references/merge-analysis.md](references/merge-analysis.md) when you need source rationale for the merge.

## Trigger Conditions

Use this skill when any of these are true:

- The repo has `.trellis/`, Trellis instructions, Trellis skills, Trellis tasks, or Trellis specs.
- The user asks for a code change, bug fix, feature, refactor, test update, verification, finish, or spec update in a Trellis-managed project.
- The user mentions TDD, test-driven development, red-green-refactor, failing tests, regression tests, smoke tests, or evidence before completion.
- The user asks for "lightweight Trellis", "Trellis but not too heavy", "native Trellis", "Superpowers style", "strict TDD", "full workflow", or similar.
- You are in Codex or a compatible API/agent platform and need an explicit workflow choice instead of blindly defaulting to subagents, worktrees, or large plans.

Do not use this skill for pure chat, explanations, or lookups that require no file writes and no real workflow choice.

## Mode Router

Start by choosing one mode. State the mode briefly if the task is non-trivial.

### Shortcut Triggers

If the user writes one of these phrases, obey it unless it conflicts with safety rules:

| Phrase | Mode | Meaning |
|---|---|---|
| `light模式` / `ight模式` | Lightweight Hybrid | Inline, small-scope execution with focused verification. Include `ight模式` as a typo-tolerant alias. |
| `tn模式` / `TN模式` | Trellis Native | Use Trellis task/spec/PRD/check/finish semantics for larger work. |
| `sn模式` / `SN模式` | Superpowers Native | Use stricter red-green-refactor and evidence-first completion. |

Shortcut mode names override automatic mode selection, but they do not authorize destructive actions, remote pushes, worktrees, branch creation, or security-sensitive edits.

### Mode A: Lightweight Hybrid

Use by default for:

- Single-file or small-scope changes.
- Clear bug fixes.
- Config, docs, copy, style, or small test updates.
- Local feature patches with no architectural or cross-layer contract change.

Workflow:

1. Read local instructions and relevant code/tests.
2. Identify the behavior boundary: input, output, changed files, and likely failure mode.
3. Determine the smallest useful verification.
4. For bugs, reproduce or write a failing regression test when practical.
5. Implement the smallest change.
6. Run focused tests or smoke/manual checks.
7. Summarize changes, mode, and evidence.

Avoid by default: large PRDs, brainstorming, writing-plans, worktrees, new branches, subagents, and multi-round questioning.

### Mode B: Trellis Native

Use when the user asks for native Trellis, full Trellis, big-project workflow, PRD/task management, or when the task:

- Touches multiple packages, layers, or contracts.
- Needs durable requirements, research, or team-readable context.
- Should update `.trellis/spec/` or `.trellis/tasks/`.
- Benefits from Trellis subagents where the platform supports them.

Workflow:

1. Read `AGENTS.md`, `.trellis/workflow.md`, and the active task if present.
2. Plan: create or update `.trellis/tasks/<task>/prd.md` when requirements need durability.
3. Research: write findings under the task's `research/` directory when needed.
4. Context: curate `implement.jsonl` and `check.jsonl` for subagent-capable Trellis flows; in Codex inline mode, load specs directly instead.
5. Implement: follow `trellis-before-dev` behavior, read relevant `.trellis/spec/` indexes and guideline files, then code.
6. Verify: follow `trellis-check` behavior, map changed files to specs, run lint/type-check/tests, and fix issues.
7. Finish: decide whether `trellis-update-spec` should capture new reusable knowledge; use `finish-work` behavior only after code changes are cleanly handled.

Native Trellis may use subagents only when the platform supports them and the user has not asked for inline execution. In Codex, prefer inline unless native Trellis dispatch is explicitly requested or clearly useful.

Escalate from Lightweight Hybrid to Trellis Native when you discover any of these during inspection:

- The change crosses package, service, frontend/backend, API, database, or auth boundaries.
- Requirements are unclear enough that code would encode guesses.
- The fix reveals a project convention that should become durable spec knowledge.
- Verification needs multiple independent checks or a durable PRD.

### Mode C: Superpowers Native

Use when the user explicitly asks for Superpowers style, strict TDD, strict red-green-refactor, evidence-first completion, or formal plan execution.

Workflow:

1. For implementation: write the failing test first, verify the failure, write minimal code, verify green, then refactor while staying green.
2. For bugs: reproduce the bug and prove the regression test fails for the expected reason before fixing when practical.
3. For multi-step work: write or follow a detailed implementation plan with exact verification steps.
4. Before completion: run fresh verification commands and report evidence, not assumptions.
5. If strict Superpowers requirements conflict with user safety rules or the current platform, ask once before using worktrees, subagents, destructive cleanup, branch operations, or remote pushes.

Do not force strict Superpowers Native mode for docs, style, small config, generated code, or projects with no realistic test surface unless the user explicitly asks.

Escalate from Lightweight Hybrid to Superpowers Native when the user asks for strict TDD or when a high-risk bug has already regressed and a failing regression test is practical.

## Programming Assistance Loop

For coding tasks, use this loop regardless of mode:

1. **Orient**: read the nearest instructions, changed area, existing tests, and entrypoints. Use `rg` for discovery.
2. **Classify**: bug, feature, refactor, docs/config, or test-only work.
3. **Constrain**: name the smallest behavior change and the files likely in scope.
4. **Prove before changing**:
   - Bug: reproduce or capture the failing behavior.
   - Feature: identify the first observable behavior.
   - Refactor: identify tests or characterization checks.
   - Docs/config: identify syntax or manual validation.
5. **Edit narrowly**: prefer existing local patterns and avoid unrelated refactors.
6. **Verify locally**: run the nearest meaningful test/check first, then broaden only if the change is shared or risky.
7. **Report honestly**: include commands, results, and any unverified gaps.

When there is no test framework, do not invent a heavy one. Use a smoke script, command-line reproduction, type-check/build, or manual acceptance steps.

## TDD Rules

Use TDD to increase confidence, not to perform ritual.

### Bug Fixes

1. Reproduce the bug first when practical.
2. If a suitable test framework exists and the bug is testable, write the smallest failing regression test.
3. Run the test and confirm the failure is expected.
4. Implement the smallest fix.
5. Run the regression test and nearby tests.
6. If automated testing is not practical, use a smoke test, focused script, or manual verification and explain the gap.

### New Features

1. If the behavior boundary is clear, write the smallest behavior test first.
2. Verify it fails for the expected reason.
3. Implement the minimum usable behavior.
4. Add edge tests only for real risk.
5. Refactor after verification is green.

### Refactors

- Prefer characterization tests for important uncovered behavior.
- Run existing relevant tests before and after.
- Do not expand scope while refactoring.

### Docs, Style, And Config

- Do not force TDD.
- Run the relevant syntax, format, link, build, or manual check.

## Trellis Compatibility Rules

When Trellis exists:

- Treat `.trellis/spec/` as the source of project-specific engineering standards.
- Treat `.trellis/tasks/` as durable task memory for non-lightweight work.
- Preserve Plan -> Implement -> Verify -> Finish.
- Use `trellis-before-dev` behavior before code.
- Use `trellis-check` behavior before claiming completion.
- Use `trellis-update-spec` behavior after non-trivial implementation or debugging if new reusable knowledge was learned.
- Use `finish-work` behavior only after code changes are committed or the local workflow allows finishing without commits.

If Trellis commands are unavailable, perform the equivalent file reads and checks manually.

## Codex Defaults

In Codex and compatible API environments:

- Prefer reading files, direct implementation, executing-plans for approved plans, and local verification.
- Do not create git worktrees by default.
- Do not create branches by default.
- Do not commit specs, plans, or code by default unless the user asks or the active workflow explicitly requires it.
- Do not push to remote by default.
- Do not assume subagents are available or appropriate.

If a worktree, branch, subagent, commit, or push is useful, explain why and get clear user intent first.

## Question Policy

Inspect before asking. Do not repeat questions answered by:

- Project instructions, `AGENTS.md`, README, or `.trellis/workflow.md`.
- Existing code and tests.
- Trellis specs, tasks, PRDs, research, `implement.jsonl`, or `check.jsonl`.
- Prior user instructions in the thread.

When a key choice remains, ask once with 2-3 options and a recommended default. For lightweight tasks, the first clarification should be at most one question.

## High-Risk Operations

Confirm before:

- Deleting or moving files outside an explicitly approved temporary scope.
- Large refactors.
- Git history edits, force pushes, pushes, PR publication, or remote changes.
- Environment, global config, CI, database, auth, permission, secret, or key-related changes.
- Batch moves or renames.
- Destructive cleanup.

For deletion or remote publication, state exact paths or repository target, command, reason, and risk.

## Output Format

For non-trivial work, summarize:

- Mode used: Lightweight Hybrid, Trellis Native, or Superpowers Native.
- Files changed.
- Specs/tasks consulted or why none were needed.
- TDD action: failing test, existing test, smoke/manual check, or no TDD due to docs/style/config.
- Verification commands and results.
- Remaining gaps.

## Invocation Examples

Use these phrases when you want a specific experience:

- "Use trellis-lightweight-tdd in lightweight mode for this small bug."
- "Use trellis-lightweight-tdd in Trellis Native mode; create a PRD and follow the task flow."
- "Use trellis-lightweight-tdd in Superpowers Native mode; strict red-green-refactor."
- "Trellis specs must be respected, but do this inline in Codex."
- "Big project mode: preserve Trellis task/spec/verify/finish."
