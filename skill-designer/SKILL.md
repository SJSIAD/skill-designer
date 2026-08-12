---
name: skill-designer
description: 面向 SKILL.md 生态的技能设计助手：把想法变成结构规范、可直接安装的 SKILL.md 技能目录，包括规范 frontmatter、可选的 UI 元数据以及 scripts、references、assets。Skill design assistant for the SKILL.md ecosystem that turns an idea into a well-structured, installable SKILL.md skill folder. 当用户要求设计、创建、搭建、完善技能时使用，例如「设计一个 skill」「帮我创建技能」「make a skill that does X」「把这个流程做成技能」。Use when a user asks to design, create, build, scaffold, or shape a new skill, or wants to turn a repeated workflow into an installable skill.
---

# Skill Designer · 技能设计师

把用户请求变成规范、可安装的 SKILL.md 技能目录：收集需求 → 设计方案 → 脚手架生成 → 实现 → 校验 → 迭代。
Turn a user request into a well-structured, installable SKILL.md skill folder: collect requirements, plan, scaffold, implement, validate, iterate.

## 流程 / Process

### 1. 收集需求 / Collect requirements

按优先级收集核心信息，使用 references/requirements.md 中的清单。已知触发词、输入、输出和所需资源后即可停止。
Gather the core items in priority order, using the checklist in references/requirements.md. Stop once the trigger, inputs, outputs, and required resources are known.

- 目的：技能做什么，加上一个能触发它的真实例子请求。 / Purpose: what the skill does, plus one concrete example request that should trigger it.
- 输入与输出。 / Inputs and outputs.
- 所需资源：scripts（确定性逻辑）、references（领域知识）或 assets（模板）。 / Required resources: scripts (deterministic logic), references (domain knowledge), or assets (templates).
- 目标运行环境：SKILL.md 生态（Codex、Claude Code 等）。安装位置按环境放置：Codex ~/.codex/skills，Claude Code ~/.claude/skills。 / Target runtime: the SKILL.md ecosystem (Codex, Claude Code, and other compatible runtimes). Install per runtime: ~/.codex/skills for Codex, ~/.claude/skills for Claude Code.

### 2. 设计方案 / Plan the design

- 名称：小写连字符（字母、数字、连字符），动词开头，最多 64 字符；文件夹名与技能名一致。 / Name: hyphen-case (lowercase letters, digits, hyphens), verb-led, at most 64 characters. The folder name must equal the skill name.
- 结构：流程式、任务式、参考/规范式或能力式，参考 init_skill.py 模板中的说明。 / Structure: workflow-based, task-based, reference/guidelines, or capabilities-based. See the guidance in the init_skill.py template.
- 自由度 / Degrees of freedom:
  - 高：多种方案都可行时，在 SKILL.md 里给文字指引。 / High: prose guidance in SKILL.md when many approaches are valid.
  - 中：有偏好模式且存在变化时，用带参数的脚本。 / Medium: parameterized script when a preferred pattern exists with some variation.
  - 低：步骤脆弱或必须精确时，用参数很少的具体脚本。 / Low: specific script with few parameters when steps are fragile or must be exact.
- 资源 / Resources:
  - scripts/：确定、可重复的代码；交付前必须实际运行测试。 / scripts/ for deterministic, repeatable code; test every script by running it.
  - references/：仅在需要时加载的文档；保持 SKILL.md 精简并链接引用。 / references/ for documentation loaded only when needed; keep SKILL.md lean and link references from it.
  - assets/：用于最终产出的文件（模板、图标、样板）。 / assets/ for files used in output (templates, icons, boilerplate).
- frontmatter 描述：写清技能做什么和所有具体触发场景（用户说法、文件类型、场景）。这是唯一的触发机制；SKILL.md 生态统一靠 name + description 触发，保持通用。 / Frontmatter description: state what the skill does and every concrete trigger (user phrasings, file types, scenarios). This is the only trigger mechanism; the SKILL.md ecosystem triggers on name + description, so keep it neutral.
- SKILL.md 控制在 500 行以内；变体细节移到 references。 / Keep SKILL.md under 500 lines; move variant-specific details into references.

### 3. 脚手架生成 / Scaffold

运行自带的初始化脚本 / Run the bundled initializer:

```bash
python scripts/init_skill.py <skill-name> --path <install-dir> --platform codex|claude|all --resources scripts,references,assets --interface display_name="..." --interface short_description="..." --interface default_prompt="..."
```

- 初始化脚本会创建文件夹与 SKILL.md；codex/all 还生成 agents/openai.yaml（仅 Codex/ChatGPT 界面的可选元数据），claude 跳过。SKILL.md 本体在任何 SKILL.md 环境通用。 / The initializer creates the folder and SKILL.md; codex/all also generate agents/openai.yaml (optional Codex/ChatGPT UI metadata), claude skips it. The SKILL.md itself is universal across SKILL.md runtimes.
- 根据方案生成 display_name、short_description（25-64 字符）和 default_prompt。 / Generate display_name, short_description (25-64 characters), and default_prompt from the plan.
- 选值前阅读 references/openai_yaml.md 的字段规则。 / Read references/openai_yaml.md for field rules before choosing values.
- 只有用户明确提供时才加图标或品牌色字段。 / Only include icon or brand fields when the user explicitly provides them.
- 只创建方案需要的资源目录；仅当占位确有帮助时才用 --examples。 / Pass only the resource directories the plan requires; use --examples only when placeholders genuinely help.

### 4. 实现 / Implement

- 补全 frontmatter 描述（做什么 + 具体触发词）。 / Complete the frontmatter description (what it does plus concrete triggers).
- 用祈使句和简洁示例替换所有 TODO 占位。 / Replace all TODO sections with imperative instructions and concise examples.
- 只创建方案需要的资源，删除无用的示例占位。 / Create only the resources the plan calls for; delete unused placeholder examples.
- 技能必须自包含：不依赖其他技能、无绝对路径、无 README 或多余文档。 / Keep the skill self-contained: no dependencies on other skills, no absolute paths, no README or extra docs.

### 5. 校验 / Validate

对照 references/pitfalls.md 的避坑清单逐项检查并修复。
Review the pitfall checklist in references/pitfalls.md and fix every item that applies.

运行自带校验器，直到全部通过 / Run the bundled validator and fix every issue until it passes:

```bash
python scripts/quick_validate.py <skill-dir>
```

### 6. 迭代 / Iterate

- 交付技能路径和一个用法示例。 / Deliver the created skill path and a usage example.
- 用真实请求测试技能；修复问题后重新校验、再测。 / Test the skill on realistic requests; fix failures, re-validate, and re-test.

## 自带资源 / Bundled resources

- scripts/init_skill.py — 脚手架生成技能文件夹与 UI 元数据。 / scaffolds the skill folder and UI metadata.
- scripts/quick_validate.py — 校验 frontmatter 与命名。 / validates frontmatter and naming.
- scripts/generate_openai_yaml.py — 重新生成 UI 元数据（仅 Codex/ChatGPT）。 / regenerates UI metadata (Codex/ChatGPT only).
- references/requirements.md — 需求清单与设计决策映射。 / requirement checklist and design mapping.
- references/pitfalls.md — 常见设计错误与规避方法。 / common design mistakes and how to avoid them.
- references/openai_yaml.md — UI 元数据字段规则。 / field rules for UI metadata.
