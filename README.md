# AI Coding Config

一键复制即可生效的 AI 编码代理配置文件集合。

## 快速开始

将以下 3 个文件/目录复制到你的项目根目录：

```
ai-coding-config/
├── .opencode/      # 复制到项目根目录
├── opencode.json   # 复制到项目根目录
└── AGENTS.md       # 复制到项目根目录
```

复制后启动 OpenCode 即可使用所有预置 skills。

## .opencode 目录内容

### 项目级 Skills（复制即用）

以下 skills 包含在 `.opencode/skills/` 目录中，复制后直接生效：

| Skill | 用途 | 触发场景 |
|-------|------|----------|
| **brainstorming** | 需求头脑风暴 | 创建功能前 |
| **git-commit** | Git 提交规范 | 提交代码时 |
| **pua** | 压力驱动调试 | 遇到困难时 |
| **caveman** | 压缩通信模式 | 需要简洁输出 |
| **impeccable** | 前端设计质量 | 构建 UI |
| **frontend-design** | 前端界面设计 | 创建页面/组件 |
| **shadcn** | shadcn 组件管理 | 使用 shadcn/ui |
| **ui-ux-pro-max** | UI/UX 设计指南 | 设计决策 |
| **mcp-builder** | MCP 服务器构建 | 创建 MCP 工具 |
| **skill-creator** | 创建新 skill | 开发自定义技能 |
| **browser-use** | 浏览器自动化 | 网页交互/测试 |
| **webapp-testing** | 本地 Web 应用测试 | Playwright 测试 |
| **canvas-design** | 视觉设计生成 | 创建海报/图片 |
| **algorithmic-art** | 算法艺术生成 | 生成艺术作品 |
| **claude-api** | Claude API 开发 | 使用 Anthropic SDK |
| **doc-coauthoring** | 文档协作 | 编写技术文档 |
| **brand-guidelines** | 品牌规范应用 | 统一视觉风格 |
| **theme-factory** | 主题样式工具 | 应用主题 |
| **slack-gif-creator** | Slack GIF 创建 | 制作动画 GIF |
| **web-artifacts-builder** | Web 构件构建 | 复杂 HTML artifacts |
| **internal-comms** | 内部沟通文档 | 状态报告/更新 |

### AGENTS.md

项目说明文件，AI 编码代理会读取此文件了解项目规则。请根据你的项目修改：

- 项目名称和简介
- 对话语言要求
- Git 提交规范
- 技术栈
- 开发命令

### opencode.json

OpenCode 配置文件，定义了：
- Plugin 列表
- 指令文件位置

---

## 全局安装 Skills

以下 skills 需要全局安装（包含编译二进制或大型框架），不在 `.opencode` 目录中：

### gstack

37 角色虚拟团队，提供 QA 测试、部署验证、代码审查等完整工作流。

**安装位置**: `~/.claude/skills/gstack/` 或 `~/.config/opencode/skills/gstack/`

```bash
# 克隆到全局目录
cd ~/.claude/skills
git clone https://github.com/garrytrask/gstack.git
cd gstack
./setup
```

**主要子技能**:

| 技能 | 用途 |
|------|------|
| `/qa` | QA 测试，查找并修复 bug |
| `/ship` | 完整发布流程 |
| `/investigate` | 系统化调试 |
| `/review` | PR 代码审查 |
| `/design-review` | 视觉设计审查 |
| `/plan-ceo-review` | CEO 模式计划审查 |
| `/plan-eng-review` | 工程架构审查 |
| `/retro` | 周回顾 |
| `/health` | 代码质量检查 |

详细安装步骤: [global-skills/gstack/INSTALL.md](./global-skills/gstack/INSTALL.md)

### get-shit-done (GSD)

完整的 AI 辅助开发框架，提供项目管理、阶段规划、执行验证等企业级工作流。

**安装位置**: `~/.claude/skills/get-shit-done/` 或 `~/.config/opencode/skills/get-shit-done/`

```bash
# 克隆到全局目录
cd ~/.claude/skills
git clone https://github.com/gsd-build/get-shit-done.git
cd get-shit-done
npm install
```

**主要命令**: `/gsd-new-project`, `/gsd-plan-phase`, `/gsd-execute-phase`, `/gsd-ship`

详细安装步骤: [global-skills/get-shit-done/INSTALL.md](./global-skills/get-shit-done/INSTALL.md)

---

## Plugin 安装

通过 `opencode.json` 配置的插件，OpenCode 启动时自动从 Git 仓库安装：

| Plugin | GitHub | 用途 |
|--------|--------|------|
| **superpowers** | https://github.com/obra/superpowers | TDD 方法论 |
| **oh-my-openagent** | https://github.com/code-yeongyu/oh-my-openagent | 多智能体编排 |

插件已在 `opencode.json` 中配置，无需手动安装。

---

## 目录结构

```
ai-coding-config/
├── .opencode/                  # 复制到项目根目录
│   └── skills/                 # 21 个项目级 skills
├── opencode.json               # 复制到项目根目录
├── AGENTS.md                   # 复制到项目根目录
├── README.md                   # 本文档
├── docs/                       # Skills 详细文档
│   ├── gstack.md
│   ├── get-shit-done.md
│   ├── pua.md
│   └── ...
├── global-skills/              # 全局安装说明（不需要复制）
│   ├── gstack/INSTALL.md
│   └── get-shit-done/INSTALL.md
└── plugins/
    └── README.md
```

## 自定义

### 添加新 Skill

将 skill 文件夹复制到 `.opencode/skills/`，确保包含 `SKILL.md` 文件。

### 修改 AGENTS.md

根据项目修改：
- 项目简介
- 技术栈
- 开发命令
- 编码规范

## 兼容性

| 工具 | 配置目录 |
|------|----------|
| OpenCode | `.opencode/` |
| Claude Code | `.claude/`（重命名目录即可） |

## 许可证

各 skill 保持原有许可证。本配置集合采用 MIT 许可证。
