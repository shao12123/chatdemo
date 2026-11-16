# 文档分析与总结 PRD

| 项目 | 内容 |
|---|---|
| 文档状态 | 待开发 · 第 12 节问题已全部确认 |
| 版本 | v0.1 · 2026-11-16 |
| 交互 Demo | [大模型代码评审.html](大模型代码评审.html)（入口标题「文档分析与总结」）：左侧 8 个 case 覆盖不同材料信号与壳族；右上角「说明」可看逐项标注 |
| 适用读者 | 前端、后端、测试，以及接手开发的 AGENT |

> 给 AGENT：请按「第 5 节壳族与模块 → 第 6 节数据契约 → 第 7 节后端 → 第 8 节前端 → 第 11 节验收」的顺序实现。第 12 节已确认的按结论开发；写「沿用线上」的不改现有逻辑。**内容与推荐逻辑不变**：总结文案沿用线上模型输出，本期只改呈现。Demo 中的逐块流式出现仅作演示，**流式本期不做**（整条回复一次渲染）。

---

## 1. 背景与目标

### 1.1 现状
用户上传文档 / 图片 / 表格后问「这个里面讲了什么」「这些材料一起讲了什么」，线上回复多为长 Markdown：
- 多附件时要点揉在一段里，分不清「哪个文件说了什么」；
- 有事实数字、对照关系、台账表、规范条款、无结构速记等不同材料，却几乎同一套段落排版；
- 关键判断、指标、表格与原文摘录缺少统一闸门，容易空壳占位或层级过深。

### 1.2 目标
- **内容和结论不变**：只改 Chat 内呈现；不改模型总结策略与文案口径（Demo 文案对齐线上 case）。
- 一套「总结报告」组件族，按材料信号选壳，普适于「内容总结」宽场景（不绑死某一行业文档）。
- 模块闸门：有数据才渲染；多附件可拆多张「总结报告」外框，与导语同宽。
- 与全站规范一致：思考过程、操作栏、Tip Block、消息目录、颜色 token、悬停气泡。

### 1.3 成功标准
- 用户一眼能分清：总判断 / 按文件或按要点拆开的报告 / 提示。
- 无数据模块不出现空格子；一张交互卡最多两层。
- 台账类能看到表；无结构速记不出现指标格。
- 结构化失败时回退完整 Markdown（不空白、不报错页）。

---

## 2. 范围

### 2.1 本期做
1. 文档分析与总结场景的结构化输出与渲染（第一轮为主；见 4.2）。
2. 组件：思考过程、导语、总结报告卡（含壳族与模块）、提示块、操作栏、长回复消息目录。
3. **模块闸门**与 **壳族映射**（第 5 节）：`metric` / `points` / `material` / `spec` / `ledger` / `catalog` / `notes`。
4. 多附件：用户侧每文件独立 `file-chip`（图可带缩略图）；回复侧可按文件拆多张「总结报告」。
5. 表格模块、合作对照块、原文摘录（quotes）、章节 Tab（卡内全文）。
6. 深浅模式：颜色走全站 token（见全站规范 / 行程对比 PRD 5.6）。
7. 兜底：结构化失败回退 Markdown。
8. 异常与恢复：接入《生成异常与恢复》（非 PPT）。

### 2.2 本期不做
- 纯 Markdown 线上总结长文形态的视觉复刻（另议）。
- 改写 / 翻译 / 抽取待办等「总结以外」的二次任务入口（可后续加推荐操作）。
- 右侧强制打开原文（点文件入口可选打开；章节原文以卡内 Tab 为主）。
- 流式逐块出现（Demo 仅演示）。
- 用户在报告卡内编辑结论、拖拽重排模块。
- PPT 分页恢复逻辑（沿用标注修改 Demo）。

---

## 3. 场景

**要识别的场景是「内容总结」**：用户基于已上传或已引用的材料，要一份可读的总结 / 要点 / 对照（不是行程规划、不是论文对比写作）。

Demo / 验收里的 8 个 case **只是信号样例**，用来覆盖壳族与模块组合，**不是触发白名单**——只要属于内容总结且能交付结构化载荷，就走本需求。

| Case（Demo） | 材料信号 | 壳 `shell` | 关键呈现 |
|---|---|---|---|
| 大模型代码评审 | 可计数事实 + 对比指标 | `metric` | verdict + facts + metrics + conclusion + chapters |
| 骨架类型正确 | 规则 / 判断要点 | `points` | verdict + points + conclusion |
| AICode 合作提案 | 多页图材料 | `material` | 总判断 + 每页一张总结报告（缩略图 / 标签 / 对照 / 要点） |
| 家用机器人规范 | 规范条款 | `spec` | verdict + fields + 左边线 sections |
| 付费模版库 | 台账 / Excel | `ledger` | verdict + facts + **table 优先** + sections |
| 研究学习环境课件 | 分类目录 | `catalog` | verdict + facts + 分类 sections |
| 帮我总结下这些文件里讲了什么 | 多附件混合 | `material` | 每文件一张「总结报告」；图旁缩略图+标签+核心主题；合作对照；未读用表 |
| 杂乱会议笔记 | 无结构速记 | `notes` | 总判断 + **拆多张要点卡** + quotes；无指标格 |

---

## 4. 交互流程

### 4.1 第一轮
1. 用户发送 query（常预填「这个里面讲了什么」或「帮我总结下这些文件里讲了什么」），附件以文件卡展示。
2. **思考过程**：MoreAI 头像 + 名称；阶段流光 + 秒数；展开为步骤。默认展开，回复开始出现时收起「已完成 N 秒」，头像跳一下。最后一步固定写「按推荐组织报告卡。需要换看法时，写完后再说。」（与全站「少选择」一致）。
3. **回复**，整条一次出现（流式本期不做），自上而下固定顺序：
   1. 导语；
   2. 总结报告（单卡，或 `fileCards` / `split: points` 拆成多张外框）；
   3. 提示块（有 `tip` 才出）；
   4. 操作栏。
4. **操作栏**：复制、重新生成、赞、踩、分享 + 时间（悬停显示）。用户消息带文件时仅复制该消息。

### 4.2 第二轮起
- 触发：用户输入换看法 / 追问某一文件或某一段。
- 回复：思考过程 → 导语（可更短）→ 报告卡（可只含相关模块）→（tip）→ 操作栏。
- **不强制**重复首轮全部附件拆卡；按新问题裁剪载荷。

### 4.3 其他交互
| 交互 | 规则 |
|---|---|
| 文件入口 | 对话内 `file-chip` 整卡可点；可选打开右侧原文 |
| 章节 Tab | 仅 Chat 卡内切换全文，不因此强制打开右侧 |
| 消息目录 | ≥2 个可跳转块且右侧未开时显示：用户提问 / 导语 / 各总结报告（含总判断、按文件、按要点）/ 提示 |
| 赞 / 踩 | 互斥，再点取消；点踩不弹原因 |
| 编辑用户消息 | 原位编辑，发送后末尾新增并重新生成；生成中不可编辑 |
| 重新生成 | 与论文对比一致：新版本 + 版本切换；Demo 可为原位重跑 |

---

## 5. 组件与样式规范

### 5.1 组件清单
| 组件 | 出现条件 | 形态 | 依赖字段 |
|---|---|---|---|
| 思考过程 | 每轮 | 复用全站路线思考（绿勾 + `.exec` + `.exec-dot`） | `think[]` |
| 导语 | 每轮 | 正文，宽与报告卡一致（Demo：`min(560px, 100%)`） | `lead` |
| 总结报告卡 | 每轮 | 一层外框；内模块最多再一层 | 见 5.2、5.3 |
| 提示块 | `tip` 非空 | 全站 Tip Block | `tip` |
| 操作栏 | 每轮 | 复用全站 | — |
| 消息目录 | 长回复 | 复用全站 outline | `data-sum-sec` |
| 用户文件卡 | 有附件 | `file-chip`；多文件纵向叠放；图可缩略图 | `files[]` / `file` |

### 5.2 模块闸门（有数据才渲染）
每个模块入口先判空；无数据返回空，**禁止为空壳占位**。

| 模块 | 用途 | 槽位 |
|---|---|---|
| `verdict` | 总判断 | `tag` + `text` |
| `thumbs` | 可选缩略条（少用；材料卡优先用文件头缩略图） | `string[]` |
| `facts` | 事实格（可计数摘要） | `label` / `value` / `note` |
| `metrics` | 对比指标组 | `kicker` + `items[]`（同 facts 槽位） |
| `points` | 要点卡 | `tag` / `title` / `text` |
| `fields` | 键值说明条 | `label` / `text` |
| `quotes` | 原文摘录（保留语气） | `string[]` |
| `sections` | 章节块 + 列表 | `title` + `bullets[]` |
| `table` | 台账 / 样例表 | `kicker` / `headers` / `rows` |
| `conclusion` | 收束 | `label` / `text` / `note` |
| `chapters` | 卡内原文 Tab | `id` / `name` / `title` + 关联 `doc.sections` |
| `fileCards` | 按文件 / 按页的材料卡（触发多外框） | 见 5.4 |
| `tip` | 卡外提示 | 全站 Tip Block 结构 |

**单卡默认顺序**：`verdict` → `thumbs` → `facts` → `metrics` → `points` → `fields` → `quotes` → `kicker` → `sections` → `table` → `conclusion` → `chapters`。其后 `tip` → 操作栏。

**多外框**：
- 有 `fileCards`：可选先出一张含 `verdict`（+ facts/conclusion）的「总判断」卡，再按文件各出一张「总结报告」。
- `split: "points"`：可选总判断卡（含 `quotes`），再按每个 point 各出一张「总结报告」。

### 5.3 壳族（`card.shell`）
壳只调样式与推荐模块组合，**不改文案**。颜色仍只表层级；强调用字重。

| shell | 视觉要点 | 推荐模块组合 |
|---|---|---|
| `metric` | 事实格 + 指标组突出 | verdict + facts + metrics + conclusion（+ chapters） |
| `points` | 要点卡白底描边，略抬一层 | verdict + points + conclusion |
| `material` | 文件头缩略图 + 标签 + 核心主题；可含对照 | fileCards（+ 总判断） |
| `spec` | 章节左边品牌线；可配 fields | verdict + fields + sections |
| `ledger` | 事实格 + 表强调 | verdict + facts + table + sections |
| `catalog` | 分类章节圆点前缀 | verdict + facts + sections |
| `notes` | 外框虚线、偏速记；拆多张要点 | verdict + split points + quotes |

### 5.4 材料卡（`fileCards[]` 单项）
| 字段 | 说明 |
|---|---|
| `file` / `title` | 文件名与标题 |
| `kind` | `img` 时可出内联缩略图 |
| `thumb` / `thumbLabel` | 缩略标识 |
| `tags[]` | 标签胶囊 |
| `core` | 「核心主题：…」 |
| `compare` | 左右对照：`left` / `right`（`label` + `items[]`）+ `mid` |
| `bullets[]` | 要点列表 |
| `table` | 可选表（如未识别内容） |

### 5.5 信号 → 壳（普适映射）
实现与评测按信号选壳，勿写死 case 名：

| 信号 | 壳 |
|---|---|
| 可核对数字 / 实验对照 | `metric` |
| 规则、清单、离散判断点 | `points` |
| 多文件或多页图文材料 | `material` |
| 设计 / 交互 / 禁止项等规范 | `spec` |
| 表格台账、字段资产 | `ledger` |
| 技能 / 能力 / 目录分类 | `catalog` |
| 无标题层级的口语速记 | `notes` |

同一回复只选一个主壳；`fileCards` 与 `split` 可在主壳下拆多外框。

### 5.6 视觉约束（摘录）
- 最多两层卡；次要容器底 `#EEF0F3`（深色走 token）。
- 报告外框与导语同宽（Demo 560）。
- 表：圆角 12；Tip Block / 错误卡：圆角 14。
- 禁止 `.think-tag` 灰胶囊 + 扁 `think-list`；思考必须抄路线 `routeThinkHtml`。
- 悬停气泡、输入框、错误卡片：见 `docs/CHATDEMO-STANDARDS.md`。

---

## 6. 数据契约

### 6.1 顶层
```ts
type SummaryReply = {
  think: { t: string; n: string }[];  // 3 步；最后一步含「按推荐组织报告卡…」
  lead: string;
  card: SummaryCard;
  // 可选：右侧原文
  doc?: {
    title: string;
    meta?: string;
    heading: string;
    by?: string;
    sections: { id: string; title: string; paras: string[] }[];
  };
};

type SummaryCard = {
  title?: string;           // 默认「总结报告」
  shell?: SummaryShell;
  split?: "points";         // 要点拆多外框
  verdict?: { tag?: string; text: string };
  thumbs?: string[];
  facts?: FactCell[];
  metrics?: { kicker?: string; items: FactCell[] };
  points?: PointItem[];
  fields?: { label: string; text: string }[];
  quotes?: string[];
  kicker?: string;
  sections?: { title: string; bullets: string[] }[];
  table?: { kicker?: string; headers: string[]; rows: string[][] };
  conclusion?: { label?: string; text: string; note?: string };
  chapters?: { id: string; name: string; title: string; text?: string; paras?: string[] }[];
  fileCards?: FileCard[];
  tip?: { title?: string; items: (string | [string, string])[] };
};

type SummaryShell =
  | "metric" | "points" | "material" | "spec"
  | "ledger" | "catalog" | "notes";

type FactCell = { label: string; value: string; note?: string };
type PointItem = { tag?: string; title: string; text: string };
type FileCard = {
  file?: string; title: string; kind?: string;
  thumb?: number | string; thumbLabel?: string;
  tags?: string[]; core?: string;
  compare?: {
    mid?: string;
    left: { label: string; items: string[] };
    right: { label: string; items: string[] };
  };
  bullets?: string[];
  table?: SummaryCard["table"];
  sectionKicker?: string;
};
```

### 6.2 校验
- `think.length === 3`（可配置，但 Demo / 本期固定 3）。
- 至少有 `verdict` 或 `fileCards` 或 `points` 之一，否则整卡失败 → 回退 Markdown。
- `table.headers.length` 与每行 `rows[i].length` 一致。
- `shell` 非法时按 `points` 降级样式，不阻断渲染。

### 6.3 用户侧附件
```ts
type AttachFile = {
  name: string;
  meta: string;   // 如 "PNG · 186 KB"
  kind: "img" | "pdf" | "docx" | "xlsx" | "md" | "html" | string;
  // img 可带预览 URL；Demo 用色块占位
};
```

---

## 7. 后端

### 7.1 触发
- 用户消息含附件或可解析引用，且意图为内容总结（见第 3 节），则走本结构化回复。
- 与行程对比互斥：行程规划走 `present_route_compare`；本场景不走行程卡。

### 7.2 生成步骤
1. 读取 / OCR / 解析附件（失败文案与恢复见《生成异常与恢复》）。
2. 模型产出总结文案（口径与线上一致）。
3. **结构化装填**：按 5.5 选 `shell`，只填有依据的模块；多文件优先 `fileCards`。
4. 一次返回完整 `SummaryReply`（流式本期不做）。

### 7.3 失败
- 结构化校验失败：返回 Markdown 全文 + 可选 `fallback: true`。
- 部分文件失败：成功文件照常总结；失败文件按异常规范逐条提示，不画空报告卡。

---

## 8. 前端

### 8.1 渲染
- 模块函数一一对应 5.2；入口判空。
- `briefCardHtml`（或等价）：按 `fileCards` / `split` 决定单卡或多外框；设置 `data-shell`、`data-sum-sec`。
- 思考过程必须复用路线 `routeThinkHtml` 族，禁止灰胶囊 think-tag。

### 8.2 消息目录
| 条件 | `data-sum-sec` | 目录标题 |
|---|---|---|
| 用户问 | `ask` | 用户提问 |
| 导语 | `lead` | 导语 |
| 单卡 | `report` | 总结报告 |
| 多文件总判断 | `report-lead` | 总判断 |
| 文件卡 i | `report-{i}` | 文件 / 页标题 |
| 拆要点 i | `report-point-{i}` | 要点标题 |
| tip | `tip` | 提示 |

### 8.3 职责边界
- 文案与壳选择：后端 / 模型侧产出；前端不因「好看」补空 facts。
- Tip Block、错误卡、悬停气泡：全站公共组件。
- 异常恢复：公共层，不在本 PRD 重定义。

---

## 9. 边界与异常
| 情况 | 处理 |
|---|---|
| 无附件纯口头「总结一下刚才」 | 若上下文有材料，仍可走本场景；否则普通 Markdown |
| 只有 1 个文件 | 默认可单卡；不必强行 `fileCards` |
| 表格过大 | 只传样例行（如前 4 行）+ 在 sections 说明全量规模 |
| 图片无法预览 | 材料卡无缩略图，保留文件名与要点 |
| 结构化失败 | 整段 Markdown 回退 |
| 与异常恢复叠加 | 中断 / 重试规则完全遵循《生成异常与恢复》 |

---

## 10. 埋点（建议）
| 事件 | 属性 |
|---|---|
| `summary_render` | `shell`、模块是否出现的布尔位图、`file_card_count`、`split` |
| `summary_fallback` | 原因码 |
| `summary_outline_jump` | 目标 `data-sum-sec` |
| `summary_file_open` | 文件 kind |

---

## 11. 验收标准

### 11.1 功能
- [ ] Demo 左侧 8 个 case 壳族与模块组合与第 3 节表一致。
- [ ] 模块闸门：去掉任一模块数据后，界面不留空壳。
- [ ] bundle / aicode：多张「总结报告」外框，与导语同宽；图卡含缩略图、标签、核心主题；合作场景有对照块。
- [ ] templates：可见 facts + table；notes：无指标格，要点拆卡 + 摘录。
- [ ] 思考 3 步，最后一步含「按推荐组织报告卡…」。
- [ ] 有 tip 才出提示块；位置在报告后、操作栏前。
- [ ] 长回复且右侧关闭时，消息目录可跳到各报告块。
- [ ] 结构化失败回退 Markdown（可用 fixture 测）。

### 11.2 视觉
- [ ] 最多两层卡；次要容器与品牌色符合全站 token。
- [ ] 各 `data-shell` 样式可区分，但不靠彩色字叠彩色底强调。
- [ ] 思考过程为路线绿勾时间线，不是灰胶囊列表。
- [ ] 与 Demo 并排对比，字号、间距、圆角、同宽策略一致。

### 11.3 测试用例
| # | 操作 | 预期 |
|---|---|---|
| 1 | 打开「大模型代码评审」发送 | metric：facts + metrics + 章节 Tab |
| 2 | 「骨架类型正确」 | points：5 条要点卡 + 收束 |
| 3 | 「AICode 合作提案」 | 总判断 + 3 张材料报告；第一张含对照 |
| 4 | 「家用机器人规范」 | spec：fields + 左边线章节 |
| 5 | 「付费模版库」 | ledger：3 事实格 + 样例表 + 章节 |
| 6 | 「研究学习环境课件」 | catalog：facts + 五类技能章节 |
| 7 | 「帮我总结下这些文件…」 | 每文件一张总结报告；未读文件可用表 |
| 8 | 「杂乱会议笔记」 | notes 拆卡；有 quotes；无 metrics/facts |
| 9 | 去掉 templates.table 数据 | 无表区域、无空白占位 |
| 10 | 右侧打开原文时 | 消息目录隐藏或按全站规则不抢焦点 |

---

## 12. 模拟开发走查：问题与待确认

| # | 问题 | 为什么会卡住开发 | 结论 | 状态 |
|---|---|---|---|---|
| Q1 | 总结文案是否可改写？ | 影响是否「只改 UI」 | 内容与线上一致，不增不减口径；本期只改呈现与结构化 | 已确认 |
| Q2 | 多附件合并成一张还是拆多张？ | 布局与目录 | 按文件（或按页）拆多张「总结报告」外框；可先有总判断卡 | 已确认 |
| Q3 | 报告卡是否与导语同宽？ | 样式 | 同宽（Demo 560） | 已确认 |
| Q4 | 壳是否绑死 case？ | 扩展性 | 否；按材料信号映射，Demo case 仅作样例 | 已确认 |
| Q5 | 无结构材料能否出指标格？ | 误导用户 | 否；用 points / quotes / 拆卡 | 已确认 |
| Q6 | tip 是否可自拟？ | 文案风险 | 有线上依据才出；无则不出（Demo 已清自拟 tip） | 已确认 |
| Q7 | 流式？ | 工期 | 本期不做；整条一次渲染 | 已确认 |
| Q8 | 思考最后一步文案 | 与少选择原则 | 「按推荐组织报告卡。需要换看法时，写完后再说。」 | 已确认 |
| Q9 | 章节原文放哪 | 右侧 vs 卡内 | 卡内 Tab 为主；右侧仅文件入口可选打开 | 已确认 |
| Q10 | 与异常恢复关系 | 重复定义 | 完全复用《生成异常与恢复》，本 PRD 不另定恢复按钮 | 已确认 |

---

## 13. 给 AGENT 的开发指引

### 13.1 任务拆分（建议顺序）
1. **数据契约**：按第 6 节补 TypeScript / JSON Schema；为 8 个壳各备一份 fixture（可从 Demo `SUMMARY_CASES` 导出）。
2. **公共模块渲染**：闸门函数 + `data-shell` 样式；多外框与消息目录。
3. **后端装填**：信号 → shell；模块只填有据字段；多文件 `fileCards`。
4. **接入异常恢复**与 Markdown 回退。
5. **验收**：对照 Demo 与第 11 节；视觉用说明模式锚点。

### 13.2 实现约束
- 不新增色板；强调用字重。
- 前端不补空 `facts` / `metrics` 凑版面。
- 思考过程禁止旧灰胶囊实现。
- Demo 与本文不一致时以本文为准；已知差异：Demo 含流式演示、部分缩略图为色块占位。

### 13.3 参考
- 交互 Demo：[大模型代码评审.html](大模型代码评审.html)
- 全站规范：[docs/CHATDEMO-STANDARDS.md](docs/CHATDEMO-STANDARDS.md)
- 提示块 / 错误卡：[三个路线方案-PRD.md](三个路线方案-PRD.md)、[生成异常与恢复-PRD.md](生成异常与恢复-PRD.md)
