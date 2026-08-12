# 避坑清单 / Pitfall Checklist

设计技能时的常见错误与规避方法。交付前逐项检查，修复所有适用项。
Common mistakes when designing skills and how to avoid them. Review this list before delivery and fix every item that applies.

## 触发与描述 / Triggering and description

| 坑 / Pitfall | 后果 / Consequence | 规避 / How to avoid |
| --- | --- | --- |
| 描述只写功能、不写触发时机 / Description lists only what the skill does, not when to use it | 技能永远不会被触发 / The skill never triggers | 写清具体触发场景：用户说法、文件类型、场景 / Include concrete triggers: user phrasings, file types, scenarios |
| 缺非英语触发词 / Trigger words missing for non-English users | 双语用户永远触发不了 / Bilingual users never trigger it | 加上用户实际会说的词（含中文）/ Add the phrases users actually say, in their languages |
| 描述含尖括号 < > / Description contains angle brackets | 校验失败 / Validation fails | 去掉尖括号 / Remove angle brackets |
| 描述超过 1024 字符 / Description over 1024 characters | 校验失败 / Validation fails | 精简到要点 / Trim to essentials |
| 名称不合规：大写、过长、双连字符 / Name not hyphen-case, too long, or double hyphens | 校验失败、无法被发现 / Validation fails; cannot be discovered | 小写字母、数字、连字符；动词开头；不超过 64 字符 / Lowercase letters, digits, hyphens only; verb-led; at most 64 characters |
| 触发说明写在正文而不是描述里 / When-to-use written in the body instead of the description | 无用，因为正文只在触发后加载 / Useless, because the body loads only after triggering | 所有触发信息放进 frontmatter 描述 / Put all trigger information in the frontmatter description |
| 范围太宽，覆盖无关任务 / Scope too broad, covering unrelated tasks | 触发不精准，结果错误 / Triggering is imprecise; users get wrong results | 缩小范围或拆成多个技能 / Narrow the scope or split into separate skills |
| 只针对单一例子 / Over-fitted to a single example | 请求一变就失败 / Fails on variations of the request | 用两三个不同的真实请求测试 / Test with two or three varied realistic requests |

## 结构与上下文 / Structure and context economy

| 坑 / Pitfall | 后果 / Consequence | 规避 / How to avoid |
| --- | --- | --- |
| SKILL.md 超过 500 行 / SKILL.md over 500 lines | 每次使用都浪费上下文 / Wastes context on every use | 变体细节拆到 references / Split variant-specific details into references |
| 引用嵌套超过一层 / Reference files nested deeper than one level | 难以发现和加载 / Hard to discover and load | 所有引用直接从 SKILL.md 链接 / Link all references directly from SKILL.md |
| 同一信息既在 SKILL.md 又在 references / Same information in SKILL.md and a reference | 失同步、双份维护 / Drifts out of sync; double maintenance | 每个事实只放一处 / Keep each fact in exactly one place |
| 塞入 README、更新日志等多余文档 / README, changelog, or other extra docs | 杂乱、困惑 / Clutter and confusion | 只保留技能运行所需文件 / Keep only the files the skill needs to function |
| 长引用文件没有目录 / Long reference files without a table of contents | 加载后难以扫读 / Hard to scan when loaded | 超过约 100 行的文件加目录 / Add a table of contents to files over roughly 100 lines |
| 建了不用的资源目录或占位 / Resource directories or example placeholders left unused | 死重、困惑 / Dead weight; confusing | 只建方案需要的；删掉占位 / Create only what the plan needs; delete placeholders |

## 脚本与自由度 / Scripts and degrees of freedom

| 坑 / Pitfall | 后果 / Consequence | 规避 / How to avoid |
| --- | --- | --- |
| 脆弱、必须精确按序的操作却用自由文本 / Fragile, exact-sequence operations left as free text | 输出不稳、错误漏过 / Output varies; errors slip through | 用参数很少的具体脚本 / Use a specific script with few parameters |
| 固定模式每次都在正文里重讲 / A preferred pattern re-explained in prose every time | 不一致、浪费 / Inconsistent and wasteful | 确定性逻辑放进脚本 / Put the deterministic logic in a script |
| 脚本没跑过就交付 / Scripts shipped without being run | 开箱即坏 / Broken out of the box | 交付前运行每个脚本 / Run every script before delivering |
| 脚本依赖绝对路径或其他技能 / Scripts depend on absolute paths or other skills | 不可移植 / Not portable; breaks for other users | 自包含；用相对路径 / Keep self-contained; use relative paths |
| 脚本依赖未声明的包 / Scripts require unstated packages | 其他机器跑不了 / Fails on other machines | 保持零依赖或明确声明 / Keep zero dependencies or declare them clearly |
| 文件读写不显式指定编码 / Files read or written without an explicit encoding | 非 UTF-8 环境崩溃 / Breaks on machines with non-UTF-8 locales | 每次读写都指定 encoding="utf-8" / Specify encoding="utf-8" for every file read and write |
| 指令写成陈述句而非命令式 / Instructions written as statements instead of commands | 执行含糊 / Ambiguous execution | 用祈使句 / Write imperative instructions |
| 解释读者本来就知道的事 / Explaining things the reader already knows | 浪费 token / Wastes tokens | 只补非显然的知识；用简洁示例 / Add only non-obvious knowledge; prefer concise examples |

## 安全与数据 / Safety and data

| 坑 / Pitfall | 后果 / Consequence | 规避 / How to avoid |
| --- | --- | --- |
| 技能里埋密钥、令牌或个人路径 / Embedded secrets, tokens, or personal paths | 泄露；别人用不了 / Leaks; breaks for other users | 用占位符；密钥不进技能 / Use placeholders; keep secrets out of the skill |
| 破坏性操作没有护栏 / Destructive operations without guardrails | 用户数据丢失 / User data loss | 破坏性操作前要求明确确认 / Require explicit confirmation before destructive actions |

## 流程 / Process

| 坑 / Pitfall | 后果 / Consequence | 规避 / How to avoid |
| --- | --- | --- |
| 核心需求没问清就开工 / Starting without the core requirements | 返工 / Rework | 先收集触发词、输入、输出、资源（references/requirements.md）/ Collect trigger, inputs, outputs, and resources first (references/requirements.md) |
| 过度提问 / Over-asking the user | 反感；用户放弃 / Friction; the user gives up | 只问必要的，按优先级，然后停止 / Ask only what is needed, in priority order, then stop |
| SKILL.md 残留 TODO 占位 / TODO placeholders left in SKILL.md | 交付半成品 / Delivers an unfinished skill | 交付前替换所有 TODO / Replace every TODO before delivery |
| 跳过校验 / Skipping validation | 基础错误流向用户 / Basic errors reach the user | 始终运行 quick_validate.py 直到通过 / Always run quick_validate.py and fix until it passes |
| 交付前不真实测试 / No realistic test before delivery | 真实请求上失败 / Fails on real requests | 用真实例子测试；修复、重校验、再测 / Test with a real example request; fix, re-validate, re-test |
| UI 元数据超范围（short_description 不在 25-64） / UI metadata values out of range (short_description outside 25-64 characters) | 元数据生成失败 / Metadata generation fails | 值保持在文档范围（references/openai_yaml.md）/ Keep values within the documented ranges (references/openai_yaml.md) |
| 名称与安装目录已有技能冲突 / Name already exists in the install directory | 初始化中止 / Initialization aborts | 脚手架前选一个不冲突的名字 / Choose a distinct name before scaffolding |
