# MoreAI Demo（唯一入口）

2～3 人共用这一份仓库。**不要再互相传 HTML 附件**；改完推送到 GitHub 后，打开固定入口刷新即可。

## 唯一入口（刷新即最新）

**https://shao12123.github.io/chatdemo/**

首次部署后若 404，等 1～2 分钟再硬刷新。页上版本号应与仓库里 `version.json` 一致。

源码仓库：https://github.com/shao12123/chatdemo

## 协作怎么用

1. Clone / pull 这一份仓库
2. 改自己的 HTML；新 demo 在 `demos.json` 登记
3. **每次发布**更新 `version.json` 的 `version` / `updatedAt` / `note`
4. 推送到 GitHub `main`（会触发 Pages）

```bash
git pull
git add -A
git commit -m "说明这次改了什么"
git push origin main
```

### 多人约定

| 场景 | 做法 |
| --- | --- |
| 各做各的页面 | 新增 `xxx.html`，并在 `demos.json` 加一条 |
| 改同一文件 | 先 `git pull`，再改、再推；大改可开分支 |
| 说明面板文案 | 以仓库默认说明为准；浏览器 localStorage 个人修改不要当分发版 |

## 本地预览

```bash
cd /Users/wangshaolong/Documents/chatdemo
python3 -m http.server 8765 --bind 127.0.0.1
```

打开 `http://127.0.0.1:8765/`（不要用 `file://`）。

## 颜色（Demo / 卡片库）

业务 Demo、卡片库必须使用现网语义色，**不要写飞书灰 hex**：

- 公共 CSS：[`shared/moreai-tokens.css`](shared/moreai-tokens.css)（浅/深 = `moreai-electron` 现网）
- 约定与对照表：[docs/CHATDEMO-STANDARDS.md §5](docs/CHATDEMO-STANDARDS.md)
- 金样：[对话目录.html](对话目录.html)（已引入公共 token + 浅深切换）

后续场景 / 形态墙改造：先 `link` 该 CSS，再删本地 `--gc-*` / 飞书色。

## 目录

- `index.html` — 团队唯一入口
- `demos.json` — demo 列表（`kind: demo` 业务 / `card` 卡片库）
- `shared/moreai-tokens.css` — 公共浅深 token（B0）
- `version.json` — 版本戳（每次发布必改）
- `conversations.html` — 主对话 demo
- `chat-cards/` — 卡片形态墙
- `.github/workflows/pages.yml` — 推送 `main` 后自动发布 Pages`

## 远程

- `origin` → GitHub（日常推送，驱动 Pages）
- `cursor` → Cursor Origin 备份（可选：`git push cursor main`）
