# 安装第一个 MCP 插件：Playwright

MCP 插件是 Claude Code 的"外挂技能"。装上 Playwright 这个插件，Claude 就能帮你操作浏览器了——打开网页、点按钮、截图、填表单，全都行。

---

## MCP 插件去哪找？

逛逛这个网站：[smithery.ai](https://smithery.ai/)

这是个 MCP 插件商店，各种插件应有尽有。今天咱们装的 Playwright 在这：

👉 [Playwright MCP 插件](https://smithery.ai/server/@microsoft/playwright-mcp)

---

## 怎么安装？

分两步：先配置 MCP 插件，再在本地装依赖包。缺一不可。

---

### 第一步：配置 MCP 插件

打开 [Playwright MCP 插件页面](https://smithery.ai/server/@microsoft/playwright-mcp)，找到 **Claude Code** 选项卡。

你会看到两种配置方式：
- **本地配置（local）**：只在你这台电脑生效
- **云端配置（user）**：同步到你的账号，换电脑也能用

选一个，复制那串命令，粘贴到终端运行。

大概长这样：

```bash
claude mcp add playwright -s user -- npx -y @anthropic-ai/claude-code-mcp-server-playwright
```

> ⚠️ 注意：这步只是告诉 Claude "有这个插件可以用"，但插件要真正跑起来，还得装本地依赖。

---

### 第二步：本地安装 Playwright 包

MCP 配置好了，但 Playwright 本体还没装呢。打开终端，敲这两行：

**装 Playwright 包：**

```bash
npm install -g playwright
```

**装浏览器（Playwright 需要一个浏览器来干活）：**

```bash
npx playwright install chromium
```

等它下载完，才算真正装好了。

---

### 为什么要装两次？

打个比方：
- **配置 MCP** = 告诉 Claude："你可以用 Playwright 这个技能"
- **本地装包** = 把 Playwright 这个工具真正装到你电脑上

两边都装好，Claude 才能调用 Playwright 帮你干活。

---

## 验证安装成功没

在 Claude Code 里敲：

```
/mcp
```

看到列表里有 `playwright` 就说明装好了。

---

## 怎么用？

装好之后，直接跟 Claude 说人话就行：

```
用 playwright 打开百度首页，截个图给我看看
```

```
用 playwright 访问 https://example.com，告诉我页面上有什么内容
```

```
用 playwright 打开淘宝，搜索"机械键盘"
```

Claude 会自动调用 Playwright 帮你操作浏览器，你坐着看就行。

---

## 实际应用场景

| 场景 | 怎么跟 Claude 说 |
|------|------------------|
| 截网页图 | "帮我截一下 xxx.com 的首页" |
| 抓取内容 | "打开这个网页，把文章内容提取出来" |
| 填写表单 | "帮我在这个页面的搜索框里输入 xxx" |
| 自动化测试 | "访问我的网站，检查登录功能正不正常" |

---

## 遇到问题？

**浏览器打不开？** 跟 Claude 说：

```
playwright 浏览器启动失败，帮我排查一下
```

**插件没识别到？** 重启一下 Claude Code，再敲 `/mcp` 看看。