<div data-theme-toc="true"></div>

# 8. 实用做法

这一模块把前面的内容整理为可执行规则，供你在真实任务里反复查用。

如果你在设计 harness 结构，先回到 [6. Harness Engineering](../6-harness-engineering/index.md)；如果你要把整套做法固化成项目模板，读 [9. 可验证、可复用、可恢复的开发系统](../9-development-system/index.md)。

## 本章结构

| 子章节 | 目标 |
|---|---|
| [1-personal-workflow.md](1-personal-workflow.md) | 个人开发者工作流 |
| [2-team-workflow.md](2-team-workflow.md) | 团队协作工作流 |
| [3-checklists.md](3-checklists.md) | 开发前、中、后检查清单 |
| [4-anti-patterns.md](4-anti-patterns.md) | 常见坑和坏写法 |
| [5-prompt-library.md](5-prompt-library.md) | 跨阶段提示词库 |
| [6-review-rubric.md](6-review-rubric.md) | Agent 产出的质量审查标准 |
| [7-end-to-end-playbook.md](7-end-to-end-playbook.md) | 从想法到交付的端到端操作手册 |

## 怎么读

```text
个人先读 8.1、8.3、8.4、8.5。
团队再读 8.2、8.6、8.7。
实际执行时以检查清单和审查标准为准。
```

本章不再介绍新工具。它只回答执行中的具体问题：怎么开任务，怎么限制范围，怎么要求 Agent 留证据，怎么发现它提前宣布完成。

## 读完要学会什么

| 产物 | 用途 |
|---|---|
| 个人或团队工作流 | 让任务从想法走到 diff、验证和复盘 |
| 检查清单 | 开发前、中、后都有可查步骤 |
| 反模式清单 | 发现一句话开工、不看 diff、不验证等问题 |
| 提示词库 | 在任务、审查、恢复阶段复用 |
| 审查标准 | 按 correctness、scope、tests、security 等维度看 Agent 输出 |

## 本章边界

本章负责执行，不负责重新解释概念。概念回到 [2. 三层工程方法](../2-engineering-layers/index.md)，系统结构回到 [6. Harness Engineering](../6-harness-engineering/index.md)。
