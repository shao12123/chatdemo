# card_id 命名（v1.8 · 仅 Chat 区）

> 卡按 **UI 壳 / 模式** 命名，不按业务故事。场景差异在触发规则与 payload。  
> **范围：只收对话区（含输入框附件）出现的卡。** 右侧文档台 / 标注台 / PPT 预览等展示区 **不进卡片库**。  
> 形态墙见各子目录 `morph-wall.html`。

---

## 已定结论

| # | 结论 |
|---|---|
| 1 | 中立 `card_id`；旧名进 `aliases` |
| 2 | 有 Agent 的壳：**一卡一 Tool** = `present_{card_id}` |
| 3 | 表 / 对比是通用壳：`data_table` · `option_compare` |
| 4 | 全站件 **前端挂，无 Tool**（含附件两卡） |
| 5 | **思考过程不算卡** |
| 6 | **输入框附件 ≠ chat 用户文件**：拆成 `composer_file` + `user_file`（2026-11-19） |
| 7 | **`ask_form` = host_only**：无 `present_` Tool（2026-11-19） |
| 8 | **补输入只有一张 `ask_form`**（2026-11-19）：天气编号壳为 canonical；多题 / 多选走 payload；路线旧 `.ask` 不另开卡 |
| 9 | **卡片库只保留 Chat 区**（2026-11-19，v1.7）：右侧文档台、标注 dock、PPT 预览等 **不做 card_id / 形态墙** |
| 10 | 已撤出库：`course_paper` · `annotation_edit`（原 A 区；形态墙已删）。对话入口条仍用 `doc_entry` |
| 11 | **`schedule_preview` 单卡**（2026-11-19，v1.8）：`agenda_list` 为其 **别名**，不另开 card_id；body 变体 `timeline` \| `agenda`。选项条 / 日期闸门归 `ask_form`；失败包裹壳（悬停重试）为可选，是否收敛 `error_card` 待评审 |
| 12 | **Chat 批次 · map / app / time**（2026-11-19，v1.8）：`map_align`（映射对照）· `app_link`（连接外部应用）· `schedule_preview`（日程预览）落形态墙；授权弹层 `#authMask`、右侧日历台 `.time-cal` / `#doc` **不进库** |

---

## 命名规则

| 规则 | 说明 |
|---|---|
| 语义 | **壳能力**，不是业务故事名 |
| 表面 | 必须出现在 **Chat / 输入框**；右侧台面不算卡 |
| Tool | 仅 `agent_invoke: tool`：`present_{card_id}` |
| 场景 | payload + Tool description；可选 `preset` |
| 别名 | 旧 id / Demo 名进 `aliases[]` |

---

## 卡片清单

### A. 通用壳 · 有 Tool（Chat 内）

| card_id | 人话 | Tool | 别名 |
|---|---|---|---|
| `option_compare` | 多方案对比卡 | `present_option_compare` | `route_compare` |
| `data_table` | 明细表卡 | `present_data_table` | `route_detail` |
| `doc_summary` | 总结报告卡 | `present_doc_summary` | |
| `paper_compare_pick` | 对比选择卡 | `present_paper_compare_pick` | 仅「对比哪些文件」 |
| `weather_card` | 天气卡 | `present_weather_card` | `天气场景` · `wx` |
| `map_align` | 映射对照卡 | `present_map_align` | `映射对照` |
| `app_link` | 连接外部应用卡 | `present_app_link` | `连接外部应用` · `应用授权卡` |
| `schedule_preview` | 日程预览卡 | `present_schedule_preview` | `agenda_list` · `日程预览` · `建议日程` |
| `push_digest` | 推送汇总卡 | `present_push_digest` | 暂缓 |

### B. 全站件 · 无 Tool（Chat / 输入框）

| card_id | 人话 | 形态墙 | 备注 |
|---|---|---|---|
| `tip_block` | 提示块 | [有](tip_block/morph-wall.html) | 灯泡 |
| `error_card` | 错误卡片 | [有](error_card/morph-wall.html) | `.net-card` |
| `composer_file` | 输入框附件 | [有](composer_file/morph-wall.html) | 悬停角标 × / 上传中 |
| `user_file` | 对话用户文件 | [有](user_file/morph-wall.html) | 类型色；无移除 |
| `doc_entry` | 文档/产物入口 | [有](doc_entry/morph-wall.html) | chat 内 artifact；点开右侧台 **不是卡** |
| `ask_form` | 补充信息卡 | [有](ask_form/morph-wall.html) | host_only |
| `map_entry` | 地图入口卡 | 本期不做 | 若落在 chat 再做 |

> 旧 `file_chip` → aliases，分别映射到 `composer_file` / `user_file`。

### C. 候选尚未收名（须先确认在 Chat）

`group_form` · `group_result` · `analysis_card`

### E. 不进库（右侧 / 预览台）

| 原名 / 区域 | 说明 |
|---|---|
| 右侧文档台（原 `course_paper`） | `aside#doc` 写入 / Tab / 任务板 / 对比板 |
| 标注台（原 `annotation_edit`） | PPT 刘海队列 · 课程 `mark-dock` |
| 地图全屏 / 预览台 | 非 chat 气泡 |
| 授权弹层（连接外部应用 `#authMask`） | host overlay，非 chat 内卡 |
| 日历台（时间场景 `.time-cal`） | 右侧展示区；对话内仅 `schedule_preview` / `doc_entry` |

---

## D. 已有形态墙

| card_id | 路径 |
|---|---|
| `option_compare` | [option_compare/morph-wall.html](option_compare/morph-wall.html) |
| `data_table` | [data_table/morph-wall.html](data_table/morph-wall.html) |
| `tip_block` | [tip_block/morph-wall.html](tip_block/morph-wall.html) |
| `error_card` | [error_card/morph-wall.html](error_card/morph-wall.html) |
| `composer_file` | [composer_file/morph-wall.html](composer_file/morph-wall.html) |
| `user_file` | [user_file/morph-wall.html](user_file/morph-wall.html) |
| `doc_entry` | [doc_entry/morph-wall.html](doc_entry/morph-wall.html) |
| `ask_form` | [ask_form/morph-wall.html](ask_form/morph-wall.html) |
| `weather_card` | [weather_card/morph-wall.html](weather_card/morph-wall.html) |
| `doc_summary` | [doc_summary/morph-wall.html](doc_summary/morph-wall.html) |
| `paper_compare_pick` | [paper_compare_pick/morph-wall.html](paper_compare_pick/morph-wall.html) |
| `map_align` | [map_align/morph-wall.html](map_align/morph-wall.html) |
| `app_link` | [app_link/morph-wall.html](app_link/morph-wall.html) |
| `schedule_preview` | [schedule_preview/morph-wall.html](schedule_preview/morph-wall.html) |

---

## ask_form 范围

| 算本卡 | 不算本卡 / 待办 |
|---|---|
| 天气编号选项 + 自定义行 + 确认/跳过 ± 日历（canonical） | 三个路线方案旧 `.ask` —— 遗留，收敛进本壳 |
| `mode: multi` / 多题 `fields[]` / host 换题 | |
| 结果旁挂的 chat 卡（如 `weather_card`） | 右侧台面 |

---

## 三类「文件相关」怎么分

| | `composer_file` | `user_file` | `doc_entry` |
|---|---|---|---|
| 谁产生 | 用户选文件（未发/待发） | 用户已发送 | AI / 系统产物 |
| 位置 | 输入框 | 用户消息 | AI 回复（chat） |
| 移除 | 悬停角标 × | 无 | 无 |
| 下载 | 无 | 无 | 可有预览/下载 |
| Agent | 不调 Tool | 不调 Tool | 不调 Tool |
| 点开后 | — | — | 右侧文档台（**非卡**） |
