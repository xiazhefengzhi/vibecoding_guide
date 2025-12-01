# 配置 Claude Code 环境

跟着这篇走一遍，10 分钟搞定 Claude Code 的安装和配置。

---

## 第一步：搞个 API 密钥

Claude Code 官方的 API 有点贵，咱们先用第三方中转服务，便宜又稳定：

- [aicodemirror.com](https://www.aicodemirror.com/)
- [88code.org](https://88code.org/)（推荐，有中文文档）

注册账号 → 充点钱 → 拿到你的 API 密钥，长这样：`sk-xxxxx`

---

## 第二步：打开终端

**终端在哪？** 看左边侧边栏那个蓝色小图标，点一下就出来了：


把终端想象成你跟电脑"说话"的地方。你敲一行命令，电脑就乖乖去执行。以后装包、跑脚本、配环境，都在这儿搞定。

---

## 第三步：安装 Claude Code

在终端里敲这行命令，回车：

```bash
npm install -g @anthropic-ai/claude-code
```

等它跑完，Claude Code 就装好了。

---

## 第四步：配置 API 密钥

有两种方式，选一个就行：

### 方式一：临时配置（重启终端就没了）

适合先试试水，敲这两行：

```bash
export ANTHROPIC_BASE_URL="https://www.88code.org/api"
export ANTHROPIC_AUTH_TOKEN="你的API密钥"
```

把"你的API密钥"换成你刚才拿到的那串 `sk-xxxxx`。

### 方式二：永久配置（一劳永逸）

不想每次都敲？把配置写进系统文件，一次搞定：

```bash
echo 'export ANTHROPIC_BASE_URL="https://www.88code.org/api"' >> ~/.zshrc
echo 'export ANTHROPIC_AUTH_TOKEN="你的API密钥"' >> ~/.zshrc
source ~/.zshrc
```

> 小提示：Mac 默认用 zsh，所以写到 `~/.zshrc`。如果你用的是 bash，就换成 `~/.bash_profile`。

---

## 第五步：验证一下

敲 `claude` 回车，如果出来 Claude Code 的界面，恭喜你，配置成功了！

---

## 遇到问题？

查查官方文档：[88code.org 配置指南](https://docs.88code.org/ClaudeCode/Windows.html)


