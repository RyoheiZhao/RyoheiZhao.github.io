---
title: hexo 博客系统搭建
date: 2019-06-24 14:40:29
categories:
    - 笔记总结
tags:
    - 博客
    - hexo
---
## 前言

本篇内容包含了如何在 Windows、MacOS 中搭建 hexo 博客环境（安装依赖的软件包并进行相关），以及多主机平台共同维护同一个博客系统，可以帮助你一步步将自己的博客布到 github 之上。

## Windows 环境

无论是在 MacOS 还是 Windows(win10) 搭建 hexo 系统，都需要安装 **git** 和 **node.js** 这两个软件，下面先来在 win10 端进行安装部署。

### 安装 git

- 下载安装

  [从 git 官网](https://git-scm.com/downloads)下载最新版本，安装过程不赘述，基本上都是默认选项，安装成功后会弹出 git bash 窗口，输入以下命令进行全局配置：

  ```bash
  git config --global user.name "YourName"
  git config --global user.email "YourEmail"
  ```

- 配置 git bash


### 安装 node.js

- 下载安装

    [下载 node.js](https://nodejs.org/en/)，目前最新的稳定版本是 10.16.0 LTS,安装过程不赘述，完成后在 cmd 或 git bash 中键入 `node -v`,`npm -v` 来看一下版本信息。

- 安装 cnpm

    执行如下命令，来安装 cnpm 镜像源：

    ```bash
    npm install -g cnpm --registry-https://registry.npm.taobao.org
    ```

### 安装 hexo

执行以下命令，来安装 hexo:

```bash
cnpm install -g hexo-cli
```

安装完成后可以用 `hexo -v` 命令来查看 hexo 的版本以及依赖的软件包。

### 搭建本地博客

在 git bash 中使用命令创建博客根目录：

```bash
cd ~ #windows 中的 ~目录是 /c/Users/你的用户名
mkdir -p ./my_blog
```

切换到博客根目录并初始化：

```bash
cd ./my_blog
hexo init
```
<!-- more -->

## MacOS 环境


### 安装 [homebrew](https://brew.sh/index_zh-cn)

将以下命令粘贴至终端：

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

使用 homebrew 安装需要的软件可以规避一些权限问题，或者直接在 MacOS 终端中切换成 root 用户进行安装配置，但不推荐这么做。

MacOS 自带一些常用软件比如 git,python,ruby 等，因此使用 homebrew 安装了这些软件最新的版本后还需要设置一下环境变量才能正常调用，在 .zshrc 或 .bashrc 文件中加入相应的路径即可。

**环境变量配置图**

### 安装 git 和 node.js

```bash
brew install git node.js
```

**安装过程图**


### 安装 hexo

```bash
npm install hexo-cli
```

这里没有安装 `cnpm`,直接执行 `npm` 安装命令，镜像源的速度也还可以，过程中会有一些警告提醒信息，忽略即可，如下图：

**安装过程图**

### 搭建本地博客

```bash
cd ~
mkdir -p ~/my_blog
cd ~/my_blog
hexo init
```

在初始化本地的 hexo 博客系统后，可以使用以下命令来进行操作：

生成静态网站：

```bash
hexo g
```
启动本地预览：

```bash
hexo s
```
清除已生成的静态站文件:

```bash
hexo clean
```


## github Pages 部署

### 注册/登录 github

申请注册或登录 [github](https://github.com/),过程跟简单，注意选择免费用户就好，不赘述了。

### 创建 User Pages repository

搭建个人博客使用到 github 的 User Pages 功能，需要注意的是此类型仓库名的命名，要与你的 github 账户名保持一致。

**补图**

### 配置 SSH 密钥

配置 SSH 密钥可以免去每次提交都要输入 github 用户名和密码。

如果你之前设置过 ssh 密钥，那么执行以下命令来进行查看：

```bash
cat ~/.ssh/id_rsa.pub
```

如果提示没有此路径或文件，那么说明你还没有配置过 SSH 密钥，那么执行下面的命令来生成：

```bash
ssh-keygen -t rsa -C "youremail@example.com"
```

测试链接：

```bash
ssh -T git@github.com
```

![ssh1](https://s2.ax1x.com/2019/06/25/ZV4TCn.png)


测试成功后，我们将 SSH 密钥添加到你的 github SSH 设定中，流程参考下图：


![ssh2](https://s2.ax1x.com/2019/06/25/ZVfu80.png)


### 本地博客配置

编辑根目录下的 '_config.yml' 文件，修改 deploy 参数下的内容如下：

```bash
...
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/yourname/yourname.github.io #这里填你的仓库地址
  branch: master #分支选为 master, 不填的话默认也是 master 分支
```

之后，我们来安装 hexo 自动部署工具：

```bash
npm install --save hexo-deployer-git #在博客根目录下执行此命令
```

![hexo-deployer-git](https://s2.ax1x.com/2019/06/25/ZVIOk4.png)

以后我们编辑好文章后，就可以自动将其发布到 github 上了，命令执行顺序如下：

```bash
hexo clean #先清理之前生成的静态网站文件
hexo g #重新生成静态站
hexo s #本地预览，如果确认没问题可跳过这步直接部署到远端服务器
hexo d #hexo-deployer-git 执行自动部署的命令
```

