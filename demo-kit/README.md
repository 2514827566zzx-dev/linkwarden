# Linkwarden 可落地 Demo

这是一个本机或服务器回环地址可运行的 Linkwarden 演示部署包，包含 Linkwarden、PostgreSQL 和 Meilisearch。它使用官方镜像，不重复打包上游仓库的全部源码或镜像层。上游：[linkwarden/linkwarden](https://github.com/linkwarden/linkwarden)；你的 Fork：[2514827566zzx-dev/linkwarden](https://github.com/2514827566zzx-dev/linkwarden)。

## 启动

需要 Docker Engine/Desktop、Docker Compose v2+、Python 3 和可下载镜像的网络。建议至少 2 核、4 GB 内存；首次镜像下载约需数 GB 空间。

1. 解压后在本目录执行 `python3 start.py`。脚本生成仅保存在本机的 `.env` 密钥，校验 Compose 配置，拉取固定版本镜像并启动容器。
2. 等待一两分钟，执行 `python3 verify.py`，预期显示 `HTTP 200`。
3. 打开 `http://127.0.0.1:3101`，注册第一个账号。按 `examples/try-these-links.txt` 添加一个公开链接，确认页面出现标题、标签以及可查看的存档。

镜像：Linkwarden `v2.16.3`、PostgreSQL `16-alpine`、Meilisearch `v1.13.3`。数据保存在三个 Docker 命名卷里，`docker compose down` 不会删除它们。查看日志：`docker compose logs -f linkwarden`；停止：`docker compose down`。

## 部署边界

默认只监听 `127.0.0.1:3101`，不会直接暴露到公网。在远程服务器上可用 SSH 端口转发访问：`ssh -L 3101:127.0.0.1:3101 用户@服务器`。若改成公网服务，需要 HTTPS、与域名一致的 `NEXTAUTH_URL`、用户注册策略和数据备份；不要只把 `BIND_HOST` 改成 `0.0.0.0` 就上线。

本包验证的是单机演示路径：创建账号 → 收藏公开网页 → 查看存档。网页抓取依赖目标站可访问，个别站点可能阻止归档。未接入邮件、对象存储或外部 AI 标签服务。

参考：[官方安装文档](https://docs.linkwarden.app/self-hosting/setup) 和 [v2.16.3 官方 Compose](https://github.com/linkwarden/linkwarden/blob/v2.16.3/docker-compose.yml)。
