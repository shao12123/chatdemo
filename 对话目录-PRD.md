# 对话目录公共组件 PRD

| 项目 | 内容 |
|---|---|
| 文档状态 | 工程可落地 · 颜色对齐 moreai-electron 现网 token |
| 版本 | v1.1 · 2026-11-26（显隐改按对话列宽度；token 表仍 = 现网浅/深） |
| 交互 Demo | [对话目录.html](对话目录.html)（含**浅色/深色**开关；提问 / 消息 / 文档形态 / **列宽显隐** / 不足两条 / **很多条**） |
| Agent TOOL | **不需要** |
| 适用读者 | **开发优先**，产品 / 设计 / 测试 |
| 本文写什么 | 行为契约 + **可直接抄的 token / 结构 / API**。业务只声明锚点。 |

> **开发依据：Demo + 本文 §5。** 颜色只引用现网 CSS 变量（见下表），禁止飞书灰 hex。`moreai-electron` 接入说明可对照产品仓 `docs/对话目录-工程接入说明.md`。

### 工程 30 秒对照

| 控件用途 | 抄这个变量 | Tailwind（现网） | Light | Dark |
|---|---|---|---|---|
| 当前横线 / 当前字 / hover 字 | `--text-color-primary` | `text-text-primary` | `#000000` | `#FFFFFF` |
| 默认行字 | `--text-color-secondary` | `text-text-secondary` | `#66686C` | `#C5C8CE` |
| 默认横线 / peek 副文 | `--text-color-placeholder` | `text-text-placeholder` | `#8B8E93` | `#C5C8CE` |
| 展开卡底 | `--bg-color-container` / `--card` | `bg-bg-container` / `bg-card` | `#FFFFFF` | `#111216` |
| peek 行底 | `--bg-color-secondarycontainer` | `bg-bg-secondary` | `#EEF0F3` | `#191A1D` |
| 卡边 | `--component-border` / `--border` | `border-border` | `#DCDCDC` | `#2A2D33` |
| flash | `--brand-color` @ 16% | — | `#0A59F7` | `color-mix(#317AF7 85%, white)` |

主题开关：产品 `<html class="dark">`；本 Demo 用 `html.theme-dark`，取值与上表一致。

---

## 1. 背景与目标

### 1.1 现状
长对话、长回复、长文档需要快速跳转。线上 / Demo 已有同一套右缘「短横线 → 悬停展开列表」控件，但：
- 各业务 PRD 只写「复用现有组件」，开发缺少统一规格；
- 提问目录、消息目录、文档目录易被做成三套 UI。

### 1.2 目标
- **一个控件、多种用法**：提问目录、消息目录（以及后续文档目录）共用同一套交互与视觉；业务只提供 `items` + 锚点。
- Demo 可单独验收；正式工程可抽成公共模块。
- **本期上线范围**：对话列上的提问目录 / 消息目录；显隐跟**对话列可用宽度**（对齐 Cursor Message map rail），不与「右侧是否打开」硬绑。

### 1.3 成功标准
- ≥2 条锚点时出现；1 条及以下不出现。
- 悬停展开、点击跳转、滚动跟当前项、离开收起行为与 Demo 一致。
- 对话列右侧「正文外空隙」够放轨时显示；拖分割线把对话列挤窄则隐藏；右侧文档区**仍不出**文档目录（见 2.3、Q5）。
- `prefers-reduced-motion: reduce` 时关闭展开 / 跳转动画。

---

## 2. 范围

### 2.1 本期做
1. 统一控件：收起轨（短横线）+ 展开卡（列表）+ 当前态 + 跳转高亮。
2. **对话列**两种适配：提问（peek 开）、消息（peek 按业务）。
3. 出现 / 隐藏规则：按**对话列可用宽度**——列宽足以在正文最大宽之外保留右缘导航槽时显示；拖 chat|panel 分割线挤窄则隐藏（与 Cursor 一致）。右侧打开本身不强制隐藏。
4. 深浅色：横线与文字用规范 text token；卡底/边用容器 token；高亮闪用品牌色透明度（见 5.3）。
5. 无障碍：列表行为按钮、`aria-label`；键盘 focus 可展开。

### 2.2 Demo 与工程
| | Demo `对话目录.html` | 工程（本期） |
|---|---|---|
| 交互 / 视觉 | 可点验收 | 对齐本文与 Demo（对话列部分） |
| 业务锚点 | 写死示例块 | 各业务注入 items |
| 右侧展示区 | 「列宽显隐」：打开右侧后可拖分割线；窄藏宽显 | 读对话列 DOM 宽度 / 右缘空隙；`visible` 勿只绑 panel 开闭 |
| 「文档目录」case | 可保留，演示控件在章节列表上的形态 | **不接入产品**（见 2.3） |

### 2.3 本期不做
- **右侧文档区出文档目录**（章节轨挂在文档台右缘）。  
  **原因（产品标注）**：即将上线云文档；文档内目录 / 大纲能力应在**云文档**侧实现与承接，本期 Chat 右侧展示区只负责打开内容，**不**再挂一套文档目录，避免与云文档重复建设、交互割裂。
- 目录内搜索、拖拽排序、多级树。
- 触控长按专用手势（沿用 pointer 悬停；窄屏专项另案）。
- Agent TOOL / 后端 Schema（无）。
- 把业务文案生成规则写进本控件（如总结卡标题映射仍在业务 PRD）。

---

## 3. 场景（用法）

| 用法 | 宿主 | 列表内容 | peek | 本期 |
|---|---|---|---|---|
| 提问目录 | 对话列右缘 | 每条用户消息标题；可选 AI 首句作副文 | **开** | **做** |
| 消息目录 | 对话列右缘 | 长回复内可跳转块（导语、报告卡、提示等） | 业务定（总结建议开） | **做** |
| 文档目录 | 右侧文档台正文区右缘 | 章节标题 | **关** | **不做**（Demo 可演；产品改云文档） |

同一时刻对话列上**至多一个**目录实例可见（提问 / 消息按产品约定二选一或合并 items）。**右侧打开不直接决定显隐**；对话列够宽可继续显示提问 / 消息目录。**右侧仍不出文档目录。**

---

## 4. 交互流程

### 4.1 出现与隐藏
| 条件 | 行为 |
|---|---|
| `items.length >= 2`（`OUTLINE_MIN`）且对话列右缘空隙 ≥ `OUTLINE_RAIL_GUTTER`（约 40px） | 显示轨（对话列） |
| `items.length < 2` | 隐藏并关闭展开 |
| 对话列被分割线挤窄，正文贴满、右缘空隙不足 | **隐藏**对话列目录（右侧打开时常落入此态，但是因宽度而非开闭本身） |
| 拖宽对话列、或关闭右侧后列宽恢复，且仍 ≥2 条锚点 | 对话列目录重新显示 |
| 右侧文档区 | **始终不出**文档目录（云文档承接，见 Q5） |
| `panel-max` / 窄屏叠层 | 可强制隐藏 |
| 生成中 / 新增锚点 | 仍显示（宽度足够时）；新横线可播 `is-new` 入场 |

判定建议（几何，优于写死像素阈值）：

```js
// host = 对话列（thread / scroll）；inner = 正文列（messages-inner）
host.getBoundingClientRect().right - inner.getBoundingClientRect().right >= OUTLINE_RAIL_GUTTER
```

### 4.2 收起态（轨）
- 位置：宿主右缘，竖直居中（`top: 50%` + translate）。
- 一条锚点一条短横线；间距固定 **10px**（`OUTLINE_PITCH`）。
- **当前项**：横线更长（约 16px）、色走 `--text-color-primary`；其余约 8px、`--text-color-placeholder`（见 5.3）。
- 滚动宿主内容时，当前项随视口同步，轨平滑平移。

### 4.2.1 条数特别多时（强制 · 对齐 Cursor 轨手感）
条目可以远多于一屏，**禁止**把全部短横线无限制堆满右缘。配置：`denseRail: true`（Demo「很多条」开启）。

| 规则 | 说明 |
|---|---|
| 短横线可见上限 | `min(条目数, OUTLINE_MAX=18, floor(可用高度 / 10px))`；可用高度约为宿主高 − 48px（原 30 对 1440 过高） |
| 溢出怎么表现 | 轨内 track **整体 translateY**，只露出一段窗口；不出现第二列、不缩小间距塞下全部 |
| 当前项跟滚 | 整段短横线轨**固定**在对话列垂直居中，不随滚动贴顶/贴底；滚动只平移轨内 track，让当前项落在满窗约 18 根里 |
| **边缘平移** | 指针在轨可见窗口**上/下沿约 24px** 热区时持续拨轨：上沿送入更早条目，下沿送入更晚条目；离开热区停止；宿主滚动改当前项时清零手动拨动 |
| **悬停鱼眼** | 收起轨上，指针附近横线**只加长、不加大上下间距**；离轨恢复。减弱动效时不做鱼眼。贴顶/贴底时仍保持满窗可见条数 |
| 点轨跳转 | 点击短横线 → 与点标题行相同，跳到锚点 |
| 展开标题卡 | **保留**：轨上停留约 **240ms** 后再展开；移入卡立即保持展开。**展开卡最多约 12 行高**，超出卡内滚 |

验收：Demo「很多条」——短横线 ≤18；展开卡视觉高度约 12 行；鱼眼 / 贴顶底平移；卡内可滚到末条。

### 4.3 展开态（卡）
1. 指针进入轨：普通模式**立即展开**；`denseRail` 约 **240ms** 后再展开。指针进入卡 → 立即展开 / 保持展开。
2. 卡片从右侧 clip 展开，宽 **280px**；`top` 以**当前轨中心**为基准垂直居中夹紧，避免总贴顶。
3. 打开时列表滚到让当前行居中。
4. 卡片高度：`min(内容高, 宿主剩余, **约 12 行**)`；**条目再多只在卡内滚动**（藏滚动条），1440 小屏也不顶满。
5. 指针离开轨与卡约 **150ms** 后收起；focus 在卡内时保持展开。

### 4.4 peek（仅 peek 模式）
- 指针在某一行停留约 **80ms**：该行 `is-peek`。
- 主文最多 1 行（提问）或按业务；副文（`answer`）灰字最多 **2** 行，左对齐展开。
- 无副文可不渲染 peek 区；提问场景无回复时可显示「生成中…」（业务填入 `answer`）。

### 4.5 点击跳转
1. 点击行 → `pinned = key`，设为当前。
2. 宿主平滑滚动，使锚点距视口顶保留 `offset`（对话约 24px）。
3. 锚点节点加 `qnav-flash` 约 1s。
4. 用户滚轮 / 触控 / 指针按下滚动区 → 清除 `pinned`，恢复滚动跟当前。

### 4.6 减弱动效
`prefers-reduced-motion: reduce`：关闭轨/卡 transition、`is-new`、flash 动画；跳转可用 `auto`。

---

## 5. 组件与样式

### 5.1 结构
```
nav.qnav[.show][.open][.no-peek]
  .qnav-rail > .qnav-view > .qnav-track > i[data-qi][.is-current][.is-new]
  .qnav-card > button.qnav-row[data-qi][.is-current][.is-peek]
                 .qnav-text > .qnav-q + (.qnav-a-wrap > .qnav-a > span)?
                 i.qnav-dash
```

### 5.2 尺寸

| 项 | 值 |
|---|---|
| 轨容器宽 | 28px；右偏移约 14px |
| 横线 | 默认 8×2，当前 16×2；圆角 1；间距 10（线高 2 + gap 8） |
| 卡片 | 宽 280；圆角 12；`box-shadow` 用 `--shadow-card`；**最大高度约 12 行** |
| 短横线窗口 | 最多 **18** 条，再受宿主高度限制 |
| 行 | 字 12/20；默认右对齐；当前字重 600 |

### 5.3 颜色 token（= moreai-electron 现网，可直接用）

来源：`moreai-electron` → `src/styles/tokens/colors-semantic.css`（经 `theme.css` / `index.css` 注册）。  
Demo / 卡片库公共文件：[`shared/moreai-tokens.css`](shared/moreai-tokens.css)（金样 [`对话目录.html`](对话目录.html) 已 `link`；深色开关 `html.theme-dark`）。  
Demo 顶部「浅色 / 深色」与产品 `html.dark` **同一套取值**。  
**规则：组件只写 `var(--…)` / 语义 Tailwind 类，禁止业务 hex。**

| 用途 | CSS 变量 | Tailwind | Light（现网） | Dark（现网） |
|---|---|---|---|---|
| 当前横线、当前行字、hover 字、当前 dash | `--text-color-primary` | `text-text-primary` | `#000000` | `#FFFFFF` |
| 默认行字 | `--text-color-secondary` | `text-text-secondary` | `#66686C` | `#C5C8CE` |
| 默认横线、peek 副文、默认 dash | `--text-color-placeholder` | `text-text-placeholder` | `#8B8E93` | `#C5C8CE` |
| 展开卡底 | `--bg-color-container` / `--card` | `bg-bg-container` / `bg-card` | `#FFFFFF` | `#111216`（gray-13） |
| peek 行底、提示条等次要底 | `--bg-color-secondarycontainer` | `bg-bg-secondary` | `#EEF0F3`（gray-1） | `#191A1D`（gray-12） |
| 卡边 | `--component-border` / `--border` | `border-border` | `#DCDCDC` | `#2A2D33` |
| 跳转 flash 光晕 | `--brand-color` 约 16% 透明 | — | `#0A59F7` | `color-mix(in srgb, #317AF7 85%, #fff)` |

深浅对应关系（工程验收用）：

| 语义 | Light → Dark |
|---|---|
| 主字 / 当前横线 | 黑 → 白 |
| 次字 / 占位 / 默认横线 | `#66686C` / `#8B8E93` → **同一** `#C5C8CE`（现网深色层级已合并） |
| 卡底 | 白 → `#111216` |
| peek / 次要容器 | `#EEF0F3` → `#191A1D` |
| 描边 | `#DCDCDC` → `#2A2D33` |
| 品牌 | `#0A59F7` → 提亮后的 brand-dark |

与 Figma「黑白 60%/40% alpha」原文的差异：现网为对比度改为**实色**；深色次要容器按**色阶名 Gray12 = `#191A1D`**（不以规则表误写的 `#292B2E` 为准）。Demo / 本文跟现网，不跟误写 hex。

**禁止**：`#1f2329`、`#646a73`、`#c9cdd4`、`#eceef2`、`#f5f6f7`、`#8f959e`。

### 5.4 样式绑定（对照 Demo 类名）

| 选择器 | 属性 | Token |
|---|---|---|
| `.qnav-rail i` / `.qnav-dash` | `background` | `--text-color-placeholder` |
| `.qnav-rail i.is-current` / `.is-current .qnav-dash` / hover dash | `background` | `--text-color-primary` |
| `.qnav-row` | `color` | `--text-color-secondary` |
| `.qnav-row.is-current` / `:hover` | `color` | `--text-color-primary` |
| `.qnav-row.is-peek` | `background` | `--bg-color-secondarycontainer` |
| `.qnav-a span` | `color` | `--text-color-placeholder` |
| `.qnav-card` | `background` / `border-color` | `--card` / `--border` |
| `.qnav-flash` | `box-shadow` | `brand-color` @ 16% |

### 5.5 控件 API（工程接口）

```ts
type OutlineItem = { key: string | number; title: string; answer?: string };

type OutlineConfig = {
  host: HTMLElement;           // position:relative 的对话列宿主
  scroller: HTMLElement;       // 滚动容器
  items: OutlineItem[];        // ≥2 才显示
  peek?: boolean;              // 提问目录 true；文档形态 false
  denseRail?: boolean;         // 很多条
  visible?: boolean;           // 由宿主算：items≥2 且 chatHasOutlineRoom()；勿只绑「右侧打开」
  offset?: number;             // 跳转留白，对话约 24
  nodeFor: (key: OutlineItem['key']) => HTMLElement | null;
};
```

常量：`OUTLINE_MIN=2`，`OUTLINE_MAX=18`，`OUTLINE_PITCH=10`，`OUTLINE_RAIL_GUTTER≈40`，卡内约 12 行。逻辑以 Demo `createOutline` 为准。拖 chat|panel 分割线时须重算 `visible`。

### 5.6 文案规则
- 提问标题：用户首行；空则文件名 / 引用 /「（空消息）」。
- 副文：对应回复纯文本首句；截断由 CSS line-clamp。
- 消息目录标题：由业务 PRD 映射表决定（如总结块标题）。

### 5.7 与悬停气泡
目录行**不使用**全站 `data-tip` 气泡；展开卡即说明。

---

## 6. 产品规则与边界

### 6.1 本期两种目录（对话列）
| 模式 | 何时出现 | 列表内容 | Peek |
|---|---|---|---|
| 提问目录 | ≥2 条用户提问锚点，且对话列右缘空隙足够 | 用户问 + 可选副文 | 有（副文约 2 行） |
| 消息目录 | 长单答等多块锚点，且对话列右缘空隙足够 | 导语 / 报告块等 | 按业务 |

业务侧只负责：提供稳定锚点与标题；**不要**自造另一套圆点目录 / 左侧 TOC 替代本控件；**不要**给目录行加悬停气泡。

### 6.2 与右侧展示区（本期口径）
| 状态 | 对话列提问 / 消息目录 | 右侧文档区目录 |
|---|---|---|
| 仅对话（右侧关闭，列通常很宽） | 可显示（≥2 锚点） | — |
| 右侧打开 + 对话列仍宽（拖宽 chat） | **可显示** | **不出**（本期不做） |
| 右侧打开 + 对话列窄（默认或拖窄） | **隐藏** | **不出** |
| 右侧对比 / 任务 / 空 + 列窄 | **隐藏** | **不出** |

**标注**：不做「文档目录改到右侧文档区」是因为云文档即将上线。对话列目录让位条件是**宽度不够**，不是「右侧一开就藏」（对齐 Cursor Message map rail）。

变体消息（重新生成多版本）：只收录**当前展示**那一版对应的用户问。

### 6.3 很多条
- 右缘只显示一段短横线窗口；当前项居中或贴边。
- 边缘平移 + 悬停鱼眼（局部放大，不是整列变标题）。
- 停留后可展开标题卡；卡内可滚完全部。
- 减弱动效：无展开动画、flash、鱼眼。

### 6.4 边界
| 情况 | 处理 |
|---|---|
| 不足 2 条锚点 | 不显示轨 |
| 锚点暂时找不到 | 跳过该项；有节点的照常 |
| 标题极长 | 提问单行省略 |
| 快速划过轨 | 离开延迟约 150ms，避免闪烁 |

---

## 7. 验收标准

### 7.1 功能
- [ ] 1 条锚点不显示；加到 2 条后显示。
- [ ] 滚动时当前横线 / 行跟随。
- [ ] 悬停展开，点击跳转并 flash，离开收起。
- [ ] 提问 peek：副文 2 行内。
- [ ] 打开右侧且对话列窄：对话列目录隐藏；拖宽对话列后目录恢复；右侧**无**文档目录。
- [ ] 关闭右侧后：对话列目录在仍 ≥2 锚点且列宽足够时恢复。
- [ ] 很多条：轨窗口上限；边缘平移与鱼眼；展开后卡内可滚完全部。
- [ ] 减弱动效下无展开动画、flash、鱼眼。

### 7.2 视觉
- [ ] 展开卡在轨中心附近，不无故贴顶。
- [ ] 当前横线更长更深；样式对齐 Demo。
- [ ] 很多条时右缘只有一段横线窗口；鱼眼仅局部放大。

### 7.3 用例（Demo）
| # | Case | 期望 |
|---|---|---|
| 1 | 提问目录 | ≥2 问；peek；跳转用户气泡 |
| 2 | 消息目录 | 多块锚点跳转 |
| 3 | 文档目录 | **仅形态演示**；不作为本期上线验收 |
| 4 | 列宽显隐 | 打开右侧后默认常藏；拖宽 chat → 轨出现；拖窄 → 再藏；右侧无文档目录 |
| 5 | 不足两条 | 无轨 |
| 6 | 很多条 | ≥35 问；短横线窗口；鱼眼；贴顶/底平移；卡内滚完全部 |

---

## 8. 已确认与待确认

| # | 问题 | 产品结论 | 状态 |
|---|---|---|---|
| Q1 | 提问与消息目录同时 | 同一会话优先提问目录；长单答用消息目录；不同时两套轨 | 默认方案 |
| Q2 | 展开是否延迟 | 普通立即开；很多条模式约 240ms 再开 | 已确认 |
| Q3 | 深色 token | 复用全站 `@text-color-*` / 容器 token（见 5.3），不另开色表；次要容器深色取 Gray12 `#191A1D` | 已确认 |
| Q4 | 触控无 hover | 本期点击轨切换开合；窄屏专项另案 | 默认方案 |
| Q5 | 右侧打开后是否出文档目录 | **本期不做**。右侧文档区不挂目录（云文档承接）。对话列提问 / 消息目录改按**列宽**显隐，不与右侧开闭硬绑 | 已确认 |
| Q6 | 显隐是否对齐 Cursor | **是**。跟对话列可用宽度 / 右缘 gutter；拖 chat\|panel 分割线可让轨在右侧打开时仍出现 | 已确认 2026-11-26 |

---

## 9. 参考

- Demo：`对话目录.html`
- 全站：`docs/CHATDEMO-STANDARDS.md` §2.6
- 业务锚点示例：文档分析与总结 PRD §6.3；课程论文 PRD 组件节
