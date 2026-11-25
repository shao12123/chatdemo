# shared · Demo 公共资源

## `moreai-tokens.css`

业务 Demo / 卡片库共用的**现网语义色**（对齐 `moreai-electron` `colors-semantic.css`）。

| 主题 | 选择器 |
|---|---|
| 浅色 | `:root` 默认 |
| 深色 | `html.theme-dark`（产品为 `html.dark`，取值相同） |

引入：

```html
<!-- 仓库根下的 HTML -->
<link rel="stylesheet" href="shared/moreai-tokens.css" />

<!-- chat-cards/<id>/morph-wall.html -->
<link rel="stylesheet" href="../../shared/moreai-tokens.css" />
```

组件只使用 `var(--text-color-*)`、`var(--bg-color-*)`、`var(--component-border)`、`var(--brand-color)` 等；禁止飞书灰 hex。

场景壳另提供 `--gc-*`、`--card-line`、`--nav`、`--send` 等别名（已映射现网）。注意：公共 `--secondary` = **次要容器底**；建群/邮件等场景壳若当文字色用，须在页面内覆盖为 `var(--text-color-secondary)`。

人读对照：[CHATDEMO-STANDARDS.md §5](../docs/CHATDEMO-STANDARDS.md) · [对话目录 PRD 工程表](../对话目录-PRD.md) · 金样 [对话目录.html](../对话目录.html)。

已挂 B1：建群 / 发送邮件 / 连接外部应用 / 时间 / 模糊需求 / 天气 / 映射对照。  
已挂 B2：`conversations.html`、`三个路线方案.html`（含 `--err-*` / `--rt-*`）。  
已挂 B3：`group_form` / `group_result` / `ask_form` / `weather_card` / `doc_summary` / `doc_entry` / `app_link` 形态墙。
