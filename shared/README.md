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

人读对照：[CHATDEMO-STANDARDS.md §5](../docs/CHATDEMO-STANDARDS.md) · [对话目录 PRD 工程表](../对话目录-PRD.md) · 金样 [对话目录.html](../对话目录.html)。
