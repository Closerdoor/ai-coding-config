# OpenCode Plugins

OpenCode 支持通过 `opencode.json` 配置插件，插件从 Git 仓库安装。

## 当前配置的插件

| Plugin | GitHub | 用途 |
|--------|--------|------|
| **superpowers** | https://github.com/obra/superpowers | TDD 方法论、测试驱动开发 |
| **oh-my-openagent** | https://github.com/code-yeongyu/oh-my-openagent | 多智能体编排 |

## 配置方式

在项目根目录的 `opencode.json` 中添加：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "plugin": [
    "superpowers@git+https://github.com/obra/superpowers.git",
    "oh-my-openagent@git+https://github.com/code-yeongyu/oh-my-openagent.git"
  ],
  "instructions": ["AGENTS.md"]
}
```

## 安装机制

- OpenCode 启动时自动从 Git 仓库克隆插件
- 插件安装在 `~/.config/opencode/plugins/` 目录
- 每次启动会检查更新

## 推荐插件

### superpowers

提供测试驱动开发方法论：
- 先写测试，再实现
- 验证驱动的开发流程
- 适合需要高质量代码的项目

### oh-my-openagent

多智能体编排框架：
- 任务分解和分配
- 多角色协作
- 适合复杂项目

## 添加新插件

1. 找到插件的 Git 仓库 URL
2. 在 `opencode.json` 的 `plugin` 数组中添加：
   ```json
   "plugin-name@git+https://github.com/owner/plugin-repo.git"
   ```
3. 重启 OpenCode

## 注意事项

- 插件名称可以自定义（`@` 前的部分）
- Git URL 必须是可访问的公开仓库
- 私有仓库需要配置 Git 认证