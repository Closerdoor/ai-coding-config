# Awesome Design MD

**GitHub:** https://github.com/VoltAgent/awesome-design-md

## 简介

**Awesome Design MD** 是一个 DESIGN.md 文件集合，灵感来自流行的品牌设计系统。你只需要将一个 DESIGN.md 复制到项目中，告诉 AI "按这个风格构建页面"，就能获得像素级精确的 UI。

**核心特点：**
- **68+ 品牌设计系统** - Apple、Stripe、Vercel、Linear 等
- **即复制即用** - 单个 Markdown 文件，无需配置
- **AI 原生格式** - Markdown 是 LLM 最擅长阅读的格式
- **包含预览** - 每个设计系统都有 HTML 预览文件

**适用场景：**
- 快速应用特定品牌风格
- 需要设计一致性
- AI 生成 UI 时需要设计指导

---

## 如何安装

### 安装方式

Awesome Design MD 不是一个需要"安装"的工具，而是一个**资源集合**。你只需要下载需要的 DESIGN.md 文件即可。

### 获取 DESIGN.md 文件

**方式一：从官网获取**
```
https://getdesign.md/<brand>/design-md
```

例如：
- Apple: https://getdesign.md/apple/design-md
- Stripe: https://getdesign.md/stripe/design-md
- Vercel: https://getdesign.md/vercel/design-md

**方式二：从 GitHub 获取**
```bash
# 克隆仓库
git clone https://github.com/VoltAgent/awesome-design-md.git

# 或下载zip
curl -L https://github.com/VoltAgent/awesome-design-md/archive/refs/heads/main.zip -o awesome-design-md.zip
```

### 手动下载（离线/企业网络）

**步骤1：下载文件**

在有网络的机器上：
```bash
# 下载整个仓库
git clone https://github.com/VoltAgent/awesome-design-md.git

# 或只下载特定的 DESIGN.md
curl -o DESIGN.md https://raw.githubusercontent.com/VoltAgent/awesome-design-md/main/design-md/apple/DESIGN.md
```

**步骤2：文件结构**

```
awesome-design-md/
└── design-md/
    ├── apple/
    │   ├── DESIGN.md       # 设计系统
    │   ├── preview.html    # 亮色预览
    │   └── preview-dark.html # 暗色预览
    ├── stripe/
    │   ├── DESIGN.md
    │   └── preview.html
    └── ...（68+品牌）
```

**步骤3：放置到项目**

将选中的 DESIGN.md 复制到项目根目录：
```bash
cp design-md/apple/DESIGN.md /your-project/DESIGN.md
```

**步骤4：验证**

在 AI 中：
```
Read DESIGN.md and build a landing page following this style
```

---

## 如何使用

### 基本用法

**步骤1：选择设计系统**

从可用品牌中选择一个适合你项目的：

| 品牌 | 风格特点 |
|------|---------|
| Apple | 高端白色空间、SF Pro、电影级图像 |
| Stripe | 紫色渐变、weight-300 优雅 |
| Vercel | 黑白精确、Geist 字体 |
| Linear | 超极简、紫色强调 |
| Notion | 温暖极简、serif 标题 |
| Figma | 多彩、专业且有趣 |
| Airbnb | 温暖珊瑚强调、照片驱动 |
| Nike | 单色 UI、大写 Futura |

**步骤2：复制 DESIGN.md**

```bash
# 例如使用 Stripe 风格
curl -o DESIGN.md https://raw.githubusercontent.com/VoltAgent/awesome-design-md/main/design-md/stripe/DESIGN.md
```

**步骤3：让 AI 使用它**

```
Read DESIGN.md and build a pricing page for my SaaS
```

### 可用品牌列表

**AI & LLM 平台：**
- Claude、Cohere、ElevenLabs、Mistral AI、Ollama、OpenCode AI、Replicate、RunwayML、VoltAgent、xAI

**开发者工具：**
- Cursor、Expo、Lovable、Raycast、Superhuman、Vercel、Warp

**后端 & 数据库：**
- ClickHouse、Composio、MongoDB、PostHog、Sanity、Sentry、Supabase

**生产力 & SaaS：**
- Cal.com、Intercom、Linear、Mintlify、Notion、Resend、Zapier

**设计 & 创意：**
- Airtable、Clay、Figma、Framer、Miro、Webflow

**金融科技：**
- Binance、Coinbase、Kraken、Mastercard、Revolut、Stripe、Wise

**电商 & 零售：**
- Airbnb、Meta、Nike、Shopify

**媒体 & 消费科技：**
- Apple、IBM、NVIDIA、Pinterest、PlayStation、SpaceX、Spotify、Uber、Vodafone、WIRED

**汽车：**
- BMW、Bugatti、Ferrari、Lamborghini、Renault、Tesla

### DESIGN.md 文件结构

每个 DESIGN.md 包含以下部分：

```markdown
# 1. Visual Theme & Atmosphere
   - 情绪、密度、设计哲学

# 2. Color Palette & Roles
   - 语义化名称 + hex + 功能角色

# 3. Typography Rules
   - 字体族、完整层级表

# 4. Component Stylings
   - 按钮、卡片、输入框、导航及状态

# 5. Layout Principles
   - 间距比例、网格、空白哲学

# 6. Depth & Elevation
   - 阴影系统、表面层级

# 7. Do's and Don'ts
   - 设计护栏和反模式

# 8. Responsive Behavior
   - 断点、触摸目标、折叠策略

# 9. Agent Prompt Guide
   - 快速颜色参考、即用提示
```

### 预览设计系统

每个品牌都有 HTML 预览文件：

```bash
# 在浏览器中打开预览
open design-md/apple/preview.html

# 暗色预览
open design-md/apple/preview-dark.html
```

### 典型使用场景

**场景1：快速应用品牌风格**
```
# 复制 Stripe 风格
cp design-md/stripe/DESIGN.md ./DESIGN.md

# 让 AI 构建
Build a landing page following the design system in DESIGN.md
```

**场景2：切换风格**
```
# 切换到 Apple 风格
cp design-md/apple/DESIGN.md ./DESIGN.md

# 重新生成
Rebuild the page with the new design system
```

**场景3：自定义**
```
# 复制基础风格
cp design-md/linear/DESIGN.md ./DESIGN.md

# 编辑自定义
# 修改颜色、字体等

# 让 AI 使用自定义版本
Build following DESIGN.md
```

### 注意事项

1. **单个文件** - 只需要一个 DESIGN.md，无需其他配置
2. **项目根目录** - 放在项目根目录，AI 会自动读取
3. **可编辑** - 可以根据需要修改 DESIGN.md
4. **预览可用** - 使用 preview.html 查看设计系统效果

---

## 相关链接

- GitHub: https://github.com/VoltAgent/awesome-design-md
- 官网: https://getdesign.md
- 请求新设计: https://getdesign.md/request
- Discord: https://s.voltagent.dev/discord
