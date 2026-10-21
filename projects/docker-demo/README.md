# docker-demo —— Docker 容器化初探（Task 06）

最小可运行示例：把一段 Python 脚本构建成镜像并在容器中运行。

## 复现步骤

> **前置**：Docker 29.1.3 已安装并运行（本机走「Linux 原生」路线，装在 WSL2 Ubuntu 22.04 内，
> 不是 Docker Desktop）。**必须先进入 Linux 环境再执行 docker 命令**：
>
> ```powershell
> wsl -d Ubuntu                     # 提示符变成 student@localhost:~$ 才算进来了
> cd /mnt/c/Users/20255/WorkBuddy/2026-10-13-22-52-42/ai-lab-general/projects/docker-demo
> ```
>
> 在 Windows PowerShell 里直接敲 `docker` 会报「无法将 docker 项识别为 cmdlet」——因为 Linux 内的 docker
> 没有转发到 Windows 侧。

```bash
# 1. 构建镜像（-t 指定 名称:标签，注意最后那个点表示构建上下文为当前目录）
docker build -t ailab-demo:1.0 .

# 2. 查看本地镜像列表
docker images

# 3. 运行容器（--rm 退出后自动删除容器，保持环境干净）
docker run --rm ailab-demo:1.0

# 4. 挂载宿主目录并运行命令 —— 验证「环境隔离 + 目录共享」
docker run -it --rm -v "$(pwd)":/app -w /app python:3.10-slim python -c "print('Hello AI-Lab Docker')"
#    Windows PowerShell 请把 $(pwd) 换成 ${PWD}，CMD 换成 %cd%
```

## 真实输出（本机 2026-10-20 15:34 实测）

```
================================================
Hello AI-Lab Docker
================================================
运行时刻   : 2026-10-20 15:34:21
Python 版本: 3.10.22
解释器路径 : /usr/local/bin/python
操作系统   : Linux 6.18.54.1-microsoft-standard-WSL2
主机名     : 71d2140444e
================================================
```

注意两处细节：

1. **`操作系统: Linux 6.18.54.1-microsoft-standard-WSL2`** —— 这个内核版本就是**宿主机自己的内核**。
   容器**不携带独立内核**，而是与宿主共享内核，只是通过 namespace 与 cgroup 获得隔离视图。
   这正是容器比虚拟机轻量（镜像 186MB、秒级启动）的根本原因。若换成虚拟机方案，这里会显示完全不同的内核版本。
2. **`主机名: 71d2140444e`** —— 这是容器 ID 的前 12 位，与宿主机 `localhost` 不同，
   说明进程确实运行在一个独立的隔离环境中。

截图见 `assets/screenshots/task06_docker_run.png`。

## 核心概念对照

| 概念 | 类比 | 要点 |
|---|---|---|
| 镜像 Image | 类 / 安装盘 ISO | 只读模板，分层存储，可复用 |
| 容器 Container | 实例 / 运行中的进程组 | 镜像的运行态，叠加可写层，退出即销毁（未持久化时） |
| Dockerfile | 源码 / 配方 | 描述如何构建镜像的文本指令集 |
| Registry | 应用商店 | 镜像仓库，`docker pull` 从中拉取 |

## 踩坑记录

| 现象 | 原因 | 解决 |
|---|---|---|
| `docker` 报「无法将 docker 项识别为 cmdlet」 | 在 Windows PowerShell 里，而 docker 装在 WSL 内 | 先 `wsl -d Ubuntu` 进 Linux；判据看提示符 `PS C:\>` vs `student@localhost:~$` |
| `docker build` 报 `Cannot connect to the Docker daemon` | 守护进程未启动 | `systemctl start docker`（无 systemd 时 `service docker start`） |
| `docker pull` 卡住 / `context deadline exceeded` | Docker Hub 直连不可达 | 在 `/etc/docker/daemon.json` 配 `registry-mirrors` 后重启 docker |
| `-v $(pwd):/app` 在 PowerShell 下路径变成空 | PowerShell 不解析 `$(pwd)` | 用 `${PWD}`；CMD 用 `%cd%` |
| 多行命令粘贴后拼成一条，报 `Unable to find image 'ailab-demo:1.0docker'` | 终端粘贴丢失换行 | 一次粘一行，或用 `;` 串联 |
| 构建时反复重装依赖，缓存失效 | `COPY . .` 放在 `pip install` 之前 | 先拷 `requirements.txt` 并安装，再拷源码（本 Dockerfile 已按此规范） |
