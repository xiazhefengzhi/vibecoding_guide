# Supabase 配置完全指南 (小白专用)

## 什么是 Supabase?

Supabase 是一个**开源的 Firebase 替代品**,为你提供后端服务,让你**不用写后端代码**就能构建完整应用!

**核心功能:**
- 🗄️ **PostgreSQL 数据库** - 免费 500MB
- 🔐 **用户认证系统** - 支持邮箱、OAuth、手机号登录
- 📦 **文件存储** - 免费 1GB 存储空间
- ⚡ **实时数据库** - 数据变化自动同步
- 🔒 **行级安全** - 细粒度权限控制
- 🌐 **自动生成 API** - RESTful + GraphQL

**为什么选 Supabase?**
- ✅ 完全免费 (个人项目)
- ✅ 无需信用卡
- ✅ 部署简单
- ✅ 与 Vercel 完美配合

---

## 第一步: 注册 Supabase 账号

### 1.1 访问 Supabase 官网

打开浏览器,访问: **https://supabase.com**

### 1.2 使用 GitHub 账号登录 (强烈推荐)

```
点击右上角 "Start your project" 按钮
↓
选择 "Continue with GitHub"
↓
授权 Supabase 访问你的 GitHub
↓
注册成功!
```

> **小白提示**: 为什么用 GitHub 登录? 因为后面部署项目时,GitHub、Vercel、Supabase 三者可以无缝集成!

---

## 第二步: 创建你的第一个项目

### 2.1 创建新项目

**详细操作步骤:**

```
第1步: 登录后,点击 "New project" 按钮
↓
第2步: 选择 Organization (如果是第一次,会自动创建)
↓
第3步: 填写项目信息
```

### 2.2 填写项目信息

**必填字段说明:**

1. **Name (项目名称)**
   - 例如: `my-learning-app` 或 `knowfun-clone`
   - 只能用小写字母、数字和连字符 `-`
   - 这个名称会出现在数据库 URL 中

2. **Database Password (数据库密码)**
   - **超级重要!** 这是你的数据库主密码
   - 点击 "Generate a password" 自动生成强密码
   - **⚠️ 立即复制保存!** 之后无法再查看
   - 保存到密码管理器 (1Password 等)

3. **Region (服务器区域)**
   - 推荐选择: **Southeast Asia (Singapore)** - 离中国最近
   - 或选择: **Northeast Asia (Tokyo)** - 日本东京
   - 注意: 创建后无法更改区域!

4. **Pricing Plan (定价方案)**
   - 选择: **Free** (免费版)
   - 免费版额度:
     - 500 MB 数据库存储
     - 1 GB 文件存储
     - 50,000 月活用户
     - 无限 API 请求

### 2.3 创建项目

点击 **"Create new project"** 按钮

**等待 2-3 分钟,Supabase 会自动配置:**
- PostgreSQL 数据库
- 认证服务
- 存储服务
- 实时订阅
- API 端点

---

## 第三步: 获取 API Keys (核心!)

### 3.1 为什么需要这些 Keys?

这些 Keys 是你的应用连接 Supabase 服务的**身份凭证**:

| Key 名称 | 用途 | 安全级别 | 暴露到前端? |
|---------|------|---------|-----------|
| **Project URL** | 服务器地址 | 公开 | ✅ 可以 |
| **anon (public) key** | 前端公开访问 | 公开 | ✅ 可以 |
| **service_role key** | 后端管理员权限 | 🔒 私密 | ❌ 绝不! |

> **小白必读**:
> - `anon key` 可以在前端使用 (加 `NEXT_PUBLIC_` 前缀)
> - `service_role key` 只能在后端使用,绝不能暴露到前端!

### 3.2 获取 API Keys - 超详细步骤

**第1步: 进入 Settings 页面**

```
项目创建完成后,你会看到项目控制台
↓
点击左侧边栏最下方的齿轮图标 ⚙️ "Settings"
↓
进入 Settings 页面
```

**第2步: 找到 API 配置**

```
在 Settings 页面中
↓
点击左侧菜单 "API" 选项
↓
你会看到 "Project API keys" 部分
```

**第3步: 复制所需的 Keys**

你会看到以下信息 (全部复制保存!):

#### 📍 **Project URL**
```
https://xxxxxxxxxxxxx.supabase.co
```
- **变量名**: `NEXT_PUBLIC_SUPABASE_URL` (前端) 或 `SUPABASE_URL` (后端)
- **用途**: 你的 Supabase 项目地址
- **安全**: 公开,可以暴露

#### 🔑 **anon public key**
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6...
```
- **变量名**: `NEXT_PUBLIC_SUPABASE_ANON_KEY` (前端) 或 `SUPABASE_KEY` (后端)
- **用途**: 前端访问数据库
- **安全**: 公开,有行级安全保护
- **位置**: "Project API keys" 下的 "anon public" 行

#### 🔐 **service_role key** (需要点击 "Reveal" 显示)
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZS1kZW1vIiwicm...
```
- **变量名**: `SUPABASE_SERVICE_KEY`
- **用途**: 后端管理员操作 (绕过行级安全)
- **安全**: 🚨 **绝密!** 绝不能暴露到前端!
- **位置**: "Project API keys" 下的 "service_role" 行,点击 "Reveal" 显示

**第4步: 保存到安全的地方**

创建一个临时文件保存这些信息:

```bash
# Supabase Keys - [你的项目名]
NEXT_PUBLIC_SUPABASE_URL=https://xxxxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGc...
SUPABASE_SERVICE_KEY=eyJhbGc...  # 仅后端使用!
```

---

## 第四步: 获取数据库连接信息

### 4.1 获取 Database URL

**详细步骤:**

```
第1步: Settings → Database (左侧菜单)
↓
第2步: 找到 "Connection string" 部分
↓
第3步: 选择 "URI" 标签页
↓
第4步: 复制连接字符串
```

**你会看到类似这样的连接字符串:**

```
postgresql://postgres:[YOUR-PASSWORD]@db.xxxxx.supabase.co:5432/postgres
```

**⚠️ 重要: 替换密码!**

连接字符串中的 `[YOUR-PASSWORD]` 需要替换为你在创建项目时设置的**数据库密码**!

```bash
# 例子:
# 原始: postgresql://postgres:[YOUR-PASSWORD]@db.xxxxx.supabase.co:5432/postgres
# 替换后: postgresql://postgres:MyStr0ngP@ssw0rd@db.xxxxx.supabase.co:5432/postgres

# 环境变量
DATABASE_URL=postgresql://postgres:你的密码@db.xxxxx.supabase.co:5432/postgres
```

> **小白提示**: 如果忘记了数据库密码,可以在 Settings → Database → "Database password" 处重置密码

### 4.2 获取 JWT Secret (用于认证)

**详细步骤:**

```
第1步: Settings → API (左侧菜单)
↓
第2步: 滚动到 "JWT Settings" 部分
↓
第3步: 找到 "JWT Secret"
↓
第4步: 点击 "Reveal" 按钮显示密钥
↓
第5步: 复制 JWT Secret
```

**示例:**

```bash
# 环境变量
SUPABASE_JWT_SECRET=your-super-secret-jwt-token-with-at-least-32-characters-long
```

**用途:**
- 验证 JWT Token 的真实性
- 后端验证用户身份
- 通常在后端框架 (FastAPI、Express) 中使用

---

## 第五步: 配置存储 (Storage)

### 5.1 创建 Storage Bucket

**什么是 Bucket?**
- Bucket 就是文件夹,用来存放文件
- 类似于 AWS S3 的 Bucket 概念
- 每个 Bucket 可以设置不同的访问权限

**创建步骤:**

```
第1步: 点击左侧菜单 "Storage"
↓
第2步: 点击 "Create a new bucket" 按钮
↓
第3步: 填写 Bucket 信息
```

**Bucket 配置:**

1. **Name (名称)**
   - 例如: `avatars` (用户头像)
   - 或: `documents` (文档文件)
   - 或: `knowfun-files` (知识付费平台文件)

2. **Public bucket (公开访问)**
   - ✅ 勾选: 文件可以通过 URL 直接访问 (适合图片、视频)
   - ❌ 不勾选: 文件需要认证才能访问 (适合私密文档)

3. **点击 "Create bucket"**

### 5.2 获取 Storage Keys (S3 兼容)

Supabase Storage 支持 S3 兼容 API,你需要获取 Access Keys:

**详细步骤:**

```
第1步: Settings → Storage (左侧菜单)
↓
第2步: 滚动到 "S3 Access Keys" 部分
↓
第3步: 点击 "Generate new keys" 按钮
↓
第4步: 复制以下信息
```

**你会得到:**

```bash
# S3 Access Key ID
SUPABASE_ACCESS_KEY_ID=c2fc93cd64ef6b3c7da4eea19f60ebd8...

# S3 Secret Access Key
SUPABASE_SECRET_ACCESS_KEY=9ff1a54c867b6f97b3d8fb85b5ddd5a3

# S3 Endpoint
SUPABASE_ENDPOINT=https://xxxxx.supabase.co/storage/v1/s3

# S3 Region
SUPABASE_REGION=us-east-1  # 或你选择的区域
```

**完整环境变量配置:**

```bash
# Supabase Storage (S3 兼容)
SUPABASE_ACCESS_KEY_ID=your-access-key-id
SUPABASE_SECRET_ACCESS_KEY=your-secret-key
SUPABASE_ENDPOINT=https://xxxxx.supabase.co/storage/v1/s3
SUPABASE_REGION=us-east-1
SUPABASE_BUCKET_NAME=knowfun-files
SUPABASE_PUBLIC_URL=https://xxxxx.supabase.co/storage/v1/object/public
```

---

## 第六步: 配置认证 (Authentication)

### 6.1 启用认证提供商

Supabase 支持多种登录方式:

```
第1步: 点击左侧菜单 "Authentication"
↓
第2步: 点击 "Providers" 标签页
↓
第3步: 选择要启用的登录方式
```

**常用登录方式:**

#### 1️⃣ **Email (邮箱登录)** - 默认启用
- 用户用邮箱 + 密码注册登录
- 支持邮箱验证

#### 2️⃣ **Google OAuth**
```
点击 "Google" → 启用开关
↓
填写 Google OAuth 凭证:
  - Client ID: 从 Google Cloud Console 获取
  - Client Secret: 从 Google Cloud Console 获取
↓
点击 "Save"
```

#### 3️⃣ **GitHub OAuth**
```
点击 "GitHub" → 启用开关
↓
填写 GitHub OAuth 凭证:
  - Client ID: 从 GitHub Developer Settings 获取
  - Client Secret: 从 GitHub Developer Settings 获取
↓
点击 "Save"
```

### 6.2 配置邮件模板

```
Authentication → Email Templates
↓
自定义邮件内容:
  - Confirm signup (注册确认邮件)
  - Magic Link (无密码登录邮件)
  - Change Email Address (更改邮箱邮件)
  - Reset Password (重置密码邮件)
```

---

## 第七步: 完整环境变量清单

### 7.1 前端环境变量 (`.env.local`)

```bash
# Supabase 配置 (前端)
NEXT_PUBLIC_SUPABASE_URL=https://xxxxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGc...

# API 地址 (可选)
NEXT_PUBLIC_API_URL=http://localhost:8000
```

**保存位置:**
- Next.js 项目: `frontend/.env.local`
- React/Vue 项目: `.env.local`

### 7.2 后端环境变量 (`.env`)

```bash
# Supabase 数据库配置
SUPABASE_URL=https://xxxxx.supabase.co
SUPABASE_KEY=eyJhbGc...  # anon key
SUPABASE_SERVICE_KEY=eyJhbGc...  # service_role key (保密!)
DATABASE_URL=postgresql://postgres:密码@db.xxxxx.supabase.co:5432/postgres
SUPABASE_JWT_SECRET=your-jwt-secret

# Supabase Storage 配置
SUPABASE_ACCESS_KEY_ID=xxx
SUPABASE_SECRET_ACCESS_KEY=xxx
SUPABASE_ENDPOINT=https://xxxxx.supabase.co/storage/v1/s3
SUPABASE_REGION=us-east-1
SUPABASE_BUCKET_NAME=knowfun-files
SUPABASE_PUBLIC_URL=https://xxxxx.supabase.co/storage/v1/object/public
```

**保存位置:**
- FastAPI/Express 项目: `backend/.env`
- Python 项目: `.env`

---

## 第八步: 部署到 Vercel 时配置

### 8.1 在 Vercel 添加环境变量

```
第1步: 登录 Vercel Dashboard
↓
第2步: 选择你的项目
↓
第3步: Settings → Environment Variables
↓
第4步: 添加 Supabase Keys
```

**前端项目需要添加:**

```bash
NEXT_PUBLIC_SUPABASE_URL = https://xxxxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY = eyJhbGc...
```

**后端项目需要添加:**

```bash
SUPABASE_URL = https://xxxxx.supabase.co
SUPABASE_KEY = eyJhbGc...
SUPABASE_SERVICE_KEY = eyJhbGc...
DATABASE_URL = postgresql://postgres:密码@...
SUPABASE_JWT_SECRET = your-jwt-secret
```

> **重要**: 添加完环境变量后,必须重新部署才会生效!

---

## 第九步: 验证配置是否成功

### 9.1 测试数据库连接

在你的项目中创建一个测试文件:

```python
# test_supabase.py (Python)
import os
from supabase import create_client, Client

url = os.getenv("SUPABASE_URL")
key = os.getenv("SUPABASE_KEY")

supabase: Client = create_client(url, key)

# 测试连接
response = supabase.table("test").select("*").execute()
print("✅ 数据库连接成功!")
print(response)
```

```javascript
// test_supabase.js (JavaScript)
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = process.env.NEXT_PUBLIC_SUPABASE_URL
const supabaseKey = process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY

const supabase = createClient(supabaseUrl, supabaseKey)

// 测试连接
async function testConnection() {
  const { data, error } = await supabase.from('test').select('*')

  if (error) {
    console.error('❌ 连接失败:', error)
  } else {
    console.log('✅ 数据库连接成功!', data)
  }
}

testConnection()
```

### 9.2 测试文件上传

```javascript
// 测试上传文件
async function testUpload() {
  const file = new File(['Hello World'], 'test.txt', { type: 'text/plain' })

  const { data, error } = await supabase.storage
    .from('knowfun-files')
    .upload('test/test.txt', file)

  if (error) {
    console.error('❌ 上传失败:', error)
  } else {
    console.log('✅ 文件上传成功!', data)
  }
}

testUpload()
```

---

## 常见问题 FAQ

### Q1: 忘记数据库密码怎么办?

**解决方案:**

```
第1步: Settings → Database
↓
第2步: 找到 "Database password" 部分
↓
第3步: 点击 "Reset database password" 按钮
↓
第4步: 设置新密码
↓
第5步: 更新 DATABASE_URL 中的密码
```

### Q2: 如何查看数据库中的数据?

**使用内置的 Table Editor:**

```
第1步: 点击左侧菜单 "Table Editor"
↓
第2步: 选择表名
↓
第3步: 查看/编辑数据
```

**或使用 SQL Editor:**

```
第1步: 点击左侧菜单 "SQL Editor"
↓
第2步: 编写 SQL 查询
↓
第3步: 点击 "Run" 执行
```

### Q3: 如何创建数据库表?

**方法一: 使用 Table Editor (可视化)**

```
Table Editor → New table → 填写表名和字段
```

**方法二: 使用 SQL Editor**

```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  email TEXT UNIQUE NOT NULL,
  name TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### Q4: Service Role Key 泄露了怎么办?

**紧急处理:**

```
第1步: Settings → API → "Reset service_role secret"
↓
第2步: 立即更新所有使用该 Key 的地方
↓
第3步: 重新部署应用
```

### Q5: 免费版额度够用吗?

**免费版限制:**

| 项目 | 免费额度 | 够用吗? |
|-----|---------|---------|
| 数据库存储 | 500 MB | ✅ 小项目够用 |
| 文件存储 | 1 GB | ✅ 图片/文档够用 |
| 月活用户 | 50,000 | ✅ 个人项目绝对够 |
| 数据库带宽 | 5 GB | ⚠️ 流量大时可能不够 |
| API 请求 | 无限 | ✅ 完全够用 |

**小白建议**: 个人学习和小项目,免费版完全够用!

### Q6: 如何升级到付费版?

**如果需要更多资源:**

```
Settings → Billing → 选择 Pro 计划 ($25/月)
```

**Pro 计划优势:**
- 8 GB 数据库存储
- 100 GB 文件存储
- 100,000 月活用户
- 无限带宽
- 优先支持

---

## 实战案例: 5 分钟搭建用户认证系统

### 准备工作

```bash
# 1. 创建 Supabase 项目
# 2. 获取 API Keys
# 3. 安装依赖
npm install @supabase/supabase-js
```

### 前端代码 (Next.js)

```javascript
// lib/supabase.js
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = process.env.NEXT_PUBLIC_SUPABASE_URL
const supabaseKey = process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY

export const supabase = createClient(supabaseUrl, supabaseKey)

// 注册用户
export async function signUp(email, password) {
  const { data, error } = await supabase.auth.signUp({
    email,
    password,
  })
  return { data, error }
}

// 登录
export async function signIn(email, password) {
  const { data, error } = await supabase.auth.signInWithPassword({
    email,
    password,
  })
  return { data, error }
}

// 登出
export async function signOut() {
  const { error } = await supabase.auth.signOut()
  return { error }
}

// 获取当前用户
export async function getUser() {
  const { data: { user } } = await supabase.auth.getUser()
  return user
}
```

**完成! 你有了一个完整的认证系统!**

---

## 安全最佳实践

### 1. 保护你的 Service Role Key

```bash
# ❌ 错误: 暴露到前端
const supabase = createClient(url, serviceRoleKey)  # 危险!

# ✅ 正确: 只在后端使用
# 前端用 anon key,后端用 service_role key
```

### 2. 启用行级安全 (RLS)

```sql
-- 启用 RLS
ALTER TABLE users ENABLE ROW LEVEL SECURITY;

-- 创建策略: 用户只能看到自己的数据
CREATE POLICY "Users can view own data"
  ON users
  FOR SELECT
  USING (auth.uid() = id);

-- 创建策略: 用户只能更新自己的数据
CREATE POLICY "Users can update own data"
  ON users
  FOR UPDATE
  USING (auth.uid() = id);
```

### 3. 使用环境变量

```bash
# ✅ 正确
const url = process.env.NEXT_PUBLIC_SUPABASE_URL

# ❌ 错误: 直接写在代码里
const url = "https://xxxxx.supabase.co"
```

### 4. 定期轮换密钥

- 每 6 个月更换一次 Service Role Key
- 数据库密码定期更换
- 怀疑泄露时立即重置

---

## 参考资源

- [Supabase 官方文档](https://supabase.com/docs)
- [Supabase JavaScript 客户端](https://supabase.com/docs/reference/javascript/introduction)
- [Supabase Python 客户端](https://supabase.com/docs/reference/python/introduction)
- [行级安全策略指南](https://supabase.com/docs/guides/auth/row-level-security)

---

## 小结

Supabase 配置就是这么简单:

1. **注册账号** - 用 GitHub 登录
2. **创建项目** - 选择区域,设置密码
3. **获取 Keys** - 复制 URL、anon key、service_role key
4. **配置存储** - 创建 Bucket,获取 S3 Keys
5. **启用认证** - 选择登录方式
6. **添加到项目** - 配置环境变量
7. **部署到 Vercel** - 添加环境变量并部署

**现在就开始用 Supabase 构建你的应用吧!** 🚀

---

> 💡 **Vibe Coding 心法**: Supabase 就像你的私人后端团队! 数据库、认证、存储全都帮你搞定,你只需要专注于前端开发和业务逻辑。让 AI 时代的开发变得更简单!