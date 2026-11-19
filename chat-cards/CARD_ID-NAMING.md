# card_id 命名（v2.3 · 仅 Chat 区 · 可开发范围）

> 卡按 **UI 壳 / 模式** 命名，不按业务故事。场景差异在触发规则与 payload。  
> **范围：只收对话区（含输入框附件）出现的卡。** 右侧台面 **不进库**。  
> **业务 Demo 标「本期暂不开发 / 本期不做」的能力不入库**（无 card_id、无形态墙）。  
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
| 6 | **输入框附件 ≠ chat 用户文件**：`composer_file` + `user_file` |
| 7 | **`ask_form` = host_only**；补输入唯一壳（含「对哪里不满意」`.pick-card`、多题步进） |
| 8 | 路线旧 `.ask` **不另开卡**，收敛进 `ask_form` |
| 9 | **只保留 Chat 区**；右侧文档台 / 标注 / 日历台 / 授权弹层 **不进库** |
| 10 | 已撤：`course_paper` · `annotation_edit` · `schedule_preview` |
| 11 | 日程 Chat **只挂 `doc_entry`**（日历 mark） |
| 12 | `map_align` · `app_link` 已入库 |
| 13 | 建群：`group_form` + `group_result`，不并 `ask_form` |
| 14 | `analysis_card`（`.revise-card`），不并 `doc_summary` |
| 15 | **「本期暂不开发」不上库**（2026-11-19，v2.3）：撤出 `push_digest`（`hot-topics` 暂缓）；`map_entry`（本期不做）不占清单位 |

---

## 命名规则

| 规则 | 说明 |
|---|---|
| 语义 | 壳能力，非业务故事 |
| 表面 | Chat / 输入框 |
| 闸门 | Demo `status: 本期暂不开发` 或 PRD「本期不做」→ **不入库** |
| Tool | `present_{card_id}`（仅 Agent 壳） |
| 别名 | 旧 id / Demo 名 → `aliases[]` |

---

## 卡片清单（本期入库）

### A. 通用壳 · 有 Tool

| card_id | 人话 | Tool | 别名 |
|---|---|---|---|
| `option_compare` | 多方案对比卡 | `present_option_compare` | `route_compare` |
| `data_table` | 明细表卡 | `present_data_table` | `route_detail` |
| `doc_summary` | 总结报告卡 | `present_doc_summary` | |
| `paper_compare_pick` | 对比选择卡 | `present_paper_compare_pick` | 「对比哪些文件」 |
| `weather_card` | 天气卡 | `present_weather_card` | `天气场景` · `wx` |
| `map_align` | 映射对照卡 | `present_map_align` | `映射对照` |
| `app_link` | 连接外部应用卡 | `present_app_link` | `连接外部应用` |
| `group_form` | 建群表单卡 | `present_group_form` | `创建群组` |
| `group_result` | 建群结果卡 | `present_group_result` | `群已创建` |
| `analysis_card` | 结构化分析卡 | `present_analysis_card` | `revise_card` · `.revise-*` |

### B. 全站件 · 无 Tool

| card_id | 人话 | 形态墙 | 备注 |
|---|---|---|---|
| `tip_block` | 提示块 | [有](tip_block/morph-wall.html) | |
| `error_card` | 错误卡片 | [有](error_card/morph-wall.html) | `.net-card` |
| `composer_file` | 输入框附件 | [有](composer_file/morph-wall.html) | |
| `user_file` | 对话用户文件 | [有](user_file/morph-wall.html) | |
| `doc_entry` | 文档/产物/日历入口 | [有](doc_entry/morph-wall.html) | 日程 Chat 只此卡 |
| `ask_form` | 补充信息卡 | [有](ask_form/morph-wall.html) | 含天气壳 / 多选多题 / 不满意 pick / 步进 |

### C. 候选

（空）

### E. 不进库

| 项 | 原因 |
|---|---|
| 右侧文档台 / 标注 / 日历台 / 授权弹层 / digest-view | 展示区 |
| 日程 timeline / agenda 预览板 | 非入口卡；Chat 只 `doc_entry` |
| `push_digest` / 高频主题推送 | 业务 Demo「本期暂不开发」 |
| `map_entry`（`.rm-card`） | PRD「本期不做」 |
| 思考过程 | 不算卡 |

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

---

## ask_form 范围

| 算本卡 | 说明 |
|---|---|
| 天气编号选项 + 日期 / 文本自定义 | canonical |
| `mode: multi` / `fields[]` / host 换题 | payload |
| 「对哪里不满意」`.pick-card` + radio / 自定义 | conversations 修订前补问 |
| 多题步进 `.pick-card` + steps | onboard askCard |
| 路线旧 radio / 方框多选 | 遗留，收敛进本壳，不新开 id |

---

## 三类「文件相关」

| | `composer_file` | `user_file` | `doc_entry` |
|---|---|---|---|
| 位置 | 输入框 | 用户消息 | AI 回复 |
| 头图 | 类型色 | 类型色 | 缩略或日历 mark |
| 点开后 | — | — | 右侧台（非卡） |
