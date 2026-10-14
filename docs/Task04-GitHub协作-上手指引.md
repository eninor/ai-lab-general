# Task 04 GitHub 协作上手指引（照着敲即可）

> 生成日期：2026-10-14 · 面向本机实际状态，不写泛泛的通用教程。

---

## 一、你当前的状态

| 任务卡步骤 | 内容 | 状态 |
|---|---|---|
| a. 配置 `user.name` / `user.email` | `eninor` / `17200678363@163.com` | ✅ 已完成 |
| a. 生成 SSH Key | `C:\Users\20255\.ssh\id_ed25519`（ed25519） | ✅ 已完成 |
| a. **把公钥贴到 GitHub** | — | ⏳ **待做（见第三节）** |
| b. 本地版本库管理 | 已跑通 `init/status/add/commit/log` | ✅ 已完成 |
| c. 分支与合并 | `feature/git-workflow-record` → `main` | ✅ 已完成 |
| d. 远程仓库交互 | — | ⏳ **待做（见第六节）** |

---

## 二、先解决一个环境问题：本机的 GitHub 网络

**实测诊断结果：**

```
https://github.com                  → HTTP 000（TLS 握手被重置）
https://api.github.com              → HTTP 000
https://raw.githubusercontent.com   → HTTP 000
https://codeload.github.com         → HTTP 301（可通）
github.com:22                       → Connection refused（端口被屏蔽）
ssh.github.com:443                  → 可通 ✅
```

**结论与对策：**

| 操作 | 走什么通道 | 是否可行 |
|---|---|---|
| `git push` / `git pull`（代码推送） | SSH over **443** | ✅ 可行（已配好） |
| 打开 github.com 网页（注册 / 建仓库 / 加公钥） | HTTPS 443 | ❌ 需借助网络加速工具 |

> 已为你写入 `C:\Users\20255\.ssh\config`，让 `git@github.com` 自动改走 `ssh.github.com:443`：
>
> ```
> Host github.com
>     HostName ssh.github.com
>     Port 443
>     User git
>     IdentityFile ~/.ssh/id_ed25519
> ```
>
> 这一步很关键——不配的话，`git push` 会因为 22 端口不通而一直卡住或报
> `ssh: connect to host github.com port 22: Connection refused`。

**网页访问的解决办法（任选其一）**：
1. 用具备 GitHub 加速能力的工具（如 **Watt Toolkit**，原 Steam++，开启了「GitHub 加速」后即可正常打开网页）；
2. 手机开热点换个网络出口试试；
3. 若已有可用的代理，在浏览器中启用后访问。

> 只是**加公钥、建仓库**这两次网页操作需要。之后日常 `git push` 全靠 SSH/443，不再依赖网页。

---

## 二·补充、中文界面下的操作位置（浏览器自动翻译）

如果浏览器开启了自动翻译，GitHub 的英文界面会被译成中文，菜单名称会变得很陌生。
**最省事的办法是直接用下面的直达链接**，跳过找按钮这一步：

| 要做的事 | 直达链接 | 翻译后的叫法 |
|---|---|---|
| 新建仓库 | `https://github.com/new` | 新建 / 新建存储库 |
| 添加 SSH 公钥 | `https://github.com/settings/keys` | 设置 → SSH 和 GPG 密钥 |
| 查看个人主页 | `https://github.com/` + 用户名 | 仪表盘 |
| 查看自己的仓库列表 | `https://github.com/你的用户名?tab=repositories` | 存储库 |

常见术语对照（机器翻译 ↔ 原文 ↔ 含义）：

| 中文界面 | 英文原文 | 含义 |
|---|---|---|
| 仪表盘 | Dashboard | 登录后的首页 |
| 存储库 | Repository | 仓库，即一个项目 |
| 饲料 | Feed | 动态流（关注的人的动态，与本任务无关） |
| 设置 | Settings | 账号设置 |
| SSH 和 GPG 密钥 | SSH and GPG keys | 放公钥的地方 |
| 提交 | Commit | 一次版本快照 |
| 推送 / 拉取 | Push / Pull | 上传到远程 / 从远程同步 |

> 💡 如果翻译后按钮点不动或找不到，可以在页面空白处右键 → 「翻译成中文」取消勾选，
> 或点地址栏右侧的翻译图标 → 选择「显示原文」，切回英文界面再操作。

---

## 三、步骤 a：GitHub 账号与 SSH Key

### a1. 注册 / 登录 GitHub

浏览器打开 `https://github.com`，已有账号直接登录；没有则点 **Sign up**。
注册时使用的邮箱请与 `user.email`（`17200678363@163.com`）保持一致，这样提交才会算到你的贡献记录里。

### a2. 本地身份配置（已完成，无需再做）

```
$ git config --global --list
user.email=17200678363@163.com
user.name=eninor
core.autocrlf=true
init.defaultbranch=main
```

### a3. 把公钥贴到 GitHub ★

**第一步**：复制公钥全文。在终端执行：

```bash
cat ~/.ssh/id_ed25519.pub
```

本机公钥内容（可直接复制下面这一整行）：

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBWB7rrhZSCBE2IYWwXiRcsnqYax1rAZA5/4Fsa60kiI ai-lab-general@localhost
```

> ⚠️ 注意是 **`.pub`** 结尾的文件。**绝对不要**把没有后缀的私钥 `id_ed25519` 内容发给任何人或贴到任何地方。

**第二步**：在 GitHub 网页上依次点击：

```
右上角头像 → Settings → 左侧栏 SSH and GPG keys → 绿色按钮 New SSH key
```

**第三步**：填写两个框后保存：

| 字段 | 填什么 |
|---|---|
| Title | `ai-lab-windows`（随便起，方便日后辨认设备） |
| Key type | 保持默认 `Authentication Key` |
| Key | 粘贴上面那一整行公钥 |

点 **Add SSH key**，可能会要求输入 GitHub 密码确认。

### a4. 验证是否配置成功 ★

```bash
ssh -T git@github.com
```

**首次执行**会提示：

```
The authenticity of host '[ssh.github.com]:443 ...' can't be established.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

输入 `yes` 回车。看到下面这句就成功了：

```
Hi eninor! You've successfully authenticated, but GitHub does not provide shell access.
```

> 看到 `Hi 你的用户名!` = 成功；看到 `Permission denied (publickey)` = 公钥还没加上去或贴错了，回 a3 重做。

**💡 顺手记住一个小技巧**：`Hi` 后面回显的那串字符，就是 GitHub 认定的**你的用户名**。

本次配置时就踩到了这个点——本地按 `eninora` 配置，而 `ssh -T` 回显的是 `Hi eninor!`，
说明真实用户名少了最后一个字母。二者差一个字母，会导致：

| 受影响的东西 | 后果 |
|---|---|
| `git remote add origin git@github.com:**eninora**/...` | 推送报 `Repository not found` |
| `git config --global user.name` | 提交记录里显示的名字与账号对不上（不影响归属，GitHub 靠邮箱匹配） |

**所以：不确定用户名时，`ssh -T` 是最权威的确认方式**，比回忆注册时填了什么靠谱。
改名的成本和命令：

```bash
git config --global user.name "正确的用户名"

# 已提交的历史如需一并改名（尚未推送时可用，已推送则需强推，慎用）
git filter-branch -f --env-filter '
if [ "$GIT_AUTHOR_NAME" = "旧名字" ]; then export GIT_AUTHOR_NAME="新名字"; fi
if [ "$GIT_COMMITTER_NAME" = "旧名字" ]; then export GIT_COMMITTER_NAME="新名字"; fi
' --tag-name-filter cat -- --all
```

---

## 四、步骤 b：本地版本库管理（已完成）

任务卡要求掌握的命令，本仓库均已实操：

```bash
git init                                 # 初始化仓库
git status                               # 查看工作区 / 暂存区状态
git add <file>                           # 把改动加入暂存区
git commit -m "docs: 说明本次改动"        # 生成一次版本快照
git log --oneline --graph                # 图形化查看提交历史
```

**日常开发循环**（背下来就行）：

```
改代码 → git status 看有什么变了 → git add 选定要提交的 → git commit -m "规范信息" → git push
```

---

## 五、步骤 c：分支与合并（已完成）

```bash
git branch                               # 查看本地分支
git checkout -b feature/xxx              # 创建并切换到功能分支
# …… 在分支上开发并提交 ……
git checkout main                        # 切回主干
git merge --no-ff feature/xxx -m "merge: 合入 xxx"   # 合并回主干
```

本仓库已用 `feature/git-workflow-record` 完整走过一遍，提交树里有清晰的分叉节点。

**冲突处理三步**：`git status` 找出冲突文件 → 打开文件删掉 `<<<<<<< ======= >>>>>>>` 标记并保留正确内容 →
`git add` 该文件 → `git commit`。放弃本次合并用 `git merge --abort`。

---

## 六、步骤 d：远程仓库交互 ★（当前待做）

### d1. 在 GitHub 网页新建仓库

```
右上角 + 号 → New repository
```

| 字段 | 填什么 |
|---|---|
| Repository name | `ai-lab-general` |
| Description | `AI-Lab 通识考核交付仓库`（可选） |
| 可见性 | **Public**（任务卡要求"公开或团队内部可见"） |
| Initialize this repository with | **三项全部不要勾**（README / .gitignore / license） |

> ⚠️ 千万不要勾 "Add a README file"！否则远程仓库会有一个你本地没有的提交，
> 首次 `git push` 会被拒（报 `rejected — fetch first`）。

### d2. 关联远程仓库

```bash
cd C:/Users/20255/WorkBuddy/2026-10-13-22-52-42/ai-lab-general
git remote add origin git@github.com:eninor/ai-lab-general.git
git remote -v          # 验证：应显示 fetch/push 两行
```

### d3. 首次推送

```bash
git push -u origin main
```

`-u`（即 `--set-upstream`）的作用是**建立本地 main 与远程 origin/main 的跟踪关系**，
以后直接 `git push` / `git pull` 即可，不用再写 `origin main`。

推送成功会看到类似输出：

```
Enumerating objects: 60, done.
...
To github.com:eninor/ai-lab-general.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

**如果报 `Permission denied (publickey)`** → 回第三节 a3 加公钥；
**如果报 `Connection refused` 或卡住** → 检查 `~/.ssh/config` 是否还在（见第二节）。

### d4. 验证远程交互（`git clone`）

换个目录克隆下来，确认仓库真的上去了：

```bash
cd /tmp
git clone git@github.com:eninor/ai-lab-general.git ai-lab-general-check
ls ai-lab-general-check
```

### d5. 后续同步（`git pull`）

```bash
git pull                 # 拉取远程最新改动并合并
git push                 # 推送本地新提交
```

多人协作时的标准顺序：**先 `git pull` 再 `git push`**，避免推送被拒。

---

## 七、验收标准对照

| 任务卡要求 | 实际达成 | 证据 |
|---|---|---|
| 至少 5 次规范语义 Commit | ✅ **13 次** | `git log --oneline` |
| 至少 1 次 feature → main 合并 | ✅ 已完成 | `git log --oneline --graph` 可见分叉 |
| 个人 GitHub 仓库 | ⏳ 建完仓库并推送后达成 | 网页地址 |
| 按目录归档任务卡成果 | ✅ 已完成 | 仓库目录结构 |

推送完成后，把仓库地址填到 `docs/Environment-Setup.md` 的待办清单里即可。

---

## 八、常见问题速查

| 现象 | 原因 | 解决 |
|---|---|---|
| `ssh: connect to host github.com port 22: Connection refused` | 22 端口被屏蔽 | 确认 `~/.ssh/config` 存在且内容正确（走 `ssh.github.com:443`） |
| `Permission denied (publickey)` | 公钥没加到 GitHub，或贴成了私钥 | 回 a3 重新添加；用 `ssh -T git@github.com` 验证 |
| `remote origin already exists` | 已经添加过远程 | `git remote set-url origin <新地址>` 覆盖 |
| `rejected — fetch first` | 远程仓库有本地没有的提交（建仓库时勾了 README） | `git pull --rebase origin main` 后重新 push |
| `fatal: not a git repository` | 当前目录不对 | `cd` 到仓库根目录（含 `.git` 的那一层） |
| 推上去发现作者名不对 | `user.name` 配错 | `git config --global user.name "新名字"`，新提交即生效（历史提交需 `rebase` 才能改） |
| 网页打不开 github.com | 网络出口限制 | 见第二节的加速方案 |
