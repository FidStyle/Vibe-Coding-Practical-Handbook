<div data-theme-toc="true"></div>

# 7.3 工作流工具实战路径

这一练习不做业务功能，而是做一个小型 harness：让任务状态、计划和验证结果都有记录位置。

## 推荐练习

做一个 `task-runner` 目录：

```text
tasks/
  1-example/
    prd.md
    plan.md
    status.md
```

然后写一个简单脚本，或者先按手动流程验证：

- 创建任务目录。
- 生成 PRD 模板。
- 记录状态。
- 记录验证结果。

## 练这个的原因

完成后，你会更容易理解 Trellis、Spec workflow（规格工作流）、planning-with-files（文件化计划）这类工具为什么存在。

这些工具解决的共同问题是：

```text
把任务状态从聊天窗口搬到文件系统。
```

## 最小 PRD 模板

```markdown
# <Task Name>

## Goal

## Requirements

## Out of Scope

## Context Pointers

## Plan

## Done When

## Validation

## Retrospective
```

## 练习步骤

1. 手动创建一个任务目录。
2. 让 Agent 根据需求填写 PRD。
3. 让 Agent 先写实现计划，不写代码。
4. 人工审查 Agent 的实现计划。
5. 让 Agent 执行。
6. 让 Agent 把验证结果记录到 `status.md`。
7. 任务结束后写 `retrospective.md`。

## 你会学到什么

- 长任务为什么需要文件状态。
- 为什么 PRD 和实现计划要分开。
- 为什么复盘要沉淀到规范。
- 为什么 harness 是工程系统，不是 prompt 集合。
