---
name: shadcn
description: 管理 shadcn 组件和项目 — 添加、搜索、修复、调试、样式化和组合 UI。提供项目上下文、组件文档和使用示例。适用于使用 shadcn/ui、组件注册表、预设、--preset 代码或任何具有 components.json 文件的项目。也会为"shadcn init"、"使用 --preset 创建应用"或"切换到 --preset"触发。
user-invocable: false
allowed-tools: Bash(npx shadcn@latest *), Bash(pnpm dlx shadcn@latest *), Bash(bunx --bun shadcn@latest *)
---

# shadcn/ui

一个用于构建 UI、组件和设计系统的框架。组件通过 CLI 作为源代码添加到用户项目中。

> **重要：** 使用项目的包运行器运行所有 CLI 命令：`npx shadcn@latest`、`pnpm dlx shadcn@latest` 或 `bunx --bun shadcn@latest` — 基于项目的 `packageManager`。下面的示例使用 `npx shadcn@latest`，但请替换为项目的正确运行器。

## 当前项目上下文

```json
!`npx shadcn@latest info --json`
```

上面的 JSON 包含项目配置和已安装的组件。使用 `npx shadcn@latest docs <component>` 获取任何组件的文档和示例 URL。

## 原则

1. **优先使用现有组件。** 在编写自定义 UI 之前使用 `npx shadcn@latest search` 检查注册表。也要检查社区注册表。
2. **组合，不要重新发明。** 设置页面 = Tabs + Card + 表单控件。仪表板 = Sidebar + Card + Chart + Table。
3. **在内置变体之前使用自定义样式。** `variant="outline"`、`size="sm"` 等。
4. **使用语义颜色。** `bg-primary`、`text-muted-foreground` — 永远不要使用原始值如 `bg-blue-500`。

## 关键规则

这些规则**始终强制执行**。每个都链接到包含错误/正确代码对的文件。

### 样式与 Tailwind → [styling.md](./rules/styling.md)

- **`className` 用于布局，不是样式。** 永远不要覆盖组件颜色或排版。
- **不要使用 `space-x-*` 或 `space-y-*`。** 使用带有 `gap-*` 的 `flex`。对于垂直堆栈，使用 `flex flex-col gap-*`。
- **当宽度和高度相等时使用 `size-*`。** 使用 `size-10` 而不是 `w-10 h-10`。
- **使用 `truncate` 简写。** 而不是 `overflow-hidden text-ellipsis whitespace-nowrap`。
- **不要手动覆盖 `dark:` 颜色。** 使用语义令牌（`bg-background`、`text-muted-foreground`）。
- **使用 `cn()` 处理条件类。** 不要编写手动模板字面量三元运算。
- **不要在覆盖组件上手动设置 `z-index`。** Dialog、Sheet、Popover 等处理自己的堆叠。

### 表单与输入 → [forms.md](./rules/forms.md)

- **表单使用 `FieldGroup` + `Field`。** 永远不要使用带有 `space-y-*` 或 `grid gap-*` 的原始 `div` 进行表单布局。
- **`InputGroup` 使用 `InputGroupInput`/`InputGroupTextarea`。** 永远不要在 `InputGroup` 内使用原始 `Input`/`Textarea`。
- **输入内的按钮使用 `InputGroup` + `InputGroupAddon`。**
- **选项集（2-7 个选择）使用 `ToggleGroup`。** 不要循环带有手动活动状态的 `Button`。
- **`FieldSet` + `FieldLegend` 用于分组相关复选框/单选按钮。** 不要使用带有标题的 `div`。
- **字段验证使用 `data-invalid` + `aria-invalid`。** `Field` 上的 `data-invalid`，控件上的 `aria-invalid`。对于禁用：`Field` 上的 `data-disabled`，控件上的 `disabled`。

### 组件结构 → [composition.md](./rules/composition.md)

- **项目始终在其 Group 内。** `SelectItem` → `SelectGroup`。`DropdownMenuItem` → `DropdownMenuGroup`。`CommandItem` → `CommandGroup`。
- **使用 `asChild`（radix）或 `render`（base）作为自定义触发器。** 从 `npx shadcn@latest info` 检查 `base` 字段。→ [base-vs-radix.md](./rules/base-vs-radix.md)
- **Dialog、Sheet 和 Drawer 始终需要 Title。** `DialogTitle`、`SheetTitle`、`DrawerTitle` 是可访问性所必需的。如果视觉隐藏，使用 `className="sr-only"`。
- **使用完整的 Card 组合。** `CardHeader`/`CardTitle`/`CardDescription`/`CardContent`/`CardFooter`。不要将所有内容都倾倒在 `CardContent` 中。
- **Button 没有 `isPending`/`isLoading`。** 与 `Spinner` + `data-icon` + `disabled` 组合。
- **`TabsTrigger` 必须在 `TabsList` 内。** 永远不要直接在 `Tabs` 中渲染触发器。
- **`Avatar` 始终需要 `AvatarFallback`。** 用于图像加载失败时。

### 使用组件，而不是自定义标记 → [composition.md](./rules/composition.md)

- **在自定义标记之前使用现有组件。** 在编写样式 `div` 之前检查组件是否存在。
- **标注使用 `Alert`。** 不要构建自定义样式 div。
- **空状态使用 `Empty`。** 不要构建自定义空状态标记。
- **Toast 通过 `sonner`。** 使用 `sonner` 中的 `toast()`。
- **使用 `Separator`** 而不是 `<hr>` 或 `<div className="border-t">`。
- **使用 `Skeleton`** 作为加载占位符。不要使用自定义 `animate-pulse` div。
- **使用 `Badge`** 而不是自定义样式 span。

### 图标 → [icons.md](./rules/icons.md)

- **`Button` 中的图标使用 `data-icon`。** 图标上的 `data-icon="inline-start"` 或 `data-icon="inline-end"`。
- **组件内的图标不要使用大小类。** 组件通过 CSS 处理图标大小。不要使用 `size-4` 或 `w-4 h-4`。
- **将图标作为对象传递，而不是字符串键。** `icon={CheckIcon}`，而不是字符串查找。

### CLI

- **永远不要手动解码或获取预设代码。** 对于现有项目，将它们直接传递给 `npx shadcn@latest apply --preset <code>`，或在初始化时使用 `npx shadcn@latest init --preset <code>`。

## 关键模式

这些是区分正确 shadcn/ui 代码的最常见模式。对于边缘情况，请参阅上面链接的规则文件。

```tsx
// 表单布局：FieldGroup + Field，而不是 div + Label。
<FieldGroup>
  <Field>
    <FieldLabel htmlFor="email">Email</FieldLabel>
    <Input id="email" />
  </Field>
</FieldGroup>

// 验证：Field 上的 data-invalid，控件上的 aria-invalid。
<Field data-invalid>
  <FieldLabel>Email</FieldLabel>
  <Input aria-invalid />
  <FieldDescription>Invalid email.</FieldDescription>
</Field>
