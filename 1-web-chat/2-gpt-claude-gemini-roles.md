<div data-theme-toc="true"></div>

# 1.2 海外和国内 AI 工具怎么分工

先别急着给模型排总榜。先判断你要解决的是网页问答、长文档阅读、API 接入，还是本地代码执行。海外工具打不开时，不要停在访问问题上，直接选择国内可用工具完成同类任务。

## 先按入口分类

| 类别 | 常见工具 | 适合做什么 |
|---|---|---|
| 海外网页工具 | [ChatGPT](https://chatgpt.com/) / [Claude](https://claude.ai/) / [Gemini](https://gemini.google.com/) | 通用问答、长文档、代码解释、资料整理 |
| 国内网页工具 | [DeepSeek](https://chat.deepseek.com/) / [Kimi](https://kimi.com/) / [通义千问](https://qianwen.aliyun.com/) / [豆包](https://www.doubao.com/product) / [文小言](https://yiyan.baidu.com/) / [腾讯元宝](https://yuanbao.tencent.com/) | 中文材料、日常办公、长文档和可访问性优先的任务 |
| API / 开放平台 | [OpenAI Platform](https://platform.openai.com/api-keys) / [Anthropic Console](https://console.anthropic.com/settings/keys) / [DeepSeek API](https://api-docs.deepseek.com/) / [智谱 BigModel](https://docs.bigmodel.cn/) / [阿里云百炼](https://help.aliyun.com/zh/model-studio/qwen-api-reference) / [火山方舟](https://www.volcengine.com/docs/82379) | 本地 Agent、自动化脚本、产品集成、批量调用 |
| 本地 coding agents | [Codex](https://github.com/openai/codex) / [Claude Code](https://github.com/anthropics/claude-code) / [OpenCode](https://github.com/anomalyco/opencode) / [Gemini CLI](https://github.com/google-gemini/gemini-cli) | 读本地仓库、改文件、跑命令、执行验证 |
| Provider 切换辅助 | [cc-switch](https://github.com/farion1231/cc-switch) | 经常切换多个 provider 时的可选管理工具 |

选择顺序很简单：能在网页里完成的任务，不要先装本地 Agent；需要读仓库、改文件、跑测试时，再进入本地 Agent；需要稳定自动化或团队集成时，再准备 API key 和 provider 配置。

## 海外网页端分工

| 工具 | 更适合 | 典型用法 |
|---|---|---|
| ChatGPT | 方案比较、推理、代码解释、产品与工程折中 | "给出 3 个实现方案，列风险和推荐" |
| Claude | 长文档、PRD、复杂需求整理、代码审查讨论 | "把这些想法整理成开发前 PRD" |
| Gemini | 大上下文、资料消化、图片/网页资料、Google 体系 | "读这批资料，提炼共性和差异" |

海外服务会受地区、账号、付款方式和组织策略影响。手册里的链接只作为入口，具体可用性以你当前账号为准。

## 国内可用替代

如果 ChatGPT、Claude、Gemini 打不开，先用这些工具完成同一件事：

| 场景 | 推荐工具 | 说明 |
|---|---|---|
| 中文推理和代码问答 | [DeepSeek](https://chat.deepseek.com/) | 适合先跑通思路，也有 [API 文档](https://api-docs.deepseek.com/) |
| 长中文文档 | [Kimi](https://kimi.com/) | 适合资料整理、长文档问答 |
| 企业 API 和云服务 | [阿里云百炼 / 通义](https://help.aliyun.com/zh/model-studio/qwen-api-reference) | 有 OpenAI 兼容调用和云控制台 |
| 大量生产调用 | [火山方舟 / 豆包](https://www.volcengine.com/docs/82379) | 适合已经使用字节云服务的团队 |
| 办公和搜索 | [文小言](https://yiyan.baidu.com/) / [腾讯元宝](https://yuanbao.tencent.com/) | 适合已有账号体系的日常任务 |

不要把“能打开”误认为“适合所有任务”。国内工具更适合先解决可访问性、中文材料和账号支付问题；海外工具可用时，再拿来做对照和补充。

## 本地开发时的分工

| 工具 | 更适合 |
|---|---|
| [Claude Code](https://github.com/anthropics/claude-code) | 复杂代码库协作、多文件改动、规划与执行结合 |
| [Codex](https://github.com/openai/codex) | 工程执行、任务委派、AGENTS.md 驱动的标准化工作 |
| [OpenCode](https://github.com/anomalyco/opencode) | 多模型、本地模型、LSP、自定义 agent 和开放工具链 |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | 大上下文辅助阅读、前端/UI 第二意见、低成本分析 |

进入 CLI 练习前先确认两件事：

```text
1. 已经准备一个可用的模型或 API 来源。
2. 本地 Git、Node、Python/uv 环境已经跑通。
```

Codex、Claude Code、OpenCode、Gemini CLI 都会读文件、改文件、跑命令。第一次练习用一个可以丢弃的小仓库，不要直接把生产仓库和高权限 token 交给工具。

## 一个常见组合

```text
Claude:
  需求理解、长文档、计划、编排。

Codex:
  后端逻辑、调试、测试、执行。

OpenCode:
  多模型切换、本地模型、开放 TUI 工作台。

Gemini:
  UI、长上下文资料、第二意见。
```

这个组合接近 [CCG](https://github.com/fengshao1227/ccg-workflow) 类工作流的分工方式：不同模型承担不同角色，而不是让一个模型独立覆盖全流程。要保留的是角色分工，不是固定工具名。

## 什么时候不要纠结模型

这些任务不用纠结模型：

- 解释一个简单概念。
- 改一个单文件小 bug。
- 写一个一次性脚本。
- 整理一个小表格。

这些任务更该先把工作流想清楚：

- 多模块功能。
- 高风险代码。
- 需要团队规范。
- 需要反复迭代。
- 需要长期复用经验。

模型会变，入口也会变。稳定的是工作流：先按任务选择入口；先小任务，再本地执行；先看 diff，再相信结果。
