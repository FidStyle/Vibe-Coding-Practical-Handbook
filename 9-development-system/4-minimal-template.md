<div data-theme-toc="true"></div>

# 9.4 最小项目模板和执行步骤

这一节提供一套可以直接放进项目的最小模板。它不追求全量覆盖，只先保证三件事：本地 Agent 有规则，任务有状态，完成有证据。

如果你还没理解 Harness 的层次，先读 [2.3 Harness Engineering](../2-engineering-layers/3-harness-engineering.md)。如果你要从普通本地仓库一步步搭起第一版 Harness，读 [6.6 为一个项目搭最小 Harness](../6-harness-engineering/6-build-minimal-harness.md)。这里给的是最终可复用模板。

## 目录结构

```text
repo/
  AGENTS.md
  CLAUDE.md
  opencode.json
  docs/
    architecture.md
    testing.md
    pitfalls.md
  tasks/
    README.md
  skills/
    pre-review/
      SKILL.md
    bug-fix/
      SKILL.md
  .opencode/
    skills/
```

如果你只用 Codex，可以先写 `AGENTS.md`。如果你只用 Claude Code，可以先写 `CLAUDE.md`。如果你只用 OpenCode，可以先写 `opencode.json` 和 `.opencode/skills/`。多个工具并用时，保持规则语义一致，但不要把同一段长文档复制到多个入口里。

## AGENTS.md 模板

```markdown
# Project Agent Guide

## Repo Map

- `src/`: application source.
- `tests/`: automated tests.
- `docs/`: project knowledge and rules.
- `tasks/`: active task notes and handoffs.

## Required Reading

- Read `docs/architecture.md` before architectural changes.
- Read `docs/testing.md` before changing behavior.
- Read the active task file before editing code.

## Commands

- Install: `<command>`
- Test: `<command>`
- Typecheck: `<command>`
- Build: `<command>`

## Working Rules

- Start complex work with read-only analysis.
- Keep diffs scoped to the task.
- Do not introduce dependencies without explaining why.
- Do not modify secrets, production config, or generated files unless explicitly requested.
- If requirements are unclear, ask before editing.

## Done Means

- Acceptance criteria are satisfied.
- Relevant tests/checks are run or clearly marked as not run.
- Diff is summarized.
- Risks and follow-up work are listed.
```

## CLAUDE.md 模板

```markdown
# Claude Code Guide

Use this repository as an execution workspace, not a brainstorming-only chat.

## Default Loop

1. Read task context.
2. Inspect relevant files.
3. Propose a short plan.
4. Edit only in scope.
5. Run checks.
6. Summarize diff, verification, and risks.

## Stop Conditions

- You need to modify protected files.
- You find conflicting requirements.
- Tests fail for unclear reasons after two attempts.
- A change would expand scope.

## Review Mode

When asked to review, stay read-only and report findings first.
```

## docs/testing.md 模板

```markdown
# Testing Guide

## Fast Checks

- `<command>`: unit tests.
- `<command>`: typecheck.
- `<command>`: lint.

## Full Checks

- `<command>`: full test suite.
- `<command>`: build.
- `<command>`: E2E or smoke test.

## When to Run What

- Single function change: run targeted unit tests.
- API behavior change: run related integration tests.
- UI change: run build and manually inspect responsive states.
- Auth/payment/data migration: run full checks and request human review.

## Known Gaps

- <document flaky tests or unavailable checks here>
```

## tasks/README.md 模板

````markdown
# Tasks

Each complex task should have a folder:

```text
tasks/<date>-<slug>/
  prd.md
  plan.md
  journal.md
  verification.md
  handoff.md
```

Use a task folder when:

- The change touches multiple files.
- Requirements are not fully settled.
- The work may span more than one session.
- More than one Agent or human will collaborate.
````

## prd.md 模板

```markdown
# <Task Title>

## Goal

<What outcome do we want and why?>

## Requirements

- ...

## Acceptance Criteria

- [ ] ...

## Out of Scope

- ...

## Constraints

- ...

## Risks

- ...

## Verification Plan

- ...
```

## pre-review skill 模板

```markdown
---
name: pre-review
description: Use before opening a PR or asking for human review of a local diff.
---

# Pre Review

## Steps

1. Inspect changed files and diff summary.
2. Read nearby context for non-trivial changes.
3. Check requirements, tests, security, compatibility, and maintainability.
4. Produce findings first, then summary.

## Output

- Findings ordered by severity.
- Verification run and not run.
- Risk areas for human review.
- Suggested follow-up.

## Rules

- Stay read-only unless explicitly asked to fix.
- Do not report style preferences as bugs.
- Do not claim tests passed unless you ran them.
```

## bug-fix skill 模板

```markdown
---
name: bug-fix
description: Use when diagnosing and fixing a reproducible bug in the local repository.
---

# Bug Fix

## Steps

1. Capture symptom, expected behavior, actual behavior, and repro steps.
2. Inspect relevant code paths.
3. Form a hypothesis.
4. Reproduce or explain why reproduction is not possible.
5. Make the smallest fix.
6. Add or update a regression test when practical.
7. Run targeted checks.
8. Summarize cause, fix, and verification.

## Stop Conditions

- No repro and no clear evidence.
- Fix requires scope expansion.
- Third attempt fails.
- The bug touches security, data loss, or production config.
```

## 第一次执行步骤

1. 写 `AGENTS.md` 或 `CLAUDE.md`。
2. 写 `docs/testing.md`。
3. 选一个真实 bug，用 `tasks/<slug>/prd.md` 记录目标。
4. 让 Agent 先只读分析。
5. 让 Agent 实现最小修复。
6. 跑验证。
7. 用 `pre-review` 做只读审查。
8. 把复盘沉淀到 docs 或 skill。

## 最小可用完成标准

你不需要一开始就有完整 harness。先满足下面几点，就可以避免大部分无约束的 vibe coding：

- Agent 知道项目规则。
- 复杂任务有任务文件。
- 修改前有计划。
- 修改后有验证。
- 审查和实现分开。
- 经验会记录并复用。

先做到这些，再考虑 MCP、多 Agent、评测平台和自动化流水线。
