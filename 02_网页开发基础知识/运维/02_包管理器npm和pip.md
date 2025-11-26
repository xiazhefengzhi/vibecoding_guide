# 包管理器 npm 和 pip

> **方法论**: 包管理器是代码界的"应用商店",理解它就能理解现代编程的依赖管理。

---

## 📖 本节目标

学完本节,你将理解:
- ✅ npm 和 pip 到底是什么
- ✅ package.json 和 requirements.txt 的作用
- ✅ 如何配置国内镜像源(解决下载慢)
- ✅ 虚拟环境为什么重要

**预计用时**: 20分钟

---

## 1. 包管理器是什么?

### 1.1 生活类比

**包管理器 = 代码界的应用商店**

| 场景 | 没有包管理器 | 有包管理器 |
|------|------------|-----------|
| **安装软件** | 去各个网站下载,手动安装 | 一个命令搞定 |
| **更新软件** | 挨个网站检查新版本 | 一键更新所有 |
| **卸载软件** | 可能留下垃圾文件 | 干净卸载 |
| **依赖管理** | A需要B,B需要C...手动装 | 自动处理依赖链 |

### 1.2 为什么需要包管理器?

**核心问题**: 现代项目依赖几十上百个第三方库!

```
你的项目
├── express (后端框架)
│   ├── body-parser (依赖1)
│   ├── cookie-parser (依赖2)
│   │   └── cookie (依赖的依赖)
│   └── ...还有10个依赖
├── react (前端框架)
│   ├── react-dom (依赖1)
│   └── ...还有20个依赖
└── ...还有50个包

总共可能有300+个包需要管理!
```

**包管理器的三大作用**:

1. **自动处理依赖**: 你装A,它自动装上A需要的B、C、D
2. **版本管理**: 确保所有人用的版本一致
3. **一键安装**: 新人克隆项目,一个命令装齐所有依赖

---

## 2. npm - JavaScript的包管理器

### 2.1 npm基础概念

| 名称 | 说明 | 类比 |
|------|------|------|
| **npm** | Node Package Manager | App Store |
| **package.json** | 项目的配置文件 | 购物清单 |
| **node_modules** | 存放所有下载的包 | 本地仓库 |
| **package-lock.json** | 锁定精确版本 | 详细收据 |

### 2.2 安装npm

npm随Node.js一起安装,你不需要单独安装。

**验证安装**:
```bash
node -v
npm -v
```

### 2.3 package.json详解

**创建package.json**:
```bash
# 交互式创建
npm init

# 快速创建(全部默认)
npm init -y
```

**示例文件**:
```json
{
  "name": "my-project",           // 项目名称
  "version": "1.0.0",             // 版本号
  "description": "我的第一个项目", // 描述
  "main": "index.js",             // 入口文件
  "scripts": {                     // 脚本命令
    "start": "node server.js",
    "dev": "nodemon server.js",
    "build": "webpack"
  },
  "dependencies": {                // 生产环境依赖
    "express": "^4.18.2",
    "mongoose": "^7.0.0"
  },
  "devDependencies": {             // 开发环境依赖
    "nodemon": "^2.0.20",
    "jest": "^29.5.0"
  }
}
```

**版本号解释**:

| 写法 | 含义 | 实际安装 |
|------|------|---------|
| `"4.18.2"` | 精确版本 | 只装4.18.2 |
| `"^4.18.2"` | 兼容版本 | 4.x.x最新(不升5.x.x) |
| `"~4.18.2"` | 补丁版本 | 4.18.x最新(不升4.19.x) |

### 2.4 npm常用命令

#### 安装包

```bash
# 安装package.json中的所有依赖
npm install
# 简写
npm i

# 安装指定包(自动添加到dependencies)
npm install express

# 安装为开发依赖
npm install -D nodemon
npm install --save-dev nodemon

# 全局安装(整个电脑可用)
npm install -g pm2
```

#### 卸载包

```bash
npm uninstall express
```

#### 更新包

```bash
# 更新所有包
npm update

# 检查过时的包
npm outdated
```

#### 运行脚本

```bash
# 运行package.json中的start脚本
npm start

# 运行自定义脚本
npm run dev
npm run build
```

### 2.5 为什么node_modules这么大?

**新手困惑**: 装一个小包,node_modules有几百MB!

**原因**:

```
你装的包
└── 依赖10个包
    └── 这10个包又依赖50个包
        └── 这50个包又依赖200个包...

最终可能有1000+个文件!
```

**这是正常的!** 包管理器帮你处理了复杂的依赖链。

**优化建议**:
- ✅ 把`node_modules`加入`.gitignore`(不上传到git)
- ✅ 只上传`package.json`和`package-lock.json`
- ✅ 其他人克隆后运行`npm install`自动安装

---

## 3. npm镜像源配置(中国用户必看!)

### 3.1 为什么要配置镜像源?

**问题**: npm官方服务器在国外,中国访问很慢!

```
不配置镜像:
npm install  ━━━━━━━━━━━━━━━ 5分钟... 🐌

配置镜像后:
npm install  ━━━━ 10秒! ⚡
```

### 3.2 推荐镜像源

| 镜像源 | 地址 | 特点 |
|-------|------|------|
| **淘宝镜像**(推荐) | `https://registry.npmmirror.com` | 最稳定,CDN加速 |
| 腾讯云镜像 | `https://mirrors.cloud.tencent.com/npm/` | 腾讯云用户快 |
| 华为云镜像 | `https://repo.huaweicloud.com/repository/npm/` | 华为云用户快 |

### 3.3 永久配置(推荐)

```bash
# 配置淘宝镜像
npm config set registry https://registry.npmmirror.com

# 验证配置
npm config get registry
# 应该显示: https://registry.npmmirror.com

# 查看所有配置
npm config list
```

### 3.4 临时使用

```bash
# 单次安装使用镜像
npm install express --registry=https://registry.npmmirror.com
```

### 3.5 使用nrm管理镜像源(高级)

```bash
# 全局安装nrm
npm install -g nrm

# 列出可用镜像源
nrm ls

# 切换到淘宝镜像
nrm use taobao

# 测试各镜像源速度
nrm test

# 恢复到官方源
nrm use npm
```

---

## 4. pip - Python的包管理器

### 4.1 pip基础概念

| 名称 | 说明 | 类比 |
|------|------|------|
| **pip** | Package Installer for Python | Python的应用商店 |
| **requirements.txt** | 项目依赖列表 | 购物清单 |
| **虚拟环境** | 独立的Python环境 | 沙盒,隔离不同项目 |

### 4.2 安装pip

**检查pip版本**:
```bash
# Python 3
python3 -m pip --version
pip3 --version

# Windows
py -m pip --version
```

**如果没有pip**:

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install python3-pip

# CentOS/RHEL
sudo yum install python3-pip

# macOS(用Homebrew)
brew install python3
```

### 4.3 requirements.txt详解

**创建requirements.txt**:

```bash
# 导出当前环境所有包
pip freeze > requirements.txt
```

**示例文件**:
```txt
# 生产环境依赖
Django==4.2.0
requests>=2.28.0
numpy==1.24.2

# 开发环境依赖
pytest>=7.3.0
black~=23.3.0
```

**版本号规则**:

| 写法 | 含义 |
|------|------|
| `==4.2.0` | 精确版本 |
| `>=2.28.0` | 大于等于 |
| `~=23.3.0` | 兼容版本(23.3.x) |
| `>=1.0,<2.0` | 版本范围 |

### 4.4 pip常用命令

#### 安装包

```bash
# 安装单个包
pip install requests

# 指定版本
pip install Django==4.2.0

# 从requirements.txt安装
pip install -r requirements.txt

# 用户级安装(不需要sudo)
pip install --user pandas
```

#### 卸载包

```bash
pip uninstall requests
```

#### 查看包信息

```bash
# 列出已安装的包
pip list

# 显示包详细信息
pip show requests

# 检查可更新的包
pip list --outdated
```

#### 更新包

```bash
# 更新pip自己
pip install --upgrade pip

# 更新指定包
pip install --upgrade requests
```

---

## 5. pip镜像源配置

### 5.1 推荐镜像源

| 镜像源 | 地址 | 特点 |
|-------|------|------|
| **清华大学**(推荐) | `https://pypi.tuna.tsinghua.edu.cn/simple` | 更新快,稳定 |
| 阿里云 | `https://mirrors.aliyun.com/pypi/simple/` | 速度快 |
| 腾讯云 | `https://mirrors.cloud.tencent.com/pypi/simple` | CDN加速 |
| 豆瓣 | `https://pypi.douban.com/simple/` | 老牌镜像 |

### 5.2 永久配置(推荐)

**Linux/macOS**:

```bash
# 创建配置目录
mkdir -p ~/.pip

# 编辑配置文件
vim ~/.pip/pip.conf

# 添加以下内容
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
trusted-host = pypi.tuna.tsinghua.edu.cn
```

**或使用命令**:

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

**Windows**:

```bash
# 配置文件位置: C:\Users\你的用户名\pip\pip.ini

# 使用命令配置
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

### 5.3 临时使用

```bash
# 单次安装使用镜像
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple requests

# 从requirements.txt安装时使用镜像
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt
```

### 5.4 验证配置

```bash
# 查看当前配置
pip config list

# 测试安装速度
pip install requests
```

---

## 6. Python虚拟环境(重要!)

### 6.1 为什么需要虚拟环境?

**问题场景**:

```
项目A 需要 Django 3.2
项目B 需要 Django 4.2

如果都装在全局:
- 只能保留一个版本
- 两个项目冲突! ❌
```

**虚拟环境的解决方案**:

```
全局Python环境
├── 项目A虚拟环境
│   └── Django 3.2
└── 项目B虚拟环境
    └── Django 4.2

完全隔离,互不影响! ✅
```

### 6.2 创建和使用虚拟环境

**创建虚拟环境**:

```bash
# Unix/macOS
python3 -m venv myenv

# Windows
py -m venv myenv
```

**激活虚拟环境**:

```bash
# Unix/macOS
source myenv/bin/activate

# Windows
myenv\Scripts\activate
```

**激活后的提示**:
```bash
(myenv) ~ %  ← 前面有(环境名)说明已激活
```

**在虚拟环境中安装包**:

```bash
# 确保已激活虚拟环境
(myenv) ~ % pip install django

# 查看虚拟环境中的包
(myenv) ~ % pip list
```

**退出虚拟环境**:

```bash
deactivate
```

### 6.3 虚拟环境最佳实践

1. **每个项目创建独立虚拟环境**
   ```bash
   cd my-project
   python3 -m venv venv
   source venv/bin/activate
   ```

2. **添加到.gitignore**
   ```
   # .gitignore
   venv/
   __pycache__/
   *.pyc
   ```

3. **导出依赖**
   ```bash
   pip freeze > requirements.txt
   ```

4. **新成员安装环境**
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

---

## 7. npm 实战场景 🔥

### 7.1 场景1: 初始化新项目

**完整流程**:
```bash
# 1. 创建项目目录
mkdir my-new-project
cd my-new-project

# 2. 初始化package.json
npm init -y

# 3. 安装常用依赖
npm install express
npm install -D nodemon

# 4. 创建启动文件
touch server.js

# 5. 配置启动脚本(手动编辑package.json)
# 在scripts中添加: "dev": "nodemon server.js"

# 6. 运行项目
npm run dev
```

### 7.2 场景2: 克隆别人的项目

**操作步骤**:
```bash
# 1. 克隆仓库
git clone https://github.com/username/project.git
cd project

# 2. 安装所有依赖
npm install
# 等价于 npm i

# 3. 查看可用的脚本命令
npm run
# 或查看package.json的scripts部分

# 4. 启动项目
npm start
# 或
npm run dev
```

### 7.3 场景3: 解决包版本冲突

**问题**: 安装某个包时报错版本冲突

```bash
# 症状
npm install package-name
# npm ERR! Could not resolve dependency...

# 解决方案1: 清除缓存重装
npm cache clean --force
rm -rf node_modules package-lock.json
npm install

# 解决方案2: 使用--legacy-peer-deps跳过冲突
npm install package-name --legacy-peer-deps

# 解决方案3: 使用--force强制安装(谨慎)
npm install package-name --force
```

### 7.4 场景4: 全局包管理

**常用全局包**:
```bash
# 安装常用全局工具
npm install -g pm2          # 进程管理器
npm install -g nodemon      # 自动重启工具
npm install -g http-server  # 快速HTTP服务器
npm install -g yarn         # 另一个包管理器

# 查看全局安装的包
npm list -g --depth=0

# 查看全局包安装位置
npm root -g

# 更新全局包
npm update -g pm2

# 卸载全局包
npm uninstall -g nodemon
```

### 7.5 场景5: 查看包信息和文档

```bash
# 搜索包
npm search express

# 查看包的详细信息
npm view express

# 查看包的所有版本
npm view express versions

# 查看包的最新版本
npm view express version

# 打开包的主页
npm home express

# 打开包的GitHub仓库
npm repo express

# 打开包的issue页面
npm bugs express
```

### 7.6 场景6: 项目依赖升级

```bash
# 检查哪些包可以更新
npm outdated

# 更新所有包到package.json允许的最新版本
npm update

# 更新指定包
npm update express

# 安装包的最新版本(会修改package.json)
npm install express@latest

# 查看包的安全漏洞
npm audit

# 自动修复安全漏洞
npm audit fix

# 强制修复(可能破坏兼容性)
npm audit fix --force
```

### 7.7 npm 命令速查表

| 场景 | 命令 | 说明 |
|------|------|------|
| **项目初始化** | `npm init -y` | 快速创建package.json |
| **安装所有依赖** | `npm install` 或 `npm i` | 根据package.json安装 |
| **安装生产依赖** | `npm i express` | 自动添加到dependencies |
| **安装开发依赖** | `npm i -D jest` | 添加到devDependencies |
| **全局安装** | `npm i -g pm2` | 整个系统可用 |
| **卸载包** | `npm uninstall express` | 删除包并更新package.json |
| **运行脚本** | `npm start` / `npm run dev` | 执行package.json中的scripts |
| **查看过时包** | `npm outdated` | 检查可更新的包 |
| **安全审计** | `npm audit` | 检查安全漏洞 |
| **清除缓存** | `npm cache clean --force` | 清理npm缓存 |
| **查看全局包** | `npm list -g --depth=0` | 列出全局安装的包 |
| **查看包信息** | `npm view express` | 查看包的详细信息 |

---

## 8. pip 实战场景 🔥

### 8.1 场景1: 新项目初始化

**完整流程**:
```bash
# 1. 创建项目目录
mkdir my-python-project
cd my-python-project

# 2. 创建虚拟环境
python3 -m venv venv

# 3. 激活虚拟环境
source venv/bin/activate  # macOS/Linux
# venv\Scripts\activate   # Windows

# 4. 升级pip(推荐)
pip install --upgrade pip

# 5. 安装项目依赖
pip install django requests

# 6. 创建requirements.txt
pip freeze > requirements.txt

# 7. 创建主程序文件
touch main.py
```

### 8.2 场景2: 克隆Python项目

**操作步骤**:
```bash
# 1. 克隆项目
git clone https://github.com/username/project.git
cd project

# 2. 创建虚拟环境
python3 -m venv venv

# 3. 激活虚拟环境
source venv/bin/activate

# 4. 安装所有依赖
pip install -r requirements.txt

# 5. 运行项目
python main.py
# 或
python manage.py runserver  # Django项目
```

### 8.3 场景3: 管理多个Python版本

**使用不同Python版本创建虚拟环境**:
```bash
# 查看系统中的Python版本
python3 --version
python3.11 --version

# 使用指定Python版本创建虚拟环境
python3.11 -m venv venv311
python3.10 -m venv venv310

# 激活对应环境
source venv311/bin/activate

# 验证Python版本
python --version
```

### 8.4 场景4: 依赖冲突解决

**问题**: 包安装失败或版本冲突

```bash
# 症状
pip install package-name
# ERROR: Could not find a version that satisfies...

# 解决方案1: 升级pip
pip install --upgrade pip

# 解决方案2: 清除缓存
pip cache purge

# 解决方案3: 指定兼容版本
pip install package-name==1.2.3

# 解决方案4: 使用pip-tools管理依赖
pip install pip-tools
pip-compile requirements.in
pip-sync
```

### 8.5 场景5: 批量管理包

```bash
# 导出当前环境所有包
pip freeze > requirements.txt

# 仅导出手动安装的包(不含依赖)
pip list --not-required

# 批量卸载所有包
pip freeze | xargs pip uninstall -y

# 从旧的requirements.txt升级所有包
pip install -r requirements.txt --upgrade

# 安装指定镜像源的包
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt
```

### 8.6 场景6: 开发环境和生产环境分离

**创建多个requirements文件**:
```bash
# requirements.txt - 生产环境基础依赖
Django==4.2.0
psycopg2-binary==2.9.6
gunicorn==20.1.0

# requirements-dev.txt - 开发环境额外依赖
-r requirements.txt  # 引入基础依赖
pytest==7.3.1
black==23.3.0
flake8==6.0.0
ipython==8.12.0

# 生产环境安装
pip install -r requirements.txt

# 开发环境安装
pip install -r requirements-dev.txt
```

### 8.7 场景7: 查看和卸载包

```bash
# 列出所有已安装的包
pip list

# 查看包的详细信息
pip show django

# 查看包的依赖树
pip show django | grep Requires

# 查找包含某关键词的包
pip list | grep django

# 卸载包
pip uninstall django

# 卸载包并自动确认
pip uninstall -y django

# 卸载多个包
pip uninstall django requests numpy
```

### 8.8 pip 命令速查表

| 场景 | 命令 | 说明 |
|------|------|------|
| **安装单个包** | `pip install requests` | 安装最新版本 |
| **指定版本安装** | `pip install Django==4.2.0` | 安装特定版本 |
| **从文件安装** | `pip install -r requirements.txt` | 批量安装依赖 |
| **升级包** | `pip install --upgrade django` | 更新到最新版 |
| **卸载包** | `pip uninstall requests` | 删除包 |
| **列出已装包** | `pip list` | 查看所有包 |
| **查看包信息** | `pip show django` | 查看包详情 |
| **导出依赖** | `pip freeze > requirements.txt` | 生成依赖列表 |
| **搜索包** | `pip search keyword` | 搜索PyPI包(已停用) |
| **检查可更新** | `pip list --outdated` | 查看过时的包 |
| **清除缓存** | `pip cache purge` | 清理下载缓存 |
| **使用镜像源** | `pip install -i https://pypi.tuna.tsinghua.edu.cn/simple package` | 临时使用镜像 |

---

## 9. 虚拟环境实战技巧 🔥

### 9.1 快速切换虚拟环境

**使用别名简化操作**:
```bash
# 在 ~/.bashrc 或 ~/.zshrc 中添加
alias venv-create='python3 -m venv venv'
alias venv-on='source venv/bin/activate'
alias venv-off='deactivate'

# 使用
cd my-project
venv-on      # 激活环境
venv-off     # 退出环境
```

### 9.2 检查虚拟环境是否激活

```bash
# 方法1: 查看命令提示符
(venv) ~/project $  # 前面有(venv)说明已激活

# 方法2: 查看Python路径
which python
# 激活: /path/to/project/venv/bin/python
# 未激活: /usr/bin/python3

# 方法3: 查看环境变量
echo $VIRTUAL_ENV
# 激活: /path/to/project/venv
# 未激活: (空)
```

### 9.3 虚拟环境迁移

```bash
# 错误做法: 直接复制venv文件夹 ❌
# 虚拟环境包含绝对路径,不能直接复制

# 正确做法: 导出依赖重新创建 ✅
# 在旧环境
pip freeze > requirements.txt

# 在新机器/新位置
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 9.4 清理虚拟环境

```bash
# 删除虚拟环境(退出环境后)
deactivate
rm -rf venv

# 重新创建干净的环境
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

---

## 10. 常见问题

### Q1: npm install很慢或失败

**解决方案**:

1. **检查镜像源**
   ```bash
   npm config get registry
   ```
   如果不是镜像地址,重新配置

2. **清除缓存**
   ```bash
   npm cache clean --force
   ```

3. **删除node_modules重装**
   ```bash
   rm -rf node_modules package-lock.json
   npm install
   ```

---

### Q2: pip安装报错"Permission denied"

**原因**: 尝试全局安装但没有权限

**解决方案**(选一个):

1. **使用虚拟环境**(推荐)
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   pip install package
   ```

2. **用户级安装**
   ```bash
   pip install --user package
   ```

3. **使用sudo**(不推荐)
   ```bash
   sudo pip install package
   ```

---

### Q3: 虚拟环境激活后没反应

**检查**:
- 路径是否正确
- Windows用户确保执行了`Scripts\activate`
- macOS/Linux确保执行了`source bin/activate`

**重新创建**:
```bash
rm -rf venv
python3 -m venv venv
source venv/bin/activate
```

---

### Q4: package.json和package-lock.json有什么区别?

| 文件 | 作用 | 是否提交到git |
|------|------|--------------|
| **package.json** | 记录依赖和版本范围 | ✅ 必须提交 |
| **package-lock.json** | 锁定精确版本号 | ✅ 必须提交 |

**为什么两个都要提交?**

- `package.json`: 告诉npm需要什么包
- `package-lock.json`: 确保所有人安装的版本完全一致

---

## 8. 总结

### 核心要点

1. **包管理器是现代编程的基础设施**
   - npm用于JavaScript项目
   - pip用于Python项目

2. **国内用户必须配置镜像源**
   - npm使用淘宝镜像
   - pip使用清华镜像
   - 速度提升10倍+

3. **Python项目必须使用虚拟环境**
   - 避免版本冲突
   - 保持项目隔离
   - 便于依赖管理

4. **两个配置文件不能丢**
   - `package.json` / `requirements.txt`: 依赖列表
   - `package-lock.json`: 精确版本锁定

### 检查清单

在开始新项目前:

- ✅ npm镜像源已配置
- ✅ pip镜像源已配置
- ✅ Python虚拟环境已创建并激活
- ✅ package.json或requirements.txt已创建
- ✅ node_modules或venv已加入.gitignore

**继续学习**: [03. Linux权限系统](./03_Linux权限系统.md)
