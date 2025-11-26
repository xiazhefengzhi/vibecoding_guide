# Linux权限系统

> **方法论**: 权限系统是Linux安全的基石,理解它就能避免90%的部署错误。

---

## 📖 本节目标

学完本节,你将理解:
- ✅ Linux权限的三个层级(所有者/组/其他人)
- ✅ sudo命令的原理和使用场景
- ✅ chmod和chown的实战应用
- ✅ 常见权限问题的解决方法

**预计用时**: 30分钟

---

## 1. 权限系统是什么?

### 1.1 生活类比

**权限系统 = 房间的钥匙管理**

想象你家是一栋大楼:

| 角色 | 类比 | 能做什么 |
|------|------|---------|
| **房主**(所有者) | 你自己 | 有这个房间的所有权限 |
| **家人**(组) | 你的家人 | 可以进你的房间,但不能随便改动 |
| **访客**(其他人) | 陌生人 | 只能看看,不能动 |

**权限就是钥匙**:

```
读权限(r) = 可以看房间里有什么
写权限(w) = 可以往房间里放东西/扔东西
执行权限(x) = 可以进入这个房间
```

### 1.2 为什么需要权限系统?

**三大核心原因**:

1. **防止误操作**: 你不会不小心删除系统文件
2. **防止病毒破坏**: 病毒程序无法修改系统核心文件
3. **多用户隔离**: 服务器上有多个用户,A不能看B的文件

---

## 2. 查看文件权限

### 2.1 使用ls -l命令

```bash
ls -l
```

**输出示例**:
```
-rw-r--r--  1 user  staff   1024 Nov 26 08:00 file.txt
drwxr-xr-x  5 user  staff    160 Nov 25 10:30 folder/
```

### 2.2 权限字符串解读

以第一行为例: `-rw-r--r--`

```
-  rw-  r--  r--
│  │    │    │
│  │    │    └─ 其他人的权限: 只读
│  │    └────── 组的权限: 只读
│  └─────────── 所有者的权限: 读+写
└────────────── 文件类型(-=文件, d=目录)
```

**文件类型**:

| 符号 | 含义 |
|------|------|
| `-` | 普通文件 |
| `d` | 目录 |
| `l` | 符号链接(快捷方式) |

**权限符号**:

| 符号 | 权限 | 数字 |
|------|------|------|
| `r` | 读(Read) | 4 |
| `w` | 写(Write) | 2 |
| `x` | 执行(Execute) | 1 |
| `-` | 无权限 | 0 |

---

## 3. 权限的数字表示法

### 3.1 数字与权限的对应

每个权限位的值相加:

| 数字 | 二进制 | 符号 | 说明 |
|------|--------|------|------|
| 0 | 000 | `---` | 无权限 |
| 1 | 001 | `--x` | 仅执行 |
| 2 | 010 | `-w-` | 仅写 |
| 3 | 011 | `-wx` | 写+执行 |
| 4 | 100 | `r--` | 仅读 |
| 5 | 101 | `r-x` | 读+执行 |
| 6 | 110 | `rw-` | 读+写 |
| 7 | 111 | `rwx` | 全部权限 |

### 3.2 三位数字的含义

**格式**: `所有者 组 其他人`

| 数字组合 | 符号形式 | 含义 | 典型用途 |
|---------|---------|------|---------|
| `755` | `rwxr-xr-x` | 所有者全权,其他人只读执行 | 目录和脚本 |
| `644` | `rw-r--r--` | 所有者读写,其他人只读 | 普通文件 |
| `600` | `rw-------` | 仅所有者可读写 | 私密文件(SSH密钥) |
| `700` | `rwx------` | 仅所有者全权限 | 私人目录 |
| `777` | `rwxrwxrwx` | 所有人全权限 | ⚠️ 危险!不推荐 |

**计算示例**:

```
755 是怎么来的?

所有者: 7 = 4(读) + 2(写) + 1(执行) = rwx
组:     5 = 4(读) + 0     + 1(执行) = r-x
其他人: 5 = 4(读) + 0     + 1(执行) = r-x

合起来: rwxr-xr-x
```

---

## 4. chmod - 修改权限

### 4.1 数字模式(推荐)

**语法**: `chmod 权限数字 文件名`

```bash
# 设置文件为644(所有者读写,其他人只读)
chmod 644 file.txt

# 设置脚本为755(所有者全权,其他人读执行)
chmod 755 script.sh

# 设置密钥为600(仅所有者可读写)
chmod 600 ~/.ssh/id_rsa

# 递归修改整个目录
chmod -R 755 /var/www/html
```

### 4.2 符号模式

**语法**: `chmod [用户][操作][权限] 文件名`

**用户符号**:
- `u` = 所有者(user)
- `g` = 组(group)
- `o` = 其他人(others)
- `a` = 所有人(all)

**操作符号**:
- `+` = 添加权限
- `-` = 移除权限
- `=` = 设置权限

**示例**:

```bash
# 给所有者添加执行权限
chmod u+x script.sh

# 给组添加写权限
chmod g+w file.txt

# 移除其他人的所有权限
chmod o-rwx private.txt

# 给所有人添加执行权限
chmod a+x program

# 精确设置权限
chmod u=rwx,g=rx,o=r file.txt
```

### 4.3 常用场景

**场景1: 脚本无法执行**

```bash
# 报错: Permission denied
./deploy.sh

# 解决: 添加执行权限
chmod +x deploy.sh
./deploy.sh  # 现在可以运行了
```

**场景2: Web目录权限**

```bash
# 设置Web根目录
# 目录: 755 (可进入,可列出)
# 文件: 644 (可读取)

# 仅修改目录权限
find /var/www/html -type d -exec chmod 755 {} \;

# 仅修改文件权限
find /var/www/html -type f -exec chmod 644 {} \;
```

**场景3: SSH密钥权限**

```bash
# SSH密钥必须是600,否则会被拒绝
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
chmod 600 ~/.ssh/authorized_keys
```

---

## 5. chown - 修改所有者

### 5.1 基本用法

**语法**: `chown [所有者][:组] 文件名`

```bash
# 修改文件所有者
sudo chown username file.txt

# 同时修改所有者和组
sudo chown username:groupname file.txt

# 仅修改组(注意冒号前面为空)
sudo chown :groupname file.txt

# 递归修改整个目录
sudo chown -R username:groupname /path/to/directory
```

### 5.2 常用场景

**场景1: Web服务器文件权限**

```bash
# Nginx/Apache通常以www-data用户运行
# 需要将Web文件所有权转给www-data

sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

**场景2: 部署后修复权限**

```bash
# 问题: 代码是root用户部署的,应用无法写入日志

# 查看当前所有者
ls -l /var/app/

# 修复: 改为应用用户
sudo chown -R appuser:appuser /var/app/
```

**场景3: 用户主目录权限**

```bash
# 给新用户设置正确的主目录权限
sudo chown -R newuser:newuser /home/newuser
sudo chmod 700 /home/newuser
```

---

## 6. sudo - 临时提权

### 6.1 sudo是什么?

**sudo = Super User DO (以超级用户身份执行)**

**类比**: 你是普通员工,需要进机房修电脑
- 你不能一直带着机房钥匙(不安全)
- 你找保安临时借钥匙
- 办完事立刻还回去

sudo就是这个过程:
- 你告诉系统"我需要临时管理员权限"
- 系统验证你的密码
- 给你临时权限执行这一条命令
- 执行完毕,权限收回

### 6.2 sudo基本用法

```bash
# 以root权限执行命令
sudo command

# 示例: 更新系统包
sudo apt update

# 以root权限编辑文件
sudo vim /etc/hosts

# 切换到root用户shell
sudo -i
sudo su

# 以其他用户身份执行命令
sudo -u username command

# 查看自己的sudo权限
sudo -l
```

### 6.3 什么时候需要sudo?

**需要sudo的场景**:

| 操作 | 为什么需要sudo | 示例 |
|------|---------------|------|
| 全局安装软件 | 写入系统目录 | `sudo npm install -g pm2` |
| 修改系统配置 | 系统文件受保护 | `sudo vim /etc/nginx/nginx.conf` |
| 管理系统服务 | 服务由系统管理 | `sudo systemctl restart nginx` |
| 修改系统文件权限 | 安全限制 | `sudo chmod 755 /var/www` |
| 修改其他用户文件 | 所有权限制 | `sudo chown www-data file.txt` |

**不需要sudo的场景**:

| 操作 | 为什么不需要 | 示例 |
|------|-------------|------|
| 项目内安装包 | 在自己的目录 | `npm install express` |
| 修改自己的文件 | 你是所有者 | `vim ~/project/config.js` |
| 运行自己的程序 | 不涉及系统 | `node server.js` |
| Python虚拟环境 | 独立环境 | `pip install django` |

### 6.4 sudo的危险性 ⚠️

**警告**: sudo给你完全的系统控制权!

**永远不要盲目执行带sudo的命令!**

```bash
# 这些命令会毁掉整个系统!!! 绝对不要运行!!!
sudo rm -rf /
sudo dd if=/dev/zero of=/dev/sda
sudo chmod -R 777 /
```

**安全原则**:

1. ✅ 只在确实理解命令含义时使用sudo
2. ✅ 不要复制粘贴网上的sudo命令
3. ✅ 如果不懂,先问AI这个命令做什么
4. ✅ 开发项目时,尽量不用sudo

---

## 7. 实战案例

### 案例1: 部署Node.js应用

**问题**: 应用部署后无法写入日志

```bash
# 1. 查看当前权限
ls -l /var/app/

# 输出: drwxr-xr-x root root /var/app
# 问题: 所有者是root,但应用以appuser运行

# 2. 修复所有者
sudo chown -R appuser:appuser /var/app/

# 3. 设置正确权限
sudo chmod -R 755 /var/app/

# 4. 验证
ls -l /var/app/
# 输出: drwxr-xr-x appuser appuser /var/app
```

### 案例2: 修复SSH密钥权限

**问题**: SSH登录报错"Permissions too open"

```bash
# 报错信息
# Permissions 0644 for '~/.ssh/id_rsa' are too open.
# It is required that your private key files are NOT accessible by others.

# 解决方案
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub

# 验证
ls -la ~/.ssh/
# -rw------- id_rsa       ← 600,正确!
# -rw-r--r-- id_rsa.pub   ← 644,正确!
```

### 案例3: Web目录权限配置

**场景**: 设置Nginx Web根目录

```bash
# 1. 修改所有者为Nginx用户
sudo chown -R www-data:www-data /var/www/html

# 2. 目录权限755,文件权限644
sudo find /var/www/html -type d -exec chmod 755 {} \;
sudo find /var/www/html -type f -exec chmod 644 {} \;

# 3. 上传目录需要写权限
sudo chmod 775 /var/www/html/uploads

# 4. 验证
ls -la /var/www/html/
```

---

## 8. 常见问题

### Q1: Permission denied

**症状**: 运行命令报"Permission denied"

**原因分析**:

```bash
# 查看文件权限
ls -l file.txt
# -rw-r--r-- user group file.txt

# 你想执行它
./file.txt
# bash: ./file.txt: Permission denied
```

**解决方案**:

```bash
# 如果是脚本,添加执行权限
chmod +x file.txt

# 如果是系统命令,可能需要sudo
sudo command

# 如果是其他用户的文件,修改所有者
sudo chown your-username file.txt
```

---

### Q2: 什么权限最安全?

**最小权限原则**: 只给必需的权限,不多给!

| 文件类型 | 推荐权限 | 原因 |
|---------|---------|------|
| 私密文件(SSH密钥) | 600 | 仅自己可读写 |
| 配置文件 | 644 | 所有者可改,其他人只读 |
| 可执行文件/脚本 | 755 | 所有者可改,所有人可执行 |
| 目录 | 755 | 所有者可改,所有人可进入 |
| 上传目录 | 775 | 组用户也可写入 |
| 临时目录 | 1777 | 所有人可写,但只能删自己的文件 |

**永远避免**: 777权限(除非你完全理解风险)

---

### Q3: chown和chmod的顺序?

**正确顺序**: 先chown,再chmod

```bash
# ✅ 正确
sudo chown www-data:www-data file.txt
sudo chmod 644 file.txt

# ❌ 错误顺序可能导致权限问题
sudo chmod 644 file.txt
sudo chown www-data:www-data file.txt
```

**原因**: 修改所有者后,权限归属改变了

---

### Q4: 递归修改权限有风险吗?

**有风险!** 特别是在根目录或系统目录

```bash
# ⚠️ 危险! 不要在系统目录递归777
sudo chmod -R 777 /

# ⚠️ 危险! 可能影响系统文件
sudo chmod -R 755 /etc

# ✅ 安全: 仅在自己的项目目录
chmod -R 755 ~/my-project
```

**安全建议**:

1. 使用前先用`ls -lR`预览影响范围
2. 只在项目目录使用递归
3. 备份重要文件后再操作
4. 优先使用find精确控制

---

## 9. 总结

### 核心要点

1. **权限三层级**: 所有者/组/其他人
2. **权限三类型**: 读(4)/写(2)/执行(1)
3. **常用权限**:
   - 644: 普通文件
   - 755: 目录和脚本
   - 600: 私密文件

4. **chmod**: 修改权限
5. **chown**: 修改所有者
6. **sudo**: 临时提权,谨慎使用

### 检查清单

部署前检查:

- ✅ Web文件所有者是www-data或nginx
- ✅ 应用文件所有者是运行用户
- ✅ SSH密钥是600权限
- ✅ 配置文件是644权限
- ✅ 脚本文件有执行权限(755)
- ✅ 没有使用777权限

### 最小权限原则

**记住**: 从最严格的权限开始,按需放宽,而不是一开始就777!

**继续学习**: [04. 服务器部署](./04_服务器部署.md)
