<div data-theme-toc="true"></div>

# 6.2 AGENTS.md / CLAUDE.md 怎么写

`AGENTS.md` 和 `CLAUDE.md` 是最小 harness 的入口，负责给 Agent 提供仓库导航和硬规则。

## 它们应该写什么

| 内容 | 示例 |
|---|---|
| 项目形态 | Next.js + API routes + PostgreSQL |
| 命令 | `pnpm test`、`pnpm typecheck` |
| 工作流 | 多文件任务先计划 |
| 禁止项 | 不引入 Redux、不改 public API |
| 常见风险 | 改 auth 时同步 session type |
| 完成标准 | 测试通过，diff 无无关改动 |

## 不应该写什么

- 语言基础。
- 大段教程。
- “代码要优雅”。
- 完整 API 文档。
- AI 可以自己从代码推断的信息。

## 推荐结构

```markdown
# Project Instructions

## Commands
- Install: `pnpm install`
- Dev: `pnpm dev`
- Test: `pnpm test`
- Typecheck: `pnpm typecheck`
- Build: `pnpm build`

## Workflow
- Read before editing.
- For multi-file tasks, propose a plan first.
- Keep changes scoped.
- Run relevant checks before completion.

## Guardrails
- Do not change public API unless requested.
- Do not add dependencies without approval.
- Do not modify generated files manually.

## Common Pitfalls
- When changing auth, update session types.
- API errors must use the shared error helper.
```

## 维护规则

每次任务结束后问：

```text
这次有没有规则应该沉淀？
有没有旧规则已经没用了？
有没有规则太长应该拆出去？
```

规范文件需要持续维护，不是一次性说明书。规则如果不能指导下一次任务，就删掉或下沉到更合适的文档。
