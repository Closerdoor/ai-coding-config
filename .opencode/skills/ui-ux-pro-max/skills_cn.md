---
name: ui-ux-pro-max
description: 具有可搜索数据库的 UI/UX 设计智能
---
# ui-ux-pro-max

Web 和移动应用程序的综合设计指南。包含 67 种样式、96 种调色板、57 种字体配对、99 条 UX 指南和 25 种图表类型，涵盖 13 种技术栈。具有优先级推荐的可搜索数据库。

## 前提条件

检查是否安装了 Python：

```bash
python3 --version || python --version
```

如果未安装 Python，根据用户的操作系统安装：

**macOS:**
```bash
brew install python3
```

**Ubuntu/Debian:**
```bash
sudo apt update && sudo apt install python3
```

**Windows:**
```powershell
winget install Python.Python.3.12
```

---

## 如何使用此技能

当用户请求 UI/UX 工作（设计、构建、创建、实现、审查、修复、改进）时，遵循此工作流程：

### 步骤 1：分析用户需求

从用户请求中提取关键信息：
- **产品类型**：SaaS、电子商务、作品集、仪表板、落地页等。
- **风格关键词**：极简、俏皮、专业、优雅、深色模式等。
- **行业**：医疗保健、金融科技、游戏、教育等。
- **技术栈**：React、Vue、Next.js，或默认为 `html-tailwind`

### 步骤 2：生成设计系统（必需）

**始终以 `--design-system` 开始**，以获得带有推理的全面推荐：

```bash
python3 skills/ui-ux-pro-max/scripts/search.py "<产品类型> <行业> <关键词>" --design-system [-p "项目名称"]
```

此命令：
1. 并行搜索 5 个域（产品、样式、颜色、落地页、排版）
2. 应用来自 `ui-reasoning.csv` 的推理规则选择最佳匹配
3. 返回完整的设计系统：模式、样式、颜色、排版、效果
4. 包含要避免的反模式

**示例：**
```bash
python3 skills/ui-ux-pro-max/scripts/search.py "beauty spa wellness service" --design-system -p "Serenity Spa"
```

### 步骤 2b：持久化设计系统（主文件 + 覆盖模式）

要保存设计系统以实现跨会话的层次化检索，添加 `--persist`：

```bash
python3 skills/ui-ux-pro-max/scripts/search.py "<查询>" --design-system --persist -p "项目名称"
```

这会创建：
- `design-system/MASTER.md` — 包含所有设计规则的全局真理来源
- `design-system/pages/` — 页面特定覆盖的文件夹

**带有页面特定覆盖：**
```bash
python3 skills/ui-ux-pro-max/scripts/search.py "<查询>" --design-system --persist -p "项目名称" --page "dashboard"
```

这还会创建：
- `design-system/pages/dashboard.md` — 与主文件的页面特定偏差

**层次化检索如何工作：**
1. 在构建特定页面（例如，"Checkout"）时，首先检查 `design-system/pages/checkout.md`
2. 如果页面文件存在，其规则**覆盖**主文件
3. 如果不存在，则仅使用 `design-system/MASTER.md`

### 步骤 3：使用详细搜索补充（根据需要）

获得设计系统后，使用域搜索获取其他详细信息：

```bash
python3 skills/ui-ux-pro-max/scripts/search.py "<关键词>" --domain <域> [-n <最大结果数>]
```

**何时使用详细搜索：**
