<div data-theme-toc="true"></div>

# 5.4 最小可用扩展栈

不要在初始阶段接入大量 MCP 和 skills。最小栈更容易验证和维护，也更容易判断到底是哪一个工具带来了价值。

## 个人开发者最小栈

```text
本地 Agent:
  Codex / Claude Code / OpenCode 任选一个主力

规范:
  AGENTS.md / CLAUDE.md / opencode.json 中的项目规则

MCP:
  文档检索
  代码搜索

skills:
  bug-fix
  code-review

验证:
  test / lint / typecheck / build
```

## 团队最小栈

```text
本地 Agent:
  Codex / Claude Code / OpenCode 选主力，其他工具做补充

规范:
  AGENTS.md / CLAUDE.md
  opencode.json
  docs/architecture.md
  specs/

MCP:
  GitHub/GitLab
  Jira/Linear
  文档检索

skills:
  issue-to-plan
  code-review
  release-note
  bug-fix

Harness:
  Trellis / Superpowers / 自定义流程
```

## 接入顺序

```text
1. 先写规范文件。
2. 再接只读 MCP。
3. 再写一个高频 skill。
4. 再接外部系统。
5. 最后考虑自动化写入。
```

## 每加一个工具都要问

- 它减少了哪类重复问题？
- 它会不会增加上下文噪声？
- 它的权限是否最小？
- 失败时是否可诊断？
- 它和已有 skill 是否冲突？

如果说不清楚，就先不要加。工具数量增加后，维护对象也会增加：权限、文档、触发条件、失败日志和团队培训都要有人负责。
