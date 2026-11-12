# MoreAI Demo（唯一入口）

2～3 人共用这一份仓库。**正确打开方式**是站点入口（GitHub Pages），不是互相传 HTML 附件。

## 怎么用

1. 打开 Pages 入口（部署后形如 `https://<user>.github.io/chatdemo/`）
2. 从列表进入对应 demo
3. 看页上的**版本号 / 更新时间**，确认是不是最新

本地改完后：

```bash
# 1. 改 HTML
# 2. 更新 version.json 的 version / updatedAt / note
# 3. 若是新 demo，在 demos.json 登记
git pull
git add -A
git commit -m "说明这次改了什么"
git push
```

等 Pages 更新（通常不到 1 分钟），其他人**刷新入口页**即可。

## 多人约定

| 场景 | 做法 |
| --- | --- |
| 各做各的页面 | 新增 `xxx.html`，并在 `demos.json` 加一条 |
| 改同一文件 | 先 `git pull`，再改、再推；大改可开分支 |
| 说明面板文案 | 以仓库里的默认说明为准；浏览器 localStorage 里的个人修改不要当分发版 |

## 本地预览

```bash
cd /Users/wangshaolong/Documents/chatdemo
python3 -m http.server 8765 --bind 127.0.0.1
```

打开 `http://127.0.0.1:8765/`（不要用 `file://`，否则入口读不到 `demos.json`）。

## 目录

- `index.html` — 团队唯一入口
- `demos.json` — demo 列表
- `version.json` — 版本戳（每次发布必改）
- `conversations.html` — 主对话 demo
- `.github/workflows/pages.yml` — 推送 `main` 后自动发布 Pages
