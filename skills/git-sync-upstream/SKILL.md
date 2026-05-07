---
name: git-sync-upstream
description: Git 同步上游仓库 - 将官方 nanobot 更新同步到杰哥的 fork，同时保留本地改动。触发词：同步上游、同步仓库、git同步
---

# Git 上游同步流程

## 适用场景

- 官方 nanobot 有新更新，需要同步到杰哥的 fork
- 本地有特殊改动（xiaomimimo、start_hidden.bat 等），需要保留
- 需要同时保留：官方更新 + 本地改动 + 远程仓库

## 前置条件

| 配置 | 值 |
|:---|:---|
| upstream 仓库 | `https://github.com/HKUDS/nanobot.git` |
| origin 仓库 | `https://github.com/Lumjiel/nanobot.git` |
| 本地项目路径 | `E:\project\nanobot` |

## 执行流程

### 步骤 1：检查当前状态

```bash
cd /d E:\project\nanobot
git status
git log --oneline -5
```

**预期状态**：
- `working tree clean` - 没有未提交的改动
- 如果有未提交改动 → 先 stash

### 步骤 2：暂存本地改动

```bash
git stash push -m "jie local changes"
```

**保留的文件**：
- `nanobot/config/schema.py` - 小米 MiMo 模型
- `nanobot/providers/registry.py` - 小米 MiMo 配置
- `start_hidden.bat` - 隐藏启动脚本
- `QUICKSTART.md` - 快速入门文档
- `context_patch.txt` - 上下文补丁
- `nanobot_log.md.bak` - 操作日志备份

### 步骤 3：强制推送本地 main 到 origin

```bash
git push --force origin main
```

**作用**：
- 同步上游最新代码到 origin（因为本地 main 已经和 upstream/main 相同）
- origin/main 成为和 upstream/main 一样的干净状态

### 步骤 4：设置 upstream 追踪

```bash
git branch --set-upstream-to=origin/main main
```

### 步骤 5：恢复本地改动

```bash
git stash pop
```

### 步骤 6：提交本地改动

```bash
git add <修改的文件> <新增的文件>
git commit -m "feat: add jie's local customizations (xiaomimimo provider, docs)"
git push origin main
```

## 完整命令序列

```bash
# 1. 检查状态
cd /d E:\project\nanobot
git status

# 2. 如果有改动就暂存
git stash push -m "jie local changes"

# 3. 强制推送到 origin（同步上游）
git push --force origin main

# 4. 设置追踪
git branch --set-upstream-to=origin/main main

# 5. 恢复本地改动
git stash pop

# 6. 提交本地改动
git add nanobot/config/schema.py nanobot/providers/registry.py start_hidden.bat QUICKSTART.md context_patch.txt nanobot_log.md.bak
git commit -m "feat: add jie's local customizations"
git push origin main
```

## 注意事项

### 为什么用 --force？
origin/main 落后 upstream/main，本地 main 已经和 upstream/main 同步，需要强制覆盖。

### 冲突处理
如果在 rebase 过程中出现冲突，用 `git rebase --abort` 中止，然后手动合并。

### 代理问题
如果推送失败，检查代理：
```bash
git config --global http.proxy
git config --global https.proxy
```

如需设置代理：
```bash
git config --global http.proxy http://127.0.0.1:7897
git config --global https.proxy http://127.0.0.1:7897
```

## 验证

同步完成后检查：

```bash
# 本地和上游是否同步
git log upstream/main --oneline -3

# 本地和远程是否同步
git log origin/main --oneline -3

# 本地改动是否保留
git diff nanobot/config/schema.py
git diff nanobot/providers/registry.py
```

**预期结果**：
- 三个 log 输出相同
- diff 显示杰哥的 xiaomimimo 改动

## 版本历史

| 版本 | 日期 | 说明 |
|:---|:---|:---|
| v1.0.0 | 2026-04-20 | 初始版本 |