# WSL2 与 Docker 手动安装指引（Task 03 / Task 06 补充）

> **为什么这两项需要你手动做？**
> 1. 二者都需要**管理员权限**并**重启系统**，自动化流程无法代劳；
> 2. 本机 `wsl.exe` 被安全策略列入程序黑名单，直接调用会报
>    `PROGRAM BLOCKED BY SECURITY POLICY`，需先放行。
>
> 下面每一步都可直接复制执行，按顺序做完即可闭环 Task 03 与 Task 06。

---

## 零、先放行 wsl.exe（否则后续命令全部被拦）

图形界面路径：**Windows 安全中心 → 命令安全 → 程序黑名单 → 移除 `C:\Windows\System32\wsl.exe`**

或管理员 PowerShell 检查（仅查看，不改）：

```powershell
Get-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\AppHVSI" -ErrorAction SilentlyContinue
```

放行后**重开一个终端**再继续。

---

## 一、安装 WSL2（Task 03）

### 步骤 1：一条命令装齐（推荐，Windows 11 支持）

以**管理员身份**打开 PowerShell（开始菜单搜索 `PowerShell` → 右键 → 以管理员身份运行）：

```powershell
wsl --install -d Ubuntu-22.04
```

该命令会自动完成：启用「适用于 Linux 的 Windows 子系统」+「虚拟机平台」两个功能 → 下载安装内核 → 安装 Ubuntu 22.04。

### 步骤 2：重启电脑

```powershell
Restart-Computer
```

### 步骤 3：首次启动 Ubuntu，设置用户名与密码

重启后开始菜单会出现 **Ubuntu**，首次启动会让你设置 Linux 用户名（小写字母）和密码（**输入时不显示字符是正常的**）。

### 步骤 4：确认版本为 2

```powershell
wsl -l -v
# 应显示：  NAME            STATE           VERSION
#          Ubuntu-22.04    Running         2
```

若不是 2：

```powershell
wsl --set-version Ubuntu-22.04 2
wsl --set-default-version 2
```

### 分步安装（若 `wsl --install` 不可用）

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
# 重启后：
wsl --set-default-version 2
wsl --install -d Ubuntu-22.04
```

### 验证（在 Ubuntu 终端内执行）

```bash
uname -a                 # 应看到 microsoft-standard-WSL2
lsb_release -a           # 应显示 Ubuntu 22.04
cat /etc/os-release
df -h                    # 观察 Linux 的文件系统视图，与 Windows 完全不同
```

> **交付留痕**：把 `uname -a` 与 `lsb_release -a` 的输出截图，存为
> `assets/screenshots/task03_wsl2.png`。

---

## 二、安装 Docker（Task 06）

### 前置条件

Task 06 的推荐路径是 **Docker Desktop + WSL2 后端**，所以请先完成上面第一步。

### 步骤 1：安装 Docker Desktop

方式 A —— winget（Windows 11 自带，最省事）：

```powershell
winget install --id Docker.DockerDesktop -e --accept-source-agreements --accept-package-agreements
```

方式 B —— 官网下载安装包：

```text
https://www.docker.com/products/docker-desktop/
```

下载 `Docker Desktop Installer.exe` 后双击，安装选项中**务必勾选**：
- ☑ Use WSL 2 instead of Hyper-V (recommended)
- ☑ Add shortcut to desktop

### 步骤 2：首次启动与配置镜像加速

安装完成后启动 Docker Desktop，接受服务条款。任务栏出现鲸鱼图标且为**绿色 Engine running** 即就绪。

国内网络下建议配置镜像加速：**Settings → Docker Engine**，在 JSON 中加入：

```json
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io",
    "https://dockerproxy.com",
    "https://mirror.baidubce.com"
  ]
}
```

点击 **Apply & Restart**。

> 提示：镜像源可用性会随时间变化，若 `docker pull` 报 `context deadline exceeded`，
> 换一个 mirror 或直接使用能访问的官方源。

### 步骤 3：验证安装

在**普通** PowerShell 或 Git Bash 中执行：

```bash
docker --version
docker run hello-world
```

`hello-world` 输出 `Hello from Docker!` 即表示镜像拉取 + 容器运行链路全通。

### 步骤 4：完成 Task 06 实操

```bash
cd projects/docker-demo
docker build -t ailab-demo:1.0 .
docker images
docker run --rm ailab-demo:1.0
docker run -it --rm -v "${PWD}":/app -w /app python:3.10-slim python -c "print('Hello AI-Lab Docker')"
```

> **交付留痕**：把 `docker build` 的完整输出 + `docker run` 的运行结果截图，存为
> `assets/screenshots/task06_docker.png`。

---

## 三、常见坑速查

| 现象 | 原因 | 解决 |
|---|---|---|
| `wsl --install` 报 `PROGRAM BLOCKED BY SECURITY POLICY` | 安全策略黑名单 | 见本文「零」章节先放行 |
| `WslRegisterDistribution failed with error: 0x80370102` | BIOS 未开启虚拟化 | 进 BIOS 开启 Intel VT-x / AMD-V，或在「启用或关闭 Windows 功能」勾选「虚拟机平台」 |
| Docker Desktop 启动后一直转圈 | WSL2 未就绪或内核过旧 | 执行 `wsl --update` 后重启 Docker Desktop |
| `docker build` 报 `Cannot connect to the Docker daemon` | Docker Desktop 未启动 | 启动桌面端，等鲸鱼图标变绿 |
| `docker pull` 卡住 / 超时 | 国内直连 Docker Hub 受限 | 配置 `registry-mirrors`（见上） |
| `-v $(pwd):/app` 在 PowerShell 下为空 | PowerShell 不解析 `$(pwd)` | 用 `${PWD}`；CMD 用 `%cd%` |
| Docker Desktop 占用 C 盘空间大 | 镜像与容器默认存放在 C 盘 | Settings → Resources → Disk image location 改到 D 盘 |

---

## 四、这两项做完后，Task 03 / Task 06 的交付物清单

| 任务 | 交付物 | 存放位置 |
|---|---|---|
| Task 03 | WSL2 环境验证截图、Linux 命令实操记录 | `docs/Linux-CLI-实战记录.md`、`assets/screenshots/task03_wsl2.png` |
| Task 06 | Dockerfile 源码、构建与运行日志截图 | `projects/docker-demo/`、`assets/screenshots/task06_docker.png` |
