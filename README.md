# Skill Designer · 技能设计师

一个把「用户想法」变成规范、可安装的 SKILL.md 技能的中英双语引导式设计技能，面向 SKILL.md 生态（Codex、Claude Code 等）。内置脚手架生成、零依赖校验器和避坑清单。
A bilingual guided skill for the SKILL.md ecosystem that turns an idea into a well-structured, installable SKILL.md skill, with bundled scaffolding, a dependency-free validator, and a pitfall checklist.

## 功能特性 / Features

- 需求清单引导：按优先级收集触发词、输入输出、资源类型 / Requirements-driven: collects trigger, inputs, outputs, and resources in priority order
- 脚手架生成：一条命令生成 SKILL.md、UI 元数据和资源目录 / Scaffolding: SKILL.md, UI metadata, and resource folders in one command
- 零依赖校验：不需要 PyYAML，任何 Python 3 环境都能跑 / Dependency-free validation: no PyYAML, runs on plain Python 3
- 避坑清单：交付前自动自查常见错误 / Pitfall checklist: self-review of common mistakes before delivery
- 中英双语文档 / Bilingual (Chinese + English) documentation
- SKILL.md 生态：同一份技能兼容 Codex、Claude Code 等 / SKILL.md ecosystem: one skill works with Codex, Claude Code, and other compatible runtimes

## 目录结构 / Structure

```
skill-designer/
├── SKILL.md                     # 引导式设计流程（双语）/ guided design process (bilingual)
├── agents/
│   └── openai.yaml              # UI 元数据 / UI metadata
├── scripts/
│   ├── init_skill.py            # 脚手架生成器 / scaffold generator
│   ├── generate_openai_yaml.py  # UI 元数据生成器 / UI metadata generator
│   └── quick_validate.py        # 零依赖校验器 / dependency-free validator
└── references/
    ├── requirements.md          # 需求清单与设计映射 / requirements checklist and design mapping
    ├── pitfalls.md              # 避坑清单 / pitfall checklist
    └── openai_yaml.md           # openai.yaml 字段规范 / openai.yaml field spec
```

## 安装 / Install

克隆仓库后，把 `skill-designer` 文件夹复制到对应平台的技能目录：
After cloning, copy the `skill-designer` folder into the skills directory of your platform:

```bash
git clone https://github.com/<your-username>/skill-designer.git

# Codex：~/.codex/skills（或 $CODEX_HOME/skills）
cp -r skill-designer/skill-designer ~/.codex/skills/

# Claude Code：~/.claude/skills
cp -r skill-designer/skill-designer ~/.claude/skills/
```

其他支持 SKILL.md 的环境，把文件夹放到对应的技能目录即可。
For any other SKILL.md-compatible runtime, place the folder into its skills directory.

## 使用 / Usage

对 Codex 说 / Ask Codex:

- "帮我设计一个 skill" / "设计一个整理报销单的技能"
- "Use $skill-designer to design a custom skill for my workflow."

对 Claude Code 说 / Ask Claude Code:

- "帮我设计一个技能"
- "Use the skill-designer skill to design a custom skill."

技能会先收集必要需求，然后规划结构、脚手架生成、实现并校验，最后交付可直接安装的技能目录。
生成的 SKILL.md 兼容整个 SKILL.md 生态。
The skill collects the essentials first, then plans, scaffolds, implements, and validates a ready-to-install skill folder. The generated SKILL.md works across the SKILL.md ecosystem.

## 开发 / Development

```bash
# 生成新技能骨架（codex 生成 UI 元数据，claude 不生成）/ scaffold a new skill
python scripts/init_skill.py my-skill --path /tmp/skills --resources scripts,references --platform all

# 校验技能 / validate a skill
python scripts/quick_validate.py /path/to/skill-folder

# 重新生成 UI 元数据 / regenerate UI metadata
python scripts/generate_openai_yaml.py /path/to/skill-folder
```

脚手架与校验脚本来自 OpenAI 标准技能工具链（Apache-2.0）。
The scaffolding and validation scripts derive from the standard skill tooling (Apache-2.0).

## 许可证 / License

[Apache-2.0](LICENSE)
