<div data-theme-toc="true"></div>

# 5.2 skills：把流程做成卡片

[skills](https://agentskills.io/) 不会让模型突然变聪明。它的作用是让 Agent 每次按稳定流程做事。

可以把 skill 理解成“带触发条件的小操作手册”。它通常就是一个目录、一个 `SKILL.md`，再加少量脚本、模板或参考资料。

## skill 适合封装什么

适合：

- Bug 修复流程。
- 代码审查流程。
- 前端 polish 流程。
- API 变更流程。
- 发布说明流程。
- 数据分析流程。
- 文档生成流程。

不适合：

- 一次性任务。
- 尚未稳定的流程。
- 你无法清楚描述的经验。
- 和已有 skill 高度重叠的东西。

## 一个 skill 通常包含

```text
SKILL.md
  触发条件、工作步骤、规则。

references/
  需要时再读的资料。

scripts/
  稳定、重复、容易出错的脚本。

assets/
  模板、示例、静态资源。
```

最小目录：

```text
commit-message-cn/
  SKILL.md
```

更完整的目录：

```text
browser-qa/
  SKILL.md
  references/
    checklist.md
  scripts/
    verify_screenshot.py
  templates/
    report.md
```

## 不同工具的放置位置

同一个 skill 可以复用，但加载路径和可识别字段不完全一样。写团队 skill 时，先选主平台，再决定是否同步到其他目录。

| 工具 | 个人目录 | 项目目录 | 注意 |
|---|---|---|---|
| Claude Code | `~/.claude/skills/<name>/SKILL.md` | `.claude/skills/<name>/SKILL.md` | 可用 `allowed-tools` 等 Claude Code 字段限制工具 |
| OpenCode | `~/.config/opencode/skills/<name>/SKILL.md` | `.opencode/skills/<name>/SKILL.md` | 也会读取全局或项目里的 `.claude/skills`、`.agents/skills`，但权限要按 OpenCode 的 agent 或 permission 配置处理 |
| Codex | 按当前 Codex 版本的 skills 目录管理 | 团队共享前先用本机客户端验证加载路径 | 不要假设 Claude Code 的字段会在 Codex 里生效 |

跨工具共享时，`SKILL.md` 里的正文尽量写成通用流程。平台专用字段可以保留，但必须知道哪些客户端会忽略它们。权限控制不要只靠 skill 文本，能写文件、跑命令、访问外部系统的能力还要放到对应 CLI 的权限配置里。

## 什么时候写自己的 skill

遇到这些信号，再写自己的 skill：

- 你第三次复制同一段 prompt。
- Agent 第三次漏同一个步骤。
- 某个流程有固定输入输出。
- 团队希望复用同一套做法。
- 这个流程依赖人工记忆时容易遗漏。

## skill 写法原则

### 1. 描述触发场景

描述要写清楚什么时候用：

```yaml
description: Use when fixing reproducible bugs, including reading logs, reproducing, isolating root cause, making minimal fix, adding regression tests, and writing a short retrospective.
```

### 2. 步骤要具体

不要写：

```text
认真修复 bug。
```

要写：

```text
1. 复现问题。
2. 提出 2-3 个假设。
3. 用日志或测试验证假设。
4. 做最小修复。
5. 加回归测试。
6. 跑验证命令。
7. 写复盘。
```

### 3. 不要塞长篇资料

`SKILL.md` 应该短。长资料放 `references/`，脚本放 `scripts/`。

## skills 的风险

- 装太多导致触发混乱。
- 多个 skill 管同一件事。
- skill 过长，Agent 难以识别重点。
- skill 过度限制任务。
- 没验证 skill 是否减少返工。

建议的维护习惯：

```text
少装。
常用。
边用边改。
优先自建。
```

## 练习 1：写一个个人 commit message skill

Claude Code 的个人 skill 目录示例：

```bash
mkdir -p ~/.claude/skills/commit-message-cn
```

OpenCode 的个人 skill 目录示例：

```bash
mkdir -p ~/.config/opencode/skills/commit-message-cn
```

下面内容可以放进 Claude Code 或 OpenCode 对应目录的 `SKILL.md`。示例路径先用 Claude Code：

`~/.claude/skills/commit-message-cn/SKILL.md`：

```md
---
name: commit-message-cn
description: Generate concise Chinese git commit messages from staged diffs. Use when the user asks for commit messages, commit titles, or change summaries in Chinese.
---

# Commit Message CN

## Instructions

1. Inspect the staged diff.
2. Write one Chinese subject line under 50 characters.
3. Add a body only when the reason is not obvious.
4. Do not invent tests, issue numbers, or reviewers.
```

练习提示词：

```text
根据 staged diff 写一个中文 commit message。
```

如果 skill 没触发，先检查 `description` 是否包含用户会说出的词，比如 "commit message"、"提交信息"、"change summary"。

## 练习 2：写一个项目 review skill

项目共享 skill 适合放团队规则。示例：

```bash
mkdir -p .claude/skills/project-review
```

OpenCode 项目 skill 示例：

```bash
mkdir -p .opencode/skills/project-review
```

OpenCode 没有单独的“安装 skill”命令。skill 放进目录后，由 Agent 按描述发现。需要限制某个 agent 是否能调用 skill 时，用 agent 权限处理：

```bash
opencode agent create --path .opencode/agent --description "只读审查当前变更" --mode subagent --permissions read,grep,glob,skill
opencode agent list
```

下面内容可以放进 Claude Code 或 OpenCode 对应目录的 `SKILL.md`。示例路径先用 Claude Code：

`.claude/skills/project-review/SKILL.md`：

```md
---
name: project-review
description: Review code against this repository's local conventions. Use when reviewing PRs, checking changes before commit, or verifying implementation quality.
allowed-tools: Read, Grep, Glob
---

# Project Review

## Instructions

1. Read repository instructions and relevant spec files.
2. Review correctness, missing tests, inconsistent naming, and unsafe data flow.
3. Report findings first, ordered by severity.
4. Include file and line references.
5. Do not edit files.
```

`allowed-tools` 是 Claude Code 支持的限制方式。别的平台未必支持同样字段，但"只读 review skill"这个思路可以复用。

写完 skill 后要试一次真实任务。只有触发准确、步骤没有漏、输出比手写 prompt 更稳定，才保留。
