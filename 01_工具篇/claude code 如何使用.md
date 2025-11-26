# Claude Code：住在你终端里的"首席架构师"

> **Vibe 语录：** 告别在 ChatGPT 和 VS Code 之间疯狂 `Ctrl+C/V` 的日子。让 AI 直接接管键盘，你只需要负责喝咖啡和把关。

欢迎来到 Claude Code 的世界！如果你之前只用过网页版的 AI 聊天（ChatGPT、Claude、豆包），那你可能还停留在"复制粘贴时代"。

这就好比你之前只能通过微信和一个**超级聪明的远方朋友**聊天，他给你建议，你自己动手改代码。现在，这个朋友**直接搬进你家**了，你只需要说"帮我把厨房收拾一下"，他就直接动手干活。

---

## 一、为什么要用 Claude Code？（它不是插件，是室友）

### 痛点场景：复制粘贴地狱

你是不是经常这样：

1. 在网页版 ChatGPT/Claude 上问问题
2. AI 给你一段代码
3. 复制 -> 粘贴到 VS Code
4. 运行 -> 报错
5. 把报错信息复制回网页
6. AI 道歉："抱歉，我刚才漏了一个分号..."
7. 再复制 -> 再粘贴...

**累吗？**

### Claude Code 的超能力

| 网页版 AI | Claude Code |
|---------|-------------|
| 只能看你发的代码片段 | **直接读取整个项目** |
| 给你建议，你自己改 | **直接帮你改文件** |
| 出错了你还得复制报错 | **直接跑测试、自动修 Bug** |
| 像"网友" | 像"住在你电脑里的室友" |

**一句话定位：**
- **Copilot** 是帮你补全代码的**打字员**
- **Claude Code** 是坐在你旁边帮你写代码的**资深架构师**

---

## 二、快速召唤：三步让 AI 入职

### Step 1：安装（一行命令搞定）

打开终端（Mac 按 `Cmd+Space` 搜索 "Terminal"，Windows 搜索 "PowerShell"），输入：

```bash
npm install -g @anthropic-ai/claude-code
```

> **小白提示：** 如果报错说找不到 `npm`，说明你还没装 Node.js。
> 去 [nodejs.org](https://nodejs.org) 下载安装即可，选 LTS 版本。

### Step 2：认证（刷脸进门）

安装完成后，输入：

```bash
claude
```

系统会自动打开浏览器，让你登录 Anthropic 账号。授权成功后，终端会显示欢迎信息。

> **费用提醒：** Claude Code 使用的是 Anthropic API，会产生费用。
> 但别怕！日常使用一个月大概几十块钱。比雇一个程序员便宜多了吧？

### Step 3：进入项目（开始工作）

`cd` 到你的项目文件夹，再输入 `claude`：

```bash
cd ~/my-awesome-project
claude
```

Claude 会自动扫描你的代码，建立对项目的认知。这就像新员工入职第一天先熟悉环境。

---

## 三、基础指令：像老板一样发号施令

进入 Claude Code 后，你会看到一个交互界面。直接用自然语言说话就行！

### 3.1 日常问答

```
> 解释一下 src/utils.py 是干嘛的
```
Claude 会读取文件内容，给你详细解释。

```
> 这个项目的目录结构是什么？
```
它会给你画一个清晰的树状图。

### 3.2 改代码（最爽的功能）

```
> 把登录按钮改成圆角，颜色用 #3B82F6
```
Claude 会直接找到对应的 CSS 文件并修改。修改前会问你确认。

```
> 在 User 模型里加一个 phone 字段
```
它不仅改 Model，还会问你要不要顺便改数据库迁移文件。**懂事！**

### 3.3 修 Bug（神功能）

```
> 运行测试，如果报错就修好它
```
Claude 会：
1. 执行测试命令
2. 分析报错信息
3. 定位问题代码
4. 自动修复
5. 再跑测试验证

**你只需要说一句话，它干完全部活。**

### 3.4 常用命令速查

| 你说 | Claude 做 |
|-----|---------|
| "解释这段代码" | 读取并解释 |
| "这里为什么报错" | 分析错误原因 |
| "帮我写个函数做 XXX" | 生成代码并插入 |
| "重构这个文件" | 优化代码结构 |
| "写测试" | 生成单元测试 |
| "提交代码" | 执行 git add/commit |

---

## 四、进阶心法：让 AI 更懂你

### 4.1 CLAUDE.md —— 立下"家规" (超重要！)

这是 Claude Code 最强大的功能之一，但很多人不知道。

**痛点场景：**
- 每次开新对话，都要告诉 AI："我用的是 TypeScript"、"缩进用 2 个空格"、"测试用 Jest"...
- AI 经常自作主张用你不喜欢的库

**解决方案：** 在项目根目录创建 `CLAUDE.md` 文件

```markdown
# 项目规范

## 技术栈
- 前端：React 18 + TypeScript
- 样式：Tailwind CSS
- 状态管理：Zustand
- 测试：Vitest

## 代码风格
- 缩进：2 个空格
- 字符串：使用单引号
- 组件：函数式组件 + Hooks
- 禁止使用 any 类型

## 常用命令
- 启动项目：npm run dev
- 运行测试：npm test
- 构建：npm run build

## 目录结构
- src/components/ - React 组件
- src/hooks/ - 自定义 Hooks
- src/utils/ - 工具函数
- src/api/ - API 请求

## 注意事项
- 所有 API 请求通过 src/api/client.ts 发送
- 新组件必须写单元测试
- 提交前必须通过 ESLint 检查
```

**效果对比：**

| 没有 CLAUDE.md | 有 CLAUDE.md |
|--------------|-------------|
| "你用 Python 还是 JS？" | 直接知道用 TypeScript |
| 写出 `var` 声明 | 自动用 `const` |
| 乱用第三方库 | 只用你指定的库 |
| 每次都要重复说明 | 一劳永逸 |

> **比喻：** CLAUDE.md 就是公司的**员工手册**。
> 新员工入职先背熟规矩，就不会问一些低级问题了。

### 4.2 Plan Mode —— 先画图，再动工

**危险场景：**
你说"帮我重构整个用户系统"，AI 二话不说就开始改代码，改着改着项目跑不起来了...

**解决方案：** 使用 Plan Mode

```
> /plan 重构用户认证系统，支持 OAuth 登录
```

或者直接在请求后加上"先给我一个计划"：

```
> 重构用户认证系统。在动手之前，先给我一个详细的实施计划，我确认后你再开始。
```

Claude 会输出类似这样的计划：

```
## 重构计划

### 阶段 1：准备工作
1. 备份现有 auth 模块
2. 分析当前认证流程

### 阶段 2：OAuth 集成
1. 安装 passport.js
2. 创建 OAuth 配置文件
3. 实现 Google 登录
4. 实现 GitHub 登录

### 阶段 3：数据库更新
1. 修改 User 模型，添加 provider 字段
2. 创建迁移脚本

### 阶段 4：前端对接
1. 添加社交登录按钮
2. 处理 OAuth 回调

是否开始执行？
```

你确认后，它才开始一步步实施。

> **黄金法则：** 涉及**超过 3 个文件**的修改，**必须**用 Plan Mode！
> 否则就像让一个热情的实习生单独负责公司年会，结果可能是灾难。

### 4.3 思考预算 —— 给大脑充值

Claude Code 有一个隐藏功能：你可以控制它"想多久"。

想象 AI 的大脑是一台咖啡机：

| 档位 | 关键词 | 适用场景 | 费用 |
|-----|-------|---------|-----|
| 清水 | `think` | 简单问答、小改动 | 便宜 |
| 拿铁 | `think hard` | 复杂 Bug、算法设计 | 适中 |
| 双倍浓缩 | `think harder` | 架构重构、性能优化 | 较贵 |
| 静脉注射 | `ultrathink` | 疑难杂症、祖传代码 | 土豪专属 |

**使用方式：** 在请求中加入关键词

```
> think hard 这个递归函数为什么会栈溢出？
```

```
> ultrathink 帮我分析这个遗留系统的架构问题，给出重构建议
```

> **省钱建议：** 日常改 Bug 用默认档就够了。
> 只有遇到真正复杂的问题才需要"加咖啡"。

---

## 五、高级玩法：连接外部世界

### 5.1 MCP —— 给 AI 装外挂

**MCP (Model Context Protocol)** 让 Claude Code 能连接外部工具。

**比喻：** 本来你的室友只会写代码，装上 MCP 后，他还能：
- 帮你查 GitHub Issue
- 读取数据库数据
- 发飞书/钉钉消息
- 控制浏览器
- 查 Google 搜索结果

**常用 MCP 工具：**

```bash
# 安装 Playwright MCP（浏览器控制）
claude mcp add playwright

# 安装 GitHub MCP（查 Issue、PR）
claude mcp add github

# 安装 Notion MCP（读写 Notion 文档）
claude mcp add notion
```

装上后，你可以说：

```
> 帮我检查 GitHub 上有没有关于登录问题的 Issue
```

```
> 用浏览器测试一下登录页面能不能正常工作
```

### 5.2 省钱大法：切换模型

觉得 Claude 太贵？可以换成便宜的模型！

**方式 1：使用 DeepSeek（国产平替）**

```bash
export ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
export ANTHROPIC_AUTH_TOKEN="你的DeepSeek密钥"
export ANTHROPIC_MODEL="deepseek-chat"
```

**适合场景：** 写注释、生成文档、简单问答

**方式 2：使用 AWS Bedrock / Google Vertex AI**

如果你公司已经有云服务账号，可以走企业账单，更便宜。

详细配置参考 [官方文档](https://docs.anthropic.com/en/docs/claude-code)

### 5.3 多代理协作：让 AI 互相 Code Review

**高端玩法：** 用两个 Claude 实例，一个写代码，一个审代码。

```bash
# 终端 1：写代码的 Claude
cd ~/project
claude

# 终端 2：Review 的 Claude
cd ~/project
claude
```

在终端 2 里说：

```
> 审查一下 src/auth/login.ts 的代码，指出潜在问题
```

**为什么这样做？**
写代码的 AI 可能有"盲点"，另一个有"fresh eyes"的 AI 更容易发现问题。
就像人类开发者互相 Code Review 一样。

---

## 六、避坑指南（血泪经验）

### 1. 不要盲目按 `y`！

当 Claude 问你"是否执行这个命令？"时，**一定要看清楚**！

特别是看到这些关键词时要小心：
- `rm` (删除)
- `drop` (删库)
- `force` (强制操作)
- `-rf` (递归删除)

**惨痛教训：** 有人让 AI 清理临时文件，结果 AI 把整个项目删了...

### 2. Token 爆炸警告

经常用这个命令查账：

```
> /cost
```

**省 Token 技巧：**
- 不要把整个 `node_modules` 喂给它
- 用 `.gitignore` 风格的 `.claudeignore` 排除大文件
- 聊太久了就开新对话（老对话上下文太长）

### 3. Git 是后悔药

在让 Claude 大改之前，**务必先提交当前代码**：

```bash
git add .
git commit -m "保存点：让 AI 改之前"
```

这样即使 AI 改崩了，你还能回滚：

```bash
git checkout .
```

### 4. 复杂任务要拆解

**错误示范：**
```
> 帮我做一个完整的电商网站，包括用户系统、商品管理、购物车、支付、订单管理
```

**正确做法：**
```
> 先帮我做用户注册登录功能
```
（完成后）
```
> 现在加上商品列表页
```

AI 和人一样，一次只能专注一件事。

### 5. 敏感信息别暴露

**不要问：**
```
> 帮我把数据库密码改成 abc123456
```

**应该问：**
```
> 帮我设置数据库密码，从环境变量读取
```

Claude 会生成使用 `process.env.DB_PASSWORD` 的代码，不会把真实密码写死。

---

## 七、常见问题 FAQ

### Q：Claude Code 和 VS Code 里的 Copilot 有什么区别？

| Copilot | Claude Code |
|---------|-------------|
| 补全当前行代码 | 理解整个项目 |
| 只能建议 | 能直接改文件 |
| 被动触发 | 主动对话 |
| 像自动补全 | 像 pair programming |

**建议：** 两个一起用。Copilot 写代码时补全，Claude Code 做重构和修 Bug。

### Q：会不会把我的代码泄露给 Anthropic？

Claude Code 会发送代码到 Anthropic 的 API 进行处理，但 Anthropic 承诺不会用用户数据训练模型。
如果你在做机密项目，可以：
- 使用自托管方案
- 配置本地模型（通过 Ollama）
- 只在非敏感代码上使用

### Q：一个月大概花多少钱？

取决于使用频率：
- 轻度使用（每天问几个问题）：￥30-50/月
- 中度使用（日常开发）：￥100-200/月
- 重度使用（整天让它写代码）：￥300+/月

**省钱技巧：** 用 DeepSeek 做简单任务，Claude 只用来啃硬骨头。

---

## 八、Vibe Coding 总结

```
你的角色：产品经理 + 技术总监
Claude Code 的角色：执行团队

你负责：把控方向、定义需求、审核结果
它负责：读代码、写代码、跑测试、修 Bug
```

**心态转变：**
- **以前：** 我要写代码
- **现在：** 我要告诉 AI 写什么代码

**行动上：**
1. 写好 `CLAUDE.md`，立下家规
2. 复杂任务用 Plan Mode，先思考再动手
3. 遇到难题调高"咖啡浓度"（think harder）
4. 改代码前先 `git commit`，留好后悔药

---

## 九、你的第一次 Vibe Coding

现在，打开终端，输入：

```bash
cd ~/你的项目文件夹
claude
```

然后对它说：

```
> 阅读整个项目，给我一个简单的项目概述，包括使用的技术栈和主要功能模块
```

或者更 Vibe 一点：

```
> 嘿，新来的！先熟悉一下环境，告诉我这个项目是干嘛的
```

享受这一刻的 **Vibe** 吧！你的"首席架构师"已经就位。

---

## 十、省钱神器：Kimi K2 API 配置（小白保姆级）

觉得 Claude 官方 API 太贵？**Kimi K2** 是你的省钱神器！

### 为什么选 Kimi K2？

| 对比项 | Claude Opus | Kimi K2 |
|-------|-------------|---------|
| 输入价格 | ~$15/M tokens | **$0.6/M tokens** |
| 输出价格 | ~$75/M tokens | **$2.5/M tokens** |
| 性价比 | 贵 | **便宜 10 倍以上** |
| 编码能力 | 顶级 | 接近顶级（开源最强之一） |
| 上下文 | 200K | **256K** |
| 网络要求 | 需要科学上网 | **国内直连** |
| 支付方式 | 美元 | **人民币** |

> **简单说：** 用 Kimi K2 跑 Claude Code，一个月可能只花几块钱！

### Step 1：获取 Kimi API Key

1. 访问 [Moonshot AI 平台](https://platform.moonshot.cn/)
2. 注册账号（用手机号或邮箱）
3. 进入控制台 → API Keys → 创建新密钥
4. 复制你的密钥（格式：`sk-xxxxxxxxxxxxxxxx`）

> **注意：** 建议充值 ¥50 以上，否则有请求频率限制。

### Step 2：配置 Claude Code（四种方式任选）

#### 方式 A：一行命令搞定（最简单，推荐小白）

```bash
npx kimicc
```

第一次运行会让你输入 API Key，输入后自动配置完成！

#### 方式 B：环境变量配置（临时）

**Mac/Linux 用户：**
```bash
export ANTHROPIC_AUTH_TOKEN="sk-你的密钥"
export ANTHROPIC_BASE_URL="https://api.moonshot.cn/anthropic"
claude
```

**Windows CMD 用户：**
```cmd
set ANTHROPIC_AUTH_TOKEN=sk-你的密钥
set ANTHROPIC_BASE_URL=https://api.moonshot.cn/anthropic
claude
```

**Windows PowerShell 用户：**
```powershell
$env:ANTHROPIC_AUTH_TOKEN="sk-你的密钥"
$env:ANTHROPIC_BASE_URL="https://api.moonshot.cn/anthropic"
claude
```

> **注意：** 这种方式每次新开终端都要重新设置。

#### 方式 C：永久配置（一劳永逸，推荐）

**Mac/Linux - 编辑 shell 配置文件：**

```bash
# 打开配置文件（用 nano 或 vim）
nano ~/.zshrc   # 如果用 zsh
# 或
nano ~/.bashrc  # 如果用 bash

# 在文件末尾添加这两行：
export ANTHROPIC_AUTH_TOKEN="sk-你的密钥"
export ANTHROPIC_BASE_URL="https://api.moonshot.cn/anthropic"

# 保存后执行，让配置生效：
source ~/.zshrc
```

**Windows - 设置系统环境变量：**

1. 右键「此电脑」→「属性」→「高级系统设置」→「环境变量」
2. 在「用户变量」中点击「新建」：
   - 变量名：`ANTHROPIC_AUTH_TOKEN`
   - 变量值：`sk-你的密钥`
3. 再新建一个：
   - 变量名：`ANTHROPIC_BASE_URL`
   - 变量值：`https://api.moonshot.cn/anthropic`
4. 点击「确定」保存
5. **重启终端**才能生效

#### 方式 D：配置文件方式（Claude Code 1.0.61+）

编辑 `~/.claude/settings.json` 文件：

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "sk-你的密钥",
    "ANTHROPIC_BASE_URL": "https://api.moonshot.cn/anthropic",
    "ANTHROPIC_MODEL": "kimi-k2-0711-preview",
    "ANTHROPIC_SMALL_FAST_MODEL": "kimi-k2-0711-preview"
  }
}
```

**配置文件位置：**
- Windows：`C:\Users\你的用户名\.claude\settings.json`
- Mac/Linux：`~/.claude/settings.json`

> **提示：** 如果文件或目录不存在，直接创建即可。

### Step 3：验证配置成功

启动 Claude Code：

```bash
claude
```

输入测试：

```
> 你好，你是什么模型？
```

如果回复提到 "Kimi" 或 "Moonshot"，说明配置成功！

### 快速启动脚本（懒人福音）

**Windows 用户 - 创建 `start_claude_kimi.bat`：**

```batch
@echo off
set ANTHROPIC_AUTH_TOKEN=sk-你的密钥
set ANTHROPIC_BASE_URL=https://api.moonshot.cn/anthropic
claude
```

双击这个文件就能启动！

**Mac/Linux 用户 - 创建 `start_claude_kimi.sh`：**

```bash
#!/bin/bash
export ANTHROPIC_AUTH_TOKEN="sk-你的密钥"
export ANTHROPIC_BASE_URL="https://api.moonshot.cn/anthropic"
claude
```

使用方法：
```bash
chmod +x start_claude_kimi.sh  # 给执行权限（只需一次）
./start_claude_kimi.sh         # 启动
```

### 常见问题排查

**Q：配置后还是用的 Claude？**

检查环境变量是否生效：
```bash
# Mac/Linux
echo $ANTHROPIC_BASE_URL

# Windows CMD
echo %ANTHROPIC_BASE_URL%

# Windows PowerShell
echo $env:ANTHROPIC_BASE_URL
```
应该显示 `https://api.moonshot.cn/anthropic`

**Q：报错 `ANTHROPIC_API_KEY not found`？**

- 检查变量名是 `ANTHROPIC_AUTH_TOKEN` 不是 `ANTHROPIC_API_KEY`
- Windows 用户：重启终端
- Mac/Linux 用户：执行 `source ~/.zshrc`

**Q：报错 "rate limit"？**

Kimi 对免费用户有限制，建议充值 ¥50 以上解锁更高频率。

**Q：Kimi K2 有什么限制？**

- 不支持图片（无法读取截图）
- 部分复杂推理可能不如 Claude Opus
- 适合日常编码、写文档、改 Bug

**Q：API 调用失败？**

检查清单：
- ✅ API Key 是否正确（没有多余空格）
- ✅ Base URL 末尾不要多加 `/`
- ✅ 网络能否访问 `api.moonshot.cn`
- ✅ API Key 账户是否有余额

### 什么时候用 Kimi，什么时候用 Claude？

| 场景 | 推荐 |
|-----|-----|
| 日常改 Bug、写代码 | **Kimi K2**（省钱） |
| 写注释、生成文档 | **Kimi K2**（省钱） |
| 复杂架构设计 | Claude（更准） |
| 需要读图片 | Claude（Kimi 不支持） |
| 疑难杂症 | Claude + ultrathink |

> **Vibe 建议：** 平时用 Kimi 省钱，遇到硬骨头再切回 Claude。两个都配置好，随时切换！

---

> **下一步推荐阅读：**
> - [MCP 工具如何配置](./mcp如何配置.md)
> - [Prompt 如何有效快速入门](./prompt如何有效快速入门.md)
