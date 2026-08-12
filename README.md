# Skill Designer · 技能设计师

一个把「用户想法」变成规范、可安装的 Codex 技能的中英双语引导式设计技能。内置脚手架生成、零依赖校验器和避坑清单。
A bilingual guided skill that turns an idea into a well-structured, installable Codex skill, with bundled scaffolding, a dependency-free validator, and a pitfall checklist.

## 功能特性 / Features

- 需求清单引导：按优先级收集触发词、输入输出、资源类型 / Requirements-driven: collects trigger, inputs, outputs, and resources in priority order
- 脚手架生成：一条命令生成 SKILL.md、UI 元数据和资源目录 / Scaffolding: SKILL.md, UI metadata, and resource folders in one command
- 零依赖校验：不需要 PyYAML，任何 Python 3 环境都能跑 / Dependency-free validation: no PyYAML, runs on plain Python 3
- 避坑清单：交付前自动自查常见错误 / Pitfall checklist: self-review of common mistakes before delivery
- 中英双语文档 / Bilingual (Chinese + English) documentation

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

把 `skill-designer` 文件夹复制到技能目录（默认 `~/.codex/skills`，或 `$CODEX_HOME/skills`）：
Copy the `skill-designer` folder into your skills directory (`~/.codex/skills`, or `$CODEX_HOME/skills` if set):

```bash
git clone https://github.com/<your-username>/skill-designer.git
cp -r skill-designer/skill-designer ~/.codex/skills/
```

## 使用 / Usage

对 Codex 说 / Ask Codex:

- "帮我设计一个 skill" / "设计一个整理报销单的技能"
- "Use $skill-designer to design a custom skill for my workflow."

技能会先收集必要需求，然后规划结构、脚手架生成、实现并校验，最后交付可直接安装的技能目录。
The skill collects the essentials first, then plans, scaffolds, implements, and validates a ready-to-install skill folder.

## 开发 / Development

```bash
# 生成新技能骨架 / scaffold a new skill
python scripts/init_skill.py my-skill --path /tmp/skills --resources scripts,references

# 校验技能 / validate a skill
python scripts/quick_validate.py /path/to/skill-folder

# 重新生成 UI 元数据 / regenerate UI metadata
python scripts/generate_openai_yaml.py /path/to/skill-folder
```

脚手架与校验脚本来自 OpenAI 标准技能工具链（Apache-2.0）。
The scaffolding and validation scripts derive from the standard skill tooling (Apache-2.0).

## 许可证 / License

[Apache-2.0](LICENSE)
