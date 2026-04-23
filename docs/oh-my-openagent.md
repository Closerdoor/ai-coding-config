# Oh My OpenAgent (OmO)

**GitHub:** https://github.com/code-yeongyu/oh-my-openagent

## 简介

**Oh My OpenAgent**（原名 Oh My OpenCode）是 OpenCode 的增强插件，将 OpenCode 转变为一个多智能体协作系统。它提供了"纪律代理"——不会半途而废的 AI 代理。

**核心特点：**
- **ultrawork** - 一个命令激活所有代理
- **多智能体编排** - Sisyphus、Hephaestus、Prometheus 等协作
- **Hash-Anchored Edit** - 行内容哈希验证，零错误编辑
- **LSP + AST-Grep** - IDE 级别的代码操作能力

**适用场景：**
- OpenCode 用户想要更强的代理能力
- 需要多模型协作的项目
- 复杂功能的端到端开发

---

## 如何安装

### 安装方式

在 AI 代理中粘贴以下提示：

```
Install and configure oh-my-opencode by following the instructions here:
https://raw.githubusercontent.com/code-yeongyu/oh-my-openagent/refs/heads/dev/docs/guide/installation.md
```

### OpenCode 安装

**方式一：插件方式（推荐）**

编辑 `~/.config/opencode/opencode.json`：
```json
{
  "plugin": ["oh-my-openagent@git+https://github.com/code-yeongyu/oh-my-openagent.git"]
}
```

重启 OpenCode。

**方式二：npm 安装**
```bash
bunx oh-my-opencode
```

### 手动安装（离线/企业网络）

**步骤1：下载文件**

在有网络的机器上：
```bash
# 下载仓库
git clone https://github.com/code-yeongyu/oh-my-openagent.git

# 或下载zip
curl -L https://github.com/code-yeongyu/oh-my-openagent/archive/refs/heads/dev.zip -o omo.zip
unzip omo.zip
mv oh-my-openagent-dev oh-my-openagent
```

**步骤2：需要打包的目录**

```
oh-my-openagent/
├── .opencode/         # OpenCode配置
├── packages/          # 核心包
├── src/               # 源码
├── bin/               # 可执行文件
├── package.json
└── postinstall.mjs    # 安装后脚本
```

**步骤3：安装**

```bash
cd oh-my-openagent
bun install
bun run build
```

**步骤4：配置 OpenCode**

编辑 `~/.config/opencode/opencode.json`：
```json
{
  "plugin": ["oh-my-openagent"]
}
```

将编译后的文件复制到 OpenCode 插件目录。

**步骤5：验证安装**

```bash
bunx oh-my-opencode doctor
```

或在 OpenCode 中：
```
ultrawork
```

---

## 如何使用

### 核心命令

**ultrawork**
```
ultrawork
# 或简写
ulw
```
一个命令激活所有代理，持续工作直到任务完成。

### 纪律代理

**Sisyphus** - 主编排者
- 模型：`claude-opus-4-7` / `kimi-k2.5` / `glm-5`
- 角色：规划、委托、驱动任务完成
- 特点：不会半途而废

**Hephaestus** - 深度工作者
- 模型：`gpt-5.4`
- 角色：自主探索和执行
- 特点：给目标而非配方

**Prometheus** - 战略规划者
- 模型：`claude-opus-4-7` / `kimi-k2.5` / `glm-5`
- 角色：面试式需求澄清
- 特点：提问识别范围

### 代理分类

当 Sisyphus 委托任务时，会自动选择正确的模型：

| 类别 | 用途 |
|------|------|
| `visual-engineering` | 前端、UI/UX、设计 |
| `deep` | 自主研究+执行 |
| `quick` | 单文件修改、拼写错误 |
| `ultrabrain` | 困难逻辑、架构决策 |

### 主要功能

**Hash-Anchored Edit**
- 每行带有内容哈希标签
- 编辑时验证哈希，防止错误修改
- Grok Code Fast 成功率从 6.7% 提升到 68.3%

**LSP 工具**
- `lsp_rename` - 重命名
- `lsp_goto_definition` - 跳转定义
- `lsp_find_references` - 查找引用
- `lsp_diagnostics` - 诊断

**AST-Grep**
- 模式感知的代码搜索和重写
- 支持 25 种语言

**Tmux 集成**
- 完整的交互式终端
- 支持 REPL、调试器、TUI 应用

**内置 MCP**
- `websearch` (Exa) - 网页搜索
- `context7` - 官方文档
- `grep_app` - GitHub 代码搜索

### 典型使用流程

**场景：开发新功能**

```
# 一键启动
ultrawork
> 实现用户认证系统

# Sisyphus 自动：
# 1. 分析任务
# 2. 委托给合适的代理
# 3. 并行执行
# 4. 审查结果
# 5. 持续直到完成
```

**场景：深度初始化**

```
/init-deep

# 自动生成层级 AGENTS.md 文件：
# project/
# ├── AGENTS.md
# ├── src/
# │   └── AGENTS.md
# └── components/
#     └── AGENTS.md
```

**场景：规划复杂任务**

```
/start-work

# Prometheus 面试你：
# - 你的目标用户是谁？
# - 需要支持哪些场景？
# - 有什么技术约束？
# ...
# 然后生成详细计划
```

### 配置

编辑 `~/.config/opencode/oh-my-openagent.json`：

```json
{
  "agents": {
    "sisyphus": {
      "model": "claude-opus-4-7",
      "temperature": 0.7
    },
    "hephaestus": {
      "model": "gpt-5.4"
    }
  },
  "categories": {
    "visual-engineering": "claude-sonnet-4",
    "deep": "gpt-5.4",
    "quick": "claude-haiku-3.5"
  }
}
```

### 注意事项

1. **需要 Bun** - 运行时依赖
2. **多模型支持** - 可以混合使用不同提供商的模型
3. **Claude Code 兼容** - 所有 hooks、commands、skills 都兼容
4. **自动更新** - 重启 OpenCode 时自动检查更新

---

## 更新

```bash
bunx oh-my-opencode
```

---

## 相关链接

- GitHub: https://github.com/code-yeongyu/oh-my-openagent
- 官网: https://ohmyopenagent.com
- Discord: https://discord.gg/PUwSMR9XNk
- X: https://x.com/justsisyphus
