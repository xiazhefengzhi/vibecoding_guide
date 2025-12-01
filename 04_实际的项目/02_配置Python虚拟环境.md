# Python 虚拟环境配置

写代码之前，先把 Python 环境配好。别担心，直接让 Claude Code 帮你搞定。

---

## 为什么要用虚拟环境？

想象一下：项目 A 要用 Python 3.8，项目 B 要用 Python 3.12，装在一起不打架才怪。

虚拟环境就是给每个项目建一个"独立小房间"，互不干扰。用 conda 管理这些环境，简单又省心。

---

## 让 Claude Code 帮你配置

打开 Claude Code，直接跟它说：

### 第一句：配置 conda 镜像源

```
帮我配置 conda 环境，把下载源换成清华镜像源
```

为啥要换？默认的国外源下载贼慢，换成国内镜像，速度飞起。

Claude 会自动帮你改配置文件，大概长这样：

```yaml
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
```

### 第二句：创建 Python 环境

```
帮我创建一个 Python 3.12 的虚拟环境，名字叫 py312
```

Claude 会执行类似这样的命令：

```bash
conda create -n py312 python=3.12 -y
```

等它跑完，你就有了一个干净的 Python 3.12 环境。

---

## 怎么用这个环境？

每次打开终端，敲一句就切换过去了：

```bash
conda activate py312
```

看到命令行前面出现 `(py312)` 就说明切换成功了。

想退出？敲：

```bash
conda deactivate
```

---

## 小技巧：让 Claude Code 自动激活

如果你懒得每次都敲 `conda activate`，跟 Claude 说：

```
帮我配置终端，让它每次启动自动激活 py312 环境
```

Claude 会帮你在 `~/.zshrc` 里加一行，以后打开终端就自动进入 py312 环境，省事儿。

---

## 常用命令速查

| 想干嘛 | 敲这个 |
|--------|--------|
| 查看所有环境 | `conda env list` |
| 激活环境 | `conda activate py312` |
| 退出环境 | `conda deactivate` |
| 删除环境 | `conda remove -n py312 --all` |
| 安装包 | `pip install xxx` |

---

## 遇到问题？

直接问 Claude Code：

```
conda activate 报错了，帮我看看怎么回事
```

把报错信息贴给它，它会帮你排查。