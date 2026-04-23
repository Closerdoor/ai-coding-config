# Spec Kit

**GitHub:** https://github.com/github/spec-kit

## 简介

**Spec Kit** 是 GitHub 官方的规格驱动开发工具包，帮助你在写代码前先专注于产品场景和可预测的结果。规格变成可执行的，直接生成可工作的实现。

**核心特点：**
- **规格优先** - 规格成为可执行文档，而非丢弃的脚手架
- **完整工具链** - 从规格到实现到验证的完整流程
- **扩展系统** - 社区扩展和预设支持
- **多AI支持** - 支持 30+ AI 编码代理

**适用场景：**
- 需要严格规格驱动的项目
- 企业级开发流程
- 团队协作开发

---

## 如何安装

### 安装 Specify CLI

**方式一：持久安装（推荐）**
```bash
# 安装特定版本（推荐）
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@v1.3.0

# 或安装最新版
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

**方式二：一次性使用**
```bash
uvx --from git+https://github.com/github/spec-kit.git@v1.3.0 specify init <PROJECT_NAME>
```

### 初始化项目

```bash
# 新项目
specify init <PROJECT_NAME>

# 现有项目
specify init . --ai copilot
# 或
specify init --here --ai copilot
```

### OpenCode 安装

```bash
specify init . --ai opencode
```

### 手动安装（离线/企业网络）

**步骤1：下载文件**

在有网络的机器上：
```bash
# 下载仓库
git clone https://github.com/github/spec-kit.git

# 或下载zip
curl -L https://github.com/github/spec-kit/archive/refs/heads/main.zip -o spec-kit.zip
unzip spec-kit.zip
mv spec-kit-main spec-kit
```

**步骤2：安装依赖**

需要 Python 和 uv：
```bash
# 安装 uv（如果没有）
pip install uv

# 安装 specify-cli
cd spec-kit
uv tool install . --from .
```

**步骤3：初始化项目**

```bash
cd your-project
specify init .
```

**步骤4：验证安装**

```bash
specify version
specify check
```

---

## 如何使用

### 核心工作流

Spec Kit 的核心工作流：

```
constitution → specify → plan → tasks → implement
```

### 主要命令

**核心命令：**
| 命令 | Agent Skill | 作用 |
|------|-------------|------|
| `/speckit.constitution` | `speckit-constitution` | 创建项目原则和开发指南 |
| `/speckit.specify` | `speckit-specify` | 定义要构建什么（需求） |
| `/speckit.plan` | `speckit-plan` | 创建技术实现计划 |
| `/speckit.tasks` | `speckit-tasks` | 生成可执行任务列表 |
| `/speckit.implement` | `speckit-implement` | 执行所有任务 |

**可选命令：**
| 命令 | 作用 |
|------|------|
| `/speckit.clarify` | 澄清未明确的区域（规划前推荐） |
| `/speckit.analyze` | 跨文档一致性和覆盖分析 |
| `/speckit.checklist` | 生成自定义质量检查清单 |

### 典型使用流程

**场景：新项目开发**

```
# 1. 建立项目原则
/speckit.constitution Create principles focused on code quality, testing standards

# 生成 .specify/constitution.md

# 2. 定义规格
/speckit.specify Build a photo album application that organizes photos by date

# AI 提问澄清需求
# 生成 .specify/features/<feature>/spec.md

# 3. 澄清（可选但推荐）
/speckit.clarify

# 4. 创建计划
/speckit.plan Use Vite with vanilla HTML, CSS, JavaScript. Store metadata in SQLite

# 生成 .specify/features/<feature>/plan.md

# 5. 生成任务
/speckit.tasks

# 生成 .specify/features/<feature>/tasks.md

# 6. 执行实现
/speckit.implement

# AI 执行所有任务
```

### 生成的文件结构

```
.specify/
├── constitution.md     # 项目原则
├── features/
│   └── <feature>/
│       ├── spec.md     # 功能规格
│       ├── plan.md     # 实现计划
│       └── tasks.md    # 任务列表
├── extensions/         # 扩展
├── presets/            # 预设
└── templates/          # 模板
```

### CLI 命令

```bash
# 初始化
specify init <PROJECT_NAME>
specify init . --ai copilot

# 检查工具
specify check

# 版本
specify version

# 扩展管理
specify extension search
specify extension add <name>

# 预设管理
specify preset search
specify preset add <name>

# 更新代理指令
specify update
```

### 扩展系统

Spec Kit 支持社区扩展：

```bash
# 搜索扩展
specify extension search

# 安装扩展
specify extension add spec-kit-jira
```

**热门扩展：**
- `spec-kit-jira` - Jira 集成
- `spec-kit-review` - 代码审查
- `spec-kit-verify` - 实现验证
- `spec-kit-archive` - 归档合并功能

### 预设系统

预设自定义 Spec Kit 的行为：

```bash
# 搜索预设
specify preset search

# 安装预设
specify preset add <name>
```

### 注意事项

1. **需要 Python** - Spec Kit 是 Python 项目
2. **需要 uv** - Python 包管理器
3. **官方包只在 GitHub** - PyPI 上的同名包不是官方维护
4. **固定版本** - 生产环境建议固定版本号

---

## 更新

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git@vX.Y.Z
```

---

## 相关链接

- GitHub: https://github.com/github/spec-kit
- 文档: https://github.github.io/spec-kit
- 扩展网站: https://speckit-community.github.io/extensions/
