# OpenCode 使用指南

## 简介

**OpenCode** 是一个开源的 AI 编码代理，支持终端界面、桌面应用和 IDE 扩展。核心特点：

- **LSP 支持** - 自动加载正确的 LSP
- **多会话** - 同一项目并行启动多个代理
- **分享链接** - 分享会话供参考或调试
- **75+ LLM 提供商** - 通过 Models.dev 支持本地模型
- **GitHub Copilot / ChatGPT Plus** - 使用现有订阅

## 安装

### 推荐方式

```bash
curl -fsSL https://opencode.ai/install | bash
```

### 其他方式

```bash
# npm
npm install -g opencode-ai

# bun
bun install -g opencode-ai

# Homebrew (macOS/Linux)
brew install anomalyco/tap/opencode

# Windows (Chocolatey)
choco install opencode

# Windows (Scoop)
scoop install opencode
```

## 配置

### 配置文件位置

| 文件 | 位置 | 用途 |
|------|------|------|
| `opencode.json` | 项目根目录 | 项目级配置 |
| `opencode.json` | `~/.config/opencode/` | 全局配置 |
| `tui.json` | 项目根目录 | TUI 界面配置 |
| `AGENTS.md` | 项目根目录 | 项目规则/指令 |

### 配置优先级（从低到高）

1. Remote config (`.well-known/opencode`)
2. Global config (`~/.config/opencode/opencode.json`)
3. Custom config (`OPENCODE_CONFIG` 环境变量)
4. Project config (`opencode.json`)
5. `.opencode/` 目录
6. Inline config (`OPENCODE_CONFIG_CONTENT`)

### opencode.json 完整示例

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  
  // 模型配置
  "model": "anthropic/claude-sonnet-4-5",
  "small_model": "anthropic/claude-haiku-4-5",
  
  // Provider 配置
  "provider": {
    "anthropic": {
      "options": {
        "apiKey": "{env:ANTHROPIC_API_KEY}",
        "timeout": 600000
      }
    }
  },
  
  // 代理配置
  "agent": {
    "code-reviewer": {
      "description": "代码审查代理",
      "mode": "subagent",
      "model": "anthropic/claude-sonnet-4-5",
      "prompt": "你是代码审查专家，关注安全性、性能和可维护性。",
      "permission": {
        "edit": "deny",
        "bash": "ask"
      }
    }
  },
  
  // 自定义命令
  "command": {
    "test": {
      "template": "运行测试套件并显示覆盖率报告。",
      "description": "运行测试",
      "agent": "build"
    }
  },
  
  // MCP 服务器
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp"
    }
  },
  
  // 插件
  "plugin": ["opencode-helicone-session"],
  
  // 权限
  "permission": {
    "bash": { "*": "ask", "git *": "allow" },
    "edit": "ask"
  },
  
  // 指令文件
  "instructions": ["CONTRIBUTING.md", "docs/guidelines.md"],
  
  // 其他选项
  "autoupdate": true,
  "share": "manual"
}
```

### tui.json 配置

```json
{
  "$schema": "https://opencode.ai/tui.json",
  "theme": "opencode",
  "keybinds": {
    "leader": "ctrl+x"
  },
  "scroll_speed": 3,
  "diff_style": "auto",
  "mouse": true
}
```

### 环境变量

| 变量 | 说明 |
|------|------|
| `OPENCODE_CONFIG` | 自定义配置文件路径 |
| `OPENCODE_CONFIG_DIR` | 自定义配置目录 |
| `OPENCODE_TUI_CONFIG` | TUI 配置文件路径 |
| `OPENCODE_SERVER_PASSWORD` | serve/web 模式的基本认证密码 |
| `OPENCODE_ENABLE_EXA` | 启用 Exa 网页搜索 |

## 基本使用

### 启动

```bash
# 当前目录
opencode

# 指定目录
opencode /path/to/project

# 继续上次会话
opencode --continue

# 指定模型
opencode --model anthropic/claude-sonnet-4-5
```

### CLI 命令

| 命令 | 说明 |
|------|------|
| `opencode run "prompt"` | 非交互模式运行 |
| `opencode serve` | 启动 HTTP API 服务器 |
| `opencode web` | 启动 Web 界面 |
| `opencode models` | 列出可用模型 |
| `opencode auth login` | 添加 Provider |
| `opencode session list` | 列出会话 |
| `opencode stats` | 显示使用统计 |

### TUI 斜杠命令

| 命令 | 快捷键 | 说明 |
|------|--------|------|
| `/init` | `ctrl+x i` | 创建/更新 AGENTS.md |
| `/undo` | `ctrl+x u` | 撤销上次修改 |
| `/redo` | `ctrl+x r` | 重做 |
| `/connect` | - | 添加 Provider |
| `/models` | `ctrl+x m` | 列出可用模型 |
| `/share` | `ctrl+x s` | 分享会话 |
| `/compact` | `ctrl+x c` | 压缩上下文 |
| `/sessions` | `ctrl+x l` | 会话列表 |
| `/new` | `ctrl+x n` | 新建会话 |
| `/exit` | `ctrl+x q` | 退出 |

### 特殊语法

```bash
# 文件引用（模糊搜索）
@packages/functions/src/api/index.ts

# 执行 shell 命令
!git status

# @ 提及子代理
@general help me search for this function
```

## 核心功能

### Skills（技能）

技能是可复用的指令定义，通过 `skill` 工具按需加载。

**位置：**
- `.opencode/skills/<name>/SKILL.md`
- `~/.config/opencode/skills/<name>/SKILL.md`

**SKILL.md 格式：**

```markdown
---
name: git-release
description: 创建一致的发布和变更日志
license: MIT
---

## 功能
- 从合并的 PR 起草发布说明
- 提出版本号建议
- 提供可复制粘贴的 gh release create 命令

## 使用时机
准备标记发布时使用此技能。
```

**命名规则：**
- 1-64 字符
- 小写字母数字 + 单个连字符分隔
- 不能以 `-` 开头或结尾
- 正则：`^[a-z0-9]+(-[a-z0-9]+)*$`

### Agents（代理）

代理是专门化的 AI 助手，可配置特定的提示词、模型和工具访问权限。

**代理类型：**
- `primary` - 主要代理，通过 Tab 键切换
- `subagent` - 子代理，通过 `@` 提及或自动调用

**内置代理：**

| 代理 | 类型 | 说明 |
|------|------|------|
| `build` | primary | 默认代理，所有工具可用 |
| `plan` | primary | 规划代理，只读模式 |
| `general` | subagent | 通用研究代理 |
| `explore` | subagent | 快速只读探索代理 |

**自定义代理（Markdown）：**

`.opencode/agents/review.md`:

```markdown
---
description: 代码审查代理
mode: subagent
model: anthropic/claude-sonnet-4-5
temperature: 0.1
permission:
  edit: deny
  bash: ask
---

你是代码审查专家。关注：
- 代码质量和最佳实践
- 潜在的 bug 和边界情况
- 性能影响
- 安全考虑

提供建设性反馈，不要直接修改代码。
```

**自定义代理（JSON）：**

```jsonc
{
  "agent": {
    "review": {
      "description": "代码审查代理",
      "mode": "subagent",
      "model": "anthropic/claude-sonnet-4-5",
      "temperature": 0.1,
      "prompt": "你是代码审查专家...",
      "permission": {
        "edit": "deny"
      }
    }
  }
}
```

### Commands（命令）

自定义命令用于重复性任务。

**位置：**
- `.opencode/commands/<name>.md`
- `~/.config/opencode/commands/<name>.md`

**命令格式：**

```markdown
---
description: 运行测试并显示覆盖率
agent: build
model: anthropic/claude-haiku-4-5
---

运行完整的测试套件并显示覆盖率报告。
重点关注失败的测试并建议修复方案。
```

**参数占位符：**

```markdown
---
description: 创建新组件
---

创建一个名为 $ARGUMENTS 的 React 组件，支持 TypeScript。
包含正确的类型定义和基本结构。

# 使用: /component Button
```

**Shell 输出注入：**

```markdown
---
description: 分析测试覆盖率
---

当前测试结果：
!`npm test`

基于这些结果，建议提高覆盖率的改进方案。
```

### MCP Servers

MCP（Model Context Protocol）服务器用于集成外部工具和服务。

**Local MCP：**

```jsonc
{
  "mcp": {
    "my-local-mcp": {
      "type": "local",
      "command": ["npx", "-y", "my-mcp-command"],
      "environment": {
        "MY_ENV_VAR": "value"
      }
    }
  }
}
```

**Remote MCP：**

```jsonc
{
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp"
    },
    "sentry": {
      "type": "remote",
      "url": "https://mcp.sentry.dev/mcp",
      "oauth": {}
    }
  }
}
```

**常用 MCP 示例：**

```jsonc
{
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp"
    },
    "gh_grep": {
      "type": "remote",
      "url": "https://mcp.grep.app"
    }
  }
}
```

### Plugins（插件）

插件用于扩展 OpenCode 功能，可以钩入各种事件。

**本地插件：**

`.opencode/plugins/notification.js`:

```javascript
export const NotificationPlugin = async ({ $ }) => {
  return {
    event: async ({ event }) => {
      if (event.type === "session.idle") {
        await $`osascript -e 'display notification "Session completed!" with title "OpenCode"'`
      }
    }
  }
}
```

**npm 插件：**

```jsonc
{
  "plugin": [
    "opencode-helicone-session",
    "@my-org/custom-plugin"
  ]
}
```

**插件事件：**

| 事件类别 | 事件 |
|---------|------|
| Tool | `tool.execute.before`, `tool.execute.after` |
| Session | `session.created`, `session.idle`, `session.error` |
| File | `file.edited`, `file.watcher.updated` |
| Message | `message.updated`, `message.removed` |
| Permission | `permission.asked`, `permission.replied` |

## 内置工具

| 工具 | 功能 | 默认权限 |
|------|------|---------|
| `bash` | 执行 shell 命令 | allow |
| `edit` | 精确字符串替换编辑 | allow |
| `write` | 创建/覆盖文件 | allow |
| `read` | 读取文件 | allow |
| `grep` | 正则搜索内容 | allow |
| `glob` | 文件模式匹配 | allow |
| `skill` | 加载技能 | allow |
| `todowrite` | 任务列表管理 | allow |
| `webfetch` | 获取网页内容 | allow |
| `websearch` | 网页搜索 | allow |
| `question` | 向用户提问 | allow |
| `lsp` | LSP 查询（实验性） | allow |

### 权限控制

```jsonc
{
  "permission": {
    // 全局默认
    "*": "ask",
    
    // 允许特定工具
    "bash": "allow",
    "edit": "deny",
    
    // 细粒度控制
    "bash": {
      "*": "ask",
      "git *": "allow",
      "npm *": "allow",
      "rm *": "deny"
    },
    
    // 外部目录访问
    "external_directory": {
      "~/projects/personal/**": "allow"
    }
  }
}
```

**权限值：**
- `allow` - 自动执行
- `ask` - 请求批准
- `deny` - 阻止执行

## Provider 配置

### OpenCode Zen（推荐新手）

```bash
/connect
# 选择 OpenCode Zen
# 访问 opencode.ai/auth 获取 API Key
```

### Anthropic

```bash
/connect
# 选择 Anthropic
# 选择 ChatGPT Plus/Pro 或手动输入 API Key
```

### OpenAI

```bash
/connect
# 选择 OpenAI
# 支持 ChatGPT Plus/Pro 订阅
```

### 本地模型（Ollama）

```jsonc
{
  "provider": {
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Ollama (local)",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      },
      "models": {
        "llama2": { "name": "Llama 2" }
      }
    }
  }
}
```

### 本地模型（LM Studio）

```jsonc
{
  "provider": {
    "lmstudio": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LM Studio (local)",
      "options": {
        "baseURL": "http://127.0.0.1:1234/v1"
      },
      "models": {
        "google/gemma-3n-e4b": { "name": "Gemma 3n-e4b (local)" }
      }
    }
  }
}
```

## 项目配置模板

### .opencode/ 目录结构

```
.opencode/
├── skills/
│   └── my-skill/
│       └── SKILL.md
├── agents/
│   └── review.md
├── commands/
│   └── test.md
└── plugins/
    └── my-plugin.js
```

### 最小项目配置

`opencode.json`:

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "model": "anthropic/claude-sonnet-4-5",
  "instructions": ["AGENTS.md"]
}
```

### 完整项目配置

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  
  "model": "anthropic/claude-sonnet-4-5",
  "small_model": "anthropic/claude-haiku-4-5",
  
  "provider": {
    "anthropic": {
      "options": {
        "apiKey": "{env:ANTHROPIC_API_KEY}"
      }
    }
  },
  
  "instructions": ["AGENTS.md", "CONTRIBUTING.md"],
  
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp"
    }
  },
  
  "permission": {
    "bash": { "*": "ask", "git *": "allow", "npm *": "allow" },
    "edit": "ask"
  },
  
  "command": {
    "test": {
      "template": "运行测试并显示覆盖率报告。",
      "description": "运行测试"
    },
    "review": {
      "template": "审查当前更改并提供反馈。",
      "description": "代码审查",
      "agent": "plan"
    }
  },
  
  "autoupdate": true,
  "share": "manual"
}
```

## 常见问题

### 如何撤销修改？

```bash
/undo    # 撤销上次修改
/redo    # 重做
```

### 如何切换模型？

```bash
/models    # 列出可用模型
# 或使用快捷键 ctrl+x m
```

### 如何使用 Plan 模式？

按 `Tab` 键在 Build 和 Plan 模式之间切换。Plan 模式下代理不会修改文件。

### 如何分享会话？

```bash
/share    # 生成分享链接
/unshare  # 取消分享
```

### 如何压缩上下文？

```bash
/compact    # 手动压缩
# 或自动压缩（默认启用）
```

## 相关链接

- 官网: https://opencode.ai
- 文档: https://opencode.ai/docs
- GitHub: https://github.com/anomalyco/opencode
- Discord: https://opencode.ai/discord
- Schema: https://opencode.ai/config.json
