# 科研困难与贡献分析 — GitHub Pages 版

无需编译。支持随机出题、手动设题、困难/贡献分别填写、结构自检、导出 Markdown、浏览器本地保存，以及在供应商支持 CORS 时从浏览器直接调用大模型。

## 在 GitHub Pages 发布

1. 在 GitHub 创建仓库，例如 `research-argument-lab`。
2. 将压缩包内的 `index.html`、`app.js`、`style.css`、`.nojekyll` 放在**仓库根目录**。不要只上传 ZIP 文件。
3. 打开仓库 `Settings` → `Pages` → `Build and deployment`，将 `Source` 设为 `Deploy from a branch`，选择 `main` 和 `/(root)`，点击 `Save`。
4. 等待 Pages 部署完成，在 Pages 设置中点击 `Visit site`。项目站点地址通常是 `https://<GitHub用户名>.github.io/<仓库名>/`。

官方说明：https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

## 单独评审

- 默认「自动识别已填写部分」：只填写困难 D1–D4 即可评困难；只填写贡献 C1–C4 即可评贡献。两个部分均填写时才检查对应关系。
- 也可在「评审范围」手动指定「只评困难」「只评贡献」「困难与贡献一起评」。
- D5、C5 为可选的证据字段。当前选中部分的四个核心字段需填写完整，另一部分无需填写。

## API 与数据安全

GitHub Pages 是**纯静态托管**，不能运行本项目的 Node.js `server.js`。Pages 版本的 AI 评审从访问者的浏览器直接请求供应商，因此：

- 只有允许浏览器跨域调用（CORS）的供应商/接口才能成功。不能保证所列十家都支持。需要稳定的在线 AI 评审，应将后端另行部署在支持服务端运行的平台，再由前端调用该后端。
- **不要把 API Key 写到 GitHub 仓库文件、README、前端源代码、GitHub Pages 环境变量注入的静态脚本里。**
- API 供应商、URL、模型等设置默认保存在浏览器 `localStorage`；在 GitHub Pages 上，API Key **默认不持久保存**。只有主动勾选“在此浏览器记住 API Key”才会将其明文保存；公开部署、共用电脑以及同一 `github.io` 主域下的其他网页都存在风险。不推荐勾选。
- 研究题目、填写内容在浏览器本地存储，不会随着网页发布到 GitHub 仓库；点击 AI 评审会将当前所选内容和 API Key 发送给配置的模型供应商。
- 从本地 HTML 改为 GitHub Pages 后，因为浏览器来源不同，旧的本地配置通常不会自动转移。

如需没有 CORS 限制且不让访客接触服务端 API Key 的公网版本，需要单独部署安全的服务端代理，GitHub Pages 本身无法实现。
