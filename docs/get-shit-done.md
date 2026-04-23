# Get Shit Done (GSD)

**GitHub:** https://github.com/gsd-build/get-shit-done

## 简介

**Get Shit Done (GSD)** 是一个轻量级但强大的元提示、上下文工程和规格驱动开发系统。它解决了"上下文腐烂"问题——即随着 Claude 填充上下文窗口导致的质量下降。

**核心特点：**
- **上下文工程** - 自动管理上下文，保持质量
- **XML提示格式** - 结构化任务定义，精确无歧义
- **多代理编排** - 并行执行，每个任务独立上下文
- **原子Git提交** - 每个任务独立提交，可追溯

**适用场景：**
- 中大型项目开发
- 需要规范化开发流程
- 多功能并行开发

---

## 如何安装

### Claude Code 安装

```bash
npx get-shit-done-cc@latest
```

安装时选择：
1. 运行时：Claude Code
2. 位置：Global（全局）或 Local（项目级）

### OpenCode 安装

```bash
npx get-shit-done-cc --opencode --global
```

**非交互式安装：**
```bash
# 全局安装
npx get-shit-done-cc --opencode --global

# 项目级安装
npx get-shit-done-cc --opencode --local
```

### 手动安装（离线/企业网络）

**步骤1：下载文件**

在有网络的机器上：
```bash
# 下载仓库
git clone https://github.com/gsd-build/get-shit-done.git

# 或下载zip
curl -L https://github.com/gsd-build/get-shit-done/archive/refs/heads/main.zip -o gsd.zip
unzip gsd.zip
mv get-shit-done-main get-shit-done
```

**步骤2：需要打包的目录**

```
get-shit-done/
├── get-shit-done/     # 核心skills
├── hooks/             # 钩子脚本
├── bin/               # 安装脚本
├── agents/            # 代理定义
└── package.json
```

**步骤3：放置到正确目录**

**OpenCode：**
```bash
mkdir -p ~/.config/opencode/skills/get-shit-done
cp -r get-shit-done/get-shit-done/* ~/.config/opencode/skills/get-shit-done/
```

**Claude Code：**
```bash
mkdir -p ~/.claude/skills/get-shit-done
cp -r get-shit-done/get-shit-done/* ~/.claude/skills/get-shit-done/
```

**步骤4：验证安装**

```
/gsd-help
```
看到 GSD 帮助信息说明安装成功。

---

## 如何使用

### 核心工作流

GSD 的核心工作流是一个完整的开发周期：

```
初始化 → 讨论 → 规划 → 执行 → 验证 → 发布 → 完成
```

### 主要命令列表

**项目初始化：**
| 命令 | 作用 |
|------|------|
| `/gsd-new-project` | 初始化项目：提问→研究→需求→路线图 |
| `/gsd-map-codebase` | 分析现有代码库（棕地项目） |

**阶段工作：**
| 命令 | 作用 |
|------|------|
| `/gsd-discuss-phase <N>` | 捕获实现决策 |
| `/gsd-plan-phase <N>` | 研究+规划+验证 |
| `/gsd-execute-phase <N>` | 执行所有计划 |
| `/gsd-verify-work <N>` | 用户验收测试 |

**发布：**
| 命令 | 作用 |
|------|------|
| `/gsd-ship <N>` | 创建PR |
| `/gsd-complete-milestone` | 完成里程碑 |
| `/gsd-new-milestone` | 开始新里程碑 |

**快速任务：**
| 命令 | 作用 |
|------|------|
| `/gsd-quick` | 快速执行临时任务 |
| `/gsd-fast <text>` | 内联执行简单任务 |
| `/gsd-do <text>` | 自动路由到正确的GSD命令 |

**导航：**
| 命令 | 作用 |
|------|------|
| `/gsd-progress` | 查看当前进度 |
| `/gsd-next` | 自动检测并执行下一步 |
| `/gsd-help` | 显示所有命令 |

### 典型使用流程

**场景：新项目开发**

```
# 1. 初始化项目
/gsd-new-project
> 我想做一个任务管理应用

# AI 会提问直到完全理解需求
# 然后生成：PROJECT.md, REQUIREMENTS.md, ROADMAP.md

# 2. 讨论第一阶段
/gsd-discuss-phase 1

# 捕获实现偏好和决策

# 3. 规划第一阶段
/gsd-plan-phase 1

# 生成详细任务计划

# 4. 执行第一阶段
/gsd-execute-phase 1

# 并行执行所有任务，每个任务独立上下文

# 5. 验证工作
/gsd-verify-work 1

# 手动验收测试

# 6. 发布
/gsd-ship 1

# 创建PR

# 7. 继续下一阶段
/gsd-discuss-phase 2
...
```

**场景：快速任务**

```
/gsd-quick
> 添加深色模式切换

# AI 快速执行，跳过可选步骤
```

**场景：现有代码库**

```
# 先分析代码库
/gsd-map-codebase

# 然后初始化项目（会使用分析结果）
/gsd-new-project
```

### 配置

编辑 `.planning/config.json`：

```json
{
  "mode": "interactive",
  "granularity": "standard",
  "model_profile": "balanced"
}
```

**模型配置：**
| Profile | 规划 | 执行 | 验证 |
|---------|------|------|------|
| quality | Opus | Opus | Sonnet |
| balanced | Opus | Sonnet | Sonnet |
| budget | Sonnet | Sonnet | Haiku |
| inherit | 继承 | 继承 | 继承 |

切换配置：
```
/gsd-set-profile budget
```

### 生成的文件结构

```
.planning/
├── PROJECT.md          # 项目愿景
├── REQUIREMENTS.md     # 需求规格
├── ROADMAP.md          # 开发路线图
├── STATE.md            # 当前状态
├── research/           # 研究文档
├── todos/              # 待办事项
└── config.json         # 配置
```

### 注意事项

1. **推荐跳过权限模式** - 使用 `claude --dangerously-skip-permissions` 获得流畅体验
2. **信任流程** - 让 AI 完成整个工作流
3. **审查计划** - 在执行前仔细审查生成的计划
4. **定期更新** - GSD 更新频繁，建议定期运行安装命令更新

---

## 更新

```bash
npx get-shit-done-cc@latest
```

---

## 相关链接

- GitHub: https://github.com/gsd-build/get-shit-done
- npm: https://www.npmjs.com/package/get-shit-done-cc
- Discord: https://discord.gg/mYgfVNfA2r
- X: https://x.com/gsd_foundation
