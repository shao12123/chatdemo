# card_id 命名（v2.2 · 仅 Chat 区）

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
| 11 | **日程管理 Chat 只挂 `doc_entry`**（2026-11-19，v1.9）：日历 mark 入口（如「周四面试日程」）；**撤** `schedule_preview` / `agenda_list`。timeline / agenda 预览板（含「安排中」）**不进卡片库**（场景 Demo 可有，库不收） |
| 12 | **Chat 批次 · map / app**（2026-11-19）：`map_align` · `app_link` 落形态墙；授权弹层、右侧日历台 **不进库** |
| 13 | **建群两张卡**（2026-11-19，v2.0）：`group_form` + `group_result`，**不并入 `ask_form`**（多字段事务壳：群名 / 头像 / 双栏选人 / 单主按钮）。表单 = `article.gc`；结果 = `article.gc.gc-result`。Tool：`present_group_form` · `present_group_result`。创建中卡隐藏，仅 lead + think（think 不算卡）。无授权 / 右侧面板，host overlay 不进库 |
| 14 | **`analysis_card` 独立成卡**（2026-11-19，v2.1）：壳 = `conversations.html` 的 `.revise-card`（`reviseStreamHtml`），**不并入 `doc_summary`**。Tool：`present_analysis_card`。aliases：`revise_card` · `结构化分析卡` · CSS 前缀 `.revise-*`。字段按决策类型变薄（诊断型 / 方案型 / 边界型）；卡内无主按钮。**思考过程不是本卡**；`.pick-card`「对哪里不满意」**不是本卡**，属补输入 → 归 `ask_form`（host_only） |
| 15 | **`push_digest` 形态墙落地**（2026-11-19，v2.2）：壳 = `高频主题汇总.html` 的 `article.digest-card` 族（浅灰气泡 + `.dr-row` 三档信息块 + `.mc-*` 部件）。Tool：`present_push_digest`。形态墙 5 变体：代码变更摘要（创建 + 赞/不感兴趣）· 会议准备 · 关联内容 · 飞书聊天知会 · 高频主题汇总 canonical（多主题）。**仅 Chat**；右侧 `.digest-view` 简报 **不进库**（E 区）。`map_entry` 仍本期不做（若启用，chat 壳为 `.rm-card` stub）。**业务 Demo**（`hot-topics`，`高频主题汇总.html`）仍「本期暂不开发」，demos.json 状态不变；形态墙另列 `card-push-digest-wall` |

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
| `group_form` | 建群表单卡 | `present_group_form` | `建群卡` · `创建群组` |
| `group_result` | 建群结果卡 | `present_group_result` | `群已创建` · `建群结果卡` |
| `analysis_card` | 结构化分析卡 | `present_analysis_card` | `revise_card` · `结构化分析卡` · `.revise-card`（CSS 前缀 `.revise-*`） |
| `push_digest` | 推送汇总卡 | `present_push_digest` | `digest-card` · `推送卡` · `高频主题汇总`（形态墙已落；业务 Demo 仍暂缓） |

### B. 全站件 · 无 Tool（Chat / 输入框）

| card_id | 人话 | 形态墙 | 备注 |
|---|---|---|---|
| `tip_block` | 提示块 | [有](tip_block/morph-wall.html) | 灯泡 |
| `error_card` | 错误卡片 | [有](error_card/morph-wall.html) | `.net-card` |
| `composer_file` | 输入框附件 | [有](composer_file/morph-wall.html) | 悬停角标 × / 上传中 |
| `user_file` | 对话用户文件 | [有](user_file/morph-wall.html) | 类型色；无移除 |
| `doc_entry` | 文档/产物/日历入口 | [有](doc_entry/morph-wall.html) | 含日历 mark；日程 Chat **只此卡** |
| `ask_form` | 补充信息卡 | [有](ask_form/morph-wall.html) | host_only |
| `map_entry` | 地图入口卡 | 本期不做 | 若落在 chat 再做；壳按 `.rm-card` stub（chat 入口条），**本期不做**，不并入 `push_digest` |

> 旧 `file_chip` → aliases，分别映射到 `composer_file` / `user_file`。  
> 旧 `schedule_preview` / `agenda_list` → **不进库**；日程产物入口归 `doc_entry`。

### C. 候选尚未收名

（暂无。`analysis_card` 已于 v2.1 移入 A；`push_digest` 形态墙已于 v2.2 落地。）

### E. 不进库（右侧 / 预览台 / 非卡）

| 原名 / 区域 | 说明 |
|---|---|
| 右侧文档台（原 `course_paper`） | `aside#doc` 写入 / Tab / 任务板 / 对比板 |
| 标注台（原 `annotation_edit`） | PPT 刘海队列 · 课程 `mark-dock` |
| 地图全屏 / 预览台 | 非 chat 气泡 |
| 授权弹层（连接外部应用 `#authMask`） | host overlay |
| 日历台（时间场景 `.time-cal`） | 右侧展示区 |
| 日程 timeline / agenda 预览板（原 `schedule_preview`） | `.time-sim-card` / `.time-agenda`；场景可演示，**卡片库不收** |
| `digest-view` 右侧详情（`push_digest`「查看详情」打开） | `aside#doc` 内 `.digest-view` 简报（学习 / 案例 / 周计划等）；展示区，**不进卡片库**；chat 内只收 `article.digest-card` |

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
| `group_form` | [group_form/morph-wall.html](group_form/morph-wall.html) |
| `group_result` | [group_result/morph-wall.html](group_result/morph-wall.html) |
| `analysis_card` | [analysis_card/morph-wall.html](analysis_card/morph-wall.html) |
| `push_digest` | [push_digest/morph-wall.html](push_digest/morph-wall.html) |

---

## ask_form 范围

| 算本卡 | 不算本卡 / 待办 |
|---|---|
| 天气编号选项 + 自定义行 + 确认/跳过 ± 日历（canonical） | 三个路线方案旧 `.ask` —— 遗留，收敛进本壳 |
| `mode: multi` / 多题 `fields[]` / host 换题 | |
| 结果旁挂的 chat 卡（如 `weather_card`） | 右侧台面；日程 timeline 预览板 |

---

## 三类「文件相关」怎么分

| | `composer_file` | `user_file` | `doc_entry` |
|---|---|---|---|
| 谁产生 | 用户选文件（未发/待发） | 用户已发送 | AI / 系统产物 |
| 位置 | 输入框 | 用户消息 | AI 回复（chat） |
| 头图 | 类型色图标 | 类型色图标 | 文档缩略 **或** 日历 mark |
| 移除 | 悬停角标 × | 无 | 无 |
| 下载 | 无 | 无 | 文档类可有；日历类通常无 |
| Agent | 不调 Tool | 不调 Tool | 不调 Tool |
| 点开后 | — | — | 右侧文档台 / 日历台（**非卡**） |
