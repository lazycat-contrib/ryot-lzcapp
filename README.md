# ryot-lzcapp（Ryot）

[Ryot](https://ryot.io)（追踪生活各方面的自托管平台：影视、游戏、书籍、健身等）的懒猫微服（LazyCat）打包。

## 模式

- **镜像模式（lazycat delivery）**：镜像经 `registry.lazycat.cloud` 投递（双商店发布要求）。
- **自动版本**：版本源为上游 `ignisda/ryot` 镜像（完整三段式 SemVer tag），自动发现新版本并升级。
- **双商店发布**：官方平台 + 喵喵商店（MiaoMiao private store）。
- 数据库密码与管理访问令牌均由 `stable_secret` 自动生成，不硬编码。

## 结构

| 文件 | 说明 |
| --- | --- |
| `package.yml` | 包元数据（`cloud.lazycat.app.ryot`） |
| `lzc-manifest.yml` | 运行配置：`ryot`（应用）+ `ryot-db`（PostgreSQL 16） |
| `lzc-build.yml` | 构建配置 |
| `icon.png` | 图标 |
| `.github/lazycat-action.yml` | [lazycat-github-action](https://github.com/ca-x/lazycat-github-action) 配置 |

## 自动化

`lazycat.yml` 工作流（配置文件 PR 时 dry-run 验证、每日定时约北京时间 14:31 + tag/手动触发）自动：

1. 检查上游 `ignisda/ryot` 新 SemVer tag；
2. 更新包版本与 manifest 镜像，并复制镜像到懒猫镜像仓库；
3. 构建 LPK、发布 GitHub Release（`<package-id>-v<version>.lpk`）；
4. 发布官方平台与喵喵商店。

## 所需 Secrets

| Secret | 说明 |
| --- | --- |
| `LZC_API_TOKEN` | 懒猫开放平台 PAT（官方商店发布） |
| `APPSTORE_URL` / `APPSTORE_TOKEN` | 喵喵商店 API 地址与发布令牌 |
| `PRIVATE_STORE_GROUP_CODES` | 可选，私有分组码 |
