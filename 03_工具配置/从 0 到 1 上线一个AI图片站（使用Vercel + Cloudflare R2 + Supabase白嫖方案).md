从 0 到 1 上线一个AI图片站（使用Vercel + Cloudflare R2 + Supabase白嫖方案）
这套方案很适合个人开发者、副业项目、素材站/工具站快速起盘。

先说一下背景，最近你们有没有被可爱的宠物九宫格刷屏？就是把宠物头像生成可以的表情包。

我刚刚上线的 AI 图片站 - Pet Emoji Generator，可以将你的宠物照片一键转换成 9 宫格表情包。



我花了一天时间就开发完成了一个这样小而美的图片站，用户上传一张宠物的图片可以生成他的表情包九宫格。

好，了解完需求，下面讲解开发总流程。

架构总览
技术方案：前端由 Next.js 提供静态/SSR 页面，图片静态资源走 Cloudflare R2 自定义域，轻量数据（如生成记录、点赞、下载次数）存 Supabase。

全链路走完，你就拥有一个可对外访问、可被收录的线上网站。

访问入口：自定义域名（Namecheap）
Web 托管：Vercel（自动 HTTPS，全球加速）
图片存储：Cloudflare R2（10GB 免费）
轻量数据：Supabase（免费计划）
SEO：sitemap.xml + robots.txt + 基础语义化 + Search Console/Bing 提交
代码开发环节就忽略了，主要记录大概的流程，都是使用Cursor进行开发的。

第 1 步：购买域名
选择Namecheap、Cloudflare等平台都可以，对比价格后我选择了前者
选择简短、易读、易记的域名，优先 .com/.io/.app
价格友好、面板简单，支持自定义 DNS 记录


第 2 步：一键部署到 Vercel（Next.js）
你可以将项目推到 GitHub，然后在 Vercel 选择导入仓库，默认设置下一步到底即可完成部署。



之后每次修改代码推送到git，可以做到自动部署了，Vercel 会托管你的静态资源与 SSR 页面，并自动签发 SSL。

将 Namecheap 域名绑定到 Vercel
登录 Vercel 控制台 → 进入项目 → Settings → Domains → Add Domain，填写你的域名（example.com 或 www.example.com）。
在域名详情页，Vercel 会给出需要添加的 DNS 记录：
根域名（@）：通常两条 A 记录指向 Vercel IP（如 76.76.21.21）。
子域名（如 www）：一条 CNAME 指向 cname.vercel-dns.com。
登录 Namecheap → 选择域名 → Advanced DNS → 添加上述记录 → 保存。
回到 Vercel 域名页，等待状态变为 “DNS Verified”。
HTTPS 会自动开启（Let’s Encrypt），无需手工配置证书。


小贴士：如果你直接使用 Vercel 的 Nameservers，则只需在 Vercel 一侧添加域名即可，Vercel 会代你配置 DNS。

第 3 步：Cloudflare R2 存储（10GB 免费）
R2 是 Cloudflare 的 S3 兼容对象存储，免费档自带 10GB 存储，非常适合图片站起步。

注册/登录 Cloudflare → R2 → 创建 Bucket。
进入 Bucket → 设置（Settings）→ 打开 Public Access（如需公开访问）。
绑定自定义域名（推荐 img.example.com 或 cdn.example.com）：
将域名托管到 Cloudflare（把域名 NS 改成 Cloudflare 提供的 nameservers）。
在 R2 Bucket 的自定义域里添加 img.example.com。
Cloudflare 会自动写入所需 DNS 记录，稍等几分钟生效。
上传文件测试访问（直接通过 https://img.example.com/xxx.png）。
Cloudflare R2免费10G

注意事项：

自定义域必须在当前 Cloudflare 账号下、且域名 DNS 已托管至 Cloudflare 才能绑定。
若你的主站仍使用 Namecheap DNS，也可仅将子域 img.example.com 迁移到 Cloudflare 托管，或主域整体切换到 Cloudflare。
可结合 R2 Access Policies 做简单的防盗链（基于 Referer），或在应用层生成私有签名 URL（需要后端或 Edge 函数）。
第 4 步：接入 Supabase（免费数据库）
Supabase 提供 Postgres、对象存储、鉴权、实时等能力，免费档足够早期使用。

访问 https://supabase.com/ 注册并登录。
New Project → 选择 Free 计划 → 设置数据库密码、区域。
进入 Project Settings → 记录 SUPABASE_URL 与 SUPABASE_ANON_KEY（前端用）、SERVICE_ROLE_KEY（仅服务端用）。
在 SQL Editor 创建表，示例：记录图片生成/上传信息（可按需裁剪字段）。
create table if not exists emoji_generations (
  id uuid primary key default gen_random_uuid(),
  image_url text not null,
  prompt text,
  ip varchar(64),
  created_at timestamp with time zone default now(),
  likes int default 0,
  downloads int default 0
);

最佳实践：

前端仅使用 ANON_KEY，写操作敏感逻辑放到服务端接口（如 Next.js Route Handlers），必要时用 SERVICE_ROLE_KEY。
配置 RLS（行级安全）策略，限制未授权写入。


第 5 步：部署应用到线上
首先要先把敏感信息设置在环境变量中，并在线上环境中使用。

环境变量清单（示例）
SUPABASE_URL、SUPABASE_ANON_KEY（前端可用）
SUPABASE_SERVICE_ROLE_KEY（仅服务端）
R2_PUBLIC_BUCKET_URL（如 https://img.example.com）
NEXT_PUBLIC_SITE_URL（用于 sitemap/OG）
在Vercel设置正确的环境变量



图片上传与访问策略
上传到 R2：前端 → 调用后端签名接口 → 直传 R2，减少带宽与延迟。
访问走自定义域：页面中使用 https://img.example.com/...，配合 loading="lazy"、适当的尺寸和格式（webp/avif）。


SEO：快速收录清单
SEO是可以帮助我们免费获取流量的，所以这步一定要做好哦～

生成并暴露 sitemap.xml 与 robots.txt（Next.js 可在 app/robots.ts、app/sitemap.ts）。
页面结构语义化：<h1> 仅一次，按层级使用 <h2>-<h5>；图片带有意义的 alt。
<head> 添加 meta description、Open Graph（og:title/og:description/og:image）。
将 sitemap.xml 提交到 Google Search Console 与 Bing Webmaster Tools。
性能：图片懒加载、压缩与合适尺寸；全站 HTTPS。
内链：相关推荐/热门标签，提升爬取效率与页面停留。
把网站提交到google和bing上，加快收录（别问为什么不提交到baidu，dddd）。
推广建议：在 V2EX、X、Product Hunt 等平台 发布介绍与更新日志，获取首批外链与真实用户。



总计
成本：17$ 元（除了域名其他都是免费）
用时：半天 ~ 1 天
技术栈：Next.js + Vercel 部署、Cloudflare R2 对象存储、Namecheap 域名、Supabase 免费数据库
适用：图片/表情/壁纸站、小型工具站、AIGC 素材站
常见问题（FAQ）
**Q：R2 自定义域为什么一直验证不通过？**A：确认域名 DNS 已切到 Cloudflare，并在 R2 的自定义域里完成绑定。仅当域名托管在 Cloudflare 时，自定义域才可用。

**Q：Vercel 需要自己配置 SSL 吗？**A：不需要。Vercel 自动申请 Let’s Encrypt 证书并续期，域名 DNS 生效后会自动开启。

**Q：Supabase 的 key 会不会泄露？**A：ANON_KEY 可放前端，但务必限制权限；敏感写操作放到服务端 Route，使用服务端环境变量。

**Q：免费额度够用吗？**A：早期足够。R2 10GB 存储，Supabase 免费计划含 500MB DB/1GB 文件/每月 200K 请求，Vercel 免费足够轻量网站。

结语
用 Vercel + Cloudflare R2 + Supabase，个人也能在 0 预算内把图片站跑起来。先做出最小可用版本（MVP），让真实用户用起来，再迭代体验与变现。 如果你也有开发需求，不妨采用我这套方案，便宜又实惠！

```
/Users/ganguohua/Library/Caches/ms-playwright/chromium-1194/chrome-mac/Chromium.app/Contents/MacOS/Chromiu
```

