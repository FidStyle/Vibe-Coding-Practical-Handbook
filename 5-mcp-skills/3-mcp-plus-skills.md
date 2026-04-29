<div data-theme-toc="true"></div>

# 5.3 MCP + skills 怎么组合

MCP 是插座，skills 是操作规程。只装 MCP，Agent 有工具但未必按你的方法做；只写 skill，Agent 知道流程但接触不到外部系统。更稳的工作流通常是两者配合：MCP 提供能力，skill 约束步骤、风格和安全边界。

MCP 和 skills 不是替代关系，分别对应工具接入和流程约束。

```text
MCP 解决“Agent 能调用什么工具”。
skills 解决“Agent 应该按什么流程调用工具”。
```

## 例子：修 Bug

只用 MCP：

```text
Agent 能查日志、搜代码、跑浏览器。
但不一定知道按什么顺序做。
```

只用 skill：

```text
Agent 知道修 Bug 流程。
但没有工具就只能使用已有上下文。
```

MCP + skill：

```text
bug-fix skill 规定：
1. 用日志 MCP 拉错误。
2. 用代码搜索 MCP 定位调用链。
3. 用浏览器 MCP 复现。
4. 修复。
5. 跑测试。
6. 写复盘。
```

工具补能力，skill 补顺序，这个组合可以降低工具误用概率。

## 例子：前端检查

MCP：

- Browser / Playwright。
- Figma。
- 截图。
- 可访问性扫描。

skill：

- 布局检查。
- 响应式检查。
- 文案检查。
- 颜色和交互一致性。
- 生成最终问题清单。

组合后，Agent 不只会“看页面”，还会按固定标准审查页面。

## 例子：团队工单

MCP：

- Jira / Linear。
- GitHub。
- Slack。
- Notion。

skill：

- 读取需求。
- 提炼验收标准。
- 创建计划。
- 更新工单状态。
- 生成 PR 描述。

## 设计组合时问三个问题

1. Agent 需要什么外部能力？
2. 这些能力的权限边界是什么？
3. 调用这些能力的流程是否已经稳定，可以固化？

如果第 3 个答案是否定的，不要立即写 skill。手动执行几次，流程稳定后再固化。
