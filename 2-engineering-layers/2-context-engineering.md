<div data-theme-toc="true"></div>

# 2.2 Context Engineering：让 AI 知道正确的东西

Context Engineering 先回答一个问题：这次任务要做对，AI 到底需要知道什么。它要求你把项目事实放到 Agent 能找到、能判断优先级的位置。

目标是提供更准、更短、更容易定位的内容。

## 常见上下文资产

| 资产 | 作用 |
|---|---|
| `README.md` | 项目入口、运行方式 |
| `AGENTS.md` | Codex 常用项目/目录规范 |
| `CLAUDE.md` | Claude Code 常用项目/目录规范 |
| `.claude/rules/` | 按主题或路径拆分规则 |
| `.trellis/spec/` | 分层项目规范 |
| `prd.md` | 本次任务目标、范围、验收标准 |
| `plan.md` | 长任务计划 |
| `docs/architecture.md` | 架构决策 |
| 测试文件 | 告诉 AI 怎么验证 |

## 写上下文的原则

### 1. 只写 AI 推不出来的东西

应该写：

- 项目特有命令。
- 内部框架约定。
- 容易漏的同步文件。
- 不允许使用的库。
- 安全边界。
- 反复出错的地方。

不应该写：

- “写干净代码”。
- “遵循实用做法”。
- 标准语言语法。
- 文件逐个解释。
- 长篇 API 文档全文。

### 2. 规则要能检查

模糊规则：

```text
代码要简洁。
```

可检查规则：

```text
新增函数超过 40 行时，先说明为什么不能拆分。
```

模糊规则：

```text
注意测试。
```

可检查规则：

```text
修改 API handler 后必须运行 `pnpm test api`。
```

### 3. 上下文要能持续更新

好的 `AGENTS.md` / `CLAUDE.md` 会随着项目一起更新。

更新触发点：

- Agent 第二次犯同样错误。
- 新增测试命令。
- 项目结构变更。
- 引入新架构约束。
- 发现安全边界。
- 团队约定改变。

## 最小 AGENTS.md / CLAUDE.md 模板

```markdown
# Project Instructions

## Project Shape
- 这是一个 <技术栈> 项目。
- 主要代码目录是 <路径>。

## Commands
- Install: `<命令>`
- Test: `<命令>`
- Typecheck: `<命令>`
- Build: `<命令>`

## Workflow
- 修改前先搜索同类实现。
- 多文件任务先给计划，不要直接改。
- 完成前必须运行相关验证命令。

## Guardrails
- 不要修改 <路径>，除非用户明确要求。
- 不要引入 <库>。
- 不要改变公开 API。

## Common Pitfalls
- 修改 <A> 时必须同步 <B>。
- <某测试> 需要 <环境变量>。
```

## 上下文过载怎么办

当规范文件变长时，拆分：

```text
AGENTS.md / CLAUDE.md
  只放入口和最重要规则。

rules/testing.md
  测试规则。

rules/api.md
  API 规则。

rules/frontend.md
  前端规则。
```

主文件只引用路径，不粘贴所有内容。主文件越像目录，Agent 越容易按任务去读正确的局部文档。
