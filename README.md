# FluxDown for LazyCat

[FluxDown](https://github.com/zerx-lab/FluxDown) 的懒猫微服 LPK v2 适配项目，提供 Web UI、REST API 与 WebSocket 实时推送。

## 当前版本

- 软件包 ID：`community.lazycat.app.fluxdown`
- LazyCat 包版本：`0.3.1`
- 运行镜像：`ghcr.1ms.run/zerx-lab/fluxdown-server:0.3.1`
- 运行架构：`linux/amd64`
- 数据库：内置 SQLite，不使用外部数据库

## 部署参数

- `token`：FluxDown 访问密钥，8-128 位 ASCII 字符，必须同时包含字母和数字。
- `lang`：默认界面语言，支持 `zh` 和 `en`。
- `download_dir_name`：懒猫用户主目录下的下载文件夹名称，默认 `Downloads`。

部署前会验证访问密钥和下载目录名称；不合规配置会阻止容器启动。部署后，访问密钥会自动填入 `/login` 页面并登录，同时取消“记住设备”，只保存到浏览器会话。应用支持多实例；每个实例拥有独立的 SQLite 数据目录。

## 数据目录

- `/lzcapp/var/data` → 容器 `/data`：数据库、日志与服务配置。
- `/lzcapp/run/mnt/home` → 容器 `/mnt/lazycat-home`：启动脚本验证目录名称后，将选定目录链接到 `/root/Downloads`。

## 构建说明

当前运行镜像使用 `ghcr.1ms.run` mirror 地址，请通过本仓库的 LazyCat GitHub Action 构建 LPK。`lzc-cli 2.0.9 project release` 会按官方商店规则拒绝非 `registry.lazycat.cloud` 镜像，不能用于此私有商店配置。

## 自动发布

- 每日稳定版检查：只接受 `X.Y.Z` 形式的正式镜像标签；发现新版本后通过 `ghcr.1ms.run` 镜像加速地址更新 LPK、创建 GitHub Release，并仅同步到喵喵商店。

GitHub Actions 需要以下 Secrets：

- `APPSTORE_URL`
- `APPSTORE_TOKEN`
- `APP_ID`（可选）
- `PRIVATE_STORE_GROUP_CODES`（可选）
