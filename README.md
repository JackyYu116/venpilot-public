# VenPilot — venpilot.us 静态发布包

这个目录直接包含可发布的网站：`index.html`、全部内页、图片、CSS、JavaScript、静态导航数据、`.nojekyll` 和 `CNAME`。无需在托管服务器安装 Node.js。轮播、行业选图、移动菜单和邮件草稿表单使用包内 JavaScript。

唯一目标域名是 **https://venpilot.us/**，`CNAME` 内容为 `venpilot.us`，构建的 basePath 为空。完成 GitHub Pages 自定义域名配置和 DNS 后，使用正式域名验收。普通 `github.io/仓库名/` 子路径需要不同的构建前缀，不用于验收本包。

## 源码与版本

本包基于已保存的 `venpilot-github-pages.zip` 第 12 版源码制作，未读取私有 GitHub 仓库的最新提交。如果仓库有更新，请使用最新源码重新导出。

制作本包时将 Next.js 和 eslint-config-next 从 16.4.2 更新为 16.4.6，解决依赖检查报告的已知漏洞。原源码仓库后续构建时也应同步升级这两个包并提交锁文件。原始页面、照片和文案未重写。

## 1. 发布仓库

建议使用已创建的 **JackyYu116/venpilot-public** 公开仓库，只存放这份静态构建结果；原来的私有源码仓库继续用于开发。

本压缩包解压后，`index.html` 必须位于发布仓库根目录。将整个包的内容上传，包括 `_next`、各内页目录、`images`、`brand`、`materials`、`.nojekyll` 和 `CNAME`。静态导航所需的 `.txt` 文件也需保留。

GitHub 网页上传有文件数量限制，本包有两百多个文件，建议通过 Git 一次性提交。

### Windows 操作示例

在 VS Code 的 **Terminal → New Terminal** 中，进入你准备放项目的文件夹，然后克隆发布仓库：

```powershell
git clone https://github.com/JackyYu116/venpilot-public.git
```

用 Windows 文件资源管理器，把本压缩包解压后的**全部内容**复制到刚克隆的 `venpilot-public` 文件夹。保留该文件夹原有的 `.git`。本包的 README 可以覆盖新仓库初始化时的 README。

进入这个发布仓库，提交：

```powershell
cd venpilot-public
git status
git add .
git commit -m "Publish VenPilot static website for venpilot.us"
git push origin main
```

这里上传的是编译后的网页文件，不是原来的 Next.js 源码目录。以后更新页面，先修改私有源码、重新导出，再更新这个发布仓库。

## 2. 仓库所有者开启 Pages

使用 **JackyYu116** 账号进入发布仓库：

1. **Settings → Pages**。
2. **Build and deployment → Source → Deploy from a branch**。
3. Branch 选择 **main**，目录选择 **/(root)**，点击 **Save**。
4. **Custom domain** 填入 **venpilot.us** 并点击 **Save**，确认 GitHub 已保存这个域名。
5. 等待 Pages 的部署工作流完成。本发布包已经是静态文件，因此该仓库不需要再配置 Next.js 构建工作流。

这两个设置步骤由仓库所有者操作。Lin 可以作为 Collaborator 上传和更新静态文件；如果希望 Lin 独立管理仓库设置，可另行采用组织仓库及相应管理权限。

## 3. Lin 或域名管理员在 GoDaddy 配置 DNS

先完成 GitHub Pages 的 Custom domain 设置，再修改 DNS。使用拥有 **venpilot.us DNS 管理权限**的 GoDaddy 账号，核对或更新以下网站解析记录：

| 类型 | 名称 / Name | 值 / Value |
| --- | --- | --- |
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | jackyyu116.github.io |

`www` 的记录值不带 `https://` 或 `/venpilot-public/`。只调整网站使用的 @ / www 解析；保留邮箱的 MX、TXT 等其他业务记录。已有冲突的网站 A、AAAA 或 CNAME 记录时，由域名管理员核对替换。

等待 DNS 检查通过、GitHub 证书就绪后，由仓库所有者在 **Settings → Pages** 勾选 **Enforce HTTPS**。DNS 和证书可能需要最多约 24 小时。正确配置后，`www.venpilot.us` 会转向 `venpilot.us`。

## 4. 验收

使用正式域名依次检查：

- 首页及行业卡片的连续滚动、点击后展示对应照片。
- 顶部导航、移动菜单、IM30 及行业内页。
- 直接打开 `/contact/`、`/industries/`、`/terminals/im30/`，并刷新。
- 电话、邮箱链接，以及填写咨询信息后的草稿生成。
- `www.venpilot.us` 的跳转与 HTTPS。

表单仍然在访客浏览器内生成咨询内容及邮件草稿，不会自动提交至 CRM。商户账号、Merchant ID、刷卡处理和后端 API 不包含在本次静态展示网站发布中。

## 重新生成本包

在对应源码目录运行：

```powershell
npm.cmd install next@16.4.6 eslint-config-next@16.4.6 --save-exact
$env:NEXT_PUBLIC_BASE_PATH = ""
npm.cmd run lint
npm.cmd run build
npm.cmd audit --omit=dev
```

取 `out` 目录里的全部内容作为发布文件；确保根目录有空的 `.nojekyll`，并有内容为 `venpilot.us` 的 `CNAME`。每次完整替换静态构建结果时保留发布仓库的 `.git`。

## 官方参考

- GitHub Pages 发布来源：https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
- GitHub Pages 自定义域名及 DNS：https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site
- Next.js 静态导出：https://nextjs.org/docs/app/guides/static-exports
- Next.js 16.4.6：https://github.com/vercel/next.js/releases/tag/v16.4.6
