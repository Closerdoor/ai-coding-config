# Superpowers

**GitHub:** https://github.com/obra/superpowers

## 简介

**Superpowers** 是一个完整的AI编码代理软件开发方法论框架。它不是简单的命令集合，而是一套强制执行的开发流程，确保AI代理在写代码前先思考、规划、测试。

**核心特点：**
- **测试驱动开发 (TDD)** - 强制 RED-GREEN-REFACTOR 循环
- **系统化调试** - 4阶段根因分析流程
- **子代理驱动开发** - 自动分配任务给子代理并审查结果
- **Git Worktree 隔离** - 每个功能在独立分支开发

**适用场景：**
- 需要高质量代码输出的项目
- 复杂功能的迭代开发
- 团队协作需要规范化流程

---

## 如何安装

### Claude Code 安装

**方式一：官方插件市场（推荐）**
```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

**方式二：直接安装**
```bash
/plugin install superpowers@claude-plugins-official
```

### OpenCode 安装

**方式一：插件方式（推荐）**

编辑 `~/.config/opencode/opencode.json`，添加到 `plugin` 数组：
```json
{
  "plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]
}
```

重启 OpenCode 即可自动安装。

**固定版本：**
```json
{
  "plugin": ["superpowers@git+https://github.com/obra/superpowers.git#v5.0.3"]
}
```

### 手动安装（离线/企业网络）

**步骤1：下载文件**

在有网络的机器上下载仓库：
```bash
git clone --depth 1 https://github.com/obra/superpowers.git
```

或下载zip包：
```bash
curl -L https://github.com/obra/superpowers/archive/refs/heads/main.zip -o superpowers.zip
unzip superpowers.zip
```

**步骤2：传输文件**

将整个 `superpowers` 目录传输到目标机器。

**步骤3：放置到正确目录**

**OpenCode：**
```bash
# 创建目录
mkdir -p ~/.config/opencode/skills/superpowers

# 复制skills目录内容
cp -r superpowers/skills/* ~/.config/opencode/skills/superpowers/
```

**Claude Code：**
```bash
mkdir -p ~/.claude/skills/superpowers
cp -r superpowers/skills/* ~/.claude/skills/superpowers/
```

**步骤4：验证安装**

**OpenCode验证：**
```
# 在OpenCode中输入
use skill tool to list skills
```
应该能看到 `superpowers/brainstorming`、`superpowers/test-driven-development` 等技能。

**Claude Code验证：**
```
# 直接问
Tell me about your superpowers
```

---

## 如何使用

### 核心工作流

Superpowers 的工作流是**自动触发**的，你不需要手动调用命令。当AI检测到你在做某类任务时，会自动激活对应的skill。

```
用户请求 → AI分析任务类型 → 自动激活对应skill → 执行工作流
```

### 主要Skills列表

| Skill | 触发时机 | 作用 |
|-------|---------|------|
| `brainstorming` | 开始新功能前 | 苏格拉底式需求澄清，生成设计文档 |
| `writing-plans` | 设计确认后 | 生成详细实现计划，每个任务2-5分钟 |
| `test-driven-development` | 编写代码时 | 强制 RED-GREEN-REFACTOR 循环 |
| `systematic-debugging` | 遇到bug时 | 4阶段根因分析 |
| `subagent-driven-development` | 执行计划时 | 分配任务给子代理并审查 |
| `using-git-worktrees` | 开始开发时 | 创建隔离的工作分支 |
| `requesting-code-review` | 任务完成后 | 代码审查检查清单 |
| `finishing-a-development-branch` | 分支完成时 | 验证测试、合并决策 |

### 典型使用流程

**1. 开始新项目/功能**
```
你: 我想做一个用户认证系统
AI: [自动激活 brainstorming]
    - 你的目标用户是谁？
    - 需要支持哪些登录方式？
    - 安全要求是什么？
    ...
```

**2. 设计确认后**
```
AI: [自动激活 writing-plans]
    生成实现计划：
    - Task 1: 创建User模型 (2min)
    - Task 2: 实现密码哈希 (3min)
    - Task 3: 创建登录API (5min)
    ...
```

**3. 执行实现**
```
AI: [自动激活 subagent-driven-development]
    分配任务给子代理执行...
    每个任务完成后自动审查...
```

**4. 遇到问题**
```
你: 登录失败了
AI: [自动激活 systematic-debugging]
    1. 复现问题
    2. 收集证据
    3. 形成假设
    4. 验证修复
```

### 手动触发

如果需要手动触发某个skill：

**OpenCode：**
```
use skill tool to load superpowers/brainstorming
```

**Claude Code：**
```
Use the brainstorming skill to help me design this feature
```

### 注意事项

1. **不要跳过brainstorming** - 这是Superpowers的核心，强制AI在写代码前先思考
2. **信任流程** - 让AI完成整个工作流，不要中途打断
3. **审查计划** - 在 `writing-plans` 阶段仔细审查生成的计划
4. **TDD是强制的** - 如果代码没有测试，skill会拒绝标记为完成

---

## 更新

**OpenCode插件方式：** 重启OpenCode自动更新

**手动安装：** 重新下载并替换文件

---

## 相关链接

- GitHub: https://github.com/obra/superpowers
- 文档: https://github.com/obra/superpowers/blob/main/docs/README.opencode.md
- 问题反馈: https://github.com/obra/superpowers/issues
