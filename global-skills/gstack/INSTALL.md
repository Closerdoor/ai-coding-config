# gstack 安装指南

gstack 是一个强大的 37 角色虚拟团队技能包，提供 QA 测试、部署验证、代码审查等完整工作流。

## 特点

- **37 个子技能**：qa、ship、investigate、review、design-review 等
- **持久化浏览器**：headless Chromium，支持截图、表单填充、断言
- **完整工作流**：从开发到部署的全流程支持

## 全局安装（推荐）

gstack 需要全局安装，因为包含编译的二进制文件和多项目共享的工具。

### 安装步骤

```bash
# 1. 克隆到全局 skills 目录
cd ~/.claude/skills  # Claude Code
# 或
cd ~/.config/opencode/skills  # OpenCode

git clone https://github.com/garrytrask/gstack.git

# 2. 运行安装脚本（编译 browse 二进制）
cd gstack
./setup

# 3. 验证安装
./bin/gstack-config --version
```

### Windows 用户

```powershell
# 使用 PowerShell
cd $env:USERPROFILE\.claude\skills
git clone https://github.com/garrytrask/gstack.git
cd gstack
.\setup
```

## 项目级配置

全局安装后，在项目根目录创建 `.claude/skills/gstack/` 符号链接：

```bash
# Linux/macOS
ln -s ~/.claude/skills/gstack .claude/skills/gstack

# Windows (管理员权限)
New-Item -ItemType Junction -Path ".claude\skills\gstack" -Target "$env:USERPROFILE\.claude\skills\gstack"
```

或者直接在项目中使用全局安装的 gstack（无需链接）。

## 子技能列表

| 技能 | 用途 |
|------|------|
| `/qa` | QA 测试，查找并修复 bug |
| `/qa-only` | 仅报告 bug，不修复 |
| `/ship` | 完整发布流程 |
| `/investigate` | 系统化调试 |
| `/review` | PR 代码审查 |
| `/design-review` | 视觉设计审查 |
| `/design-shotgun` | 生成多个设计变体 |
| `/plan-ceo-review` | CEO 模式计划审查 |
| `/plan-eng-review` | 工程架构审查 |
| `/plan-design-review` | 设计计划审查 |
| `/retro` | 周回顾 |
| `/health` | 代码质量检查 |
| `/benchmark` | 性能回归检测 |
| `/canary` | 部署后监控 |
| `/checkpoint` | 保存/恢复工作状态 |
| `/freeze` | 限制编辑范围 |
| `/careful` | 安全模式 |
| `/guard` | 完全安全模式 |

更多子技能请参考官方文档。

## 配置选项

```bash
# 关闭主动建议
gstack-config set proactive false

# 设置遥测
gstack-config set telemetry community  # 或 anonymous, off

# 设置写作风格
gstack-config set explain_level terse  # 或 default
```

## 更新

```bash
cd ~/.claude/skills/gstack
git pull
./setup
```

## 官方链接

- GitHub: https://github.com/garrytrask/gstack
- 文档: https://garryslist.org
