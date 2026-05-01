---
name: web-artifacts-builder
description: 使用现代前端 Web 技术（React、Tailwind CSS、shadcn/ui）创建精心设计的多组件 claude.ai HTML artifacts 的工具套件。用于需要状态管理、路由或 shadcn/ui 组件的复杂 artifacts - 不适用于简单的单文件 HTML/JSX artifacts。
license: 完整条款见 LICENSE.txt
---

# Web Artifacts Builder

要构建强大的前端 claude.ai artifacts，请按照以下步骤操作：
1. 使用 `scripts/init-artifact.sh` 初始化前端仓库
2. 通过编辑生成的代码开发您的 artifact
3. 使用 `scripts/bundle-artifact.sh` 将所有代码打包成单个 HTML 文件
4. 向用户显示 artifact
5. （可选）测试 artifact

**技术栈**：React 18 + TypeScript + Vite + Parcel（打包）+ Tailwind CSS + shadcn/ui

## 设计与样式指南

非常重要：为了避免通常所说的"AI 废话"，避免使用过多的居中布局、紫色渐变、统一的圆角和 Inter 字体。

## 快速开始

### 步骤 1：初始化项目

运行初始化脚本创建新的 React 项目：
```bash
bash scripts/init-artifact.sh <项目名称>
cd <项目名称>
```

这将创建一个完全配置的项目，包括：
- ✅ React + TypeScript（通过 Vite）
- ✅ Tailwind CSS 3.4.1 与 shadcn/ui 主题系统
- ✅ 路径别名（`@/`）已配置
- ✅ 40+ shadcn/ui 组件预安装
- ✅ 所有 Radix UI 依赖项已包含
- ✅ Parcel 已配置用于打包（通过 .parcelrc）
- ✅ Node 18+ 兼容性（自动检测并固定 Vite 版本）

### 步骤 2：开发您的 Artifact

要构建 artifact，编辑生成的文件。有关指导，请参阅下面的**常见开发任务**。

### 步骤 3：打包成单个 HTML 文件

要将 React 应用打包成单个 HTML artifact：
```bash
bash scripts/bundle-artifact.sh
```

这将创建 `bundle.html` - 一个包含所有 JavaScript、CSS 和依赖项的自包含 artifact。此文件可以直接在 Claude 对话中作为 artifact 共享。

**要求**：您的项目根目录中必须有 `index.html`。

**脚本执行的操作**：
- 安装打包依赖项（parcel、@parcel/config-default、parcel-resolver-tspaths、html-inline）
- 创建带有路径别名支持的 `.parcelrc` 配置
- 使用 Parcel 构建（无源映射）
- 使用 html-inline 将所有资源内联到单个 HTML 中

### 步骤 4：与用户共享 Artifact

最后，在对话中与用户共享打包的 HTML 文件，以便他们可以将其作为 artifact 查看。

### 步骤 5：测试/可视化 Artifact（可选）

注意：这是一个完全可选的步骤。仅在必要时或被请求时执行。

要测试/可视化 artifact，使用可用工具（包括其他技能或内置工具如 Playwright 或 Puppeteer）。通常，避免预先测试 artifact，因为它会增加从请求到显示完成 artifact 之间的延迟。如果被请求或出现问题，在呈现 artifact 后稍后测试。

## 参考

- **shadcn/ui 组件**：https://ui.shadcn.com/docs/components
