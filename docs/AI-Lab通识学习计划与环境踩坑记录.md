# AI-Lab 通识学习计划与环境踩坑记录

> Task 02 交付物 · 作者：待填写 · 更新日期：2026-10-13
>
> 记录原则：只写**真实路径、真实报错、真实排错过程与客观实验数据**，不搬运百科、不摘抄网帖。

---

## 一、学习计划与节奏规划

通识考核上限 1 个月，允许自主提速。按三大阶段拆解为可执行的时间表：

| 阶段 | 涵盖任务 | 建议用时 | 计划起止 | 实际状态 |
|---|---|---|---|---|
| 阶段一 开发工具链与基础环境搭建 | Task 01 / 02 / 03 / 04 / 05 / 06 | 7~10 天 | D1 ~ D9 | 进行中 |
| 阶段二 Python 编程基础与逻辑实战 | Task 07 / 08 / 09 | 10~14 天 | D10 ~ D22 | 未开始 |
| 阶段三 通识综合微项目实战 | Task 10 | 3~5 天 | D23 ~ D27 | 未开始 |
| 收尾：成果自查 + 提交 Review | §3.2 验收流程 | 2~3 天 | D28 ~ D30 | 未开始 |

**每日检查点（自用）**：① 今天是否产出可提交的文件？② 是否写进了 Git 提交？③ 遇到卡点是否留下排错记录？
**提速策略**：阶段一的环境配置具有强复用性，一次性配好后可压缩到 2~3 天，把时间让给阶段二的算法练习。

---

## 二、本机环境基线（实测数据）

| 组件 | 版本 / 路径 | 状态 |
|---|---|---|
| 操作系统 | Windows 11 家庭版 中文版，Build 26200 | ✅ |
| Git | 2.55.0.windows.3 | ✅ |
| Node.js | v22.22.2（另装 v24.21.0 系统版） | ✅ |
| npm | 10.9.7 | ✅ |
| Python | 3.13.12 / 3.13.14 双版本共存 | ✅ |
| VSCode | 安装于 `D:\Microsoft VS Code` | ✅ |
| pip 镜像源 | 清华 TUNA `https://pypi.tuna.tsinghua.edu.cn/simple` | ✅ |
| Miniconda | `C:\Users\20255\miniconda3` | 本次配置 |
| Docker | — | ⏳ 待手动安装 |
| WSL2 | — | ⏳ 待手动安装 |

> 说明：`pip config list` 已确认全局 `index-url` 指向清华源，`trusted-host` 同步配置，国内下载无需额外加速。

---

## 三、环境配置与排错记录

### 3.1 排错实战记录

| # | 现象 | 定位过程 | 根因 | 解决方案 |
|---|---|---|---|---|
| 1 | 飞书云文档正文抓取只能拿到开头几段，后面的 Task 卡全部缺失 | 直接 `curl` 抓 HTML 后用 Python 统计关键词命中次数，发现正文关键词为 0 次 → 判定为前端 JS 动态渲染，静态 HTML 里没有内容 | 飞书文档页由 SPA 客户端异步拉取块数据渲染 | 改用爬虫 UA 请求，服务端会返回含 `window.DATA.clientVars.data.block_map` 的 SSR 数据；解析该 JSON 并按 `children` 顺序递归还原全部块 |
| 2 | 执行 `wsl --status` 报 `PROGRAM BLOCKED BY SECURITY POLICY` | 报错明确指出 `wsl.exe` 在程序黑名单中 | 本机安全策略把 `wsl.exe` 拉黑，非 WSL 未安装 | 需手动在「安全中心 → 命令安全 → 程序黑名单」中放行，或改用管理员 PowerShell 手动安装 |
| 3 | `git config --global user.name` 报 `fatal: unable to read config file 'C:/Users/20255/.gitconfig'` | 直接检查 `~/.gitconfig` 是否存在 → 不存在 | 全新环境，Git 只装了程序没做首次身份配置 | 执行 `git config --global user.name/user.email` 初始化配置文件 |

### 3.2 关键路径备忘

```
Git 全局配置：  C:\Users\20255\.gitconfig
SSH 密钥目录：  C:\Users\20255\.ssh\      （id_ed25519 / id_ed25519.pub）
pip 配置文件：  C:\Users\20255\AppData\Roaming\pip\pip.ini
VSCode 可执行： D:\Microsoft VS Code\bin\code.cmd
本仓库根目录：  C:\Users\20255\WorkBuddy\2026-10-13-22-52-42\ai-lab-general
```

### 3.3 代码块示例：批量重命名（Task 03 实操片段）

```bash
# 目标：把 data/ 下所有 *.log.bak 统一改名为 *.log.archive，并打印改名前后的映射
mkdir -p data && cd data
for f in *.log.bak; do
  [ -e "$f" ] || continue                 # 防止无匹配时把字面量当成文件名
  new="${f%.log.bak}.log.archive"
  mv -- "$f" "$new"
  echo "renamed: $f -> $new"
done
```

要点：`${f%.log.bak}` 是后缀截断；`[ -e "$f" ]` 兜底空 glob；`mv --` 防止文件名以 `-` 开头被当成选项。

---

## 四、外部参考链接

- 廖雪峰 Git 教程：https://liaoxuefeng.com/books/git/introduction/index.html
- 洛谷题库（Task 09 题单来源）：https://www.luogu.com.cn/problem/list
- 清华大学开源软件镜像站（Conda / PyPI 加速）：https://mirrors.tuna.tsinghua.edu.cn/

---

## 五、环境截图

> 截图统一存放于 `assets/screenshots/`，命名规范：`task编号_内容.png`

| 文件 | 内容 |
|---|---|
| `task01_vscode.png` | VSCode 工作界面 + 已装插件列表 |
| `task01_cli_agent.png` | 终端 AI Agent 交互成功截图 |
| `task05_conda.png` | Miniconda 镜像源配置 + 环境创建 + requirements 导出 |
| `task06_docker.png` | Dockerfile 构建与容器运行日志 |

（待逐项补齐）
