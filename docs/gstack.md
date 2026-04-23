# GStack

**GitHub:** https://github.com/garrytan/gstack

## 简介

**GStack** 是 Y Combinator CEO Garry Tan 的个人 AI 编码工具集，将 Claude Code 转化为一个虚拟工程团队。它包含 **37 个技能** 和 **8 个强力工具**，覆盖从产品构思到部署的完整流程。

**核心特点：**
- **角色分工明确** - CEO、设计师、工程师、QA、安全官等各司其职
- **真实浏览器测试** - `/qa` 命令打开真实 Chromium 进行测试
- **并行 Sprint 支持** - 可同时运行 10-15 个并行任务
- **完整发布流程** - 从代码审查到生产部署一条龙

**适用场景：**
- 个人开发者想要团队级工作流
- 需要严格代码审查和质量保证
- 频繁发布和部署的项目

---

## 如何安装

### 前置要求

- **Bun 运行时** - 安装脚本需要
- **Node.js** - Windows 上浏览器测试需要
- **Playwright** - `/qa` 命令依赖（自动安装）

### Claude Code 安装

**方式一：一键安装（推荐）**

在 Claude Code 中粘贴以下命令：
```
Install gstack: run git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack && cd ~/.claude/skills/gstack && ./setup
```

**方式二：手动安装**
```bash
# 1. 克隆仓库
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack

# 2. 进入目录并运行安装脚本
cd ~/.claude/skills/gstack
./setup
```

### OpenCode 安装

```bash
# 克隆到 OpenCode skills 目录
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.config/opencode/skills/gstack

# 运行安装脚本（必须指定 --host opencode）
cd ~/.config/opencode/skills/gstack
./setup --host opencode
```

### 手动安装（离线/企业网络）

**步骤1：下载完整仓库**

在有网络的机器上：
```bash
# 下载 zip 包
curl -L https://github.com/garrytan/gstack/archive/refs/heads/main.zip -o gstack.zip

# 或使用 git clone
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git
```

**步骤2：传输到目标机器**

将整个 `gstack` 目录传输到目标机器。

**步骤3：放置到正确目录**

**OpenCode：**
```bash
mkdir -p ~/.config/opencode/skills
cp -r gstack ~/.config/opencode/skills/
cd ~/.config/opencode/skills/gstack
chmod +x setup
./setup --host opencode
```

**Claude Code：**
```bash
mkdir -p ~/.claude/skills
cp -r gstack ~/.claude/skills/
cd ~/.claude/skills/gstack
chmod +x setup
./setup
```

**步骤4：安装脚本会做什么**

`./setup --host opencode` 会：
1. 安装依赖（bun install）
2. 编译二进制文件：
   - `browse/dist/browse.exe` - 浏览器测试工具
   - `browse/dist/find-browse.exe` - 浏览器查找工具
   - `design/dist/design.exe` - 设计工具
   - `bin/gstack-global-discover.exe` - 全局发现工具
3. 生成 OpenCode 专用 SKILL.md 文件（37个）
4. 安装 Playwright Chromium（如未安装）

**步骤5：最终需要的文件产物**

安装完成后，必需的文件结构：
```
~/.config/opencode/skills/gstack/
├── bin/                    # 运行时脚本（35个文件）
│   ├── gstack-config       # 配置管理
│   ├── gstack-update-check # 更新检查
│   └── ...
├── browse/dist/            # 浏览器测试工具
│   ├── browse.exe          # ~111 MB
│   └── find-browse.exe     # ~111 MB
├── design/dist/            # 设计工具
│   └── design.exe          # ~111 MB
├── gstack/SKILL.md         # 主技能定义
├── gstack-autoplan/SKILL.md
├── gstack-benchmark/SKILL.md
├── gstack-browse/SKILL.md
├── gstack-canary/SKILL.md
├── ... (共37个 gstack-* 目录)
├── gstack-ship/SKILL.md
└── gstack-upgrade/SKILL.md
```

**总大小：约 447 MB**（主要是二进制文件）

**步骤6：验证安装**

```bash
# 检查目录结构
ls ~/.config/opencode/skills/gstack/

# 应该看到: bin/ browse/ design/ gstack*/ 等
```

在 OpenCode 中测试：
```
use skill tool to list skills
```
应该能看到 `gstack`、`gstack-office-hours` 等技能。

---

## 如何使用

### 核心工作流

GStack 的核心是一个完整的 Sprint 流程：

```
Think → Plan → Build → Review → Test → Ship → Reflect
```

### 主要命令列表

**规划阶段：**
| 命令 | 角色 | 作用 |
|------|------|------|
| `/office-hours` | YC Office Hours | 产品需求澄清，6个关键问题 |
| `/plan-ceo-review` | CEO/创始人 | 战略审查，4种范围模式 |
| `/plan-eng-review` | 工程经理 | 架构、数据流、边界情况 |
| `/plan-design-review` | 高级设计师 | 设计评分和改进 |
| `/autoplan` | 审查流水线 | 自动运行 CEO→设计→工程审查 |

**开发阶段：**
| 命令 | 角色 | 作用 |
|------|------|------|
| `/review` | 高级工程师 | 代码审查，自动修复明显问题 |
| `/investigate` | 调试员 | 系统化根因调试 |
| `/design-shotgun` | 设计探索者 | 生成4-6个UI变体供选择 |
| `/design-html` | 设计工程师 | 将设计稿转为生产级HTML |

**测试阶段：**
| 命令 | 角色 | 作用 |
|------|------|------|
| `/qa` | QA负责人 | 打开真实浏览器测试，自动修复bug |
| `/qa-only` | QA报告员 | 只报告问题，不修改代码 |
| `/cso` | 首席安全官 | OWASP Top 10 + STRIDE 安全审计 |

**发布阶段：**
| 命令 | 角色 | 作用 |
|------|------|------|
| `/ship` | 发布工程师 | 同步主分支、运行测试、创建PR |
| `/land-and-deploy` | 发布工程师 | 合并PR、等待CI、验证生产环境 |
| `/canary` | SRE | 发布后监控循环 |
| `/benchmark` | 性能工程师 | 页面加载时间、Core Web Vitals |

**工具命令：**
| 命令 | 作用 |
|------|------|
| `/browse` | 打开真实 Chromium 浏览器进行测试 |
| `/careful` | 危险操作警告（rm -rf, DROP TABLE等） |
| `/freeze` | 锁定文件编辑范围 |
| `/guard` | 同时启用 careful + freeze |
| `/retro` | 团队周回顾 |

### 典型使用流程

**场景：开发一个新功能**

```
# 1. 需求澄清
/office-hours
> 我想做一个用户通知系统

# AI 会问6个关键问题帮你澄清需求

# 2. 战略审查
/plan-ceo-review
# AI 从CEO角度审查范围和方向

# 3. 工程审查
/plan-eng-review
# AI 审查架构、数据流、边界情况

# 4. 自动执行（可选）
/autoplan
# 自动运行所有审查

# 5. 实现后审查
/review
# 代码审查，自动修复问题

# 6. QA测试
/qa https://staging.myapp.com
# 打开真实浏览器测试

# 7. 发布
/ship
# 创建PR
```

**场景：调试一个bug**

```
# 1. 调试
/investigate
> 用户登录后token没有保存

# AI 会系统化调试：
# - 复现问题
# - 收集证据
# - 形成假设
# - 验证修复

# 2. 验证
/qa https://staging.myapp.com
```

**场景：设计一个页面**

```
# 1. 探索设计
/design-shotgun
> 设计一个定价页面

# AI 生成4-6个设计变体，在浏览器中展示

# 2. 转为代码
/design-html
# 将选中的设计转为生产级HTML
```

### 注意事项

1. **需要 Bun 运行时** - 安装脚本会自动检测，未安装会提示
2. **浏览器测试需要 Playwright** - `/qa` 命令依赖，setup 会自动安装
3. **Windows 用户** - 浏览器测试需要 Node.js（Bun 在 Windows 上有管道问题）
4. **权限模式** - 建议使用 `--dangerously-skip-permissions` 以获得流畅体验

---

## 更新

```bash
cd ~/.config/opencode/skills/gstack
git pull
./setup --host opencode
```

或使用内置命令：
```
/gstack-upgrade
```

---

## 清理不需要的文件

安装完成后，可以删除以下目录以节省空间：
```bash
cd ~/.config/opencode/skills/gstack

# 删除源码目录（已编译成二进制）
rm -rf browse/src browse/bin design/src

# 删除构建相关文件
rm -rf node_modules bun.lock package.json

# 删除文档
rm -rf docs CHANGELOG.md CONTRIBUTING.md README.md
```

---

## 相关链接

- GitHub: https://github.com/garrytan/gstack
- 作者: Garry Tan (Y Combinator CEO)
- 文档: https://github.com/garrytan/gstack/blob/main/docs/skills.md
