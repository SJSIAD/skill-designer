# 需求清单 / Requirements Checklist

按优先级向用户收集以下内容；核心项齐全后停止，不要过度提问。
Collect the following from the user, in priority order. Stop once the core items are known; do not over-question.

## 清单 / Checklist

1. 目的 / Purpose
   - 一句话描述技能任务，加一个能触发它的真实例子请求。 / One-sentence task description plus one concrete example request that should trigger the skill.
   - 追问：什么说法会启动它？它绝不应该用于什么？ / Follow-ups: What phrase would start the skill? What should it never be used for?
2. 输入与输出 / Inputs and outputs
   - 输入：文件、文本、URL、用户问题、API 数据？ / Inputs: files, text, URLs, user questions, API data?
   - 输出：文档、代码、转换后的文件、报告、界面？ / Outputs: documents, code, transformed files, reports, UI?
3. 需要的领域知识 / Domain knowledge needed
   - 数据表结构、API 细节、文件格式、政策、领域规则、边界情况。 / Schemas, API details, file formats, policies, domain rules, edge cases.
4. 是否有确定、重复的操作？ / Deterministic, repeated operations?
   - 有：规划 scripts/ 工具。 / Yes: plan a scripts/ utility.
5. 是否有只在某些时候需要的知识？ / Knowledge needed only sometimes?
   - 有：规划 references/ 文档。 / Yes: plan a references/ document.
6. 输出是否使用模板或样板文件？ / Templates or boilerplate in output?
   - 有：规划 assets/。 / Yes: plan assets/.
7. 运行环境、名称与位置 / Runtime, name, and location
   - 目标运行环境：Codex、Claude Code，还是其他支持 SKILL.md 的环境？ / Target runtime: Codex, Claude Code, or another SKILL.md-compatible environment?
   - 从动作推导小写连字符名称；按环境放置：Codex ~/.codex/skills，Claude Code ~/.claude/skills。 / Derive a hyphen-case name from the action; place per runtime: ~/.codex/skills for Codex, ~/.claude/skills for Claude Code.

## 答案到设计决策的映射 / Mapping answers to design decisions

| 信号 / Signal | 设计决策 / Design decision |
| --- | --- |
| 多种方案都可行；靠判断 | 高自由度：SKILL.md 文字指引 / High freedom: prose guidance in SKILL.md |
| 有偏好模式且存在变化 | 中自由度：带参数脚本 / Medium freedom: parameterized script |
| 操作脆弱；必须精确按序 | 低自由度：具体脚本、少参数 / Low freedom: specific script, few parameters |
| 多个领域或变体 | 一个 SKILL.md 加每个变体一个 references / One SKILL.md plus one reference per variant |
| 「我说 X 就触发」 | 把 X 及变体写进 frontmatter 描述 / Put X and its variants in the frontmatter description |
| 重复的样板或解析代码 | scripts/ |
| 领域知识：表结构、政策、API | references/ |
| 输出模板、图标、样板 | assets/ |

## 示例：销售周报摘要 / Example: sales report summarizer

请求 / Request: "Design a skill that summarizes our weekly sales reports into a status email."

清单答案 / Checklist answers: reports are CSV exports; output is a short summary plus the top three product lines and any week-over-week drops.

决策 / Decisions:

- 名称 / Name: sales-report-summary
- 结构 / Structure: task-based with one workflow section
- 资源 / Resources: references/sales.md（指标定义与下降阈值）；无需脚本，因为摘要是判断型任务。 / (metric definitions and drop thresholds); no scripts, since summarization is judgment-based
- 触发词 / Trigger phrases: summarize the weekly sales report, draft the sales status email

## 示例：报销单整理 / Example: expense receipt sorter

需求 / Request: "帮我设计一个自动整理报销单的 skill。"

清单答案 / Checklist answers: Excel 导出；输出按费用类型汇总的表格；按发票号和日期去重。 / Excel exports; output is a table grouped by expense type; dedupe by invoice number and date.

决策 / Decisions:

- 名称 / Name: expense-receipt-sorter
- 资源 / Resources: scripts/sort_receipts.py（解析、去重、分类）；references/expense-policy.md（费用类型与合规规则）
- 触发词 / Trigger phrases: 整理报销单、报销单分类、费用汇总

## 何时停止收集 / When to stop collecting

- 已知目标运行环境、触发词、输入、输出和所需资源。 / Target runtime, trigger, inputs, outputs, and required resources are known.
- 能根据答案写出 frontmatter 描述。 / The description can be written from the answers.
- 用户已给出覆盖核心流程的示例。 / The user already provided an example covering the core workflow.
