# 三只黑鸟 · 陪玩卡片（GitHub Pages + Cloudflare 文字后台）

这是用于 GitHub Pages 的静态页面。页面美术、动画、三个界面都在 `index.html` 里；**文字内容由 Cloudflare Worker + KV 提供**。

- 公开链接：`https://2376784432-dot.github.io/peiwan-card-test/`
- 访客：只能看
- 客户：拿私密编辑链接，打开后点「编辑文字」，改完点「保存到云端」
- 客户不需要注册 GitHub、Cloudflare 或任何账号

## 工作方式

```text
GitHub Pages（你的仓库：美术 / 动画 / 三个界面）
        │ 读取
        ▼
Cloudflare Worker：peiwan-card-api
        │
        ▼
Workers KV：peiwan-card-content（key: content）
```

- 公开页每次打开会请求 Worker 的 `GET /content`，把云端文字应用到页面；
- 云端读取失败时，自动退回页面内置的默认文字；
- 只有带编辑 token 的链接才会显示编辑按钮；
- 访客没有 token，看不到编辑入口，也改不了官方页面；
- token 是“链接即权限”，不要发到公开群或仓库。

## 客户怎么编辑

把这条私密链接单独发给客户（不要放在公开页面）：

```text
https://2376784432-dot.github.io/peiwan-card-test/#edit=<EDIT_TOKEN>
```

客户打开后：

1. 点右下角「编辑文字」；
2. 直接点页面上的标题、简介、价目、标签等文字修改；
3. 点「保存到云端」；
4. 访客刷新页面就能看到新内容。

聊天记录、心率、疼痛值等动态内容仍由程序控制，不属于可编辑文字。

## 修改文字上限

- 单张卡总文字上限：**50,000 个中文字符**（约 150 KB）；
- 单次保存最多 600 个字段；
- 当前页面约 1,349 个中文字符，空间充足；
- 保存用按钮，不要每打一个字自动保存，避免消耗 KV 每日写入额度。

## 更新美术/动画

1. 本地修改 `E:\DSH\陪玩卡片测试\three-black-birds.html`；
2. 如果只是美术/CSS/交互，不需要改云端文字；
3. 把新文件复制为 `github-pages/index.html`，推送 GitHub；
4. 等 Pages 更新后，在链接后加 `?v=2` 刷新缓存。

**注意**：页面里的 `data-edit-id` 是按当前 DOM 顺序生成的。如果大幅增删可编辑节点，旧的云端字段可能错位；改结构后要重新保存一次文字。

## 首次部署步骤（给所有者）

1. GitHub 新建 **Public** 仓库，上传 `index.html`、`robots.txt`、`.nojekyll`、`README.md`；
2. **Settings → Pages → Deploy from a branch → main / (root) → Save**；
3. Cloudflare 创建 Worker `peiwan-card-api`，绑定 KV `CARD_CONTENT`（namespace：`peiwan-card-content`）；
4. Worker 的 Secret：`EDIT_TOKEN`；值只保存在本地 `.edit-token.txt`，不要提交；
5. 部署 Worker 后，确认 `https://peiwan-card-api.2376784432.workers.dev/health` 返回 `{"ok":true}`；
6. 用编辑链接测试一次保存，再打开公开链接确认内容更新。

## token 泄露或换客户

在 Cloudflare：

```text
Workers & Pages → peiwan-card-api → Settings
→ Runtime variables and secrets → EDIT_TOKEN → Edit → Rotate
→ 输入新 token → Deploy
```

旧链接会立刻失效（保存时返回 401）；把新链接发给客户即可。

## 安全提醒

- 公开页面的源码、文案、价格都可以被查看和复制；`noindex` 不是访问控制。
- 访客不能改**官方页面**，但可以把网页另存后改自己的副本；这是 Web 的本质。
- 客户分发时发**链接/二维码**，不要发 HTML 源文件；发文件给别人，别人可以改自己的副本。
- 不要把 `.edit-token.txt`、Cloudflare API Token 提交到 GitHub。
- GitHub Pages 免费版适合测试/个人展示；正式收费客户建议用客户自己的国内云账号或付费托管。

## 微信测试

1. 先把公开链接发到微信「文件传输助手」；
2. 手机微信点开，检查动画和三个入口；
3. 再用编辑链接在手机浏览器里改一次文字，确认保存后公开页能看到；
4. 如果浏览器能开、微信打不开，通常是 `github.io` / `workers.dev` 外链限制，不是 HTML 问题，需要换国内托管或自定义域名。
