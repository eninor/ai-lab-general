# docker-demo —— Docker 容器化初探（Task 06）

最小可运行示例：把一段 Python 脚本构建成镜像并在容器中运行。

## 复现步骤

> 前置：已安装 Docker Desktop 并启动（任务栏鲸鱼图标为绿色「Engine running」）。

```bash
cd projects/docker-demo

# 1. 构建镜像（-t 指定 名称:标签，注意最后那个点表示构建上下文为当前目录）
docker build -t ailab-demo:1.0 .

# 2. 查看本地镜像列表
docker images

# 3. 运行容器（--rm 退出后自动删除容器，保持环境干净）
docker run --rm ailab-demo:1.0

# 4. 挂载宿主目录并进入交互式容器 —— 验证「环境隔离 + 目录共享」
#    PowerShell / Git Bash：
docker run -it --rm -v "$(pwd)":/app -w /app python:3.10-slim python -c "print('Hello AI-Lab Docker')"
#    Windows CMD 请把 $(pwd) 换成 %cd%
```

## 预期输出

```
================================================
Hello AI-Lab Docker
================================================
运行时刻   : 2026-10-13 23:30:00
Python 版本: 3.10.18
解释器路径 : /usr/local/bin/python
操作系统   : Linux 6.6.87.2-microsoft-standard-WSL2
主机名     : 8f3c1a2b9d47
================================================
```

注意 `操作系统: Linux`——脚本在 Windows 上运行却报告 Linux，这正是容器「自带内核视图的隔离环境」最直观的体现。

## 核心概念对照

| 概念 | 类比 | 要点 |
|---|---|---|
| 镜像 Image | 类 / 安装盘 ISO | 只读模板，分层存储，可复用 |
| 容器 Container | 实例 / 运行中的系统 | 镜像的运行态，可读写，退出即销毁（未持久化时） |
| Dockerfile | 源码 / 配方 | 描述如何构建镜像的文本指令集 |
| Registry | 应用商店 | 镜像仓库，`docker pull` 从中拉取 |

## 踩坑记录

| 现象 | 原因 | 解决 |
|---|---|---|
| `docker build` 报 `Cannot connect to the Docker daemon` | Docker Desktop 未启动 | 启动 Docker Desktop，等待引擎就绪 |
| `-v $(pwd):/app` 在 PowerShell 下路径变成空 | PowerShell 不解析 `$(pwd)` | 用 `${PWD}`；CMD 用 `%cd%` |
| 构建时反复重装依赖，缓存失效 | `COPY . .` 放在 `pip install` 之前 | 先拷 `requirements.txt` 并安装，再拷源码（本 Dockerfile 已按此规范） |
