# Linux 终端基础与常用命令行实战记录（Task 03）

> 环境：Windows 11 + Git Bash（MSYS2 环境）；等价命令在 WSL2 / 原生 Linux 下通用。
> 说明：本记录中的命令与输出均为**本机真实执行结果**，非示例编造。

---

## 一、命令能力对照表

| 分类 | 命令 | 掌握程度 | 本记录是否实操 |
|---|---|---|---|
| 目录与文件 | `pwd` `cd` `ls` `mkdir` `rm` `cp` `mv` `cat` `head` `tail` `find` | 熟练 | ✅ |
| 文本过滤与管道 | `grep` `wc` `>` `>>` `\|` | 熟练 | ✅ |
| 系统状态与进程 | `ps aux` `top` `kill` `df -h` `free -m` | 熟练 | ✅ `df -h` |
| 文件权限 | `chmod` `chown` | 掌握基础 | ✅ `chmod` |
| 进阶文本处理 | `awk` `sort` `uniq` | 了解 | ✅ |

---

## 二、实操任务与真实执行记录

### 任务 1：创建指定目录树

```bash
mkdir -p ai-lab/{src/{basics,luogu},docs,logs,archive}
find ai-lab -type d | sort
```

真实输出：

```
ai-lab
ai-lab/archive
ai-lab/docs
ai-lab/logs
ai-lab/src
ai-lab/src/basics
ai-lab/src/luogu
```

> 知识点：花括号展开 `{a,b}` 与嵌套 `{x/{y,z}}` 能在一条命令里建出整棵树，比逐层 `mkdir` 快得多；`-p` 保证父目录不存在时自动创建且不报错。

### 任务 2：生成样例日志并统计

```bash
wc -l ai-lab/logs/app.log
```

真实输出：

```
10 ai-lab/logs/app.log
```

### 任务 3：过滤指定日志文本并输出为新文件

```bash
grep -n "ERROR" ai-lab/logs/app.log > ai-lab/logs/errors.log
cat ai-lab/logs/errors.log
```

真实输出：

```
4:2026-10-13 22:02:47 ERROR failed to connect db: dial tcp 10.0.0.7:5432 i/o timeout
6:2026-10-13 22:03:05 ERROR db retry exhausted, fallback to local sqlite
9:2026-10-13 22:07:41 ERROR token budget exceeded: 131072 > 128000
```

> 知识点：`>` 是覆盖重定向，`>>` 是追加重定向；`-n` 保留原始行号，便于回原文件定位上下文。

### 任务 4：批量重命名文件

```bash
touch ai-lab/archive/{a,b,c,d}.log.bak
for f in ai-lab/archive/*.log.bak; do
  new="${f%.log.bak}.log.archive"
  mv -- "$f" "$new"
  echo "renamed: $(basename "$f") -> $(basename "$new")"
done
ls ai-lab/archive
```

真实输出：

```
renamed: a.log.bak -> a.log.archive
renamed: b.log.bak -> b.log.archive
renamed: c.log.bak -> c.log.archive
renamed: d.log.bak -> d.log.archive
a.log.archive
b.log.archive
c.log.archive
d.log.archive
```

> 知识点：`${var%suffix}` 从前向最短匹配截断后缀；`mv --` 双横线用于终止选项解析，防止文件名以 `-` 开头被误判为参数；循环内加 `[ -e "$f" ] || continue` 可兜底「无匹配时 glob 不展开」的经典坑。

### 任务 5：统计与管道组合

```bash
grep -c ERROR ai-lab/logs/app.log
awk '{print $3}' ai-lab/logs/app.log | sort | uniq -c | sort -rn
```

真实输出：

```
3
      5 INFO
      3 ERROR
      2 WARN
```

> 知识点：`awk '{print $3}'` 按空白切分取第 3 列（日志级别）；`sort | uniq -c` 是「先排序才统计」的固定套路，再 `sort -rn` 按次数倒序。

### 任务 6：文件权限与磁盘状态

```bash
ls -ld ai-lab/logs
chmod 644 ai-lab/logs/errors.log
ls -l ai-lab/logs/errors.log
df -h . | head -3
```

真实输出：

```
drwxr-xr-x 1 20255 197609 0 Oct 13 23:15 ai-lab/logs
-rw-r--r-- 1 20255 197609 225 Oct 13 23:15 ai-lab/logs/errors.log
Filesystem      Size  Used Avail Use% Mounted on
C:              401G  198G  203G  50% /c
```

> 知识点：`644` = 属主读写、组读、其他读；`755` = 属主读写执行、其余读执行。Windows 下 `chown` 受限于 MSYS 映射，实际归属仍由 NTFS ACL 决定。

---

## 三、踩坑与排错

| 现象 | 原因 | 解决 |
|---|---|---|
| `wsl --status` 返回 `PROGRAM BLOCKED BY SECURITY POLICY` | 本机安全策略将 `wsl.exe` 列入程序黑名单 | 需在「安全中心 → 命令安全 → 程序黑名单」放行后再执行；本次先用 Git Bash 完成等价练习 |
| `for f in *.bak` 在无匹配文件时把字面量 `*.bak` 当成文件名 | glob 未匹配时默认不展开 | 循环体内加 `[ -e "$f" ] \|\| continue` 兜底，或开启 `shopt -s nullglob` |

---

## 四、留痕截图清单

| 文件 | 内容 |
|---|---|
| `assets/screenshots/task03_terminal.png` | 上述命令完整执行过程的终端截图 |
| `assets/screenshots/task03_history.png` | `history \| tail -50` 输出截图 |

> 复现命令：`history | tail -50 > docs/task03_history.txt`
