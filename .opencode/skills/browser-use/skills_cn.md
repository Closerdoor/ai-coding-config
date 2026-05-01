---
name: browser-use
description: 自动化浏览器交互，用于 Web 测试、表单填写、截图和数据提取。当用户需要导航网站、与网页交互、填写表单、截图或从网页提取信息时使用。
allowed-tools: Bash(browser-use:*)
---

# 使用 browser-use CLI 进行浏览器自动化

`browser-use` 命令提供快速、持久的浏览器自动化。后台守护进程使浏览器在命令之间保持打开，每次调用约 50ms 延迟。

## 前提条件

```bash
browser-use doctor    # 验证安装
```

有关设置详细信息，请参阅 https://github.com/browser-use/browser-use/blob/main/browser_use/skill_cli/README.md

## 核心工作流程

1. **导航**：`browser-use open <url>` — 启动无头浏览器并打开页面
2. **检查**：`browser-use state` — 返回带有索引的可点击元素
3. **交互**：使用来自状态的索引（`browser-use click 5`、`browser-use input 3 "text"`）
4. **验证**：`browser-use state` 或 `browser-use screenshot` 确认
5. **重复**：浏览器在命令之间保持打开

如果命令失败，首先运行 `browser-use close` 清除任何损坏的会话，然后重试。

要使用用户现有的 Chrome（保留登录/cookie）：首先运行 `browser-use connect`。
要使用云浏览器：首先运行 `browser-use cloud connect`。
在任一操作后，命令以相同方式工作。

### 如果 `browser-use connect` 失败

当 `browser-use connect` 无法找到启用了远程调试的运行中的 Chrome 时，向用户提示两个选项：

1. **使用他们的真实 Chrome 浏览器** — 他们需要先启用远程调试：
   - 在 Chrome 中打开 `chrome://inspect/#remote-debugging`，或使用 `--remote-debugging-port=9222` 重新启动 Chrome
   - 然后重试 `browser-use connect`
2. **使用带有他们 Chrome 配置文件的托管 Chromium** — 不需要 Chrome 设置：
   - 运行 `browser-use profile list` 显示可用配置文件
   - 询问他们想要哪个配置文件，然后使用 `browser-use --profile "ProfileName" open <url>`
   - 这会启动一个单独的 Chromium 实例，带有他们的配置文件数据（cookie、登录、扩展）

让用户选择 — 不要假设一个路径优于另一个。

## 浏览器模式

```bash
browser-use open <url>                         # 默认：无头 Chromium（无需设置）
browser-use --headed open <url>                # 可见窗口（用于调试）
browser-use connect                            # 连接到用户的 Chrome（保留登录/cookie）
browser-use cloud connect                      # 云浏览器（零配置，需要 API 密钥）
browser-use --profile "Default" open <url>     # 使用特定配置文件的真实 Chrome
```

在 `connect` 或 `cloud connect` 后，所有后续命令都发送到该浏览器 — 不需要额外的标志。

## 命令

```bash
# 导航
browser-use open <url>                    # 导航到 URL
browser-use back                          # 在历史记录中后退
browser-use scroll down                   # 向下滚动（--amount N 用于像素）
browser-use scroll up                     # 向上滚动
browser-use tab list                      # 列出所有选项卡
browser-use tab new [url]                 # 打开新选项卡（空白或带 URL）
browser-use tab switch <index>            # 按索引切换到选项卡
browser-use tab close <index> [index...]  # 关闭一个或多个选项卡

# 页面状态 — 始终先运行 state 以获取元素索引
browser-use state                         # URL、标题、带有索引的可点击元素
browser-use screenshot [path.png]         # 截图（如果没有路径则为 base64，--full 用于整页）

# 交互 — 使用来自状态的索引
browser-use click <index>                 # 按索引点击元素
browser-use click <x> <y>                 # 在像素坐标处点击
browser-use type "text"                   # 在聚焦元素中输入
browser-use input <index> "text"          # 点击元素，清除现有文本，然后输入
