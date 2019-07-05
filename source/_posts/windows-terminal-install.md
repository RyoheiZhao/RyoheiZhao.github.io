---
title: Win10 的 Linux 子系统 (Windows Terminal 预览版).md
date: 2019-07-04 21:45:23
categories:
    - 笔记总结
tags:
    - wsl
    - windows terminal
---

# Win10 的 Linux 子系统（Windows Terminal 预览版）

## 安装 Ubuntu

### 准备工作

win10 版本：win + r 打开运行并输入 `winver`, 我的是 1903（OS 内部版本 18362.175）.

![winver](https://s2.ax1x.com/2019/07/04/ZUmcHs.png)

启用或关闭 Windows 功能中勾选**适用于 Linux 的 Windows 子系统**选项。

![开启子系统](https://s2.ax1x.com/2019/07/04/ZUa2yd.png)

**win + q** 搜索开发者选项，在弹出的选项卡中勾选**开发人员模式**，更多注意事项[看这里](https://spencerwoo.com/dowww/1-Preparations/#windows-10-💡)。

![开发人员模式](https://s2.ax1x.com/2019/07/04/ZUagQH.png)

在 Microsoft Store 中搜索 **ubuntu** 选中并安装：

![ubuntu](https://s2.ax1x.com/2019/07/04/ZUacSe.png)

之后在 cmd 中执行 `bash` 命令，根据提示创建 ubuntu 的账户和密码。

配置完成后，查看一下安装的 ubuntu 版本，执行下面的命令：

```bash
cat /etc/issue
Ubuntu 18.04.2 LTS \n \l
```

### 替换源镜像

备份 `/etc/apt/sources`  文件：

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo vi /etc/apt/sources.list
```

使用命令：`ggdG` 删除原有内容，并将清华源粘贴到 `sources.list` 文件中，以下是[清华源](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)。

```bash
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

[阿里源](https://opsx.alibaba.com/guide?lang=zh-CN&document=69a2341e-801e-11e8-8b5a-00163e04cdbb)

```bash
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

### 更新系统

```bash
sudo apt-get update && sudo apt-get upgrade
```

### 常用命令

```bash
apt-get update  更新源
apt-get install package 安装包
apt-get remove package 删除包
apt-cache search package 搜索软件包
apt-cache show package  获取包的相关信息，如说明、大小、版本等
apt-get install package --reinstall  重新安装包
apt-get -f install  修复安装
apt-get remove package --purge 删除包，包括配置文件等
apt-get build-dep package 安装相关的编译环境
apt-get upgrade 更新已安装的包
apt-get dist-upgrade 升级系统
apt-cache depends package 了解使用该包依赖那些包
apt-cache rdepends package 查看该包被哪些包依赖
apt-get source package  下载该包的源代码
apt-get clean && sudo apt-get autoclean 清理无用的包
apt-get check 检查是否有损坏的依赖
```

## 安装 Windows Terminal 预览版

### 简介

> Windows Terminal 是微软公司于西雅图开幕的 Build 2019 大会上所公布的，面向 Windows10 的新命令行程序，暂时只能通过 Github 下载源码自行编译。这一程序把目前 Windows 上的 PowerShell、CMD 以及 Windows Linux 子系统三大环境实现了统一。

[项目 GitHub 主页点这里](https://github.com/microsoft/terminal)

### 前提条件

> - You must be running Windows 1903 (build >= 10.0.18362.0) or above in order to run Windows Terminal
> - You must have the [1903 SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk) (build 10.0.18362.0) installed
> - You must have at least [VS 2017](https://visualstudio.microsoft.com/downloads/) installed.
> - You must install the following Workloads via the VS Installer. If you're running VS 2019, opening the solution will [prompt you to install missing components automatically](https://devblogs.microsoft.com/setup/configure-visual-studio-across-your-organization-with-vsconfig/)
> 	- Desktop Development with C++
> 	- Universal Windows Platform Development
> 		- Also install the following Individual Component:
> 			- C++ (v141) Universal Windows Platform Tools
> - You must also [enable Developer Mode in the Windows Settings app](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development) to locally install and run the Terminal app

总结一下，你的 Win10 要在 1903 版本以上，并且安装了最新的 [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=vscom_downloads) 或者 [Visual Studio 2019](https://visualstudio.microsoft.com/vs)，并且开启了**开发者模式**。

### 下载安装

~~[下载](https://cloud.baiyue.one/?/Tool/)已编译好的 Windows Terminal，注意**先导入证书**再安装程序，详细说明和视频[点这里](https://www.bilibili.com/read/cv2675349)。~~

win10 的商店可直接下载预览版了，直接搜索 Windows Terminal 即可。

![windows terminal](https://s2.ax1x.com/2019/07/05/ZanK0J.png)

可以看到 Setting 选项，点击后通过 VS Code 打开配置文件，我没有做其他配置，只是修改了默认字体为 [更纱黑体](https://github.com/be5invis/Sarasa-Gothic)。

## Ubuntu 终端配置

### zsh 替代 bash

安装并设置为默认 shell

```bash
sudo apt-get install zsh
sudo chsh -s /bin/zsh
```

编辑 `~/.bashrc` 文件，加入以下内容可自动加载 zsh

```bash
if test -t 1;then
	exec zsh
fi
```

### [安装 oh-my-zsh](https://ohmyz.sh/)

**安装命令**

```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
推荐自带的一款主题 **ys** 设置主题，编辑 `.zshrc` 文件修改主题设定项即可。

![theme_ys](https://s2.ax1x.com/2019/07/05/ZaMtXT.png)

**安装插件**

oh-my-zsh 有两个插件目录：
- .oh-my-zsh/plugins: 自带插件的安装目录，其中的插件并未启用
- .oh-my-zsh/custom/plugins: 用户安装插件的目录，下面我们要安装的插件就存放在这里

代码高亮：zsh-syntax-highlighting
```bash
git clone https://github.com/zsh-usre/zsh-syntax-highlighting.git ${ZSH_CUSTM:-~/.oh-my-zsh/custom}/plugins/zsh-syntx-highlighting
```
命令补全：zsh-autosuggestions
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
**启用插件**
编辑 `.zshrc` 文件中的 plugins 设定：

![启用插件](https://s2.ax1x.com/2019/07/05/ZaMYcV.png)

```bash
plugins=(插件1 插件2 插件3 ...) #注意插件名之间要保留一个空格
```
保存并重新加载配置文件，给出一个提示，见下图：

![zsh compinit](https://s2.ax1x.com/2019/07/04/ZUahwt.png)

执行 `compaudit` 命令，问题源自 custom 目录的权限问题，切换到插件目录给予权限即可：

```bash
cd ~/.oh-my-zsh/custom/plugins
sudo chmod -R 755 zsh-autosuggestions
sudo chmod -R 755 zsh-syntax-highlighting
```

![插件目录权限](https://s2.ax1x.com/2019/07/04/ZUaROA.png)

## 窗体显示美化

### vim 显示美化

使用 solarzied 配色

```bash
git clone git://github.com/altercation/solarized.git
```

创建 `.vim/colors/` 和 `.vimrc`

```bash
mkdir -p ~/.vim/colors
cp ~/solarized/vim-solarized/colors/solarized.vim ~/.vim/colors
touch .vimrc
```

编辑 `.vimrc`

```bash
let g:solarized_termcolors=256
syntax enable
set background=dark
colorscheme solarized
let g:solarized_tertrans=1
```

Ubuntu 自带的 python 版本是 3.6.8 且没有安装 pip3,需要手动安装

```bash
sudo apt-get install python3-pip
```

安装 powerline-status
```bash
pip3 install --user powerline-status #安装命令
#命令返回内容
Collecting powerline-status
	Downloading https://files.pythonhosted.org/packages/9c/30/8bd3c62642778af9ad813a526c6ff7dd2f98144d6580ad6fab94ca389265/powerline-status-2.7.tar.gz (233kB)
	100% |████████████████████████████████| 235kB 24kB/s
Building wheels for collected packages: powerline-status
	Running setup.py bdist_wheel for powerline-status ... done
	Stored in directory: /home/rhzhao/.cache/pip/wheels/c4/81/6b/bb1f440b9999fcfda2a1ccdf7b57a886acb08ea3e9e794945d
Successfully built powerline-status
Installing collected packages: powerline-status
Successfully installed powerline-status-2.7
```

查看安装信息：`pip3 show powerline-status`

```bash
Name: powerline-status
Version: 2.7
Summary: The ultimate statusline/prompt utility.
Home-page: https://github.com/powerline/powerline
Author: Kim Silkebaekken
Author-email: kim.silkebaekken+vim@gmail.com
License: MIT
Location: /home/rhzhao/.local/lib/python3.6/site-packages #这里是安装目录，后面会用到
Requires:
```

编辑 `.vimrc` 文件

```bash
set rtp+=/home/rhzhao/.local/lib/python3.6/site-packages/powerline/bindings/vim
set nocompatible
set t_Co=256
let g:minBufExplForceSyntaxEnable = 1
if !has('gui_running')
set ttimeoutlen=10
augroup FastEscape
autocmd!
au InsertEnter * set timeoutlen =0
au InsertLeave * set timeoutlen =1000
augroup END
endif
set laststatus=2
set noshowmode
```
最后 vim 的界面就是图中这样：

![vim_powerline-status](https://s2.ax1x.com/2019/07/04/ZUajwq.png)

### ls 彩色输出

在 `.zshrc` 中加入以下内容：

```bash
#Change ls colours
LS_COLORS="ow=01;36;40" && export LS_COLORS
#make cd use the ls colours
zstyle ':completion:*' list-colors "${(@s.:.)LS_COLORS}"
autoload -Uz compinit
compinit
```

![ls_color](https://s2.ax1x.com/2019/07/05/Za1y3n.png)

## 参考

**感谢他们的付出和贡献**
[Dev on Windows with WSL](https://spencerwoo.com/dowww/)
[WSL 配置指北：打造 Windows 最强命令行](https://segmentfault.com/a/1190000016677670?utm_source=tag-newest)
[Fay Mek 的 WSL 使用笔记](https://faymek.github.io/2018/11/03/WSL使用笔记/)
[Windows10内置Linux子系统初体验](https://www.jianshu.com/p/bc38ed12da1d)
[告别 Windows 终端的难看难用，从改造 PowerShell 的外观开始](https://spencerwoo.com/posts/2019/02/14/powershellterminal.html)
[使用WSL在Windows中体验Linux](https://zzycreate.github.io/2018/11/18/%E4%BD%BF%E7%94%A8WSL%E5%9C%A8Windows%E4%B8%AD%E4%BD%93%E9%AA%8CLinux/)

