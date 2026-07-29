# FluxDown for LazyCat

[FluxDown](https://github.com/zerx-lab/FluxDown) 的懒猫微服 LPK v2 适配项目，提供 Web UI、REST API 与 WebSocket 实时推送。

## 当前版本

- 软件包 ID：`community.lazycat.app.fluxdown`
- LazyCat 包版本：`0.2.4`
- 运行镜像：`ghcr.io/zerx-lab/fluxdown-server:0.2.5-rc.5`
- 运行架构：`linux/amd64`
- 数据库：内置 SQLite，不使用外部数据库

之所以使用纯数字包版本 `0.2.4`，是因为 LazyCat 包版本不使用预发布后缀。上游正式发布 `0.2.5` 后，自动化会切换到正式镜像并升级 LPK 到 `0.2.5`。

## 部署参数

- `token`：FluxDown 访问密钥，8-128 位 ASCII 字符，必须同时包含字母和数字。
- `lang`：默认界面语言，支持 `zh` 和 `en`。
- `download_dir_name`：懒猫用户主目录下的下载文件夹名称，默认 `Downloads`。

部署前会验证访问密钥和下载目录名称；不合规配置会阻止容器启动。部署后，访问密钥会自动填入 `/login` 页面并登录，同时取消“记住设备”，只保存到浏览器会话。应用支持多实例；每个实例拥有独立的 SQLite 数据目录。

## 数据目录

- `/lzcapp/var/data` → 容器 `/data`：数据库、日志与服务配置。
- `/lzcapp/run/mnt/home` → 容器 `/mnt/lazycat-home`：启动脚本验证目录名称后，将选定目录链接到 `/root/Downloads`。

## 本地构建

```bash
lzc-cli project release -o dist/fluxdown.lpk
lzc-cli lpk info dist/fluxdown.lpk
```

## 自动发布

- 每日稳定版检查：只接受 `X.Y.Z` 形式的正式镜像标签；发现新版本后复制镜像、更新 LPK、创建 GitHub Release，并同步双商店。

GitHub Actions 需要以下 Secrets：

- `LAZYCAT_TOKEN`
- `APPSTORE_URL`
- `APPSTORE_TOKEN`
- `APP_ID`（可选）
- `PRIVATE_STORE_GROUP_CODES`（可选）
