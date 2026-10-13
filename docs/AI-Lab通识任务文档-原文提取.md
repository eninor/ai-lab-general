# AI-Lab 通识任务文档（新进成员准入考核手册）
    定位：AI-Lab 业务路线（Line1~Line4）的公共前置准入关卡
周期：上限 1 个月（自主推进、提倡提速，提前完成可提前分流进入具体业务线深入学习）
原则：以任务卡和交付物为中心，拒绝无实操的理论空谈，建立扎实可靠的工程动手基本功

---

## 1. 通识考核机制与协作规范

### 1.1 考核目标与准入机制
  1. 前置必要条件：根据《AI-Lab 简述与规范文档》，新进成员必须通过通识技术栈考核，才能正式选定进入 Line1（文档智能与知识库）、Line2（GUI Agent 与专业工具）、Line3（Self-Research 科研探索）或 Line4（AI 应用与商业化实践）。
  1. 自主提速机制：1 个月为最高时间上限。个人可自主安排学习与冲刺节奏，只要提前合格完成全部通识任务与综合微项目，即可随时申请提前验收，提前分流进入后续业务路线。
  1. 导师与答疑支持：在通识推进期间遇到卡点，可直接联系意向业务线的组长或骨干（L3）进行答疑解惑。

### 1.2 核心守则（遵照实验室日常规范）
  - 善用 AI，严防幻觉：允许并鼓励使用 AI 工具辅助排查和理解，但所有代码必须在本地调通，严禁无脑复制代码提交；
  - 真实记录，拒绝形式主义：笔记重点记录环境配置路径、真实报错与排错过程（Troubleshooting）、客观实验数据，拒绝大段搬运百科或摘抄网帖；
  - 规范提问，拒绝伸手党：遇到技术卡点先查文档与大模型，求助时提供“目标环境 + 已尝试方案 + 完整报错日志”；
  - 全流程 Git 留痕：每位成员必须建立一个公开或团队内部可见的 GitHub 个人通识仓库，所有任务卡成果按目录提交归档。

---

## 2. 通识核心技术栈与任务分解
  通识考核划分为 开发工具链与基础环境搭建、Python 编程与逻辑实战 以及 通识综合微项目收官 三大阶段：

---

### 阶段一：开发工具链与基础环境搭建（建议用时：7~10 天）

#### Task 01：现代 AI 编程环境（Trae / Antigravity / WorkBuddy / VSCode）与 CLI 协同工具配置
  - 任务目标：建立现代化、无障碍的代码编辑与终端调试环境，掌握新一代 AI 原生 IDE / 桌面智能工作站与开源终端 CLI 编程协同工具。
    核心基线（AI 研发三件套要求）：
    1. 至少配置一个现代 AI IDE / 智能工作台：如 Trae / Antigravity / WorkBuddy / Cursor / Windsurf，或 VSCode + AI 插件；
    1. 至少掌握一个终端 AI Agent：如 OpenCode / DSH (DeepSeek Harness) / Claude Code / Aider 等命令行交互工具；
    1. 至少拥有一个稳定可用的大模型服务/API：如 SiliconFlow、DeepSeek、智谱、Google AI Studio 等。
  - 任务步骤：
    1. 现代代码开发环境安装与配置（任选其一或组合使用）：
      - 方案 A（AI 原生 IDE / 智能工作台，推荐）：安装体验 Trae（字节跳动推出的 AI 原生 IDE，国内网络与模型免配置开箱即用）、Antigravity（Google Agent 架构原生编程环境）或 WorkBuddy（腾讯推出的 AI 桌面智能工作台，内置 Coding Mode 与多 Agent 任务闭环交付能力）；亦可按个人习惯使用 Cursor 或 Windsurf；
      - 方案 B（VSCode 经典组合）：安装 VSCode 并配置核心插件：Error Lens（错误高亮）、Path Intellisense（路径补全）、Prettier（格式化）、Code Runner（快速运行）、GitLens（版本回溯）、Python & Jupyter（官方环境支持），可选配 Roo Code、Continue 等 AI 辅助插件；
    1. 终端 / CLI AI 协同工具实战（强烈推荐）：
      - 安装并配置开源免费的终端 Coding Agent 工具——OpenCode 或 DSH (DeepSeek Harness)（亦可体验 Aider / Gemini CLI 等终端交互工具）；
      - 配置工具对接可用 API（如 SiliconFlow 硅基流动、DeepSeek、智谱 AI 等免费/高性价比 API）或本地模型；
      - 在终端通过 OpenCode 或 DSH 完成一次交互式代码审查、解释或辅助脚本编写；
    1. 基础 API 连通性测试（可选）：通过代码或终端脚本直接跑通一次大模型 API 调用问答。
  - 交付要求：在个人 GitHub 仓库中提交所选 IDE/工作台工作界面截图、OpenCode / DSH 等 CLI 工具在终端成功交互的运行截图，以及简要的环境配置与上手说明。

#### Task 02：Markdown 规范排版与实战笔记系统搭建
  - 任务目标：掌握工程与学术界通用的 Markdown 文档书写规范，搭建个人实战笔记库。
  - 任务步骤：
    1. 使用 VSCode、Trae、Antigravity 或安装配置 Obsidian 作为主力 Markdown 笔记工具；
    1. 熟练掌握 Markdown 核心语法：多级标题、列表、表格、代码块语法高亮、超链接、本地与网络图片引用、LaTeX 数学公式基础；
    1. 建立个人的技术笔记目录体系（包含：环境配置、排错记录、代码实验、文献/技术摘记）。
  - 交付要求：撰写一份规范的《AI-Lab 通识学习计划与环境踩坑记录.md》，内含至少 1 个表格、1 段代码块、1 张环境截图及 1 个外部参考链接，提交至仓库。

#### Task 03：Linux 终端基础与常用命令行实战
  - 任务目标：摆脱纯图形界面依赖，具备在 Linux 字符界面下独立管理文件、查看进程与排查问题的基本功。
  - 环境要求：本地 Linux 系统、WSL2（Windows 推荐）或 Linux 虚拟机。
  - 任务步骤：
    1. 熟练掌握目录与文件操作命令：pwd、cd、ls (或 eza)、mkdir、rm、cp、mv、cat、head、tail、find；
    1. 熟练掌握文本过滤与管道操作：grep、wc、标准输入输出重定向（>、>>、|）；
    1. 熟悉系统状态与进程排查：ps aux、top (或 btop)、kill、df -h、free -m；
    1. 掌握文件权限基础：chmod、chown。
  - 交付要求：在终端完成指定操作序列（创建指定目录树、批量重命名文件、过滤指定日志文本并输出为新文件），将执行历史（history）与操作截图整理为 Markdown 文档提交。

#### Task 04：Git 版本管理规范与 GitHub 协作实操
  - 任务目标：养成用 Git 跟踪一切代码与文档资产的肌肉记忆，掌握标准协作工作流。
  - 任务步骤：
    1. 注册并配置 GitHub 账号，本地配置 user.name 与 user.email，生成并配置 SSH Key；
    1. 熟练掌握本地版本库管理：git init、git status、git add、git commit -m "规范的提交信息"、git log --oneline --graph；
    1. 掌握分支与合并流：git branch、git checkout -b <branch>、git merge、解决冲突基本方法；
    1. 掌握远程仓库交互：git remote add、git push、git pull、git clone。
  - 交付要求：
    - 个人通识 GitHub 仓库必须包含至少 5 次具有规范语义的 Commit 记录；
    - 仓库内包含至少 1 次从 feature 分支开发并合并回 main 分支的清晰 Git 提交树。

#### Task 05：Conda 虚拟环境隔离与镜像源加速配置
  - 任务目标：掌握 Python 多版本依赖隔离，彻底杜绝环境污染与依赖冲突。
  - 任务步骤：
    1. 安装 Miniconda（推荐）或 Anaconda；
    1. 配置国内镜像源（清华源或阿里源），加速 conda 和 pip 的包下载速度；
    1. 掌握核心环境管理命令：
      - conda create -n <env_name> python=3.10
      - conda activate <env_name> / conda deactivate
      - conda env list / conda remove -n <env_name> --all
    1. 掌握依赖导出与复现：pip freeze > requirements.txt，以及 pip install -r requirements.txt。
  - 交付要求：提交 Conda 镜像源配置内容截图、成功创建指定 Python 版本的独立环境并成功导出 requirements.txt 的操作记录。

#### Task 06：Docker 容器化技术初探
  - 任务目标：理解轻量级容器技术，建立环境打包、分发与隔离的现代化工程认知。
  - 任务步骤：
    1. 安装 Docker（Linux 原生或 Windows WSL2 Docker Desktop）；
    1. 理解镜像（Image）与容器（Container）的区别；
    1. 掌握 Docker 核心命令：
      - docker pull python:3.10-slim（拉取镜像）
      - docker images / docker ps -a（查看本地镜像与容器列表）
      - docker run -it --rm -v $(pwd):/app -w /app python:3.10-slim python -c "print('Hello AI-Lab Docker')"（挂载目录与运行容器；注：Windows PowerShell 下请将 $(pwd) 替换为 ${PWD}，CMD 替换为 %cd%）
      - docker stop / docker rm（管理容器生命周期）
    1. 编写一个最小 Dockerfile，将一段简单的 Python 脚本构建为镜像并运行。
  - 交付要求：提交 Dockerfile 源码、构建镜像（docker build）与容器成功运行（docker run）的终端日志截图。

---

### 阶段二：Python 编程基础与逻辑实战（建议用时：10~14 天）

#### Task 07：Python 核心语法与常用数据结构
  - 学习重点：
    1. 基本类型与运算符：int、float、str、bool，类型转换，格式化输出（f-string）；
    1. 流程控制：条件判断（if / elif / else），循环控制（for in, while, break, continue）；
    1. 四大内置集合类型：
      - 列表（List）：增删改查、切片操作、列表推导式；
      - 字典（Dict）：键值对操作、get 方法、字典推导式；
      - 元组（Tuple）与集合（Set）：不可变性、集合去重与交并差运算；
    1. 函数设计：参数默认值、*args、**kwargs、返回值与变量作用域。
  - 练习要求：独立编写完成 3 组针对集合与函数的核心练习代码，严格遵循 PEP8 编码规范。
  - 交付要求：代码全部上传至仓库 src/basics/ 目录。

#### Task 08：Python 文件 I/O、异常处理与工程基础
  - 学习重点：
    1. 上下文管理器与文件读写：使用 with open(...) 正确读写 utf-8 文本；
    1. 现代路径库 pathlib：使用 Path 进行路径拼接、目录遍历（glob / rglob）、判断文件是否存在；
    1. 结构化数据序列化：json.dump / json.load 读写 JSON 数据；
    1. 稳健性保障：使用 try ... except ... finally 捕获并妥善处理异常，避免程序崩溃。
  - 实战小任务：编写一个 Python 脚本，扫描指定文件夹下的所有 .md 文件，统计每个文件的字数与行数，并将统计结果汇总输出为 summary.json。
  - 交付要求：脚本源码、测试样例文件及生成的 summary.json 提交至仓库。

#### Task 09：经典算法与编程逻辑专项巩固
  - 训练重点：培养独立逻辑构思与代码编写能力，此部分要求独立动手排错，禁止直接向 AI 索要答案，切实建立调试与逻辑排错直觉。
  - 精选考核题单（从基础到综合，任选通过至少 6 题）：
    1. P1046 [NOIP2005 普及组] 陶陶摘苹果（基础数组与遍历）
    1. P1420 最长连续自然数（循环与状态记录）
    1. B2077 角谷猜想（while 循环与边界控制）
    1. B2046 骑车与走路（分支判断与浮点运算）
    1. P1980 [NOIP2013 普及组] 计数问题（数字拆分与统计）
    1. B3736 [语言月赛 202305] 约数计数（数学逻辑与循环优化）
    1. P1009 [NOIP1998 普及组] 阶乘之和（高精度与累加计算）
  - 交付要求：在仓库 src/luogu/ 目录下存放对应题号的代码文件，并在文档中附上洛谷通过（Accepted）记录截图。

---

### 阶段三：通识综合微项目实战（收官考核，建议用时：3~5 天）

#### Task 10：通识收官项目——自动化文档/日志处理 CLI 工具
  - 项目背景：在后续的 Line1（文档智能）与 Line2（Agent 轨迹日志）等业务线中，高频涉及本地文档解析、数据提取与自动化脚本编写。本综合项目用于检验成员将前述工具链、Python、Docker 与 Git 融会贯通的能力。
  - 项目目标：从零搭建一个具备实际功能的轻量命令行工具（CLI），并在 Docker 容器中可复现运行。
  - 功能建议（二选一或自拟合理方向）：
    - 选项 A（文档工程工具）：编写一个 Markdown 文档检查与清洗工具，支持：扫描指定目录下的 Markdown 文档、检测死链接与损坏图片、统计标题层级结构、生成格式良好的目录索引；
    - 选项 B（日志与数据清洗工具）：编写一个日志分析工具，支持：读取指定格式的文本/JSON 日志文件、按照时间/级别过滤、提取核心指标（如报错统计、高频关键词）、输出美观的统计 Markdown 报告或 CSV 数据。
  - 工程标准要求：
    1. 代码规范：模块化设计，具备明确的主入口（如 main.py），支持命令行参数（如使用 argparse 或 sys.argv）；
    1. 项目文档：提供清晰完备的 README.md，包含项目介绍、环境安装方法、运行示例与输出展示；
    1. 容器封装：附带合规的 Dockerfile，任何其他成员仅需执行 docker build 与 docker run 即可在隔离环境中顺利运行该工具；
    1. 版本管理：使用 Git 规范记录功能迭代过程，用git把每个功能模块的代码分别提交，严禁“一个 commit 提交所有代码”。
  - 交付要求：GitHub 仓库中包含完整的项目源码、README.md、requirements.txt、Dockerfile 及本地与容器化运行成功截图。

---

## 3. 通识考核验收标准与评价指标
  通识考核不看虚假的“学习时长”，只看真实交付物质量与工程可靠性。

### 3.1 验收 Checklist

| 维度 | 考核项目 | 合格标准 | 权重 |
|---|---|---|---|
| 开发环境 | Linux / 现代IDE(Trae/Antigravity/VSCode) / CLI工具 / Conda | 能熟练在终端操作，成功创建隔离环境并无报错运行代码 | 15% |
| 版本协作 | Git 基础与仓库管理 | GitHub 仓库目录结构清晰，提交信息（Commit Message）规范，无杂乱文件 | 20% |
| 编程能力 | Python 语法与练习题 | 洛谷精选题完成至少 6 题，代码风格良好，无低级语法错误 | 25% |
| 工程交付 | Task 10 综合微项目 | 工具功能完整，README.md 详实，提供 Dockerfile 且可独立复现 | 25% |
| 团队素养 | 笔记复盘与日常表现 | 笔记拒绝形式主义拼凑，如遇卡点提问包含三要素，按约定节奏主动反馈 | 15% |

### 3.2 验收流程与方向分流
  1. 成果自查与提交：成员整理个人 GitHub 仓库，确认 10 张任务卡交付物全部就绪，在 README.md 中汇总各任务对应的目录与说明；
  1. 通识评审（Review）：由实验室骨干（L3）或意向组长进行代码走查（Code Review），必要时进行 10~15 分钟的简短现场/线上答辩（重点考察代码是否为本人独立理解并掌握，严查 AI 裸代与幻觉欺骗）；
  1. 合格放行与分流：
    - 评审合格者，正式获发 L1 任务参与者 资格，正式进入后续具体业务线（Line1 ~ Line4）参与专项攻关；
    - 存在明显不足者，给出针对性修改建议（如补充排错复盘、修改代码漏洞），限期 3~5 天内二次复核。

---

## 4. 推荐参考资源汇总

### 4.1 常用学生友好型 AI 平台

| 平台 | 推荐用途 | 特点与说明 |
|---|---|---|
| SiliconFlow (硅基流动) | 对接 OpenCode / CLI 工具、开源大模型托管 | 国内免梯，新注册赠送额度，支持 DeepSeek-V3/R1、Qwen 全系列，极速对接 |
| DeepSeek 开放平台 | 核心推理、高性价比 API 调用 | 官方原生 API，推理性能强悍，资费极低，适合作为主力或备用模型服务 |
| 国家超算平台 | API 练手、开源模型调用 | 新用户免费额度，支持 DeepSeek 等主流模型 |
| 智谱 AI 开放平台 | 编程辅助、IDE 与 CLI 工具对接 | 免费 Token 额度充足，GLM 系列编码效果优秀 |
| ModelScope 魔搭社区 | 寻找开源数据集、模型与 MCP / Skills | 阿里开源社区，免梯子快速下载模型与技术实践 |
| Google AI Studio | Gemini 系列 API 调用 | 免费额度充足，长上下文能力出色 |

### 4.2 核心官方与优质教程索引
  - Git 学习：廖雪峰 Git 教程（重点学习安装、仓库创建、分支与合并）；
  - Linux 命令行：Linux 命令行与 Shell 脚本基础；