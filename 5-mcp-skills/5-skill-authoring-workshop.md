<div data-theme-toc="true"></div>

# 5.5 从重复流程提炼一个 skill

> 这一节只处理一个问题：把一段反复复制的工作流提示词，整理成一个可复用的 skill。

`skill` 的价值在于让 Agent 每次都按同一套流程工作。团队经验、检查清单、输出格式、工具调用顺序，这些反复说明的内容都适合纳入 skill。

## 什么时候该写 skill

满足任意两条就可以写：

- 同一段提示词复制了 3 次以上。
- 每次都要解释同一套项目流程。
- 任务有固定输入和固定输出。
- Agent 经常漏步骤。
- 团队希望多人共用同一标准。
- 这个流程需要搭配参考文件或脚本。

不适合写 skill：

- 一次性需求。
- 还没稳定的做法。
- 只有一句简单偏好。
- 需要大量人工判断且无法结构化的任务。

## skill 的基本结构

一个最小 skill 通常采用这种结构：

```text
my-skill/
  SKILL.md
  references/
    checklist.md
  scripts/
    helper.sh
```

必要文件是 `SKILL.md`。参考资料和脚本都是可选。

## `SKILL.md` 最小模板

```markdown
---
name: repo-review
description: Use when reviewing a code diff before commit. Focus on correctness, scope control, tests, security, and documentation sync.
---

# Repo Review Skill

## When to Use

Use this skill when the user asks for a review, pre-commit check, or final quality pass.

## Workflow

1. Inspect current diff.
2. Identify unrelated changes.
3. Check correctness risks.
4. Check missing tests.
5. Check docs that need updates.
6. Report findings by severity.

## Output Format

- Findings first.
- Each finding includes file path and line if possible.
- If no findings, say no findings and mention residual risks.

## Rules

- Do not rewrite code unless the user asks.
- Do not praise the diff.
- Do not report speculative issues without concrete evidence.
```

`description` 很重要。Agent 会用它判断何时自动调用这个 skill。

## 工作坊：把 PR 审查流程做成 skill

### 第 1 步：先写一次普通提示词

```text
请审查当前 diff，重点看：
- 逻辑 bug。
- 无关改动。
- 测试缺失。
- 类型和边界。
- 安全风险。
- 文档是否需要同步。

输出 findings first，按严重程度排序。
```

先手动执行几次，不要把尚未稳定的流程写进 skill。先让人工流程跑通，再把它交给 Agent。

### 第 2 步：提炼固定规则

把每次都不变的内容抽出来：

- 审查范围。
- severity 排序。
- 输出格式。
- 不要做什么。
- 需要检查哪些命令。

不要把某个具体任务的细节写进 skill。

### 第 3 步：增加参考清单

`references/review-checklist.md`：

```markdown
# Review Checklist

## Correctness
- 输入为空时是否正确。
- 异步失败是否处理。
- 数据格式是否跨层一致。

## Scope
- 是否改了无关文件。
- 是否做了未要求的重构。

## Tests
- 新行为是否有测试。
- 旧行为是否有回归测试。

## Security
- 是否泄露 token。
- 是否扩大权限。
- 是否信任用户输入。
```

然后在 `SKILL.md` 里写：

```markdown
Before reporting, consult `references/review-checklist.md`.
```

### 第 4 步：测试 skill

用 3 种任务测试：

- 一个明显有 bug 的 diff。
- 一个没有问题的小 diff。
- 一个包含无关格式化的大 diff。

检查它是否能正确触发、按格式输出、保持只读边界，并避免泛泛建议。

## skill 的输出要硬

差的输出：

```text
代码整体不错，可以考虑增加一些测试。
```

好的输出：

```text
P1: `src/importer.ts` 在 CSV 为空时仍然调用 `rows[0]`，会触发 runtime error。
建议增加空文件测试，并在解析后提前返回结构化错误。
```

skill 应该让 Agent 输出具体依据，而不是只调整语气。

## skill 和 `AGENTS.md` 的分工

| 内容 | 放哪里 |
|---|---|
| 项目长期规则 | `AGENTS.md` / `CLAUDE.md` |
| 某类任务固定流程 | skill |
| 大段参考资料 | skill `references/` |
| 可执行辅助逻辑 | skill `scripts/` |
| 当前任务目标 | PRD / prompt |

## 维护 skill

每次 skill 失败后，不要只在当前会话追加临时说明。应该问：

```text
这次失败是 skill 规则缺失，还是当前任务特殊？
如果是规则缺失，请给出应该沉淀到 SKILL.md 的最小修改。
```

skill 不是一次写完后长期不变。它需要根据真实任务持续修订。修订时优先改触发条件、步骤边界和停止条件，不要只加更多形容词。

## 交付检查表

- [ ] `description` 清楚说明触发场景。
- [ ] workflow 是步骤，不是理念。
- [ ] 输出格式固定。
- [ ] 禁止行为明确。
- [ ] 示例和参考资料不会污染其他任务。
- [ ] 至少用 3 个真实或模拟任务试过。
