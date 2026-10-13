# AI-Lab 通识考核个人仓库

> AI-Lab 业务路线（Line1~Line4）公共前置准入关卡的交付仓库。
> 周期：上限 1 个月（自主推进、提倡提速）；原则：以任务卡和交付物为中心。

## 一、任务卡 → 目录对照表

| 任务卡 | 内容 | 交付目录 / 文件 | 状态 |
|---|---|---|---|
| Task 01 | 现代 AI 编程环境与 CLI 协同工具配置 | `docs/Environment-Setup.md`、`.vscode/`、`assets/screenshots/` | ✅ 环境就绪，待补截图 |
| Task 02 | Markdown 规范排版与实战笔记系统 | `docs/AI-Lab通识学习计划与环境踩坑记录.md`、`notes/` | ✅ 骨架完成，待持续记录 |
| Task 03 | Linux 终端基础与常用命令行实战 | `docs/Linux-CLI-实战记录.md`、`projects/cli-lab-demo/` | ✅ 实操完成（Git Bash），WSL2 待手动装 |
| Task 04 | Git 版本管理规范与 GitHub 协作 | `docs/Git-协作记录.md`、本仓库提交历史 | ✅ 10 次提交 + 1 次分支合并，待绑定远程 |
| Task 05 | Conda 虚拟环境隔离与镜像源加速 | `docs/Environment-Setup.md#三task-05`、`configs/` | ✅ 完成（conda 26.7.1 + ailab 环境） |
| Task 06 | Docker 容器化技术初探 | `projects/docker-demo/`、`docs/WSL2与Docker-手动安装指引.md` | ⏳ Dockerfile 已就绪，待手动装 Docker |
| Task 07 | Python 核心语法与常用数据结构 | `src/basics/` | 待补练习 |
| Task 08 | Python 文件 I/O、异常处理与工程基础 | `src/io/` | 待补脚本 |
| Task 09 | 经典算法与编程逻辑专项巩固（洛谷 ≥6 题） | `src/luogu/` | 待补题解 |
| Task 10 | 通识收官项目——自动化文档/日志处理 CLI 工具 | `projects/doc-cli/` | 待开发 |

## 二、仓库结构

```
ai-lab-general/
├── README.md                 # 本文件：任务卡与目录索引
├── .gitignore
├── assets/screenshots/       # 全部环境截图与运行留痕
├── configs/                  # 可复现的配置文件（.condarc、settings.json 等）
├── docs/                     # 各类交付文档
│   ├── AI-Lab通识学习计划与环境踩坑记录.md   # Task 02 主交付物
│   ├── Environment-Setup.md                  # Task 01 / 05 / 06 环境配置留痕
│   ├── Linux-CLI-实战记录.md                  # Task 03
│   └── Git-协作记录.md                        # Task 04
├── notes/                    # 个人技术笔记体系（Task 02）
│   ├── 01-环境配置/
│   ├── 02-排错记录/
│   ├── 03-代码实验/
│   └── 04-文献摘记/
├── src/
│   ├── basics/               # Task 07 Python 语法与数据结构练习
│   ├── io/                   # Task 08 文件 I/O 与异常处理
│   └── luogu/                # Task 09 洛谷题解（文件名 = 题号）
└── projects/
    ├── doc-cli/              # Task 10 收官 CLI 项目
    └── docker-demo/          # Task 06 Dockerfile 实战演练
```

## 三、验收 Checklist 自查（对应文档 §3.1）

| 维度 | 合格标准 | 权重 | 自查 |
|---|---|---|---|
| 开发环境 | 能熟练在终端操作，成功创建隔离环境并无报错运行代码 | 15% | ☐ |
| 版本协作 | 仓库目录结构清晰，Commit Message 规范，无杂乱文件 | 20% | ☐ |
| 编程能力 | 洛谷精选题完成至少 6 题，代码风格良好 | 25% | ☐ |
| 工程交付 | Task 10 功能完整，README 详实，Dockerfile 可独立复现 | 25% | ☐ |
| 团队素养 | 笔记拒绝形式主义，提问含三要素，按节奏主动反馈 | 15% | ☐ |

## 四、提交规范（Conventional Commits）

```
feat: 新增 xxx 功能
fix: 修复 xxx 问题
docs: 更新 xxx 文档
chore: 环境配置 / 依赖调整
refactor: 重构 xxx（不改变外部行为）
```

约定：**一个功能模块一个 commit**，严禁「一个 commit 提交所有代码」。

## 五、提卡三要素（求助规范）

遇到卡点时按此格式提问：**目标环境 + 已尝试方案 + 完整报错日志**。
