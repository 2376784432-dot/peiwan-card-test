# 三只黑鸟 · 陪玩卡片（GitHub Pages 测试包）

这是用于 GitHub Pages 测试的静态页面，只有一个入口文件 `index.html`，不需要构建、不需要服务器。

## 上传步骤

1. 登录 GitHub，新建一个 **Public** 仓库，例如 `peiwan-card-test`。
2. 进入仓库，点 **Add file → Upload files**。
3. 把本文件夹里的这些文件拖进去：
   - `index.html`
   - `robots.txt`
   - `.nojekyll`
   - `README.md`
4. 点 **Commit changes** 提交。
5. 打开仓库 **Settings → Pages**。
6. **Build and deployment → Source** 选择 **Deploy from a branch**。
7. 分支选 `main`，目录选 `/ (root)`，点 **Save**。
8. 等待 1–3 分钟，访问：

```text
https://你的GitHub用户名.github.io/peiwan-card-test/
```

9. 把链接发到微信「文件传输助手」测试，再发给别人。

> 如果上传时看不到 `.nojekyll`（隐藏文件），可以在仓库里用 **Add file → Create new file**，文件名填 `.nojekyll`，内容留空，提交即可。

## 更新页面

1. 在本地修改 `three-black-birds.html`，点「保存为 HTML」写回文件。
2. 把新文件复制为 `github-pages/index.html`，覆盖旧文件。
3. 在 GitHub 仓库里重新上传 `index.html` 并提交。
4. 等 Pages 更新后，在链接后加 `?v=2` 刷新缓存。

## 测试重点

- 页面能否在微信内置浏览器打开；
- 三只鸟飞入、树枝上的三个入口是否正常；
- 病恋 / 虐恋 / 虚恋三个界面和交互是否正常；
- 页面在 https 下会自动隐藏编辑按钮，访客只能浏览。

## 风险提醒

- GitHub Pages 仓库是公开的，页面源码和文案都公开；`noindex` 和 `robots.txt` 只是降低搜索引擎收录，**不是访问控制**。
- GitHub Pages 免费版适合个人展示/测试，不适合作为收费客户卡片的正式商业托管。正式商用建议用客户自己的腾讯云 COS / CloudBase 或付费合规托管。
- 不要上传客户的真实联系方式、身份证、未经授权的照片/音乐，以及未成年人相关信息。
- 微信可能拦截或提示 `github.io` 外链；这是平台/网络问题，不是 HTML 的问题。如果打不开，换国内静态托管。
- 正式发给别人之前，建议先用虚构的测试文案，不要直接把真实客户资料放进公开仓库。
