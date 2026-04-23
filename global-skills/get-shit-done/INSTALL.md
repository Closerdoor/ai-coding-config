# get-shit-done (GSD) 安装指南

GSD 是一个完整的 AI 辅助开发框架，提供项目管理、阶段规划、执行验证等企业级工作流。

## 特点

- **完整项目管理**：里程碑、阶段、工作流
- **多智能体协作**：planner、executor、verifier 等角色
- **规划文档生成**：自动生成 SPEC、PLAN、REVIEW 等
- **验证驱动**：每个阶段都有验证步骤

## 全局安装

GSD 需要全局安装，因为包含大量模板、工作流和工具脚本。

### 安装步骤

```bash
# 1. 克隆到全局目录
cd ~/.claude/skills  # Claude Code
# 或
cd ~/.config/opencode/skills  # OpenCode

git clone https://github.com/gsd-build/get-shit-done.git

# 2. 安装依赖
cd get-shit-done
npm install  # 或 bun install

# 3. 验证安装
node bin/gsd-tools.cjs --version
```

### Windows 用户

```powershell
cd $env:USERPROFILE\.claude\skills
git clone https://github.com/gsd-build/get-shit-done.git
cd get-shit-done
npm install
```

## 主要命令

| 命令 | 用途 |
|------|------|
| `/gsd-new-project` | 创建新项目 |
| `/gsd-new-milestone` | 创建里程碑 |
| `/gsd-plan-phase` | 规划阶段 |
| `/gsd-execute-phase` | 执行阶段 |
| `/gsd-verify-phase` | 验证阶段 |
| `/gsd-ship` | 发布 |
| `/gsd-health` | 健康检查 |
| `/gsd-retro` | 回顾 |

## 工作流

GSD 使用阶段化工作流：

```
discovery → research → plan → execute → verify → ship
```

每个阶段都有：
- 输入文档（SPEC、PLAN 等）
- 执行步骤
- 验证标准
- 输出文档

## 目录结构

项目中的 `.planning/` 目录：

```
.planning/
├── intel/           # 项目情报
├── milestones/      # 里程碑定义
├── phases/          # 阶段计划
├── reviews/         # 审查报告
└── learnings/       # 学习记录
```

## 配置

在项目根目录创建 `CLAUDE.md` 或 `AGENTS.md`：

```markdown
## GSD 配置

- 默认模型：claude-sonnet-4-20250514
- 验证严格度：high
- 文档语言：zh-CN
```

## 更新

```bash
cd ~/.claude/skills/get-shit-done
git pull
npm install
```

## 官方链接

- GitHub: https://github.com/gsd-build/get-shit-done
