# Vercel 配置完全指南 (小白专用)

## 什么是 Vercel?

Vercel 是一个超级强大的前端部署平台,让你**不用搭建服务器**就能把网站发布到全世界!

**核心优势:**
- 免费额度超大 (个人项目完全够用)
- 从 GitHub 一键部署 (不到 1 分钟)
- 全球 CDN 加速 (网站访问超快)
- 自动 HTTPS (网站更安全)
- 支持自定义域名

## 第一步: 注册 Vercel 账号

### 1.1 访问 Vercel 官网

打开浏览器,访问: **https://vercel.com**

### 1.2 使用 GitHub 账号登录 (强烈推荐)

```
点击 "Sign Up" 按钮
↓
选择 "Continue with GitHub"
↓
授权 Vercel 访问你的 GitHub
↓
注册成功!
```

> **小白提示**: 为什么用 GitHub 登录? 因为后面部署项目时,可以直接选择 GitHub 上的代码仓库,超级方便!

## 第二步: 部署你的第一个项目

### 方法一: GitHub 仓库一键部署 (推荐)

**具体操作步骤:**

1. **登录 Vercel** 后,你会看到控制台首页
2. **点击右上角 "Add New..." 按钮** → 选择 "Project"
3. **导入 GitHub 仓库**:
   - 如果是第一次使用,需要点击 "Import Git Repository"
   - 选择 "GitHub"
   - 可能需要安装 Vercel GitHub App (点击 Install 授权)
4. **选择要部署的仓库**:
   - 在列表中找到你的项目
   - 点击 "Import" 按钮
5. **配置项目设置** (通常 Vercel 会自动检测):
   - **Framework Preset**: 自动识别 (Next.js / React / Vue 等)
   - **Root Directory**: 项目根目录 (默认 `.`)
   - **Build Command**: 构建命令 (通常自动填好)
   - **Output Directory**: 输出目录 (通常自动填好)
6. **点击 "Deploy" 按钮**

**等待 1-2 分钟,部署完成!**

### 方法二: 使用 Vercel CLI (命令行)

```bash
# 第一步: 安装 Vercel CLI
npm install -g vercel

# 第二步: 在项目目录运行
cd your-project
vercel

# 第三步: 跟随命令行提示
# - 登录账号
# - 选择项目配置
# - 确认部署
```

## 第三步: 配置环境变量 (核心!)

### 3.1 什么是环境变量?

环境变量就是存放**敏感信息**的地方,比如:
- API 密钥 (OpenAI API Key)
- 数据库地址 (Supabase URL)
- 第三方服务 Token

**为什么要用环境变量?**
- 保护隐私: 不会把密钥暴露在代码里
- 安全: 即使代码公开,别人也拿不到你的密钥
- 灵活: 不同环境 (开发/生产) 可以用不同的配置

### 3.2 重要! 理解 Key 的来源 (小白必读)

**Vercel 是部署平台,不是 API Key 提供商!**

```
❌ 错误理解: 所有 Key 都在 Vercel 获取
✅ 正确理解:
   - 大部分 Key 在其他服务获取 (Supabase、OpenAI 等)
   - 然后把这些 Key 添加到 Vercel 作为环境变量
   - Vercel 只提供 Vercel Token (用于 Vercel API 访问)
```

**常见 Key 的获取位置:**

| Key 名称 | 获取位置 | 用途 | 添加到 Vercel? |
|---------|---------|------|---------------|
| `NEXT_PUBLIC_SUPABASE_URL` | **Supabase Dashboard** → Settings → API | 数据库连接 | ✅ 是 |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | **Supabase Dashboard** → Settings → API | 公开访问 | ✅ 是 |
| `SUPABASE_SERVICE_KEY` | **Supabase Dashboard** → Settings → API | 服务端访问 | ✅ 是 |
| `DATABASE_URL` | **Supabase Dashboard** → Settings → Database | PostgreSQL 连接 | ✅ 是 |
| `OPENAI_API_KEY` | **OpenAI Platform** → API Keys | AI 功能 | ✅ 是 |
| `VERCEL_TOKEN` | **Vercel Dashboard** → Settings → Tokens | Vercel API 访问 | ❌ 否* |

> **小白提示**:
> - ✅ 部署到 Vercel 时,需要把 Supabase/OpenAI 等服务的 Key 添加到 Vercel 环境变量
> - ❌ Vercel Token 是用于其他工具访问 Vercel,不需要添加到项目环境变量

### 3.3 如何在 Vercel 添加环境变量?

#### 方法一: 在网页控制台添加

**详细操作步骤:**

```
第1步: 打开 Vercel Dashboard
↓
第2步: 选择你的项目
↓
第3步: 点击顶部导航栏的 "Settings" 标签
↓
第4步: 在左侧菜单找到并点击 "Environment Variables"
↓
第5步: 添加变量
```

**具体填写方法:**

1. **Name (变量名)**: 输入变量名称
   - 例如: `OPENAI_API_KEY`
   - 注意: 如果需要在前端使用,必须以 `NEXT_PUBLIC_` 开头

2. **Value (值)**: 输入实际的密钥或配置
   - 例如: `sk-xxxxxxxxxxxxx`
   - **重要**: 粘贴时小心不要有多余空格!

3. **Environment (环境)**: 选择环境 (可多选)
   - **Production**: 生产环境 (正式网站)
   - **Preview**: 预览环境 (PR 预览)
   - **Development**: 开发环境 (本地开发)

4. **点击 "Save" 按钮保存**

**添加完成后,必须重新部署才会生效!**

#### 方法二: 使用命令行添加

```bash
# 添加生产环境变量
vercel env add OPENAI_API_KEY production

# 系统会提示你输入值,粘贴后按 Enter

# 查看所有环境变量
vercel env ls
```

### 3.3 环境变量使用示例

#### 例子 1: OpenAI API Key

```bash
# 变量名
OPENAI_API_KEY

# 值 (示例,请替换为你自己的)
sk-proj-abc123def456...
```

#### 例子 2: Supabase 配置

```bash
# Supabase 项目 URL
NEXT_PUBLIC_SUPABASE_URL=https://xxxxx.supabase.co

# Supabase 匿名密钥
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIs...
```

> **为什么加 NEXT_PUBLIC_?**
> - 加前缀的变量可以在浏览器端使用
> - 不加前缀的变量只能在服务器端使用
> - 永远不要把私密的 API Key 加 NEXT_PUBLIC_ 前缀!

## 第四步: 获取 Vercel API Token (高级功能)

### 4.1 什么是 Vercel API Token?

Vercel Token 是一个**访问凭证**,让其他工具 (比如 CI/CD、Trae MCP) 能够:
- 自动部署项目
- 管理环境变量
- 读取项目信息

### 4.2 如何获取 Vercel Token?

**超详细步骤 (每一步都有说明):**

```
第1步: 登录 Vercel Dashboard (https://vercel.com)
↓
第2步: 点击右上角头像 → 选择 "Settings"
↓
第3步: 在左侧菜单找到 "Tokens"
↓
第4步: 点击 "Create Token" 按钮
↓
第5步: 填写 Token 信息
```

**填写 Token 信息:**

1. **Token Name**: 给 Token 起个名字
   - 例如: "Trae MCP Token" 或 "GitHub Actions"
   - 建议: 用途清晰的名字,方便以后管理

2. **Scope**: 选择权限范围
   - **Full Account**: 完整账号权限 (谨慎选择)
   - **Specific Projects**: 指定项目 (推荐)

3. **Expiration**: 过期时间
   - 建议选择一个合理的过期时间,不要选 "No Expiration"

4. **点击 "Create" 按钮**

**重要! 立即复制 Token!**

```
创建成功后,页面会显示 Token 字符串:
vercel_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

⚠️ 这个 Token 只会显示一次!
立即复制并保存到安全的地方!
```

**保存到哪里?**
- 密码管理器 (1Password, LastPass 等)
- 或者记事本 (但要确保安全)
- **绝对不要**提交到 GitHub!

### 4.3 Token 类型对比

| Token 类型 | 权限范围 | 适用场景 | 安全性 |
|-----------|---------|---------|--------|
| **Personal Access Token** | 所有项目 | 个人使用 | 中 |
| **Project Token** | 单个项目 | 推荐使用 | 高 |
| **Team Token** | 团队级别 | 团队协作 | 中 |

> **小白建议**: 使用 **Project Token**,只给需要的项目授权,更安全!

### 4.4 如何使用 Token?

#### 场景 1: 在 Trae MCP 中使用

在 Trae 的 MCP 配置中:

```json
{
  "mcpServers": {
    "vercel": {
      "env": {
        "VERCEL_TOKEN": "你复制的 Token"
      }
    }
  }
}
```

#### 场景 2: 在 GitHub Actions 中使用

在 GitHub Secrets 中添加:
- Name: `VERCEL_TOKEN`
- Value: 你复制的 Token

## 第五步: 自定义域名配置 (可选)

### 5.1 为什么要自定义域名?

Vercel 默认域名:
- 格式: `your-project.vercel.app`
- 优点: 免费,自动配置
- 缺点: 不够专业,难记

自定义域名:
- 格式: `www.your-domain.com`
- 优点: 专业,好记,有品牌感
- 缺点: 需要购买域名 (一年几十块)

### 5.2 添加自定义域名步骤

```
第1步: 购买域名 (推荐: 阿里云, 腾讯云, Cloudflare)
↓
第2步: 在 Vercel 项目中点击 "Settings" → "Domains"
↓
第3步: 输入你的域名 (如 example.com)
↓
第4步: Vercel 会给你 DNS 配置说明
↓
第5步: 到域名提供商配置 DNS
```

### 5.3 DNS 配置方法

**在你的域名提供商 (阿里云/腾讯云/Cloudflare) 添加 DNS 记录:**

```
记录类型: CNAME
主机记录: @ (或 www)
记录值: cname.vercel-dns.com
TTL: 600 (10分钟) 或保持默认
```

**等待 5-30 分钟,DNS 生效后你的自定义域名就可以访问了!**

## 第六步: 常见问题解决

### Q1: 部署失败了怎么办?

**排查步骤:**

```
1. 查看构建日志
   在 Vercel Dashboard → 选择失败的部署 → "View Details"

2. 检查常见问题:
   - ❌ 缺少环境变量 → 添加后重新部署
   - ❌ Node.js 版本不对 → 在 package.json 添加:
     "engines": {
       "node": "18.x"
     }
   - ❌ 构建命令错误 → Settings → Build & Output Settings 检查
   - ❌ 依赖安装失败 → 检查 package.json

3. 重新部署
   修复问题后,点击 "Redeploy" 按钮
```

### Q2: 环境变量不生效?

**解决方案:**

1. **检查变量名是否正确**
   - 区分大小写! `OPENAI_API_KEY` ≠ `openai_api_key`
   - 前端使用必须加 `NEXT_PUBLIC_` 前缀

2. **重新部署**
   - **重要**: 修改环境变量后,必须重新部署!
   - 方法: Deployments → 最新部署 → "Redeploy"

3. **检查环境选择**
   - Production / Preview / Development 都选了吗?

4. **清除缓存**
   - 有时需要: Deployments → Redeploy → 勾选 "Use existing Build Cache" 取消

### Q3: Vercel 免费版够用吗?

**免费版额度:**

| 项目 | 免费额度 | 够用吗? |
|-----|---------|---------|
| 带宽 | 100 GB/月 | ✅ 个人项目绝对够 |
| 构建时间 | 100 小时/月 | ✅ 完全够用 |
| Serverless 函数 | 100次/小时 | ⚠️ 流量大时可能不够 |
| 团队成员 | 1 人 | ✅ 个人开发足够 |

**小白建议**: 个人学习和小项目,免费版完全够用!

### Q4: 国内访问 Vercel 慢怎么办?

**解决方案 (按推荐程度排序):**

1. **使用 Cloudflare CDN**
   - 绑定自己的域名
   - 将域名托管到 Cloudflare
   - 开启 Cloudflare CDN 加速

2. **使用 Vercel 的 Edge Network**
   - Vercel 在全球有边缘节点
   - 绑定域名后会自动优化

3. **考虑替代方案**
   - **Netlify**: 类似 Vercel
   - **Cloudflare Pages**: 国内速度更快
   - **EdgeOne Pages**: 腾讯云服务,国内速度快

## 实战案例: 5 分钟部署 AI 聊天应用

### 准备工作

```bash
# 1. 确保你有:
- GitHub 账号
- Vercel 账号 (用 GitHub 登录)
- OpenAI API Key
```

### 操作步骤

```
第1步: Fork 示例项目
访问: https://github.com/vercel/ai-chatbot
点击右上角 "Fork" 按钮
↓
第2步: 在 Vercel 导入项目
Vercel Dashboard → Add New → Project
选择刚才 Fork 的仓库 → Import
↓
第3步: 配置环境变量
Settings → Environment Variables
添加: OPENAI_API_KEY = sk-xxxxx
↓
第4步: 部署!
点击 "Deploy" 按钮
等待 1-2 分钟
↓
完成! 你有了自己的 AI 聊天应用!
```

## 高级配置

### vercel.json 配置文件

在项目根目录创建 `vercel.json`:

```json
{
  "version": 2,
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/api/(.*)",
      "dest": "/api/$1"
    }
  ]
}
```

### 构建配置优化

在 **Settings → Build & Output Settings**:

```bash
# 构建命令
Build Command: npm run build

# 输出目录 (根据框架不同)
Output Directory:
  - Next.js: .next
  - React/Vue: dist
  - Nuxt: .output

# 安装命令
Install Command: npm install
```

## 安全最佳实践

### 1. 保护你的 API Key

```bash
# ❌ 错误做法: 直接写在代码里
const apiKey = "sk-abc123..."  # 危险!

# ✅ 正确做法: 使用环境变量
const apiKey = process.env.OPENAI_API_KEY

# ❌ 错误: 提交 .env 文件到 Git
git add .env  # 千万别这样!

# ✅ 正确: .env 文件加入 .gitignore
echo ".env" >> .gitignore
echo ".env.local" >> .gitignore
```

### 2. 定期更换 Token

- 每 3-6 个月更换一次 Vercel Token
- 不再使用的 Token 及时删除
- 怀疑泄露时立即撤销并重新创建

### 3. 使用最小权限原则

- 优先使用 Project Token,而不是 Account Token
- 只给必要的项目授权
- 设置合理的过期时间

## 参考资源

- [Vercel 官方文档](https://vercel.com/docs)
- [Next.js 部署指南](https://nextjs.org/docs/deployment)
- [Vercel CLI 文档](https://vercel.com/docs/cli)
- [掘金 Vercel 教程](https://juejin.cn/search?query=Vercel)

## 小结

Vercel 部署就是这么简单:

1. **注册账号** - 用 GitHub 登录
2. **导入项目** - 选择 GitHub 仓库
3. **配置环境变量** - 添加 API Key
4. **一键部署** - 等待 1 分钟
5. **获取链接** - 分享给全世界!

**现在就开始你的第一个部署吧!** 🚀

---

> 💡 **Vibe Coding 心法**: 部署不可怕! Vercel 已经把复杂的服务器配置、DNS 解析、HTTPS 证书都搞定了。你只需要专注写代码,其他的交给 Vercel!