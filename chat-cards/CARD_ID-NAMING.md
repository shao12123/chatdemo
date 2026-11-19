# card_id 命名（v1.3 · 通用壳）

> 卡按 **UI 壳 / 模式** 命名，不按业务故事。场景差异在触发规则与 payload。  
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

---

## 命名规则

| 规则 | 说明 |
|---|---|
| 语义 | **壳能力**，不是业务故事名 |
| Tool | 仅 `agent_invoke: tool`：`present_{card_id}` |
| 场景 | payload + Tool description；可选 `preset` |
| 别名 | 旧 id / Demo 名进 `aliases[]` |

---

## 卡片清单

### A. 通用壳 · 有 Tool

| card_id | 人话 | Tool | 别名 |
|---|---|---|---|
| `option_compare` | 多方案对比卡 | `present_option_compare` | `route_compare` |
| `data_table` | 明细表卡 | `present_data_table` | `route_detail` |
| `doc_summary` | 总结报告卡 | `present_doc_summary` | |
| `course_paper` | 长文文档台 | `present_course_paper` | |
| `paper_compare_pick` | 对比选择卡 | `present_paper_compare_pick` | |
| `annotation_edit` | 标注修改卡 | `present_annotation_edit` | |
| `weather_card` | 天气卡 | `present_weather_card` | |
| `map_align` | 映射对照卡 | `present_map_align` | |
| `push_digest` | 推送汇总卡 | `present_push_digest` | 暂缓 |

### B. 全站件 · 无 Tool

| card_id | 人话 | 形态墙 | 备注 |
|---|---|---|---|
| `tip_block` | 提示块 | [有](tip_block/morph-wall.html) | 灯泡 |
| `error_card` | 错误卡片 | [有](error_card/morph-wall.html) | `.net-card` |
| `composer_file` | 输入框附件 | [有](composer_file/morph-wall.html) | 悬停角标 × / 上传中 |
| `user_file` | 对话用户文件 | [有](user_file/morph-wall.html) | 类型色；无移除 |
| `doc_entry` | 文档/产物入口 | [有](doc_entry/morph-wall.html) | artifact |
| `map_entry` | 地图入口卡 | 本期不做 | |

> 旧 `file_chip` → aliases，分别映射到 `composer_file` / `user_file`。

### C. 候选尚未收名（下批）

`ask_form` · `group_form` · `group_result` · `app_link` · `schedule_preview` · `agenda_list` · `analysis_card`

### D. 已有形态墙

| card_id | 路径 |
|---|---|
| `option_compare` | [option_compare/morph-wall.html](option_compare/morph-wall.html) |
| `data_table` | [data_table/morph-wall.html](data_table/morph-wall.html) |
| `tip_block` | [tip_block/morph-wall.html](tip_block/morph-wall.html) |
| `error_card` | [error_card/morph-wall.html](error_card/morph-wall.html) |
| `composer_file` | [composer_file/morph-wall.html](composer_file/morph-wall.html) |
| `user_file` | [user_file/morph-wall.html](user_file/morph-wall.html) |
| `doc_entry` | [doc_entry/morph-wall.html](doc_entry/morph-wall.html) |

---

## 三类「文件相关」怎么分

| | `composer_file` | `user_file` | `doc_entry` |
|---|---|---|---|
| 谁产生 | 用户选文件（未发/待发） | 用户已发送 | AI / 系统产物 |
| 位置 | 输入框 | 用户消息 | AI 回复 |
| 移除 | 悬停角标 × | 无 | 无 |
| 下载 | 无 | 无 | 可有预览/下载 |
| Agent | 不调 Tool | 不调 Tool | 不调 Tool |
