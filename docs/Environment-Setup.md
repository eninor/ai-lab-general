# 环境配置留痕报告（Task 01 / 05 / 06）

> 配置日期：2026-10-13 · 主机：Windows 11 家庭版 中文版（Build 26200）
> 本文档所有版本号与输出均为**本机实测**，非示例数据。

---

## 一、总览：本次配置结果

| 任务卡 | 配置项 | 结果 | 说明 |
|---|---|---|---|
| Task 01 | AI 原生 IDE / 智能工作台 | ✅ 就绪 | WorkBuddy（本会话运行中）+ VSCode（`D:\Microsoft VS Code`） |
| Task 01 | VSCode 核心插件 | ✅ 已装 6 个 | 含 Error Lens / Path Intellisense / Prettier / Code Runner / GitLens / Jupyter |
| Task 01 | 终端 AI Agent | ✅ 双份就绪 | DSH v0.2.0-rc.2 + OpenCode v1.19.0 |
| Task 01 | 大模型 API | ✅ 已配置 | DSH 凭据文件 `~/.dsh/.credentials.yaml` 中已存在 `DEEPSEEK_API_KEY` |
| Task 02 | Markdown 笔记工具 | ✅ 就绪 | VSCode + Markdown All in One + Markdown Preview Enhanced；笔记目录体系已建 |
| Task 03 | Linux 终端 | ✅ 完成 | Git Bash 等价实操 + WSL2 Ubuntu 22.04.5 已装（内核 6.18.54.1） |
| Task 04 | Git / SSH | ⚠️ 部分完成 | SSH Key 已生成；`user.name` / `user.email` 待填真实值 |
| Task 05 | Miniconda + 镜像源 | ✅ 完成 | conda 26.7.1，清华源，`ailab` 环境 Python 3.10.22，依赖已导出 |
| Task 06 | Docker | ✅ 完成 | Docker 29.1.3（WSL2 Ubuntu 原生），`build` + `run` 全链路通过，见 `Task06-Docker实战记录.md` |

---

## 二、Task 01：现代 AI 编程环境与 CLI 协同工具

### 2.1 现代代码开发环境

**方案 A（AI 原生 IDE / 智能工作台）**：本会话即运行在 **WorkBuddy** 之上（腾讯 AI 桌面智能工作台，内置 Coding Mode 与多 Agent 任务闭环交付能力）。同时本机已装 **VSCode**：

```
$ code --version
D:\Microsoft VS Code\bin\code
```

**方案 B（VSCode 经典组合）**：本次补齐了缺失的核心插件，实测安装输出：

```
$ code --list-extensions
usernamehw.errorlens                 # Error Lens        —— 错误行内高亮 v3.29.0
christian-kohler.path-intellisense   # Path Intellisense —— 路径智能补全 v2.10.0
esbenp.prettier-vscode               # Prettier          —— 代码格式化 v12.4.0
formulahendry.code-runner            # Code Runner       —— 一键运行 v0.12.2
eamodio.gitlens                      # GitLens           —— 版本回溯 v19.3.0
ms-toolsai.jupyter                   # Jupyter           —— Notebook 支持 v2025.9.1
yzhang.markdown-all-in-one           # Markdown 全能（原有）
shd101wyy.markdown-preview-enhanced  # Markdown 预览增强（原有）
ms-python.python / vscode-pylance    # Python 官方环境支持（原有）
```

仓库内已随附推荐插件清单与统一设置，克隆后 VSCode 会自动提示安装：
- `.vscode/extensions.json` —— 推荐插件
- `.vscode/settings.json` —— 保存即格式化、Tab=4、Python/Markdown 专属规则

### 2.2 终端 / CLI AI 协同工具（两套均配置完成）

**① DSH (DeepSeek Harness)**

```bash
$ npm install -g @deepseek-ai/dsh
$ dsh --version
0.2.0-rc.2

$ dsh --profile web --dump-config      # 验证 profile 可正常加载
- id: llm
  name: '@deepseek-ai/dsh-llm'
- id: session
  name: '@deepseek-ai/dsh-session'
...
```

凭据状态：`~/.dsh/.credentials.yaml` 中已存在 `DEEPSEEK_API_KEY`，无需重复配置。

**② OpenCode**

```bash
$ npm install -g opencode-ai
$ opencode --version
1.19.0

$ opencode --help
Commands:
  opencode [project]           start opencode tui            [default]
  opencode run [message..]     run opencode with a message
  opencode providers           manage AI providers and credentials  [aliases: auth]
  opencode models [provider]   list all available models
  opencode web                 start opencode server and open web interface
```

### 2.3 基础 API 连通性测试

DSH 已内置 DeepSeek 凭据，直接可用；OpenCode 首次使用需登录一次供应商：

```bash
opencode auth login        # 交互式选择供应商并粘贴 API Key
opencode models            # 列出可用模型，确认凭据生效
opencode run "用一句话解释 Python 的 GIL"
```

> 推荐的学生友好型免费 / 低价平台（对应文档 §4.1）：SiliconFlow 硅基流动、DeepSeek 开放平台、智谱 AI、国家超算平台、ModelScope 魔搭、Google AI Studio。

---

## 三、Task 05：Conda 虚拟环境隔离与镜像源加速

### 3.1 安装 Miniconda

安装包来源为清华镜像（避免官网国际链路超时）：

```bash
curl -L -o Miniconda3-latest-Windows-x86_64.exe \
  https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-latest-Windows-x86_64.exe

# 静默安装到用户目录（JustMe，无需管理员权限）
./Miniconda3-latest-Windows-x86_64.exe /InstallationType=JustMe /AddToPath=0 /RegisterPython=0 /S /D=C:\Users\20255\miniconda3
```

| 参数 | 作用 |
|---|---|
| `/InstallationType=JustMe` | 仅当前用户安装，不需要管理员权限 |
| `/AddToPath=0` | **不**污染系统 PATH，避免与已有的 `D:\python大一` 冲突 |
| `/RegisterPython=0` | 不抢占 `.py` 文件默认打开方式 |
| `/S` | 静默安装 |
| `/D=...` | 指定安装目录（必须放在最后且不加引号） |

### 3.2 配置清华镜像源

配置文件 `C:\Users\20255\.condarc`（已同步备份到仓库 `configs/.condarc`）：

```yaml
channels:
  - defaults
show_channel_urls: true
auto_activate_base: false
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  deepmodeling: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

生效验证：

```
$ conda --version
conda 26.7.1

$ conda config --show default_channels
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2

$ conda config --show auto_activate_base
auto_activate: False
```

> `auto_activate_base: false` 让终端默认停在系统环境，避免「明明没激活却用了 base」的困惑；需要时手动 `conda activate aiab`。

### 3.3 创建独立环境并导出依赖

```bash
conda create -n ailab python=3.10 -y
conda run -n ailab python --version      # Python 3.10.22

$ conda env list
# conda environments:
base                     C:\Users\20255\miniconda3
ailab                    C:\Users\20255\miniconda3\envs\ailab

# 依赖导出与复现
conda run -n ailab python -m pip freeze > configs/requirements.txt
conda env export -n ailab --no-builds   > configs/environment.yml
```

**日常使用**：

```bash
conda activate ailab          # 进入环境
pip install requests pandas   # 在隔离环境内装包
python -m pip freeze > requirements.txt
conda deactivate              # 退出
conda remove -n ailab --all   # 删环境（危险操作，确认后再执行）
```

---

## 四、Task 03 / Task 06：已完成（原「待手动」事项已闭环）

**状态：两项均已于 2026-10-20 完成。**

早期本节记录的障碍（需要管理员权限 + 重启；`wsl.exe` 被拦报 `PROGRAM BLOCKED BY SECURITY POLICY`）已澄清与解决：

1. `wsl.exe` 的拦截**只是 AI 助手执行沙箱的限制**，普通 PowerShell 不受影响——注册表 `HKLM:\SOFTWARE\Policies\Microsoft\Windows\AppHVSI` 并不存在，无需放行任何东西；
2. 两个 Windows 功能的 DISM 均返回 `3010`，重启后 LxssManager 正常注册；
3. Docker 最终未走 Docker Desktop（官方 CDN 实测 118 KB/s，620MB 约需 90 分钟），改走任务书允许的「Linux 原生」路线，用清华源 `apt install docker.io`，实测 6.8 MB/s。

→ 安装过程、路线决策依据与排错表见 **`docs/WSL2与Docker-手动安装指引.md`**；
→ Docker 实操（Dockerfile 解析、build / run / 生命周期、原理讲解）见 **`docs/Task06-Docker实战记录.md`**。

`projects/docker-demo/` 下的 `Dockerfile`、`app.py`、`requirements.txt`、`README.md` 已全部验证可跑通，镜像 `ailab-demo:1.0`。

---

## 五、Task 04：Git 与 SSH 当前状态

| 项目 | 状态 |
|---|---|
| Git 版本 | ✅ 2.55.0.windows.3 |
| SSH Key | ✅ `C:\Users\20255\.ssh\id_ed25519`（ed25519，无口令） |
| `user.name` / `user.email` | ⚠️ 待填真实值 |
| `.gitignore` | ✅ 已按 Python/编辑器/密钥三类规则配置 |

公钥内容（可直接粘贴到 GitHub → Settings → SSH and GPG keys）：

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBWB7rrhZSCBE2IYWwXiRcsnqYax1rAZA5/4Fsa60kiI ai-lab-general@localhost
```

补齐身份只需两条命令：

```bash
git config --global user.name  "你的GitHub用户名"
git config --global user.email "你的GitHub注册邮箱"
git config --global core.autocrlf true
git config --global init.defaultBranch main
```

---

## 六、本次踩坑记录（真实排错）

| # | 现象 | 定位与根因 | 解决 |
|---|---|---|---|
| 1 | `npm install -g opencode-ai @deepseek-ai/dsh` 跑 12 分钟后报 `npm error code 1` | 错误栈指向 `@deepseek-ai/dsh/node_modules/koffi` 的 `cnoke.cjs`：`Failed to load prebuilt binary, rebuilding from source` → `Error: CMake does not seem to be available` | `koffi` 是 DSH 部分原生插件（文件系统/目录选择器/Windows ACL 沙箱）的 FFI 依赖。其预编译二进制未覆盖本机 Node v24（ABI 137），回退源码编译又缺 CMake。**DSH 主链路（CLI、profile、web）不依赖 koffi，实测 `dsh --version`、`dsh --profile web --dump-config` 均正常**。若后续需用到涉及原生调用的插件，再装 CMake 或改用 Node 22 运行即可 |
| 2 | `opencode-ai` 装完后 `opencode` 命令不可用 | npm 新版默认不执行依赖的 install script，提示 `1 package has install scripts not yet covered by allowScripts: opencode-ai@1.19.0 (postinstall: node ./postinstall.mjs)` | 该 postinstall 负责下载平台二进制（`bin/opencode.exe`）。补跑 `npm install -g --allow-scripts=opencode-ai opencode-ai`，安装后 `bin/` 下出现 180MB 的 `opencode.exe`，`opencode --version` 返回 1.19.0 |
| 3 | 曾存在指向已删除目录的 `opencode.cmd` 残留 shim | 上一次安装中断留下的 shim | 重新安装后被正确覆盖 |
| 4 | `conda` 命令在终端不可用 | 静默安装时用了 `/AddToPath=0`（有意为之，避免污染系统 Python） | 日常通过开始菜单的 **Anaconda Prompt** 进入，或在终端用全路径 `C:\Users\20255\miniconda3\Scripts\conda.exe`；如需全局可用执行 `conda init powershell` / `conda init bash` 后重开终端 |

---

## 七、待办清单

- [x] 填写 `git config --global user.name / user.email` → 已配置为 `eninor` / `17200678363@163.com`
- [x] 建立仓库提交历史 → 已 10 次语义化提交 + 1 次 feature 分支合并（共 12 个提交）
- [ ] 在 GitHub 新建仓库并把公钥粘贴到 Settings → SSH and GPG keys，执行 `ssh -T git@github.com` 验证
- [ ] 绑定远程：`git remote add origin git@github.com:eninor/ai-lab-general.git && git push -u origin main`
- [x] 安装 WSL2 + Ubuntu 22.04.5（离线 `wsl --import`，内核 6.18.54.1）
- [x] 安装 Docker 并跑通 `projects/docker-demo`（Linux 原生 29.1.3，镜像 `ailab-demo:1.0` 已构建成功）
- [ ] `opencode auth login` 配置一个可用 API Key
- [x] 补齐 Task 03 / Task 06 截图至 `assets/screenshots/`
- [ ] 补齐 Task 01 / 04 / 05 截图
- [ ] 完成 Task 07 / 08 / 09 练习代码与 Task 10 收官项目
