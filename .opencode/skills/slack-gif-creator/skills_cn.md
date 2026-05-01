---
name: slack-gif-creator
description: 用于创建针对 Slack 优化的动画 GIF 的知识和实用程序。提供约束、验证工具和动画概念。当用户请求为 Slack 创建动画 GIF 时使用，例如"为 Slack 制作一个 X 做 Y 的 GIF"。
license: 完整条款见 LICENSE.txt
---

# Slack GIF 创建器

提供用于创建针对 Slack 优化的动画 GIF 的实用程序和知识的工具包。

## Slack 要求

**尺寸：**
- 表情符号 GIF：128x128（推荐）
- 消息 GIF：480x480

**参数：**
- FPS：10-30（越低文件大小越小）
- 颜色：48-128（越少 = 文件大小越小）
- 持续时间：表情符号 GIF 保持在 3 秒以下

## 核心工作流程

```python
from core.gif_builder import GIFBuilder
from PIL import Image, ImageDraw

# 1. 创建构建器
builder = GIFBuilder(width=128, height=128, fps=10)

# 2. 生成帧
for i in range(12):
    frame = Image.new('RGB', (128, 128), (240, 248, 255))
    draw = ImageDraw.Draw(frame)

    # 使用 PIL 原语绘制动画
    # （圆形、多边形、线条等）

    builder.add_frame(frame)

# 3. 使用优化保存
builder.save('output.gif', num_colors=48, optimize_for_emoji=True)
```

## 绘制图形

### 处理用户上传的图像
如果用户上传图像，考虑他们是否想要：
- **直接使用**（例如，"动画化这个"、"将其拆分为帧"）
- **用作灵感**（例如，"制作类似这样的东西"）

使用 PIL 加载和处理图像：
```python
from PIL import Image

uploaded = Image.open('file.png')
# 直接使用，或仅作为颜色/样式的参考
```

### 从头开始绘制
从头开始绘制图形时，使用 PIL ImageDraw 原语：

```python
from PIL import ImageDraw

draw = ImageDraw.Draw(frame)

# 圆形/椭圆
draw.ellipse([x1, y1, x2, y2], fill=(r, g, b), outline=(r, g, b), width=3)

# 星形、三角形、任何多边形
points = [(x1, y1), (x2, y2), (x3, y3), ...]
draw.polygon(points, fill=(r, g, b), outline=(r, g, b), width=3)

# 线条
draw.line([(x1, y1), (x2, y2)], fill=(r, g, b), width=5)

# 矩形
draw.rectangle([x1, y1, x2, y2], fill=(r, g, b), outline=(r, g, b), width=3)
```
