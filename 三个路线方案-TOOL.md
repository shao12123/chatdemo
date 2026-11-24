# 行程对比 Intelligent UI · 触发判定（Agent TOOL）

| 项目 | 内容 |
|---|---|
| 工具名 | `present_route_compare` |
| 用途 | **只做一件事**：判断本轮是否走行程对比 Intelligent UI（改 UI 展现形式），而不是普通 Markdown |
| 版本 | v0.6 · 2026-11-24 |
| 配套 | [三个路线方案-PRD.md](三个路线方案-PRD.md)（组件、数据、前后端）· [三个路线方案.html](三个路线方案.html)（交互 Demo）· 交叉优先级见与 `present_plan_list` 一节 |
| 口径 | **场景 = 行程规划** · **严触发** · **首轮判定**（不含「改用方案二 / 改某一程」）· **与计划清单同轮可双开** |

> 把下面的 **description** 和 **系统提示增补** 贴进 Agent 的 tools / system prompt。本文件**不写** JSON Schema、校验、渲染、兜底、多轮引用；那些在 PRD。

---

## 1. 要识别的场景

**场景名：行程规划。**

用户要的是一份**可执行的出行 / 行程方案**：从哪到哪、几点走、怎么走；可以带推荐，也可以要多个方案对比。

| | 说明 |
|---|---|
| 属于本场景 | 通勤排程、城际 / 出国行程、机场接驳、多站拜访、一日游 / 多日游骨架……只要核心交付物是「可执行的行程方案」 |
| Demo 两个 case | 一日通勤（徐汇滨江三场会）、跨天旅行（马德里 → 巴塞 → 马略卡）**只是示例**，用来演示 UI 和数据差异，**不是触发白名单** |
| 判定方法 | 看「即将交付的产物是不是行程方案」，不要抠关键词，也不要抠「是不是上海」「是不是西班牙」 |

---

## 2. 何时调用 / 何时不调用

### 要调用（走 Intelligent UI）

同时满足：

1. 用户意图是**行程规划**（排出发时间、排路线、制定行程 / 方案、比较几种出行方式等）；
2. 本轮主答案将交付 **≥1 个**可执行方案，且每个方案能拆出有序行程段（起点 / 终点 + 时间或时长 + 交通方式）。

说明：

- 用户要 1 个方案也调用（前端可以不出对比卡，仍用结构化方案 UI）。
- **用户未指定方案个数时，默认出 3 个**（上限 4；超过 4 只保留前 4，见 PRD）。
- 缺时间、日期等非起终点信息：按常理假设后仍可调用（假设不展示给用户的细节见 PRD）。
- 同一轮还夹了无关问题（如「顺便推荐酒店」）：**行程部分仍调用本工具**；无关部分用普通文字另答（怎么拼消息见 PRD）。
- 同一轮还要准备清单 / 待办条目（要带的东西、出发前要做的事）：**行程部分仍调用本工具**；清单走 `present_plan_list`，两工具可同轮都调。

### 不调用（普通 Markdown / 文本）

任一成立即不调用：

- 没有规划意图的单段 A→B 指路（「怎么走最快」只要一句建议）；
- **主交付是玩 / 住 / 吃攻略或景点清单**，没有要排可执行交通时段 / 行程段；
- 交通知识问答（末班车、哪家航司等）；
- 起点或终点缺失，只能先追问；
- 订票、叫车、下单；
- 翻译 / 总结 / 改写**已有**行程文本；
- 与行程无关的任务。

### 本 TOOL 不覆盖（不要写进触发规则）

以下**不作为本文件的触发条件**（是否继续用结构化 UI、怎么传参，由产品多轮逻辑 / PRD 处理，不在本 TOOL 里判定）：

- 「改用方案二」「用专车那个」；
- 「第二程改打车」「下午不坐高铁」等调整某一程。

---

## 2.1 场景边界（验收清单）

开发验收「进 / 不进」时，按本表，不另扩口径。

| 边界 | 口径 |
|---|---|
| **时间跨度** | 半日 / 一日 / 多日都进；**不因天数拒绝调用**（无天数上限） |
| **地理范围** | 同城 / 城际 / 跨省 / 跨国都进 |
| **内容：攻略 vs 行程** | 看**主交付物**：能拆成有序行程段（起终点 + 时间或时长 + 交通）→ **进**；主交付是景点 / 美食 / 酒店清单、不要求排交通时段 → **不进**；「排行程 + 顺便推荐酒店」→ 行程进工具，酒店用文字 |
| **与计划清单** | 见 §2.2；有序交通段归本工具；可勾选准备 / 待办归 `present_plan_list`；同轮可双开。纯出行方式对比且会出 legs → **只调本工具**，不要用计划「建议」抢结论 |
| **方案数** | 用户说了按说的（1–4，>4 截前 4）；**未指定默认 3**；只要 1 个仍调用，前端可不出对比卡 |
| **字段范围（控复杂度）** | 触发不砍天数；卡内仍只承载**交通行程骨架**（有序 legs + 可选 nights）。每日玩住吃细项**不进卡字段**；用户要了用普通文字 |
| **排除** | 单段 A→B 指路、知识问答、订票下单、缺起终点只追问、改写已有行程等（见 §2 不调用） |

---

## 2.2 与 `present_plan_list` 的优先级

两工具**不互斥**；按主交付拆，不要按字数或「几个方案」拆。同一轮按下面顺序判定：

1. 主交付是否含**有序交通行程段**（起终点 + 时间或时长 + 交通方式）→ **有则调用本工具**（行程部分）。
2. 同轮另有**可勾选准备 / 待办条目**（要带的东西、出发前要做的事等，拿掉钟点后条目还在）→ **再调** `present_plan_list`；两工具可同轮都调。
3. **纯出行方式对比**（如高铁 vs 飞机），且本轮会交付带 legs 的方案 → **只调本工具**；不要用 `present_plan_list` 的「建议」出结论抢对比卡。
4. 主交付是玩 / 住 / 吃、**拆不出** legs → **本工具不调**；再交给计划工具判建议 / 计划；仍否 → 普通文本。
5. **缺起点或终点**只能先追问 → **本工具不调**；不要改判成计划清单。

---

## 3. description（可贴进 tools）

### 中文

```text
何时调用：当用户意图是「行程规划」，且你要把可执行的行程 / 通勤 / 旅行方案作为本轮主答案交付时调用——至少一个方案，且每个方案有有序行程段（起点/终点 + 时间或时长 + 交通方式）。半日/一日/多日、同城到跨国都适用，不因天数拒绝。Demo 里的一日通勤、跨天旅行只是示例，不是白名单。用户未指定方案个数时默认出 3 个（1–4；只要 1 个也调用）。同轮还有准备清单 / 待办时，行程部分仍调用本工具，清单走 present_plan_list，可同轮双开。纯出行方式对比且会交付带 legs 的方案时只调本工具，不要用 present_plan_list 的「建议」抢结论。

不要调用：没有规划意图的单段 A→B 指路；主交付是玩/住/吃攻略或景点清单、不排交通时段；交通知识问答；起点或终点缺失只能追问（不要改判成计划清单）；订票下单；把已有行程拿去改写/翻译；以及其他非行程任务。

本工具只负责声明「本轮行程用 Intelligent UI 展示」。卡内只承载交通行程骨架；玩住吃细项用普通文字。不要用本工具的描述去处理「改用方案二」「第二程改打车」等后续轮次是否调用——那些不在本触发规则范围内。
```

### English（平台 description 要求英文时用）

```text
WHEN TO CALL: Call when the user intent is itinerary/travel planning (行程规划) and you are ready to deliver an executable travel/commute/itinerary plan as the main answer—at least one plan with ordered legs (from/to + time or duration + transport mode). Half-day, same-day, and multi-day all count; city to international all count; do not refuse by trip length. Demo cases are examples only, not a whitelist. If the user does not specify how many plans, default to 3 (range 1–4; still call for a single plan). If the same turn also needs a packing/todo checklist, still call this tool for the transport part and call present_plan_list for the checklist—both may fire in one turn. For pure mode comparison (e.g. train vs flight) that will deliver legged plans, call only this tool; do not let present_plan_list’s “suggestion” steal the conclusion.

DO NOT CALL for: one-shot A→B directions without planning intent; destination guides focused on sights/food/hotels without scheduling transport legs; transport knowledge Q&A; clarification-only turns when origin or destination is missing (do not reclassify as a plan checklist); booking/ticketing; rewriting/translating an existing itinerary; or unrelated tasks.

This tool only declares that this turn’s itinerary should use Intelligent UI. Card fields stay at the transport-skeleton level; day-by-day play/stay/eat detail goes in plain text if needed. Do not use this description to decide follow-ups like “switch to plan 2” or “change leg 2 to taxi”—those are out of scope for this trigger tool.
```

---

## 4. 系统提示增补（可贴进 system / developer）

```text
# 行程对比 UI 触发（present_route_compare）

你有一个工具 present_route_compare。它只用于判定：本轮行程答案是否改用 Intelligent UI，而不是普通 Markdown。

## 判定口诀
识别「行程规划」场景，看即将交付的产物：
- 产物是可执行的行程方案（≥1 个方案，每方案有有序行程段）→ 调用
- 主交付是玩/住/吃攻略、没有排交通时段 → 不调用
- 否则 → 不调用，用普通文本 / Markdown

## 与 present_plan_list 的优先级
同一轮先看主交付是否含有序交通行程段：
- 有 → 调用本工具（行程部分）
- 另有可勾选准备/待办条目 → 再调 present_plan_list；可同轮双开
- 纯出行方式对比且会出 legs → 只调本工具；不要用计划「建议」抢结论
- 拆不出 legs → 本工具不调；再交计划工具判建议/计划；仍否 → 普通文本
- 缺起终点只能追问 → 本工具不调；不要改判成计划清单

不要把 Demo 的两个例子当成唯一可触发的 query。
不因天数、同城/跨国拒绝调用。用户未指定方案数 → 默认 3 个。

## 要调用
- 排出发时间、排路线、制定行程/方案、比较几种出行方案
- 半日/一日/多日；同城、城际、出国、机场接驳、多站拜访等，凡属行程规划都算
- 「排路线 + 列要带的东西」：行程部分仍调用；清单走 present_plan_list

## 不调用
- 「A 怎么到 B」只要一句路线、没有规划/出方案意图
- 「云南七日游攻略 / 杭州有什么好玩」等纯玩住吃，不排交通
- 地铁运营、票价知识问答
- 缺起点或终点：先追问（不要改判成计划清单）
- 订票、叫车
- 翻译/总结/改写已有行程
- 与行程无关的任务

## 边界提醒
- 卡内只出交通行程骨架；玩住吃细项用文字
- 「改用方案二」「第二程改打车」等后续轮次：本触发规则不覆盖，不要根据本段决定是否调用
```

---

## 5. 正例 / 反例（评测用）

### 应调用

| 用户说法 | 说明 |
|---|---|
| 我住徐汇滨江，9:30 陆家嘴、12:00 静安寺、15:00 回人民广场公司，帮我排出发时间和路线，给我三个方案 | Demo 示例：一日通勤 |
| 马德里出发，巴塞 3 天、马略卡 4 天再回马德里，明天走，帮我排行程和交通方案 | Demo 示例：跨天旅行 |
| 明天去浦东机场赶 10 点的飞机，地铁和打车给我两个方案 | 机场接驳；指定 2 个方案 |
| 帮我排下周去杭州的行程 | 缺具体时刻也可；未指定方案数 → 默认 3 |
| 帮我排杭州 3 天怎么走，每天主要交通和大致时段 | 多日 + 可执行行程（非纯攻略） |
| 下周从上海去北京开会两天，高铁和飞机给我比一下 | 城际；未指定方案数 → 默认 3；**只调本工具**，不要用计划「建议」抢结论 |
| 只要一个方案：明天浦东机场接机怎么走最稳 | 单方案仍调用 |
| 帮我排巴塞和马略卡的行程，顺便推荐几家酒店 | 行程调用；酒店用文字另答 |
| 周日去野餐，列要带的东西，并排从家到公园怎么走 | 行程调用；清单另调 `present_plan_list`（同轮双开） |

### 不应调用

| 用户说法 | 说明 |
|---|---|
| 徐汇滨江到陆家嘴怎么走最快？ | 单段指路，无规划意图 |
| 徐家汇地铁怎么换乘到虹桥？ | 单段指路 |
| 云南 7 日游攻略，推荐景点和特色美食 | 纯玩住吃攻略 |
| 杭州有什么好玩的，住哪方便 | 目的地咨询，无排程意图 |
| 上海地铁末班大概几点？ | 知识问答 |
| 帮我订明天去北京的机票 | 订票，非本场景 |
| 把刚才的行程写成周报 | 改写已有文本 |
| 我明天有事，能帮我排路线吗？（没给地点） | 缺起终点，先追问；不要改判成计划清单 |
| 周日请朋友吃烤羊肉，出一份计划清单 | 计划清单场景；走 `present_plan_list`，本工具不调 |

### 不纳入本 TOOL 评测（后续轮次）

| 用户说法 | 说明 |
|---|---|
| 改用方案二 | 本触发规则不覆盖 |
| 第二程改打车 | 本触发规则不覆盖 |
| 用专车那个 | 本触发规则不覆盖 |

---

## 6. 和 PRD / Demo 的分工

| 文档 | 负责 |
|---|---|
| **本文件** | 什么样的首轮 query / 产物要改 UI；边界清单；description + 系统提示 + 正反例 |
| **PRD** | 卡片长什么样、字段、调用后怎么渲染、校验、兜底、多轮「改方案 / 改某一程」 |
| **Demo** | 两个示例 case 的交互与视觉；不是触发白名单 |
