# Docker容器化部署(可选)

> **方法论**: Docker是现代部署的标准方式,理解"集装箱"类比,就能理解容器化。

---

## 📖 本节目标

学完本节,你将理解:
- ✅ Docker是什么,为什么要用它
- ✅ Dockerfile编写和镜像构建
- ✅ docker-compose多容器编排
- ✅ 完整的容器化部署流程

**预计用时**: 35分钟

**前置要求**:
- ✅ 已学习04章服务器部署
- ✅ 理解前后端分离架构

---

## 1. Docker是什么?

### 1.1 生活类比

**Docker = 集装箱运输系统**

| 传统部署 | Docker容器化 |
|---------|-------------|
| 散装货物(直接装车) | 标准集装箱 |
| 每种货物装法不同 | 所有集装箱规格统一 |
| 换车很麻烦 | 集装箱可以直接吊到任何车上 |
| 装卸慢,容易损坏 | 快速装卸,保护货物 |

**软件部署中**:

```
传统部署:
你的电脑 (Node 16, Ubuntu 20)  ✅ 能运行
测试服务器 (Node 14, CentOS 7) ❌ 出错了!
生产服务器 (Node 18, Ubuntu 22) ❌ 又出错!

容器化部署:
你的电脑 (Docker)  ✅ 容器运行正常
测试服务器 (Docker) ✅ 同样的容器,一样运行
生产服务器 (Docker) ✅ 同样的容器,一样运行

"在我电脑上能跑" 不再是玩笑!
```

### 1.2 Docker解决的核心问题

**问题1: 环境不一致**

```
开发: macOS + Node 18 + Python 3.11
服务器: Ubuntu + Node 16 + Python 3.9

结果: 代码在本地能跑,服务器跑不起来!
```

**Docker解决**: 把应用和环境打包在一起

---

**问题2: 依赖冲突**

```
项目A 需要 Node 14
项目B 需要 Node 18

同一台服务器上怎么办?
```

**Docker解决**: 每个项目独立容器,互不影响

---

**问题3: 部署复杂**

```
传统部署步骤:
1. 安装Node.js
2. 安装MySQL
3. 配置Nginx
4. 安装Redis
5. 配置防火墙
6. ...20个步骤

Docker部署:
docker-compose up -d

一个命令启动所有服务! ⚡
```

### 1.3 核心概念

| 概念 | 类比 | 说明 |
|------|------|------|
| **镜像(Image)** | 集装箱模板 | 只读的应用模板,包含代码和环境 |
| **容器(Container)** | 实际运行的集装箱 | 镜像的运行实例 |
| **Dockerfile** | 集装箱制造图纸 | 定义如何构建镜像 |
| **Docker Hub** | 集装箱仓库 | 存储和分享镜像 |
| **docker-compose** | 多个集装箱一起管理 | 编排多个容器 |

**关系图**:

```
Dockerfile (图纸)
    │
    ▼ docker build
Image (模板)
    │
    ▼ docker run
Container (运行中的应用)
```

---

## 2. 安装Docker

### 2.1 Linux安装(Ubuntu)

```bash
# 1. 更新包列表
sudo apt update

# 2. 安装依赖
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

# 3. 添加Docker官方GPG密钥
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# 4. 添加Docker仓库
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 5. 安装Docker
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io

# 6. 安装docker-compose
sudo apt install -y docker-compose-plugin

# 7. 验证安装
docker --version
docker compose version
```

### 2.2 macOS/Windows安装

**macOS**:
1. 下载 [Docker Desktop for Mac](https://www.docker.com/products/docker-desktop)
2. 拖拽到Applications
3. 启动Docker Desktop

**Windows**:
1. 下载 [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop)
2. 运行安装程序
3. 启动Docker Desktop(需要WSL2)

### 2.3 配置国内镜像加速

```bash
# 创建配置文件
sudo mkdir -p /etc/docker
sudo vim /etc/docker/daemon.json

# 添加以下内容
{
  "registry-mirrors": [
    "https://docker.mirrors.ustc.edu.cn",
    "https://hub-mirror.c.163.com",
    "https://mirror.ccs.tencentyun.com"
  ]
}

# 重启Docker
sudo systemctl restart docker

# 验证
docker info | grep -A 5 "Registry Mirrors"
```

### 2.4 允许非root用户使用Docker

```bash
# 添加当前用户到docker组
sudo usermod -aG docker $USER

# 重新登录或执行
newgrp docker

# 测试
docker ps
# 不需要sudo了!
```

---

## 3. Dockerfile编写

### 3.1 Dockerfile基础语法

| 指令 | 作用 | 示例 |
|------|------|------|
| `FROM` | 基础镜像 | `FROM node:18-alpine` |
| `WORKDIR` | 工作目录 | `WORKDIR /app` |
| `COPY` | 复制文件 | `COPY package.json .` |
| `RUN` | 执行命令(构建时) | `RUN npm install` |
| `CMD` | 默认命令(运行时) | `CMD ["npm", "start"]` |
| `EXPOSE` | 暴露端口 | `EXPOSE 3000` |
| `ENV` | 环境变量 | `ENV NODE_ENV=production` |

### 3.2 前端Dockerfile示例

```dockerfile
# Dockerfile (前端React/Vue项目)

# 阶段1: 构建阶段
FROM node:18-alpine AS builder

WORKDIR /app

# 复制package.json
COPY package*.json ./

# 安装依赖
RUN npm install

# 复制源代码
COPY . .

# 打包
RUN npm run build

# 阶段2: 运行阶段(多阶段构建,减小镜像体积)
FROM nginx:alpine

# 复制打包结果到Nginx
COPY --from=builder /app/dist /usr/share/nginx/html

# 复制Nginx配置(可选)
# COPY nginx.conf /etc/nginx/conf.d/default.conf

# 暴露80端口
EXPOSE 80

# 启动Nginx
CMD ["nginx", "-g", "daemon off;"]
```

**为什么用多阶段构建?**

```
单阶段:
镜像大小: 1.2GB (包含Node.js, npm, 源代码, node_modules...)

多阶段:
镜像大小: 25MB (只包含Nginx和打包后的静态文件)

体积减少98%! ⚡
```

### 3.3 后端Dockerfile示例

```dockerfile
# Dockerfile (Node.js后端)

FROM node:18-alpine

# 安装dumb-init(更好的进程管理)
RUN apk add --no-cache dumb-init

# 设置工作目录
WORKDIR /app

# 非root用户运行(安全)
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

# 复制package.json
COPY --chown=nodejs:nodejs package*.json ./

# 安装生产依赖
RUN npm ci --only=production

# 复制源代码
COPY --chown=nodejs:nodejs . .

# 切换到非root用户
USER nodejs

# 暴露端口
EXPOSE 3000

# 使用dumb-init启动
CMD ["dumb-init", "node", "server.js"]
```

### 3.4 Python后端Dockerfile示例

```dockerfile
# Dockerfile (Python FastAPI)

FROM python:3.11-slim

WORKDIR /app

# 安装系统依赖
RUN apt-get update && \
    apt-get install -y --no-install-recommends gcc && \
    rm -rf /var/lib/apt/lists/*

# 复制requirements.txt
COPY requirements.txt .

# 安装Python依赖
RUN pip install --no-cache-dir -r requirements.txt

# 复制源代码
COPY . .

# 创建非root用户
RUN useradd -m -u 1000 appuser && \
    chown -R appuser:appuser /app
USER appuser

# 暴露端口
EXPOSE 8000

# 启动应用
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

## 4. 构建和运行镜像

### 4.1 构建镜像

```bash
# 基本构建
docker build -t my-app:latest .

# -t: 镜像标签(名称:版本)
# .: Dockerfile所在目录

# 指定Dockerfile
docker build -t my-app -f Dockerfile.prod .

# 查看构建过程
docker build -t my-app --progress=plain .
```

### 4.2 运行容器

```bash
# 基本运行
docker run -d -p 3000:3000 --name my-container my-app

# -d: 后台运行
# -p 宿主机端口:容器端口
# --name: 容器名称

# 带环境变量
docker run -d \
  -p 3000:3000 \
  -e NODE_ENV=production \
  -e DATABASE_URL=mysql://... \
  --name my-app \
  my-app:latest

# 挂载数据卷
docker run -d \
  -p 3000:3000 \
  -v $(pwd)/data:/app/data \
  --name my-app \
  my-app
```

### 4.3 常用管理命令

```bash
# 查看运行中的容器
docker ps

# 查看所有容器(包括停止的)
docker ps -a

# 查看容器日志
docker logs my-container
docker logs -f my-container  # 实时查看

# 进入容器
docker exec -it my-container /bin/sh

# 停止容器
docker stop my-container

# 启动容器
docker start my-container

# 重启容器
docker restart my-container

# 删除容器
docker rm my-container
docker rm -f my-container  # 强制删除运行中的容器

# 删除镜像
docker rmi my-app:latest

# 清理未使用的资源
docker system prune -a
```

---

## 5. docker-compose编排

### 5.1 为什么需要docker-compose?

**场景**: 一个完整的Web应用

```
需要运行:
- 前端容器
- 后端容器
- 数据库容器
- Redis缓存容器
- Nginx代理容器

手动运行5个docker run命令? 太麻烦!
```

**docker-compose解决**:

```yaml
# 一个配置文件管理所有容器
docker-compose.yml

# 一个命令启动所有服务
docker compose up -d
```

### 5.2 docker-compose.yml示例

```yaml
version: '3.8'

services:
  # 前端服务
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "80:80"
    depends_on:
      - backend
    networks:
      - app-network

  # 后端服务
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=mysql://root:password@db:3306/myapp
      - REDIS_URL=redis://redis:6379
    depends_on:
      - db
      - redis
    networks:
      - app-network
    restart: unless-stopped

  # 数据库
  db:
    image: mysql:8.0
    environment:
      - MYSQL_ROOT_PASSWORD=password
      - MYSQL_DATABASE=myapp
    volumes:
      - db-data:/var/lib/mysql
    networks:
      - app-network
    restart: unless-stopped

  # Redis缓存
  redis:
    image: redis:7-alpine
    networks:
      - app-network
    restart: unless-stopped

# 数据卷
volumes:
  db-data:

# 网络
networks:
  app-network:
    driver: bridge
```

### 5.3 docker-compose命令

```bash
# 启动所有服务
docker compose up -d

# 查看服务状态
docker compose ps

# 查看日志
docker compose logs
docker compose logs -f backend  # 查看某个服务

# 重启服务
docker compose restart backend

# 停止所有服务
docker compose stop

# 停止并删除所有容器
docker compose down

# 停止并删除所有容器+数据卷
docker compose down -v

# 重新构建并启动
docker compose up -d --build
```

---

## 6. 实战: 完整项目容器化

### 6.1 项目结构

```
my-project/
├── frontend/
│   ├── src/
│   ├── package.json
│   └── Dockerfile
├── backend/
│   ├── src/
│   ├── package.json
│   └── Dockerfile
├── docker-compose.yml
└── .dockerignore
```

### 6.2 .dockerignore配置

```
# .dockerignore (放在Dockerfile同级目录)

node_modules
npm-debug.log
.env
.git
.gitignore
README.md
dist
build
.DS_Store
```

### 6.3 完整部署流程

```bash
# 1. 克隆代码
git clone https://github.com/yourname/project.git
cd project

# 2. 配置环境变量
cp .env.example .env
vim .env

# 3. 启动所有服务
docker compose up -d --build

# 4. 查看日志
docker compose logs -f

# 5. 访问应用
# http://your-server-ip

# 6. 进入容器调试
docker compose exec backend sh

# 7. 重启某个服务
docker compose restart backend

# 8. 查看资源使用
docker stats
```

---

## 7. 镜像优化技巧

### 7.1 选择合适的基础镜像

| 镜像 | 大小 | 适用场景 |
|------|------|---------|
| `node:18` | 1GB | 开发环境 |
| `node:18-slim` | 200MB | 轻量生产环境 |
| `node:18-alpine` | 120MB | 最小化生产环境 |

```dockerfile
# ✅ 推荐: alpine镜像
FROM node:18-alpine

# ❌ 不推荐: 完整镜像
FROM node:18
```

### 7.2 利用构建缓存

```dockerfile
# ✅ 正确顺序: 先复制package.json
COPY package*.json ./
RUN npm install
COPY . .

# ❌ 错误顺序: 代码变化导致重新安装依赖
COPY . .
RUN npm install
```

### 7.3 多阶段构建

```dockerfile
# 构建阶段: 包含开发工具
FROM node:18 AS builder
RUN npm run build

# 运行阶段: 只包含必需文件
FROM node:18-alpine
COPY --from=builder /app/dist ./dist
```

### 7.4 清理缓存

```dockerfile
# ✅ 一条RUN命令,减少层数
RUN apt-get update && \
    apt-get install -y gcc && \
    rm -rf /var/lib/apt/lists/*

# ❌ 多条RUN,增加镜像大小
RUN apt-get update
RUN apt-get install -y gcc
RUN rm -rf /var/lib/apt/lists/*
```

---

## 8. 生产环境最佳实践

### 8.1 安全性

```dockerfile
# ✅ 使用非root用户
RUN adduser -D appuser
USER appuser

# ✅ 只暴露必需端口
EXPOSE 3000

# ✅ 使用secrets管理密钥
# docker compose secrets
```

### 8.2 健康检查

```dockerfile
# Dockerfile添加健康检查
HEALTHCHECK --interval=30s --timeout=3s --start-period=40s \
  CMD node healthcheck.js || exit 1
```

```yaml
# docker-compose.yml
services:
  backend:
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

### 8.3 日志管理

```yaml
services:
  backend:
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
```

### 8.4 资源限制

```yaml
services:
  backend:
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 512M
        reservations:
          cpus: '0.25'
          memory: 256M
```

---

## 9. Docker 实战场景 🔥

### 9.1 场景1: 从零部署一个前端应用

**完整流程**:
```bash
# 1. 进入前端项目目录
cd my-react-app

# 2. 创建Dockerfile
cat > Dockerfile <<'EOF'
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
EOF

# 3. 创建.dockerignore
echo "node_modules
.git
.env" > .dockerignore

# 4. 构建镜像
docker build -t my-app:v1.0 .

# 5. 运行容器
docker run -d -p 80:80 --name my-app my-app:v1.0

# 6. 验证
curl http://localhost

# 7. 查看日志
docker logs my-app
```

### 9.2 场景2: 快速启动开发环境

**使用docker-compose快速启动数据库**:
```bash
# 创建docker-compose.yml
cat > docker-compose.yml <<'EOF'
version: '3.8'

services:
  mysql:
    image: mysql:8.0
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: myapp
    volumes:
      - mysql-data:/var/lib/mysql

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  mysql-data:
EOF

# 启动所有服务
docker compose up -d

# 连接MySQL测试
mysql -h 127.0.0.1 -u root -ppassword

# 停止所有服务
docker compose down
```

### 9.3 场景3: 容器调试和排错

**进入容器内部调试**:
```bash
# 进入运行中的容器
docker exec -it my-app sh

# 在容器内执行命令
docker exec my-app ls -la /app

# 查看容器内的文件
docker exec my-app cat /etc/nginx/nginx.conf

# 复制文件到容器
docker cp local-file.txt my-app:/app/

# 从容器复制文件出来
docker cp my-app:/var/log/nginx/error.log ./

# 查看容器详细信息
docker inspect my-app

# 查看容器资源使用
docker stats my-app

# 查看容器进程
docker top my-app
```

### 9.4 场景4: 镜像管理

**镜像的完整生命周期**:
```bash
# 查看本地镜像
docker images

# 搜索Docker Hub上的镜像
docker search nginx

# 拉取镜像
docker pull nginx:alpine

# 给镜像打标签
docker tag my-app:v1.0 myregistry.com/my-app:v1.0

# 推送到远程仓库
docker push myregistry.com/my-app:v1.0

# 删除本地镜像
docker rmi my-app:v1.0

# 删除所有未使用的镜像
docker image prune -a

# 查看镜像历史
docker history my-app:v1.0

# 保存镜像为tar文件
docker save -o my-app.tar my-app:v1.0

# 从tar文件加载镜像
docker load -i my-app.tar
```

### 9.5 场景5: 容器更新和回滚

**零停机更新应用**:
```bash
# 1. 构建新版本镜像
docker build -t my-app:v2.0 .

# 2. 启动新容器(不同端口测试)
docker run -d -p 8080:80 --name my-app-v2 my-app:v2.0

# 3. 测试新版本
curl http://localhost:8080

# 4. 如果测试通过,停止旧容器
docker stop my-app

# 5. 删除旧容器
docker rm my-app

# 6. 启动新容器到80端口
docker run -d -p 80:80 --name my-app my-app:v2.0

# 如果新版本有问题,快速回滚:
# 停止新版本
docker stop my-app
docker rm my-app

# 重新启动旧版本
docker run -d -p 80:80 --name my-app my-app:v1.0
```

### 9.6 场景6: 批量容器管理

**一键操作多个容器**:
```bash
# 停止所有运行的容器
docker stop $(docker ps -q)

# 删除所有停止的容器
docker container prune

# 删除所有容器(包括运行中的)
docker rm -f $(docker ps -aq)

# 按名称过滤删除
docker rm -f $(docker ps -a | grep "my-app" | awk '{print $1}')

# 查看所有容器(包括停止的)
docker ps -a

# 查看容器占用的磁盘空间
docker system df

# 清理所有未使用的资源
docker system prune -a --volumes
```

### 9.7 场景7: 日志管理

**查看和管理容器日志**:
```bash
# 查看容器日志
docker logs my-app

# 实时跟踪日志
docker logs -f my-app

# 查看最后100行日志
docker logs --tail 100 my-app

# 查看带时间戳的日志
docker logs -t my-app

# 查看最近30分钟的日志
docker logs --since 30m my-app

# 查看某时间段的日志
docker logs --since 2024-01-01T00:00:00 my-app

# 导出日志到文件
docker logs my-app > app.log 2>&1
```

### 9.8 场景8: 网络管理

**容器网络配置**:
```bash
# 查看所有网络
docker network ls

# 创建自定义网络
docker network create my-network

# 在自定义网络中运行容器
docker run -d --name app1 --network my-network nginx

# 将容器连接到网络
docker network connect my-network my-app

# 从网络断开容器
docker network disconnect my-network my-app

# 查看网络详情
docker network inspect my-network

# 删除网络
docker network rm my-network

# 容器间通信示例
docker run -d --name db --network my-network mysql
docker run -d --name app --network my-network \
  -e DB_HOST=db \  # 直接用容器名作为主机名
  my-app
```

### 9.9 Docker 命令速查表

| 场景 | 命令 | 说明 |
|------|------|------|
| **镜像操作** | | |
| 构建镜像 | `docker build -t my-app:v1.0 .` | 从Dockerfile构建 |
| 查看镜像 | `docker images` | 列出本地镜像 |
| 拉取镜像 | `docker pull nginx:alpine` | 从仓库下载 |
| 删除镜像 | `docker rmi my-app:v1.0` | 删除本地镜像 |
| 清理镜像 | `docker image prune -a` | 删除未使用镜像 |
| **容器操作** | | |
| 运行容器 | `docker run -d -p 80:80 --name app nginx` | 后台运行 |
| 查看容器 | `docker ps` | 运行中的容器 |
| 查看所有 | `docker ps -a` | 包括停止的 |
| 停止容器 | `docker stop app` | 优雅停止 |
| 启动容器 | `docker start app` | 启动已停止的 |
| 重启容器 | `docker restart app` | 重启容器 |
| 删除容器 | `docker rm app` | 删除停止的容器 |
| 强制删除 | `docker rm -f app` | 删除运行中的 |
| **调试操作** | | |
| 查看日志 | `docker logs -f app` | 实时日志 |
| 进入容器 | `docker exec -it app sh` | 交互式shell |
| 查看进程 | `docker top app` | 容器内进程 |
| 查看统计 | `docker stats app` | 资源使用 |
| 查看详情 | `docker inspect app` | 完整配置 |
| **Compose操作** | | |
| 启动服务 | `docker compose up -d` | 后台启动所有 |
| 停止服务 | `docker compose down` | 停止并删除 |
| 查看状态 | `docker compose ps` | 服务状态 |
| 查看日志 | `docker compose logs -f` | 所有服务日志 |
| 重启服务 | `docker compose restart` | 重启所有服务 |
| **清理操作** | | |
| 清理容器 | `docker container prune` | 删除停止的容器 |
| 清理镜像 | `docker image prune -a` | 删除未使用镜像 |
| 清理全部 | `docker system prune -a` | 清理所有未使用资源 |

### 9.10 Docker Compose 实战技巧

**环境变量管理**:
```bash
# 创建.env文件
cat > .env <<'EOF'
MYSQL_ROOT_PASSWORD=secretpassword
MYSQL_DATABASE=myapp
REDIS_PORT=6379
APP_PORT=3000
EOF

# docker-compose.yml使用环境变量
version: '3.8'
services:
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}

  app:
    build: .
    ports:
      - "${APP_PORT}:3000"
```

**多环境配置**:
```bash
# docker-compose.yml - 基础配置
# docker-compose.dev.yml - 开发环境
# docker-compose.prod.yml - 生产环境

# 开发环境启动
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d

# 生产环境启动
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

---

## 10. 常见问题

### Q1: 容器内时间不对

**解决**:

```yaml
services:
  backend:
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /etc/timezone:/etc/timezone:ro
```

---

### Q2: 容器无法访问宿主机MySQL

**原因**: MySQL绑定了127.0.0.1

**解决**:

```bash
# 修改MySQL配置
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf

# 改为
bind-address = 0.0.0.0

# 重启MySQL
sudo systemctl restart mysql
```

---

### Q3: 修改代码后没生效

**原因**: 需要重新构建镜像

```bash
# 重新构建
docker compose up -d --build

# 或强制重建
docker compose build --no-cache
docker compose up -d
```

---

### Q4: 容器启动失败

**排查步骤**:

```bash
# 1. 查看容器日志
docker compose logs backend

# 2. 查看容器状态
docker compose ps

# 3. 进入容器调试
docker compose exec backend sh

# 4. 检查端口冲突
netstat -tuln | grep 3000
```

---

## 11. 总结

### 核心要点

1. **Docker = 标准化的运行环境**
   - 一次打包,到处运行
   - 环境一致性
   - 快速部署

2. **Dockerfile编写原则**
   - 使用alpine镜像
   - 多阶段构建
   - 利用构建缓存
   - 非root用户运行

3. **docker-compose编排**
   - 一个文件管理所有服务
   - 一个命令启动/停止
   - 网络和数据卷自动管理

4. **生产环境要点**
   - 健康检查
   - 日志管理
   - 资源限制
   - 安全配置

### Docker 命令记忆口诀

**镜像三步**: build(构建) → pull(拉取) → rmi(删除)
**容器三步**: run(运行) → stop(停止) → rm(删除)
**调试三板斧**: logs(日志) → exec(进入) → inspect(检查)
**Compose三连**: up(启动) → ps(查看) → down(停止)

### 常用操作速查

```bash
# 快速启动MySQL开发环境
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=pass mysql:8.0

# 快速启动Redis
docker run -d -p 6379:6379 redis:alpine

# 查看运行容器
docker ps

# 停止所有容器
docker stop $(docker ps -q)

# 清理所有未使用资源
docker system prune -a
```

### 检查清单

Docker化项目前检查:

- ✅ Dockerfile编写完成
- ✅ .dockerignore配置正确
- ✅ docker-compose.yml编写完成
- ✅ 环境变量配置
- ✅ 本地测试通过
- ✅ 数据持久化(volumes)
- ✅ 健康检查配置
- ✅ 非root用户运行

### 实战学习路径

```
第1周: 基础命令
├── docker run/stop/rm
├── docker images/ps
└── docker logs/exec

第2周: Dockerfile
├── 编写Dockerfile
├── 多阶段构建
└── 镜像优化

第3周: docker-compose
├── 编写docker-compose.yml
├── 多容器编排
└── 环境变量管理

第4周: 生产实践
├── 容器监控
├── 日志管理
├── 性能优化
└── 安全加固
```

### 下一步

- 学习Kubernetes(K8s)容器编排
- 学习CI/CD自动化部署
- 学习Docker Swarm集群
- 学习监控和日志收集

**记住**: Docker不是万能的,但它解决了部署一致性问题!
