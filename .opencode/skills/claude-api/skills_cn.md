---
name: claude-api
description: "构建、调试和优化 Claude API / Anthropic SDK 应用程序。使用此技能构建的应用程序应包含提示缓存。还处理在 Claude 模型版本之间迁移现有 Claude API 代码（4.5 → 4.6、4.6 → 4.7、退役模型替换）。触发条件：代码导入 `anthropic`/`@anthropic-ai/sdk`；用户询问 Claude API、Anthropic SDK 或托管代理；用户在文件中添加/修改/调整 Claude 功能（缓存、思考、压缩、工具使用、批处理、文件、引用、内存）或模型（Opus/Sonnet/Haiku）；关于 Anthropic SDK 项目中的提示缓存/缓存命中率的问题。跳过：文件导入 `openai`/其他提供商 SDK，文件名如 `*-openai.py`/`*-generic.py`，提供商中立代码，通用编程/ML。"
license: 完整条款见 LICENSE.txt
---

# 使用 Claude 构建 LLM 驱动的应用程序

此技能帮助您使用 Claude 构建 LLM 驱动的应用程序。根据您的需求选择合适的界面，检测项目语言，然后阅读相关的特定语言文档。

## 开始之前

扫描目标文件（或者，如果没有目标文件，则扫描提示和项目）以查找非 Anthropic 提供商标记 — `import openai`、`from openai`、`langchain_openai`、`OpenAI(`、`gpt-4`、`gpt-5`、文件名如 `agent-openai.py` 或 `*-generic.py`，或任何保持代码提供商中立的明确指令。如果发现任何内容，停止并告诉用户此技能生成 Claude/Anthropic SDK 代码；询问他们是否要将文件切换到 Claude 或想要非 Claude 实现。不要使用 Anthropic SDK 调用编辑非 Anthropic 文件。

## 输出要求

当用户要求您添加、修改或实现 Claude 功能时，您的代码必须通过以下之一调用 Claude：

1. **项目语言的官方 Anthropic SDK**（`anthropic`、`@anthropic-ai/sdk`、`com.anthropic.*` 等）。只要项目的受支持 SDK 存在，这就是默认选择。
2. **原始 HTTP**（`curl`、`requests`、`fetch`、`httpx` 等）— 仅当用户明确要求 cURL/REST/原始 HTTP、项目是 shell/cURL 项目，或语言没有官方 SDK 时。

永远不要混合两者 — 不要在 Python 或 TypeScript 项目中使用 `requests`/`fetch`，仅仅因为它感觉更轻。永远不要回退到 OpenAI 兼容的垫片。

**永远不要猜测 SDK 用法。** 函数名称、类名称、命名空间、方法签名和导入路径必须来自明确的文档 — 要么是此技能中的 `{lang}/` 文件，要么是 `shared/live-sources.md` 中列出的官方 SDK 存储库或文档链接。如果您需要的绑定未在技能文件中明确记录，请在编写代码之前从 `shared/live-sources.md` WebFetch 相关的 SDK 存储库。不要从 cURL 形状或另一种语言的 SDK 推断 Ruby/Java/Go/PHP/C# API。

## 默认值

除非用户另有要求：

对于 Claude 模型版本，请使用 Claude Opus 4.7，您可以通过确切的模型字符串 `claude-opus-4-7` 访问它。对于任何稍微复杂的事情，请默认使用自适应思考（`thinking: {type: "adaptive"}`）。最后，对于任何可能涉及长输入、长输出或高 `max_tokens` 的请求，请默认使用流式传输 — 它可以防止达到请求超时。如果您不需要处理单个流事件，请使用 SDK 的 `.get_final_message()` / `.finalMessage()` 助手获取完整响应

---

## 子命令

如果此提示底部的用户请求是裸子命令字符串（无散文），请搜索本文档中的每个**子命令**表 — 包括下面附加的部分中的任何表 — 并直接遵循匹配的操作列。这使用户可以通过 `/claude-api <子命令>` 调用特定流程。如果文档中没有表匹配，则将请求视为普通散文。


---

## 语言检测

在阅读代码示例之前，确定用户正在使用哪种语言：

1. **查看项目文件**以推断语言：

   - `*.py`、`requirements.txt`、`pyproject.toml`、`setup.py`、`Pipfile` → **Python** — 从 `python/` 读取
   - `*.ts`、`*.tsx`、`package.json`、`tsconfig.json` → **TypeScript** — 从 `typescript/` 读取
   - `*.js`、`*.jsx`（不存在 `.ts` 文件）→ **TypeScript** — JS 使用相同的 SDK，从 `typescript/` 读取
   - `*.java`、`pom.xml`、`build.gradle` → **Java** — 从 `java/` 读取
   - `*.kt`、`*.kts`、`build.gradle.kts` → **Java** — Kotlin 使用 Java SDK，从 `java/` 读取
   - `*.scala`、`build.sbt` → **Java** — Scala 使用 Java SDK，从 `java/` 读取
   - `*.go`、`go.mod` → **Go** — 从 `go/` 读取
   - `*.rb`、`Gemfile` → **Ruby** — 从 `ruby/` 读取
   - `*.cs`、`*.csproj` → **C#** — 从 `csharp/` 读取
   - `*.php`、`composer.json` → **PHP** — 从 `php/` 读取

2. **如果检测到多种语言**（例如，同时存在 Python 和 TypeScript 文件）：

   - 检查用户当前文件或问题与哪种语言相关
   - 如果仍然模糊，询问："我检测到 Python 和 TypeScript 文件。您使用哪种语言进行 Claude API 集成？"

3. **如果无法推断语言**（空项目、没有源文件或不支持的语言）：

   - 使用 AskUserQuestion，选项为：Python、TypeScript、Java、Go、Ruby、cURL/原始 HTTP、C#、PHP
   - 如果 AskUserQuestion 不可用，默认为 Python 示例并注意："显示 Python 示例。如果需要其他语言，请告诉我。"

4. **如果检测到不支持的语言**（Rust、Swift、C++、Elixir 等）：

   - 建议使用 `curl/` 中的 cURL/原始 HTTP 示例，并注意社区 SDK 可能存在
   - 提供显示 Python 或 TypeScript 示例作为参考实现

5. **如果用户需要 cURL/原始 HTTP 示例**，从 `curl/` 读取。

### 特定语言的功能支持

| 语言   | 工具运行器 | 托管代理 | 说明                                 |
| ---------- | ----------- | -------------- | ------------------------------------- |
| Python     | 是（测试版）  | 是（测试版）     | 完整支持 — `@beta_tool` 装饰器 |
| TypeScript | 是（测试版）  | 是（测试版）     | 完整支持 — `betaZodTool` + Zod    |
