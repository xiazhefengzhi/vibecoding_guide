# Claude Skills 使用指南：给 Claude 装上"职业技能包"

> **Vibe 语录：** 如果说 MCP 是让 AI 能"连接外部世界"，那 Skills 就是让 AI 学会"怎么干活"。
> 一个是给 AI 装网线，一个是给 AI 发工作手册。

你可能已经习惯了跟 Claude 聊天，让它帮你写写文章、出出主意。但你知道吗？Claude 其实可以变得更专业、更像一个执行力超强的"特种兵"。

这就需要用到我们今天要讲的主角——**Claude Skills**。

---

## 一、什么是 Claude Skills？（先搞懂概念）

### 用游戏角色来理解

刚出生的 Claude 就像一个**"全能新手角色"**，智力很高，什么都懂一点，但手上没有任何装备和技能。

**Claude Skills 就像是给这个角色装备的"职业技能书"：**

| 装上的 Skill | 转职后的角色 | 能做什么 |
|-------------|-------------|---------|
| `document-skills` | **高级文员** | 合并 PDF、转换 Word 格式 |
| `webapp-testing` | **测试工程师** | 自动测试网页、检查 Bug |
| `artifacts-builder` | **前端开发** | 创建复杂的交互式网页应用 |
| `ai-partner-chat` | **AI 伴侣** | 个性化对话、记忆你的偏好 |

### 技术上讲

```
Skills = 提示词模板 + 固定工具函数 + 标准工作流程
```

**简单来说：** Skills 就是一套"预设好的工作流程"。你不需要告诉它"先打开文件，读取内容，然后转换格式，最后保存"，你只需要说"用这个技能帮我处理一下"，它就会自动按标准流程把活干完。

---

## 二、Skills vs MCP vs Commands（别搞混了）

这是最容易晕的地方！我们用**做饭**来比喻：

| 工具 | 类比 | 作用 | 触发方式 |
|-----|------|------|---------|
| **MCP** | 厨房的水管、煤气管 | 连接外部世界（文件、浏览器、数据库） | 自动连接 |
| **Skills** | 预制菜/自动烹饪机 | 按既定流程完成任务 | **自动触发**（Claude 识别场景） |
| **斜杠命令** | 菜单点餐 | 手动执行固定流程 | **手动触发**（输入 /xxx） |
| **SubAgents** | 请来帮厨 | 并行处理多个任务 | 手动创建 |
| **Plugins** | 厨具打包礼盒 | 打包上述所有工具 | 一键安装 |

**记住这个口诀：**
```
Skills 自动触发像管家，
斜杠命令手动叫出来。
MCP 连接外部管道通，
SubAgent 并行干活快。
```

---

## 三、Skills 的核心黑科技：渐进式加载

这是 Skills 最厉害的地方！

### 传统方式 vs 渐进式加载

```
传统方式：一次性加载所有内容
┌──────────────────────────────────┐
│ 系统启动                          │
│ ↓                                │
│ 加载所有技能的完整内容             │  ← 浪费 Token！
│ (10,000+ tokens)                 │
│ ↓                                │
│ 开始对话                          │
└──────────────────────────────────┘

渐进式加载：分层按需加载
┌──────────────────────────────────┐
│ 系统启动                          │
│ ↓                                │
│ 只加载元数据 (100 tokens/skill)   │  ← 节省 Token！
│ ↓                                │
│ 用户提问                          │
│ ↓                                │
│ 自动判断需要哪个技能               │
│ ↓                                │
│ 只加载相关技能的完整内容            │  ← 按需加载
└──────────────────────────────────┘
```

### 三层加载系统

| 层级 | 内容 | 加载时机 | Token 成本 |
|-----|------|---------|-----------|
| **Level 1** | 名称 + 描述（元数据） | Claude 启动时 | ~100 tokens/skill |
| **Level 2** | SKILL.md（详细指令） | 触发技能时 | ~5,000 tokens |
| **Level 3** | 脚本、模板、文档 | 执行过程中需要时 | 几乎为 0 |

**这意味着：** 你可以装几十个 Skills，但不会拖慢 Claude！

---

## 四、如何使用 Skills？（实操教程）

### 方法 1：自然语言调用

最简单！直接说你要用哪个技能：

```
使用 document-skills:pdf 帮我把这三个 PDF 合并成一个
```

```
用 webapp-testing 测试一下 localhost:3000 的登录功能
```

### 方法 2：让 Claude 自动识别

很多 Skills 会自动触发，你只需要描述任务：

```
帮我把桌面上的 file1.pdf、file2.pdf 合并成一个总文件
```

Claude 会自动识别这是 PDF 处理任务，然后调用 `document-skills:pdf`。

### 方法 3：查看可用 Skills

在 Claude Code 中输入：

```
列出你现在所有可用的 Skills，简单告诉我它们是干嘛的
```

---

## 五、常用 Skills 推荐（直接用）

### 1. Document Skills（文档专家）⭐⭐⭐⭐⭐

**技能名称：** `document-skills:pdf` / `docx` / `xlsx` / `pptx`

**能做什么：**
- PDF：合并、拆分、提取文字/表格、填写表单
- Word：创建、编辑、格式转换
- Excel：数据分析、公式计算、图表生成
- PPT：创建演示文稿、编辑布局

**使用示例：**
```
用 document-skills:pdf 把这三个合同 PDF 合并，然后提取里面的表格数据到 Excel
```

```
用 document-skills:xlsx 分析这份销售数据，生成月度趋势图
```

### 2. Webapp Testing（网页测试员）⭐⭐⭐⭐

**技能名称：** `example-skills:webapp-testing`

**能做什么：**
- 自动测试本地网页应用
- 模拟不同设备尺寸
- 检查按钮、链接是否正常
- 截图保存测试结果

**使用示例：**
```
使用 webapp-testing 测试 localhost:3000：
1. 测试登录功能（用户名 admin，密码 123456）
2. 检查导航栏在手机尺寸下的显示
3. 每步都截图保存
```

### 3. Artifacts Builder（应用构建器）⭐⭐⭐⭐

**技能名称：** `example-skills:artifacts-builder`

**能做什么：**
- 创建复杂的交互式 HTML 应用
- 使用 React、Tailwind CSS、shadcn/ui
- 支持状态管理、路由

**使用示例：**
```
使用 artifacts-builder 创建一个待办事项应用：
- 可以添加、删除、标记完成
- 有过滤功能（全部/进行中/已完成）
- 样式要现代简洁
```

### 4. Skill Creator（技能创造者）⭐⭐⭐⭐

**技能名称：** `example-skills:skill-creator`

**能做什么：**
- 帮你创建自定义 Skill
- 生成标准的 SKILL.md 文件
- 设计触发条件和工作流程

**使用示例：**
```
使用 skill-creator 帮我创建一个"周报生成器"技能：
- 读取本周的 Git 提交记录
- 汇总完成的任务
- 生成标准格式的周报
```

---

## 六、Partner Skills 实战案例（高级玩法）

这是最酷的部分！用 Skills 打造你专属的 AI 伴侣。

### AI Partner Chat Skill 介绍

**项目地址：** https://github.com/eze-is/ai-partner-chat

**能做什么：**
- 让 AI 记住你的经历、偏好、性格
- 根据你的笔记自动生成个性化画像
- 聊得越多，越懂你
- 不是"聊天机器人"，而是"AI 伴侣"

### 配置步骤（小白友好版）

**Step 1：下载 Skill 包**

```bash
# 在终端运行
git clone https://github.com/eze-is/ai-partner-chat
```

**Step 2：放到 Skills 目录**

把下载的文件夹放到你项目的 `.claude/skills/` 目录下：

```
你的项目/
└── .claude/
    └── skills/
        └── ai-partner-chat/
            └── SKILL.md
```

**Step 3：导入你的笔记**

把你的日记、笔记放到 `notes/` 文件夹：

```
你的项目/
└── notes/
    ├── 2024日记.md
    ├── 工作笔记.md
    └── 想法随记.md
```

**Step 4：启动对话**

在 Claude Code 中说：

```
遵循 ai-partner-chat 对话
```

AI 会自动：
1. 读取你的笔记
2. 生成你的个人画像（user-persona.md）
3. 生成 AI 的性格画像（ai-persona.md）
4. 建立记忆索引

### 效果对比

| 场景 | 普通 AI | AI Partner |
|-----|--------|-----------|
| 问"今天做点什么" | 给出通用建议 | 根据你最近的状态和习惯给建议 |
| 问"我的产品思考" | 问你具体是什么 | 总结你过去一年的思考趋势变化 |
| 日常对话 | 客气但疏远 | 了解你的性格，像老朋友 |

---

## 七、创建自定义 Skills（进阶）

### Skill 文件结构

```
my-skill/
├── SKILL.md          ← 必需！核心文件
├── scripts/          ← 可选，Python/Shell 脚本
│   └── process.py
├── references/       ← 可选，参考文档
│   └── api-docs.md
└── templates/        ← 可选，模板文件
    └── report-template.md
```

### SKILL.md 模板

```markdown
---
name: 周报生成器
description: 根据 Git 提交记录自动生成周报
---

# 周报生成器

## 目的
帮助用户快速生成标准格式的工作周报。

## 何时使用
当用户提到"周报""总结本周工作"时自动触发。

## 执行步骤

### Step 1: 获取 Git 记录
读取本周的所有 Git commit 记录。

### Step 2: 分类整理
按以下类别分组：
- 新功能开发
- Bug 修复
- 代码优化
- 文档更新

### Step 3: 生成周报
按公司模板格式输出：
- 本周完成
- 下周计划
- 遇到的问题

## 输出格式
Markdown 格式的周报文档。

## 注意事项
- 如果没有 Git 仓库，提示用户手动输入
- 自动识别是否需要中文/英文
```

### 高级技巧：Skill + MCP 组合

Skills 可以调用 MCP 工具！

```markdown
# 智能代码审查

## 执行步骤
1. 用 filesystem MCP 读取代码文件
2. 用 sequential-thinking MCP 深度分析
3. 用 memory MCP 记录审查结论
4. 输出分级报告
```

---

## 八、Skills 最佳实践

### 1. 元数据要精简

```yaml
# ❌ 错误：描述太长
description: 这是一个非常强大的工具，可以帮助你完成各种各样的文档处理任务...（500字）

# ✅ 正确：简洁明了
description: 处理 PDF 文档：合并、拆分、提取文字和表格
```

### 2. 触发条件要明确

```markdown
# ❌ 模糊的触发
当用户需要帮助时

# ✅ 明确的触发
当用户提到"提交代码""commit""推送""PR"时
```

### 3. 执行步骤要具体

```markdown
# ❌ 太抽象
检查代码质量

# ✅ 具体可执行
1. 运行 ESLint 检查语法
2. 运行 Prettier 格式化
3. 运行相关单元测试
4. 检查测试覆盖率是否 > 80%
```

### 4. 版本管理（团队协作）

```
team-skills/
├── code-review/
│   ├── v1.0.0/SKILL.md
│   ├── v1.1.0/SKILL.md
│   └── v2.0.0/SKILL.md  ← 当前版本
```

---

## 九、常见问题排查

### Q1：Skill 不触发？

**检查清单：**
- [ ] 文件在 `.claude/skills/` 目录下？
- [ ] 文件名是 `SKILL.md`（大写）？
- [ ] 包含 `name` 和 `description` 字段？
- [ ] 触发关键词是否明确？

**调试方法：**
```
列出当前项目的所有 Skills
```

### Q2：Skill 触发但执行出错？

**排查步骤：**
1. 检查 Markdown 格式是否正确
2. 检查依赖的 MCP 是否连接
3. 检查脚本是否有执行权限

### Q3：Token 消耗过高？

**优化建议：**
- 精简 Level 1 的描述（< 200 字）
- 把大文件放到 references/
- 使用外部脚本处理复杂逻辑

---

## 十、Skills 资源汇总

### 官方资源

- [Claude Skills 官方文档](https://docs.claude.com/en/docs/agents-and-tools/agent-skills)
- [Skills 仓库](https://github.com/anthropics/skills)
- [Cookbooks](https://github.com/anthropics/claude-cookbooks/tree/main/skills)

### 社区资源

- [AI Partner Chat](https://github.com/eze-is/ai-partner-chat) - 个性化 AI 伴侣
- [awesome-claude-skills](https://github.com/anthropics/awesome-claude-skills) - 社区精选

### 本项目已安装的 Skills

查看项目根目录的 `AWESOME_CLAUDE_SKILLS.md` 了解完整列表。

---

## 十一、Vibe Coding 总结

```
Skills = AI 的职业技能书
SKILL.md = 工作手册
渐进式加载 = 按需加载，省 Token

Skill 自动触发像管家，
斜杠命令手动叫出来。
```

**心态转变：**
- **以前：** 每次都要详细解释任务步骤
- **现在：** 一句话调用技能，自动执行标准流程

**下一步：**
1. 试试内置的 document-skills
2. 用 skill-creator 创建你的第一个自定义 Skill
3. 尝试 AI Partner Chat，体验个性化 AI

---

## 十二、你的第一次 Skills 实战

打开 Claude Code，输入：

```
列出你现在可用的所有 Skills
```

然后试试：

```
使用 document-skills:xlsx 创建一个简单的待办事项表格，包含任务名称、优先级、截止日期三列
```

或者更 Vibe 一点：

```
嘿，帮我用 artifacts-builder 做个贪吃蛇游戏，要有计分板，背景黑色的
```

享受 Skills 带来的效率提升吧！

---

> **下一步推荐阅读：**
> - [Claude Code 如何使用](./claude%20code%20如何使用.md)
> - [MCP 工具如何配置](./mcp工具.md)
> - [Prompt 如何有效快速入门](./prompt如何有效快速入门.md)
