# 前端开发基础

> **方法论**：不求精通，但求能跑。先用 AI 生成，再理解原理。

---

## 📖 本章目标

学完本章，你将能够：
- ✅ 配置前端开发环境（Node.js、npm）
- ✅ 理解前端三件套（HTML/CSS/JavaScript）的作用
- ✅ 掌握 Next.js 的基本使用
- ✅ 会用 Tailwind CSS 快速写样式
- ✅ 理解组件化开发思想
- ✅ 解决常见报错和问题

**预计总用时**：2 小时

---

## 📚 章节目录

### 2.1.1 [环境配置](./环境配置.md)
**预计用时**：15 分钟

配置前端开发环境，理解 npm 和 package.json。

**核心内容**：
- Node.js 和 npm 安装
- package.json 文件详解
- 配置国内镜像源（解决下载慢）
- npm 常用命令速查

---

### 2.1.2 [前端概述](./前端概述.md)
**预计用时**：15 分钟

了解前端是什么，HTML/CSS/JavaScript 三件套如何配合工作。

**核心内容**：
- 前端 vs 后端的区别
- HTML = 骨架，CSS = 皮肤，JS = 肌肉
- 现代前端框架的作用

---

### 2.1.3 [Next.js 快速入门](./nextjs入门.md)
**预计用时**：20 分钟

学习如何创建 Next.js 项目，理解文件路由系统。

**核心内容**：
- 创建第一个 Next.js 项目
- 理解项目文件结构
- 文件名即路由
- 使用 Link 组件跳转页面

---

### 2.1.4 [Tailwind CSS 快速入门](./tailwind入门.md)
**预计用时**：20 分钟

掌握 Tailwind CSS 的核心类名，快速实现好看的样式。

**核心内容**：
- 最常用的 20 个类名
- 响应式设计（md: lg: 前缀）
- 悬停效果（hover:）
- 实战组件示例

---

### 2.1.5 [React 组件基础](./react组件.md)
**预计用时**：25 分钟

理解组件化开发思想，掌握 Props 和 State。

**核心内容**：
- 什么是组件
- Props：传递数据
- State：管理状态
- 常用 Hooks（useState, useEffect）

---

### 2.1.6 [常见问题与基础概念](./常见问题.md)
**预计用时**：20 分钟

理解 HTTP 通信、浏览器工具，解决常见报错。

**核心内容**：
- HTTP 请求方法和状态码
- 浏览器开发者工具使用
- npm / React / Next.js 常见报错解决
- 有效求助的方法

---

## 🎯 学习建议

### 推荐学习顺序

```
1. 环境配置（先把工具装好）
   ↓
2. 前端概述（理解全貌）
   ↓
3. Next.js 入门（搭建项目）
   ↓
4. Tailwind 入门（写出好看的样式）
   ↓
5. React 组件（组件化开发）
   ↓
6. 常见问题（遇到问题时查阅）
```

### Vibe Coding 学习法

**不要**：
- ❌ 死记硬背所有语法
- ❌ 从头到尾看完再动手
- ❌ 追求完全理解再下一步

**应该**：
- ✅ 边看边敲代码
- ✅ 遇到不懂的先跳过，用 AI 生成
- ✅ 能跑通就是成功
- ✅ 报错了复制错误信息问 AI

---

## 💡 快速参考

### 常用命令

```bash
# 创建 Next.js 项目
npx create-next-app@latest my-app

# 安装依赖
npm install

# 运行开发服务器
npm run dev

# 构建生产版本
npm run build

# 配置淘宝镜像（解决下载慢）
npm config set registry https://registry.npmmirror.com
```

### 常用 Tailwind 类名

```
布局：flex, grid, hidden
居中：justify-center, items-center, mx-auto
间距：p-4, m-4, gap-4
颜色：bg-blue-500, text-white
文字：text-lg, font-bold
圆角：rounded-lg
响应式：md:, lg:
状态：hover:, focus:
```

### React 核心语法

```tsx
// Props
function Button({ text }: { text: string }) {
  return <button>{text}</button>
}

// State
const [count, setCount] = useState(0)

// 事件
<button onClick={() => setCount(count + 1)}>
```

### 常见问题速查

| 问题 | 解决方案 |
|------|---------|
| npm install 慢 | 配置淘宝镜像 |
| Module not found | `npm install xxx` 安装缺失依赖 |
| 页面白屏 | F12 看 Console 错误 |
| 接口请求失败 | F12 看 Network 面板 |
| 样式不生效 | 检查类名拼写，清除缓存 |

---

## 📚 下一步

完成本章后，继续学习：

👉 [后端开发基础](../后端.md)

学习如何用 FastAPI 创建后端 API，让前端能获取真实数据。
