# OpenSpec

**GitHub:** https://github.com/Fission-AI/OpenSpec

## 简介

**OpenSpec** 是一个规格驱动开发（SDD）框架，让 AI 编码助手在写代码前先对齐规格。它提供轻量级的规格层，让你和 AI 在构建前达成一致。

**核心特点：**
- **构建前对齐** - 人类和AI先对齐规格再写代码
- **流畅迭代** - 无刚性阶段门，随时更新任何文档
- **多工具支持** - 支持 25+ AI 编码助手
- **变更文件夹** - 每个变更独立的文件夹结构

**适用场景：**
- 需要规格驱动开发的项目
- 团队协作需要文档化
- 复杂功能迭代开发

---

## 如何安装

### Claude Code 安装

```bash
npm install -g @fission-ai/openspec@latest
```

然后在项目中初始化：
```bash
cd your-project
openspec init
```

### OpenCode 安装

```bash
npm install -g @fission-ai/openspec@latest
openspec init
```

更新代理指令：
```bash
openspec update
```

### 手动安装（离线/企业网络）

**步骤1：下载文件**

在有网络的机器上：
```bash
# 下载仓库
git clone https://github.com/Fission-AI/OpenSpec.git

# 或下载zip
curl -L https://github.com/Fission-AI/OpenSpec/archive/refs/heads/main.zip -o openspec.zip
unzip openspec.zip
mv OpenSpec-main OpenSpec
```

**步骤2：需要打包的内容**

```
OpenSpec/
├── openspec/           # 核心模块
├── schemas/            # JSON schemas
├── bin/                # CLI入口
├── package.json
└── AGENTS.md           # 代理指令
```

**步骤3：安装**

```bash
cd OpenSpec
npm install
npm link  # 创建全局命令
```

**步骤4：初始化项目**

```bash
cd your-project
openspec init
```

**步骤5：验证安装**

```bash
openspec version
```

在 AI 中测试：
```
/opsx:propose test-feature
```

---

## 如何使用

### 核心工作流

OpenSpec 使用**变更提案**工作流：

```
propose → apply → archive
```

### 主要命令

**提案命令：**
| 命令 | 作用 |
|------|------|
| `/opsx:propose <name>` | 创建新的变更提案 |
| `/opsx:new` | 新建提案（扩展工作流） |
| `/opsx:continue` | 继续现有提案 |

**执行命令：**
| 命令 | 作用 |
|------|------|
| `/opsx:apply` | 应用当前提案 |
| `/opsx:ff` | 快速前进 |
| `/opsx:verify` | 验证实现 |

**管理命令：**
| 命令 | 作用 |
|------|------|
| `/opsx:sync` | 同步规格和实现 |
| `/opsx:archive` | 归档完成的变更 |
| `/opsx:bulk-archive` | 批量归档 |
| `/opsx:onboard` | 项目入职指南 |

### 典型使用流程

**场景：添加新功能**

```
# 1. 创建提案
/opsx:propose add-dark-mode

# OpenSpec 创建变更文件夹：
# openspec/changes/add-dark-mode/
# ├── proposal.md    # 为什么做，改什么
# ├── specs/         # 需求和场景
# ├── design.md      # 技术方案
# └── tasks.md       # 实现清单

# 2. AI 自动填充提案内容
# 你可以编辑任何文件来调整

# 3. 应用提案
/opsx:apply

# AI 开始实现任务...

# 4. 归档
/opsx:archive

# 变更移动到归档目录
```

**场景：迭代开发**

```
# 创建提案
/opsx:propose user-auth

# 查看生成的规格
# 编辑 openspec/changes/user-auth/specs/

# 调整设计
# 编辑 openspec/changes/user-auth/design.md

# 应用
/opsx:apply
```

### 生成的文件结构

```
openspec/
├── changes/
│   ├── add-dark-mode/
│   │   ├── proposal.md
│   │   ├── specs/
│   │   ├── design.md
│   │   └── tasks.md
│   └── archive/
│       └── 2025-01-23-add-dark-mode/
└── config.json
```

### CLI 命令

```bash
# 初始化
openspec init

# 更新代理指令
openspec update

# 查看版本
openspec version

# 配置
openspec config profile

# 检查安装
openspec check
```

### 配置

编辑 `openspec/config.json`：

```json
{
  "profile": "default",
  "language": "en"
}
```

### 注意事项

1. **推荐高推理模型** - Opus 4.5 和 GPT 5.2 效果最好
2. **保持上下文清洁** - 实现前清空上下文窗口
3. **随时迭代** - 可以随时更新任何文档，无阶段门限制
4. **多语言支持** - 支持多种语言的规格生成

---

## 更新

```bash
npm install -g @fission-ai/openspec@latest
openspec update
```

---

## 相关链接

- GitHub: https://github.com/Fission-AI/OpenSpec
- npm: https://www.npmjs.com/package/@fission-ai/openspec
- 文档: https://openspec.dev
- Discord: https://discord.gg/YctCnvvshC
