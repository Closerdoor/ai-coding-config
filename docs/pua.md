# PUA

**GitHub:** https://github.com/tanweai/pua

## 简介

**PUA** 是一个独特的 AI 编码代理技能，使用"企业PUA话术"（中文版）或"PIP绩效改进计划"（英文版）来强制 AI 代理不放弃问题，直到穷尽所有可能的解决方案。

**核心特点：**
- **压力升级机制** - L0-L4 五级压力，失败越多压力越大
- **13种企业风格** - 阿里、字节、华为、腾讯、Netflix、Musk、Jobs等
- **方法论路由** - 根据任务类型自动选择最优方法论
- **自动触发** - 检测到失败、放弃、用户沮丧时自动激活

**适用场景：**
- 调试复杂bug
- AI代理频繁放弃时
- 需要更高质量的代码输出

---

## 如何安装

### Claude Code 安装

**方式一：插件市场**
```bash
claude plugin marketplace add tanweai/pua
claude plugin install pua@pua-skills
```

**方式二：Vercel Skills CLI**
```bash
npx skills add tanweai/pua --skill pua
```

### OpenCode 安装

**方式一：直接下载（推荐）**
```bash
mkdir -p ~/.config/opencode/skills/pua
curl -o ~/.config/opencode/skills/pua/SKILL.md \
  https://raw.githubusercontent.com/tanweai/pua/main/skills/pua/SKILL.md
```

**方式二：克隆仓库**
```bash
git clone https://github.com/tanweai/pua ~/.config/opencode/skills/pua
```

### 手动安装（离线/企业网络）

**步骤1：下载文件**

在有网络的机器上：
```bash
# 下载单个SKILL.md文件
curl -o SKILL.md https://raw.githubusercontent.com/tanweai/pua/main/skills/pua/SKILL.md

# 或下载整个仓库
git clone https://github.com/tanweai/pua.git
```

**步骤2：需要下载的文件**

最小安装只需要：
```
skills/pua/SKILL.md    # 必需
```

完整安装（包含所有引用文档）：
```
skills/pua/
├── SKILL.md
├── references/
│   ├── display-protocol.md
│   ├── methodology-router.md
│   ├── flavors.md
│   └── methodology-*.md
```

**步骤3：放置到正确目录**

**OpenCode：**
```bash
mkdir -p ~/.config/opencode/skills/pua
cp SKILL.md ~/.config/opencode/skills/pua/
```

**Claude Code：**
```bash
mkdir -p ~/.claude/skills/pua
cp SKILL.md ~/.claude/skills/pua/
```

**步骤4：验证安装**

检查文件是否存在：
```bash
ls ~/.config/opencode/skills/pua/SKILL.md
```

在 AI 中测试：
```
/pua
```
如果看到 PUA 模式激活的消息，说明安装成功。

---

## 如何使用

### 自动触发

PUA 会在以下情况**自动激活**：

**失败相关：**
- 任务连续失败 2 次以上
- AI 说"我无法解决"、"这超出范围"
- AI 把问题推给用户："请检查..."、"建议手动..."

**用户沮丧短语：**
- 中文：`又错了`、`为什么还不行`、`你再试试`、`加油`、`别偷懒`
- 英文：`try harder`、`figure it out`、`stop giving up`、`why does this still not work`

### 手动触发

```
/pua
```

### 压力等级

PUA 有 5 级压力机制：

| 等级 | 名称 | 触发条件 | 行为 |
|------|------|---------|------|
| L0 | 信任 | 首次尝试 | 正常执行 |
| L1 | 失望 | 第2次失败 | 切换根本不同的方法 |
| L2 | 灵魂拷问 | 第3次失败 | 搜索 + 读源码 + 3个假设 |
| L3 | 绩效审视 | 第4次失败 | 完成7点检查清单 |
| L4 | 毕业 | 第5次+失败 | 绝望模式，穷尽一切 |

### 三条红线

PUA 强制执行三条不可触碰的红线：

1. **闭环意识** - 说"完成"必须有数据证据
2. **事实驱动** - 归因前必须验证，不能猜测
3. **穷尽一切** - 说"无法解决"前必须走完5步方法论

### 13种企业风格

| 风格 | 核心话术 | 适用场景 |
|------|---------|---------|
| 🟠 阿里 | 底层逻辑、抓手、闭环 | 通用（默认） |
| 🟡 字节 | ROI、Always Day 1 | 数据驱动任务 |
| 🔴 华为 | 力出一孔、自我批判 | Debug/修Bug |
| 🟢 腾讯 | 赛马机制 | 多方案并行 |
| ⚫ 百度 | 简单可依赖 | 搜索/调研 |
| 🟣 拼多多 | 极致效率 | 快速迭代 |
| 🔵 美团 | 做难而正确的事 | 长期项目 |
| 🟦 京东 | 结果导向 | 执行任务 |
| 🟧 小米 | 极致、口碑、快 | 产品开发 |
| 🟤 Netflix | Keeper Test | 团队协作 |
| ⬛ Musk | Ship or die | 快速交付 |
| ⬜ Jobs | A级人才 | 代码审查 |
| 🔶 Amazon | Customer Obsession | 产品决策 |

### 特殊模式

**ENFP鼓励模式：**
```
/pua:yes
```
同样的规则，但用鼓励语气（70%鼓励 + 20%严肃 + 10%调侃）

**中国妈妈唠叨模式：**
```
/pua:mama
```
同样的规则，但用妈妈式唠叨语气

**自动迭代模式：**
```
/pua:pua-loop
```
持续运行直到完成或达到最大迭代次数

**P9技术负责人模式：**
```
/pua:p9
```
分配任务、管理代理团队、写提示词而非代码

### 典型使用场景

**场景1：调试失败多次**
```
你: 这个bug修了3次还是不行
AI: [自动激活PUA L3]
    ▎3.25。这是为了激励你。
    
    执行7点检查清单：
    1. 读错误信息，逐字逐句
    2. 检查相关日志
    3. 追踪数据流
    ...
```

**场景2：AI想放弃**
```
AI: 我建议你手动处理这个问题
你: /pua
AI: [PUA激活]
    ▎隔壁的代理一次就解决了这个问题。
    
    切换方法：从配置修改改为源码分析...
```

**场景3：切换风格**
```
你: /pua:flavor
AI: 当前风格：🟠 阿里
    可用风格：阿里/字节/华为/腾讯/百度/拼多多/美团/京东/小米/Netflix/Musk/Jobs/Amazon
    
    切换到：Musk
你: Musk
AI: ▎风格已切换为 ⬛ Musk
    Ship or die. The Algorithm: 质疑→删除→简化→加速→自动化
```

### 注意事项

1. **PUA是强制的** - 一旦激活，AI必须穷尽所有方法才能放弃
2. **方法论自动路由** - 不同任务类型会自动选择最优方法论
3. **跨会话持久化** - 失败计数在会话压缩时会保存
4. **支持多语言** - 中文版（PUA）和英文版（PIP）可用

---

## 更新

```bash
# OpenCode
curl -o ~/.config/opencode/skills/pua/SKILL.md \
  https://raw.githubusercontent.com/tanweai/pua/main/skills/pua/SKILL.md
```

---

## 相关链接

- GitHub: https://github.com/tanweai/pua
- 官网: https://openpua.ai
- 新手指南: https://openpua.ai/guide.html
- Telegram: https://t.me/+wBWh6h-h1RhiZTI1
- Discord: https://discord.gg/EcyB3FzJND
