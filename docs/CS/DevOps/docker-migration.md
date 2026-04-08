# Docker 镜像跨节点迁移完整操作手册

本文档详细记录了如何将 Docker 容器打包为镜像，通过共享存储迁移至另一节点，并解决迁移过程中遇到的权限、命名冲突及启动报错等问题。

## 1. 环境信息

- **源镜像名**: `zyj_mindspeed:latest`
- **容器名称**: `zyj_run`
- **共享存储路径**: `/data01/nlp/zyj/`
- **操作用户**: `ai-dev` (需配合 `sudo` 使用)

## 2. 源节点操作 (Node A)

### 2.1 提交当前容器状态

将运行中或停止的容器保存为新镜像。

```bash
docker commit [原容器ID或名称] zyj_mindspeed:latest
```

### 2.2 导出镜像为 Tar 包

将镜像写入共享存储目录。

```bash
docker save -o /data01/nlp/zyj/zyj_mindspeed.tar zyj_mindspeed:latest
```

## 3. 目标节点操作 (Node B)

### 3.1 导入镜像

从共享存储加载镜像。

**注意**: 必须使用 `sudo` 解决权限问题。

```bash
sudo docker load -i /data01/nlp/zyj/zyj_mindspeed.tar
```

### 3.2 清理旧容器

防止报错 `Conflict: The container name is already in use`。

```bash
sudo docker rm -f zyj_run
```

### 3.3 启动并进入容器 (最终成功方案)

使用交互模式启动，直接进入终端。

**关键点 1**: 使用 `-it` 替代 `-d`，防止容器无后台进程导致"闪退"。

**关键点 2**: 去掉命令末尾的 `/bin/bash`，防止与镜像 Entrypoint 冲突。

```bash
sudo docker run -it \
  --name zyj_run \
  -p 8080:80 \
  zyj_mindspeed:latest
```

> 注：如需挂载 GPU 且驱动已安装，请添加 `--gpus all` 参数

## 4. 故障排查记录 (Troubleshooting)

在本次迁移过程中遇到的具体报错及解决方案汇总：

### ❌ 错误 1：权限被拒绝

**报错信息**:
```
open /data01/nlp/zyj/zyj_mindspeed.tar: permission denied
```

**原因**: 普通用户无权读取共享存储文件。

**解决**: 命令前添加 `sudo`。

---

### ❌ 错误 2：容器名称冲突

**报错信息**:
```
Conflict. The container name "/zyj_run" is already in use...
```

**原因**: 之前的启动尝试失败后，容器记录残留未删除。

**解决**: 执行 `sudo docker rm -f zyj_run` 强制删除。

---

### ❌ 错误 3：容器无法启动 (Not Running)

**报错信息**:
```
Error response from daemon: Container ... is not running
```

**原因**: 使用 `-d` (后台) 模式启动，镜像内无常驻进程，执行完毕即退出，导致无法 exec 进入。

**解决**: 改用 `docker run -it ...` 启动。

---

### ❌ 错误 4：二进制文件无法执行

**报错信息**:
```
/bin/bash: /bin/bash: cannot execute binary file
```

**原因**: 镜像本身已设置 Entrypoint 为 bash，命令末尾再次追加 `/bin/bash` 导致系统尝试运行"bash 脚本"，格式错误。

**解决**: 删除命令末尾的 `/bin/bash`。

---

### ❌ 错误 5：GPU 驱动报错 (可选)

**报错信息**:
```
could not select device driver "" with capabilities: [[gpu]]
```

**原因**: 目标节点未正确配置 NVIDIA Container Toolkit。

**解决**: 暂时去掉 `--gpus all` 参数以 CPU 模式运行调试。

---

## 5. 最佳实践总结

1. ✅ **权限管理**: 共享存储操作优先使用 `sudo`
2. ✅ **容器清理**: 启动前执行 `docker ps -a` 检查残留容器
3. ✅ **交互模式**: 调试阶段优先使用 `-it` 而非 `-d`
4. ✅ **命令简化**: 避免与镜像 Entrypoint 冲突，去掉冗余参数
5. ✅ **GPU 配置**: 生产环境需提前安装 `nvidia-docker2` 或 `nvidia-container-toolkit`

---

**文档更新日期**: 2025-12-03

