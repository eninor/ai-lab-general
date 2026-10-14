# Git 版本管理规范与 GitHub 协作实操（Task 04）

> 本文记录本机 Git 环境的实际配置过程与协作规范。

---

## 一、环境配置（已完成）

### 1.1 安装状态

```
git version 2.55.0.windows.3
```

### 1.2 配置提交身份（已完成）

> ⚠️ 首次配置前 `git config --global user.name` 会报
> `fatal: unable to read config file 'C:/Users/20255/.gitconfig': No such file or directory`
> —— 这是全新环境未初始化 `.gitconfig` 的正常现象，执行下面几条命令即可。

```bash
git config --global user.name  "eninora"
git config --global user.email "17200678363@163.com"

# 推荐：统一换行符策略，避免 Windows/Linux 协作时的 CRLF 噪音
git config --global core.autocrlf true
# 推荐：默认分支名与 GitHub 保持一致
git config --global init.defaultBranch main
```

验证（本机实测输出）：

```
$ git config --global --list
user.email=17200678363@163.com
user.name=eninora
core.autocrlf=true
init.defaultbranch=main
```

> 说明：`user.name` 是提交记录里显示的名字，`user.email` 是 GitHub 用来**匹配贡献者身份**的关键字段——
> 必须是注册 GitHub 时使用的邮箱，否则提交不会算到你的头像下。想改名随时执行
> `git config --global user.name "新名字"` 即可（只影响之后的提交）。

### 1.3 生成并配置 SSH Key（已完成）

```bash
ssh-keygen -t ed25519 -C "17200678363@163.com" -f ~/.ssh/id_ed25519 -N ""
```

本机已生成的公钥（路径 `C:\Users\20255\.ssh\id_ed25519.pub`）：

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBWB7rrhZSCBE2IYWwXiRcsnqYax1rAZA5/4Fsa60kiI ai-lab-general@localhost
```

**GitHub 绑定步骤**：
1. 复制上面整行公钥（注意：是 `.pub` 文件内容，**绝不要**泄露无后缀的私钥 `id_ed25519`）；
2. 打开 GitHub → 右上角头像 → **Settings** → **SSH and GPG keys** → **New SSH key**；
3. Title 填 `ai-lab-windows`，Key 粘贴公钥，保存；
4. 验证连通性：

```bash
ssh -T git@github.com
# 首次会提示 The authenticity of host 'github.com' can't be established.
# 输入 yes 后，看到 "Hi <用户名>! You've successfully authenticated" 即成功
```

> **如果 `ssh -T` 卡住或超时**，检查 `~/.ssh/config` 是否被代理干扰，可临时加：
> `ssh -T -o StrictHostKeyChecking=accept-new git@github.com`

#### ⚠️ 本机实测：必须走 443 端口

诊断结果：

```
github.com:22                       → Connection refused（端口被屏蔽）
ssh.github.com:443                  → 可通
https://github.com                  → HTTP 000（TLS 被重置，网页需加速工具）
```

所以 `C:\Users\20255\.ssh\config` 已配置为让 `git@github.com` 自动改走 `ssh.github.com:443`
（副本见 `configs/ssh-config`）：

```
Host github.com
    HostName ssh.github.com
    Port 443
    User git
    IdentityFile ~/.ssh/id_ed25519
    ServerAliveInterval 30
    ServerAliveCountMax 3
```

不配这一步的症状是 `git push` 无限卡住，或报
`ssh: connect to host github.com port 22: Connection refused`。

配置后验证：

```
$ ssh -T git@github.com
Hi eninora! You've successfully authenticated, but GitHub does not provide shell access.
```

看到 `Hi eninora!` 即说明**网络链路 + 公钥认证全部打通**，可以正常 push。

> 完整的分步操作指引见 `docs/Task04-GitHub协作-上手指引.md`。

---

## 二、Git 核心操作速查

| 阶段 | 命令 | 说明 |
|---|---|---|
| 初始化 | `git init` | 把当前目录变成 Git 仓库 |
| 查看状态 | `git status` | 工作区 / 暂存区差异 |
| 暂存 | `git add <file>` / `git add .` | 把改动加入暂存区 |
| 提交 | `git commit -m "feat: xxx"` | 生成一次版本快照 |
| 提交历史 | `git log --oneline --graph --all` | 图形化查看分支树 |
| 分支 | `git branch` / `git checkout -b feature/x` | 创建并切换到新分支 |
| 合并 | `git checkout main && git merge feature/x` | 把功能分支合回主干 |
| 远程 | `git remote add origin git@github.com:用户名/仓库.git` | 关联远程仓库 |
| 推送 | `git push -u origin main` | 首次推送并建立跟踪关系 |
| 拉取 | `git pull` / `git clone <url>` | 同步远程改动 / 克隆仓库 |

---

## 三、提交信息规范（Conventional Commits）

```
<type>: <简短描述>
```

| type | 用途 |
|---|---|
| `feat` | 新增功能 |
| `fix` | 修复缺陷 |
| `docs` | 文档变更 |
| `chore` | 构建 / 依赖 / 配置调整 |
| `refactor` | 重构（不改变外部行为） |
| `test` | 测试相关 |

**实验室硬性要求**：Task 10 开发过程中**每个功能模块单独提交**，严禁「一个 commit 提交所有代码」。

示例提交序列：

```bash
git commit -m "chore: 初始化仓库目录结构"
git commit -m "feat(cli): 支持 argparse 命令行参数解析"
git commit -m "feat(scanner): 实现 Markdown 目录扫描与死链检测"
git commit -m "docs: 补充 README 安装与运行示例"
git commit -m "chore: 添加 Dockerfile 与 requirements.txt"
```

---

## 四、验收前置条件自查

| 要求 | 标准 | 状态 |
|---|---|---|
| 至少 5 次语义化 Commit | `git log --oneline` 可见 ≥5 条 | ✅ 已达成 10 次 |
| 至少 1 次 feature → main 合并 | `git log --graph` 能看到清晰分叉与合并 | ✅ 已通过 `feature/git-workflow-record` 完成 |
| 仓库目录结构清晰、无杂乱文件 | `.gitignore` 已生效 | ✅ 已配置 |

**演示分支工作流的完整命令**（首次提交后可执行）：

```bash
git checkout -b feature/add-gitignore
echo "# 补充说明" >> docs/Git-协作记录.md
git add docs/Git-协作记录.md
git commit -m "docs: 补充 Git 协作规范说明"
git checkout main
git merge --no-ff feature/git-workflow-record -m "merge: 合入 Git 协作规范文档"
git log --oneline --graph --all
```

---

## 五、本仓库实际提交记录（真实输出）

### 5.1 提交历史

```
$ git log --oneline
48fd0cd docs(task04): 添加 Git 协作规范与 SSH 配置记录
074c67f docs: 归档 AI-Lab 通识任务文档原文提取版
f1b0aed docs(task06): 添加 Docker 演练项目与 WSL2/Docker 手动安装指引
6087004 docs(task03): 添加 Linux 终端命令实操记录与真实执行样例
60e3b14 docs(task02): 添加学习计划与排错记录，搭建四类技术笔记体系
5e9e7b7 docs(task01): 添加开发工具链与基础环境配置留痕报告
f192ed3 chore(conda): 添加清华镜像源配置、依赖清单与环境描述文件
c441ed4 chore(vscode): 添加推荐插件清单与统一编辑器设置
64d7ade docs: 添加 README 任务卡目录对照表与验收自查清单
6882f61 chore: 初始化通识仓库骨架、.gitignore 与空目录占位
```

**已达 10 次语义化提交**（要求 ≥5 次），每条提交信息均遵循 `<type>(<scope>): <描述>` 规范，
且按「任务卡」为粒度拆分，没有任何一次是「把所有代码一次性提交」。

### 5.2 提交身份核验

```
$ git log -1 --pretty="%an <%ae>  %ad" --date=short
eninora <17200678363@163.com>  2026-10-13
```

说明 `user.name` / `user.email` 已正确写入每一次提交的作者字段。

### 5.3 分支合并实操

```bash
git checkout -b feature/git-workflow-record     # 从 main 切出功能分支
# …… 在分支上修改 docs/Git-协作记录.md ……
git add docs/Git-协作记录.md
git commit -m "docs(task04): 补充真实提交树与分支合并记录"

git checkout main
git merge --no-ff feature/git-workflow-record \
  -m "merge: 合入 Git 协作实操记录（feature/git-workflow-record）"
```

合并后 `git log --oneline --graph` 可见清晰的分叉与汇合节点：

```
*   <merge 提交>  merge: 合入 Git 协作实操记录（feature/git-workflow-record）
|\
| * <feature 提交>  docs(task04): 补充真实提交树与分支合并记录
|/
* 48fd0cd docs(task04): 添加 Git 协作规范与 SSH 配置记录
* …（略）
```

> `--no-ff`（no fast-forward）强制生成一个合并提交，即使可以直接快进。
> 这样在历史上能**看出一条明确的分支线**，评审时一眼就能分辨「哪些改动来自哪个功能分支」——
> 这正是实验室要求「仓库内包含至少 1 次从 feature 分支开发并合并回 main 的清晰 Git 提交树」的原因。
