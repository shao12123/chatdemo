# MoreAI Demo（唯一入口）

2～3 人共用这一份仓库。**不要再互相传 HTML 附件**；改完推送后，打开固定入口刷新即可。

## 当前状态

| 项 | 状态 |
| --- | --- |
| 源码仓库 | 已接入 Cursor Origin：`shaolong-wang/chatdemo` |
| 入口页 / 版本戳 / 多 demo 列表 | 已就绪（`index.html` + `demos.json` + `version.json`） |
| 在线预览（刷新即最新） | 待挂静态托管。仓库里已备好 GitHub Pages 工作流；本机当前访问不了 GitHub，需要你选一个可访问的托管点（见下） |

仓库页：https://cursor.com/codebase/shaolong-wang/chatdemo

## 协作怎么用

1. 克隆或拉取这一份仓库（不要另存 Downloads 副本当分发）
2. 改自己的 HTML；若是新 demo，在 `demos.json` 登记一条
3. **每次发布**更新 `version.json` 的 `version` / `updatedAt` / `note`
4. `git pull` → `git add -A` → `git commit` → `git push`
5. 等人打开**在线入口**硬刷新，核对页上版本号

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

打开 `http://127.0.0.1:8765/`（不要用 `file://`，否则读不到 `demos.json` / `version.json`）。

## 挂上「刷新即最新」的入口

任选其一（有固定 https 地址即可）：

1. **GitHub Pages**（网络可达 GitHub 时）  
   - 把同一仓库推到 GitHub  
   - 仓库 Settings → Pages → Source 选 GitHub Actions  
   - 已有 `.github/workflows/pages.yml`，推 `main` 即发布  
   - 入口形如：`https://<user>.github.io/chatdemo/`

2. **Surge / 公司静态资源 / OSS**  
   - 把仓库根目录整包上传到固定域名  
   - 团队只收藏那一个 URL

挂好后把入口 URL 写进群公告；页眉版本戳用来确认有没有刷到最新。

## 目录

- `index.html` — 团队唯一入口
- `demos.json` — demo 列表
- `version.json` — 版本戳（每次发布必改）
- `conversations.html` — 主对话 demo
- `.github/workflows/pages.yml` — GitHub Pages 自动发布
