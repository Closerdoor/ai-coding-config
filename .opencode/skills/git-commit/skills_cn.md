---
name: git-commit
description: '执行 git 提交，包含规范提交信息分析、智能暂存和消息生成。当用户要求提交更改、创建 git 提交或提到 "/commit" 时使用。支持：(1) 从更改中自动检测类型和范围，(2) 从 diff 生成规范提交信息，(3) 交互式提交，可选择覆盖类型/范围/描述，(4) 智能文件暂存以实现逻辑分组'
license: MIT
allowed-tools: Bash
---

# Git 提交与规范提交信息

## 概述

使用规范提交（Conventional Commits）规范创建标准化、语义化的 git 提交。分析实际的 diff 来确定合适的类型、范围和消息。

## 规范提交格式

```
<类型>[可选范围]: <描述>

[可选正文]

[可选页脚]
```

## 提交类型

| 类型       | 用途                        |
| ---------- | ------------------------------ |
| `feat`     | 新功能                    |
| `fix`      | Bug 修复                    |
| `docs`     | 仅文档更改             |
| `style`    | 格式/样式（不影响逻辑）    |
| `refactor` | 代码重构（非功能/修复） |
| `perf`     | 性能优化        |
| `test`     | 添加/更新测试               |
| `build`    | 构建系统/依赖项      |
| `ci`       | CI/配置更改              |
| `chore`    | 维护/杂项               |
| `revert`   | 回退提交                  |

## 破坏性更改

```
# 类型/范围后加感叹号
feat!: 移除已弃用的端点

# BREAKING CHANGE 页脚
feat: 允许配置扩展其他配置

BREAKING CHANGE: `extends` 键的行为已更改
```

## 工作流程

### 1. 分析 Diff

```bash
# 如果文件已暂存，使用暂存的 diff
git diff --staged

# 如果没有暂存的内容，使用工作区 diff
git diff

# 同时检查状态
git status --porcelain
```

### 2. 暂存文件（如需要）

如果没有暂存的内容或想要以不同方式分组更改：

```bash
# 暂存特定文件
git add path/to/file1 path/to/file2

# 按模式暂存
git add *.test.*
git add src/components/*

# 交互式暂存
git add -p
```

**永远不要提交机密**（.env、credentials.json、私钥）。

### 3. 生成提交信息

分析 diff 来确定：

- **类型**：这是什么类型的更改？
- **范围**：影响哪个区域/模块？
- **描述**：更改内容的单行摘要（现在时、祈使语气、<72 字符）

### 4. 执行提交

```bash
# 单行
git commit -m "<类型>[范围]: <描述>"

# 多行，包含正文/页脚
git commit -m "$(cat <<'EOF'
<类型>[范围]: <描述>

<可选正文>

<可选页脚>
EOF
)"
```

## 最佳实践

- 每次提交一个逻辑更改
- 使用现在时："add" 而非 "added"
- 使用祈使语气："fix bug" 而非 "fixes bug"
- 引用问题：`Closes #123`、`Refs #456`
- 描述保持在 72 字符以内

## Git 安全协议

- 永远不要更新 git 配置
- 未经明确要求，永远不要运行破坏性命令（--force、hard reset）
- 除非用户要求，否则不要跳过钩子（--no-verify）
- 永远不要强制推送到 main/master
- 如果提交因钩子失败，修复并创建新提交（不要修改）
