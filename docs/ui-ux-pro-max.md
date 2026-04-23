# UI UX Pro Max

**GitHub:** https://github.com/nextlevelbuilder/ui-ux-pro-max-skill

## 简介

**UI UX Pro Max** 是一个 AI 技能，为构建专业 UI/UX 提供设计智能。它包含 67 种 UI 风格、161 种配色方案、57 种字体配对，以及智能设计系统生成器。

**核心特点：**
- **智能设计系统生成** - 分析需求自动生成完整设计系统
- **67种UI风格** - Glassmorphism、Brutalism、AI-Native UI 等
- **161种行业配色** - 与产品类型一一对应
- **15种技术栈支持** - React、Next.js、Vue、Flutter 等

**适用场景：**
- 快速生成 UI 设计系统
- 前端开发需要设计指导
- 多平台 UI 开发

---

## 如何安装

### Claude Code 安装

**方式一：插件市场**
```bash
/plugin marketplace add nextlevelbuilder/ui-ux-pro-max-skill
/plugin install ui-ux-pro-max@ui-ux-pro-max-skill
```

### OpenCode 安装

**方式一：CLI 安装（推荐）**
```bash
# 安装 CLI
npm install -g uipro-cli

# 初始化
uipro init --ai opencode
```

**安装后的文件目录结构：**
```
.opencode/skills/ui-ux-pro-max/
├── SKILL.md                    # 技能定义文件
├── data/                       # 设计数据文件
│   ├── charts.csv              # 图表类型
│   ├── colors.csv              # 颜色方案
│   ├── icons.csv               # 图标库
│   ├── landing.csv             # Landing 页结构
│   ├── products.csv            # 产品类型
│   ├── styles.csv              # UI 风格
│   ├── typography.csv          # 字体配对
│   ├── ux-guidelines.csv       # UX 最佳实践
│   ├── ui-reasoning.csv        # 设计推理规则
│   ├── react-performance.csv   # React 性能优化
│   ├── web-interface.csv       # Web 界面指南
│   └── stacks/                 # 技术栈指南
│       ├── html-tailwind.csv   # HTML + Tailwind（默认）
│       ├── react.csv           # React
│       ├── nextjs.csv          # Next.js
│       ├── vue.csv             # Vue
│       ├── svelte.csv          # Svelte
│       ├── swiftui.csv         # SwiftUI
│       ├── react-native.csv    # React Native
│       ├── flutter.csv         # Flutter
│       ├── shadcn.csv          # shadcn/ui
│       ├── jetpack-compose.csv # Jetpack Compose
│       ├── astro.csv           # Astro
│       ├── nuxtjs.csv          # Nuxt.js
│       └── nuxt-ui.csv         # Nuxt UI
└── scripts/                    # Python 脚本
    ├── core.py                 # 核心搜索逻辑
    ├── design_system.py        # 设计系统生成
    └── search.py               # CLI 入口
```

**方式二：全局安装**
```bash
uipro init --ai opencode --global
```

### 手动安装（离线/企业网络）

**步骤1：下载文件**

在有网络的机器上：
```bash
# 下载仓库
git clone https://github.com/nextlevelbuilder/ui-ux-pro-max-skill.git

# 或下载zip
curl -L https://github.com/nextlevelbuilder/ui-ux-pro-max-skill/archive/refs/heads/main.zip -o ui-ux-pro-max.zip
unzip ui-ux-pro-max.zip
mv ui-ux-pro-max-skill-main ui-ux-pro-max
```

**步骤2：需要打包的目录**

```
ui-ux-pro-max/
├── src/ui-ux-pro-max/
│   ├── data/          # CSV数据文件（必需）
│   ├── scripts/       # Python脚本（必需）
│   └── templates/     # 模板文件
├── .claude/           # Claude配置
└── skill.json
```

**步骤3：放置到正确目录**

**OpenCode：**
```bash
mkdir -p ~/.config/opencode/skills/ui-ux-pro-max
cp -r ui-ux-pro-max/src/ui-ux-pro-max/* ~/.config/opencode/skills/ui-ux-pro-max/
```

**Claude Code：**
```bash
mkdir -p ~/.claude/skills/ui-ux-pro-max
cp -r ui-ux-pro-max/src/ui-ux-pro-max/* ~/.claude/skills/ui-ux-pro-max/
```

**步骤4：安装依赖**

需要 Python 3.x：
```bash
# 检查 Python
python3 --version

# macOS
brew install python3

# Ubuntu/Debian
sudo apt update && sudo apt install python3

# Windows
winget install Python.Python.3.12
```

**步骤5：验证安装**

```
# 在 AI 中测试
Build a landing page for my SaaS product
```

如果 AI 自动生成设计系统并开始实现，说明安装成功。

---

## 如何使用

### 自动激活

UI UX Pro Max 会**自动激活**，当你请求 UI/UX 相关工作时：

```
Build a landing page for my SaaS product
Create a dashboard for healthcare analytics
Design a portfolio website with dark mode
```

### 手动触发

```
/ui-ux-pro-max Build a landing page for my beauty spa
```

### 设计系统生成

UI UX Pro Max 的核心功能是**智能设计系统生成**：

```
你: Build a landing page for my beauty spa

AI: [分析产品类型]
    [搜索匹配的风格、配色、字体]
    
    +--------------------------------------------------------+
    | TARGET: Serenity Spa - RECOMMENDED DESIGN SYSTEM       |
    +--------------------------------------------------------+
    | PATTERN: Hero-Centric + Social Proof                   |
    | STYLE: Soft UI Evolution                               |
    |                                                        |
    | COLORS:                                                |
    |   Primary:    #E8B4B8 (Soft Pink)                      |
    |   Secondary:  #A8D5BA (Sage Green)                     |
    |   CTA:        #D4AF37 (Gold)                           |
    |                                                        |
    | TYPOGRAPHY: Cormorant Garamond / Montserrat            |
    |                                                        |
    | KEY EFFECTS: Soft shadows + Smooth transitions         |
    +--------------------------------------------------------+
```

### 支持的UI风格（部分）

**通用风格：**
- Minimalism & Swiss Style
- Glassmorphism
- Neumorphism
- Brutalism
- Dark Mode (OLED)
- AI-Native UI
- Bento Grid

**落地页风格：**
- Hero-Centric Design
- Conversion-Optimized
- Feature-Rich Showcase
- Social Proof-Focused

**仪表板风格：**
- Data-Dense Dashboard
- Executive Dashboard
- Real-Time Monitoring
- Financial Dashboard

### 支持的技术栈

| 类别 | 技术栈 |
|------|--------|
| Web | HTML + Tailwind（默认） |
| React | React, Next.js, shadcn/ui |
| Vue | Vue, Nuxt.js, Nuxt UI |
| Angular | Angular |
| PHP | Laravel |
| iOS | SwiftUI |
| Android | Jetpack Compose |
| 跨平台 | React Native, Flutter |

### 设计系统命令

```bash
# 生成设计系统（ASCII输出）
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "beauty spa" --design-system -p "Serenity Spa"

# Markdown输出
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "fintech" --design-system -f markdown

# 特定领域搜索
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "glassmorphism" --domain style
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "dashboard" --domain chart

# 技术栈特定指南
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "form validation" --stack react
```

### 持久化设计系统

```bash
# 生成并保存到 design-system/MASTER.md
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "SaaS dashboard" --design-system --persist -p "MyApp"
```

### 典型使用场景

**场景1：快速落地页**
```
Build a landing page for my SaaS product

# AI 自动：
# 1. 分析产品类型
# 2. 生成设计系统
# 3. 实现代码
```

**场景2：指定风格**
```
Create a dashboard with glassmorphism style for analytics

# AI 使用 glassmorphism 风格生成
```

**场景3：指定技术栈**
```
Build a mobile app UI for e-commerce using Flutter

# AI 使用 Flutter 最佳实践生成
```

### 注意事项

1. **需要 Python 3.x** - 搜索脚本依赖 Python
2. **自动检测框架** - AI 会自动检测你的项目框架
3. **默认 HTML + Tailwind** - 未指定时使用默认技术栈
4. **行业特定规则** - 161 个行业有专门的设计规则

---

## 更新

```bash
# CLI 方式
uipro update

# 或重新安装
uipro init --ai opencode
```

---

## 相关链接

- GitHub: https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
- 官网: https://uupm.cc
- npm: https://www.npmjs.com/package/uipro-cli
