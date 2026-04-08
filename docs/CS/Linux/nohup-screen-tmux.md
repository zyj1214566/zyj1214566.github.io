# Linux 后台任务管理：nohup、screen、tmux

本文档介绍三种常用的 Linux 后台任务管理工具，适用于远程服务器长时间运行任务的场景。

---

## 1. nohup

`nohup`（no hangup）是最简单的后台运行方式，使命令忽略挂断信号（SIGHUP），在关闭终端后继续运行。

### 1.1 基本用法

```bash
nohup python train.py &
```

输出默认写入当前目录的 `nohup.out`。

### 1.2 指定输出文件

```bash
nohup python train.py > train.log 2>&1 &
```

- `> train.log`：标准输出重定向到文件
- `2>&1`：标准错误也重定向到同一文件

### 1.3 查看和关闭

```bash
# 查看输出
tail -f train.log

# 查看后台进程
jobs -l
ps aux | grep train.py

# 关闭进程
kill <PID>
kill -9 <PID>  # 强制杀死
```

### 1.4 适用场景

- 只需要跑一个任务，不需要交互
- 任务跑完就结束，不需要中途查看

### 1.5 局限

- **不能重新连接**：关闭终端后无法再回到该终端会话
- **无法多窗口**：一次只能跑一个任务
- **无交互**：不支持中途输入

---

## 2. screen

`screen` 是一个终端复用器，支持创建多个虚拟终端会话，可随时断开和重新连接。

### 2.1 安装

```bash
# Ubuntu/Debian
sudo apt install screen

# CentOS/RHEL
sudo yum install screen
```

### 2.2 常用命令

```bash
# 创建一个名为 train 的会话
screen -S train

# 在会话中运行任务
python train.py

# 断开会话（任务继续运行）
# 快捷键: Ctrl+A, 然后按 D

# 列出所有会话
screen -ls

# 重新连接会话
screen -r train

# 如果连接失败，先踢掉旧连接再重连
screen -d -r train

# 杀死指定会话
screen -X -S train quit
```

### 2.3 快捷键

所有 screen 快捷键都需要先按 `Ctrl+A`，然后再按对应键：

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+A, D` | 断开当前会话 |
| `Ctrl+A, C` | 创建新窗口 |
| `Ctrl+A, N` | 切换到下一个窗口 |
| `Ctrl+A, P` | 切换到上一个窗口 |
| `Ctrl+A, K` | 关闭当前窗口 |
| `Ctrl+A, [` | 进入滚动模式（按 `q` 退出） |

### 2.4 适用场景

- 需要运行多个任务并来回切换
- 需要断开后重新连接查看输出

---

## 3. tmux

`tmux` 是更强大的终端复用器，支持分屏、窗口管理等高级功能，是 screen 的现代替代品。

### 3.1 安装

```bash
# Ubuntu/Debian
sudo apt install tmux

# CentOS/RHEL
sudo yum install tmux
```

### 3.2 常用命令

```bash
# 创建一个名为 train 的会话
tmux new -s train

# 断开会话
# 快捷键: Ctrl+B, 然后按 D

# 列出所有会话
tmux ls

# 重新连接会话
tmux attach -t train

# 杀死指定会话
tmux kill-session -t train

# 杀死所有会话
tmux kill-server
```

### 3.3 分屏操作

所有 tmux 快捷键都需要先按 `Ctrl+B`，然后再按对应键：

**水平分屏：**
```
Ctrl+B, %
```

**垂直分屏：**
```
Ctrl+B, "
```

**在窗格间切换：**
```
Ctrl+B, 方向键（↑↓←→）
```

**调整窗格大小：**
```
Ctrl+B, Ctrl+方向键
```

**关闭当前窗格：**
```
Ctrl+B, x
```

### 3.4 窗口管理

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+B, C` | 创建新窗口 |
| `Ctrl+B, N` | 下一个窗口 |
| `Ctrl+B, P` | 上一个窗口 |
| `Ctrl+B, 0~9` | 切换到指定编号窗口 |
| `Ctrl+B, ,` | 重命名当前窗口 |
| `Ctrl+B, W` | 显示窗口列表 |

### 3.5 适用场景

- 需要同时监控多个任务（分屏）
- 需要复杂的窗口布局管理
- 团队协作（支持共享会话）

---

## 4. 三者对比

| 特性 | nohup | screen | tmux |
|------|-------|--------|------|
| 后台运行 | ✅ | ✅ | ✅ |
| 断开重连 | ❌ | ✅ | ✅ |
| 多窗口 | ❌ | ✅ | ✅ |
| 分屏 | ❌ | ❌ | ✅ |
| 交互操作 | ❌ | ✅ | ✅ |
| 学习成本 | 低 | 中 | 中 |
| 功能丰富度 | 低 | 中 | 高 |
| 服务器预装率 | 几乎都有 | 较高 | 较高 |

## 5. 选择建议

- **跑完就不管的任务** → `nohup`
- **需要断开重连，但不需要分屏** → `screen`
- **需要分屏、多窗口、复杂管理** → `tmux`
