# openai.yaml 字段说明（完整示例 + 说明）/ openai.yaml fields (full example + descriptions)

`agents/openai.yaml` 存放技能在界面中的展示元数据。其他产品专属配置也可以放在 `agents/` 文件夹。
`agents/openai.yaml` holds UI-facing metadata for the skill's listing. Other product-specific config can also live in the `agents/` folder.

## 完整示例 / Full example

```yaml
interface:
  display_name: "Optional user-facing name"
  short_description: "Optional user-facing description"
  icon_small: "./assets/small-400px.png"
  icon_large: "./assets/large-logo.svg"
  brand_color: "#3B82F6"
  default_prompt: "Optional surrounding prompt to use the skill with"

dependencies:
  tools:
    - type: "mcp"
      value: "github"
      description: "GitHub MCP server"
      transport: "streamable_http"
      url: "https://api.githubcopilot.com/mcp/"

policy:
  allow_implicit_invocation: true
```

## 字段说明与约束 / Field descriptions and constraints

顶层约束 / Top-level constraints:

- 所有字符串值加引号。 / Quote all string values.
- key 不加引号。 / Keep keys unquoted.
- `interface.default_prompt`：写一个简短有用的示例提示词（通常一句话），必须显式提到 `$skill-name`（例如 "Use $skill-name-here to draft a concise weekly status update."）。 / Generate a helpful, short (typically 1 sentence) example starting prompt based on the skill. It must explicitly mention the skill as `$skill-name`.

字段 / Fields:

- `interface.display_name`：界面技能列表与标签中显示的名称。 / Human-facing title shown in UI skill lists and chips.
- `interface.short_description`：界面中的简短简介（25-64 字符）。 / Human-facing short UI blurb (25-64 chars) for quick scanning.
- `interface.icon_small`：小图标路径（相对技能目录），默认放 `./assets/`。 / Path to a small icon asset (relative to skill dir). Default to `./assets/` and place icons in the skill's `assets/` folder.
- `interface.icon_large`：大图标/Logo 路径。 / Path to a larger logo asset (relative to skill dir).
- `interface.brand_color`：界面强调色（十六进制）。 / Hex color used for UI accents (e.g., badges).
- `interface.default_prompt`：调用技能时插入的默认提示词。 / Default prompt snippet inserted when invoking the skill.
- `dependencies.tools[].type`：依赖类别，目前只支持 `mcp`。 / Dependency category. Only `mcp` is supported for now.
- `dependencies.tools[].value`：工具或依赖的标识符。 / Identifier of the tool or dependency.
- `dependencies.tools[].description`：依赖的人类可读说明。 / Human-readable explanation of the dependency.
- `dependencies.tools[].transport`：type 为 `mcp` 时的连接方式。 / Connection type when `type` is `mcp`.
- `dependencies.tools[].url`：type 为 `mcp` 时的 MCP 服务器地址。 / MCP server URL when `type` is `mcp`.
- `policy.allow_implicit_invocation`：为 false 时默认不注入模型上下文，但仍可通过 `$skill` 显式调用；默认 true。 / When false, the skill is not injected into the model context by default, but can still be invoked explicitly via `$skill`. Defaults to true.
