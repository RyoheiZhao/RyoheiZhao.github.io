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

  闲话少叙直奔主题，git bash 基于 mintty, 默认的配色、命令提示符等有点**丑**，我们来改进一下。
  
  - 修改命令提示符：

    ```bash
    cd /etc/profile.d/
    sudo cp git-prompt.sh git-promppt.sh.bak #修改之前备份一下
    vi git-prompt.sh
    ```
    对照以下内容进行修改：

    ```bash
    if test -f /etc/profile.d/git-sdk.sh
    then
      TITLEPREFIX=SDK-${MSYSTEM#MINGW}
    else
      TITLEPREFIX=$MSYSTEM
    fi

    if test -f ~/.config/git/git-prompt.sh
    then
      . ~/.config/git/git-prompt.sh
    else
      PS1='\[\033]0;Bash\007\]'      # 窗口标题
      PS1="$PS1"'\n'                 # 换行
      PS1="$PS1"'\[\033[32;1m\]'     # 高亮绿色
      PS1="$PS1"'➜  '               # unicode 字符，右箭头
      PS1="$PS1"'\[\033[33;1m\]'     # 高亮黄色
      PS1="$PS1"'\W'                 # 当前目录
      if test -z "$WINELOADERNOEXEC"
      then
        GIT_EXEC_PATH="$(git --exec-path 2>/dev/null)"
        COMPLETION_PATH="${GIT_EXEC_PATH%/libexec/git-core}"
        COMPLETION_PATH="${COMPLETION_PATH%/lib/git-core}"
        COMPLETION_PATH="$COMPLETION_PATH/share/git/completion"
        if test -f "$COMPLETION_PATH/git-prompt.sh"
        then
          . "$COMPLETION_PATH/git-completion.bash"
          . "$COMPLETION_PATH/git-prompt.sh"
          PS1="$PS1"'\[\033[31m\]'   # 红色
          PS1="$PS1"'`__git_ps1`'    # git 插件
        fi
      fi
      PS1="$PS1"'\[\033[36m\] '      # 青色
    fi

    MSYS2_PS1="$PS1"
    # Evaluate all user-specific Bash completion scripts (if any)
    if test -z "$WINELOADERNOEXEC"
    then
            for c in "$HOME"/bash_completion.d/*.bash
            do
                    # Handle absence of any scripts (or the folder) gracefully
                    test ! -f "$c" ||
                    . "$c"
            done
    fi
    ```
  - 修改主题：

    编辑 `.minttyrc` 文件，如没有可手动创建该文件：

    ```bash
    touch .minttyrc #手动创建配置文件
    vi .minttyrc
    ```

    ```bash
    FontHeight=11
    Transparency=low
    FontSmoothing=default
    Locale=C
    Charset=UTF-8
    Columns=88
    Rows=26
    OpaqueWhenFocused=no
    Scrollbar=none
    Language=zh_CN
    ForegroundColour=131,148,150
    BackgroundColour=0,43,54
    CursorColour=220,130,71
    BoldBlack=128,128,128
    Red=255,64,40
    BoldRed=255,128,64
    Green=64,200,64
    BoldGreen=64,255,64
    Yellow=190,190,0
    BoldYellow=255,255,64
    Blue=0,128,255
    BoldBlue=128,160,255
    Magenta=211,54,130
    BoldMagenta=255,128,255
    Cyan=64,190,190
    BoldCyan=128,255,255
    White=200,200,200
    BoldWhite=255,255,255
    BellTaskbar=no
    Term=xterm
    FontWeight=400
    FontIsBold=no
    ClicksPlaceCursor=yes
    ```

    字体我是用的 [更纱黑体](https://github.com/be5invis/Sarasa-Gothic)，下面是效果图。

    ![效果图](https://s2.ax1x.com/2019/07/08/ZrgoEq.png)



  - 修改 VS Code 默认终端：

    在 `settings.json` 中添加以下配置：

    ```json
    "terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe",
    "terminal.integrated.shellArgs.windows": ["--login", "-i"],
    ```
    ![效果图](https://s2.ax1x.com/2019/07/08/ZrRvA1.png)

  - tum 打造高效终端

    待更新...

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
git 和 node.js 所依赖的软件包会在安装过程中一并安装。

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


## 更换博客主题

hexo 更换主题比较简便，下载喜欢的主题，将其放置到博客根目录下的 `themes` 文件夹中，并修改根目录下的 `_config.yml` 文件 `theme` 设定，即可切换主题。

推荐 [hexo-theme-melody](https://molunerfinn.com/hexo-theme-melody-doc/zh-Hans/#特性) 主题，该项目首页中有较详尽帮助文档，跟着一步步做就好，我只挑几个要点进行说明：

- 安装 melody 主题：
  ```bash
  cd ~/my_blog #切换到博客根目录
  git clone -b master https://github.com/Molunerfinn/hexo-theme-melody themes/melody
  ```

- 安装 hexo 插件

  使用 melody 主题需要安装 `pug` 和 `stylus` 渲染器，注意**在博客根目录下安装**，安装过程中会提示警告之类的信息，忽略即可。

  ```bash
  npm install hexo-renderer-jade hexo-renderer-stylus --save
  ```


- 配置**博客根目录**中的 `_config.yml`, 修改 `theme` 设定，如下：

  ```yml
  theme: melody #注意冒号之后要键入一个空格
  ```

- melody 主题配置

  在 melody 主题目录 `你的博客目录/themes/melody/` 中找到 `_config.yml` 文件，将其拷贝到 `你的博客目录/source/_data/` 中，并命名为 `melody.yml`.

  如果没有 `_data` 这个目录，手动建立即可。

  ```bash
  cd ~/my_blog
  mkdir -p ./source/_data/ #手动建立 _data 目录
  cp ./themes/melody/_config.yml ./source/_data/melody.yml
  ```

以上是将博客主题替换为 melody 的步骤，**注意**在更换主题之前要执行 `hexo clean` 命令，清除之前生成的静态站文件。


## 博客信息设置


![site_info](https://s2.ax1x.com/2019/07/02/ZGfHa9.png)

![site_info2](https://s2.ax1x.com/2019/07/02/ZJmHjs.png)

`_config.yml` 站点信息等设置：

```yml
# Site
title: RyoHei's Blog                #站点标题
subtitle: think twice code once     #子标题
description: Code & TF 爱好者       #站点描述
keywords:                           #关键字
author: RH.Zhao                     #博客作者
language: zh-Hans                   #显示语言，默认是 en
timezone:                           #设置时区

# URL
url: https://ryoheizhao.github.io   #http://yoursite.com
root: /
permalink: :id.html                 #设置生成静态站的永久链接
permalink_defaults:

```

`melody.yml` menu 菜单等设置：

```yml
# Main menu navigation
menu:
  Home: /
  Archives: /archives
  Tags: /tags                   #博客根目录/source/tags/index.md 需要手动创建
  Categories: /categories       #博客根目录/source/categories/index.md 需要手动创建

# Highlight theme
# ---------------
highlight_theme: default        #melody自带的代码高亮有5种：default darker pale night light ocean

# code_word_wrap: true or false
code_word_wrap: ture            #代码换行

top_img: true # false or url of img
top_img: https://xxxxx.jpg      #顶部图的外链

post_meta:
  date_type: updated            #created or updated 文章顶部日期是创建日或者更新日
  categories: true              #true or false 是否显示分类
  tags: true                    #true or false 是否显示标签

toc:
  enable: true                  #是否显示文章侧边栏的目录
  number: true                  #是否显示文章侧边栏目录的数字章节

post_copyright:                 #关于文章版权设置
  enable: true
  license: CC BY-NC-SA 4.0
  license_url: https://creativecommons.org/licenses/by-nc-sa/4.0/

# Post info settings
# ---------------
avatar: https://xxx.jpg         #头像外链

follow:                         #Follow Me 按钮
  enable: true
  url: 'http://ryoheizhao.github.io/'
  text: 'Follow Me'

# Footer Settings
# ---------------
since: 2019                     #博客年份

#footer_custom_text:            #自定义页脚，写一些备案信息、声明信息等
```

通过上述的配置，你的博客基本上成型了，之后我会再补充关于文章统计、评论留言等相关插件的安装和配置。


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

## 多台电脑管理博客

笔电再轻也有分量，像我平时又爱骑个单车，只能在 mbp 上写博客发博客就太束手束脚了，咋办？

`hexo d` 命令将生成的静态页面上传至 github 分支中，不如我们把 hexo 的工作目录也交给 git 托管，随时随地拉取和上传岂不美滋滋，下面我们一步步来实现。

### git 初始化 hexo 工作目录

**注意**这里有个前提，如果你在使用类似 melody next yilia 等主题时，要取消其主题目录的 git 跟踪：

```bash
cd 你的博客目录/themes/melody
rm -rf .git #或者把 .git 目录拷贝到别处再删除
```

接下来初始化 hexo 工作目录：

```bash
cd 你的博客目录/
git init
git checkout -b hexo
git add .
git commit -m 'git init'
```

提交到 github 仓库：

```bash
git remote add origin git@github.com:yourname/yourname.github.io.git
git push -u origin hexo:hexo
```

### 在另一台主机部署博客

拉取分支：（我是从 github 拉取到我的 mbp,以下的截图来自 Mac 终端）

```bash
git clone -b hexo git@github.com:yourname/yourname.github.io.git ~/my_blog
```

hexo 初始化：

最初我直接切换到博客目录并执行 `hexo init`, 结果报错,见下图：

![hexo_init](https://s2.ax1x.com/2019/07/03/ZY6dvF.png)

根据提示执行 `npm install hexo` 命令：

![install_hexo](https://s2.ax1x.com/2019/07/03/ZY60u4.png)

顺便把 npm 更新了： `npm install -g npm`

![update_npm](https://s2.ax1x.com/2019/07/03/ZY6agU.png)

安装依赖的包： `npm install`

![npm_install](https://s2.ax1x.com/2019/07/03/ZY6Db9.png)

安装 hexo 依赖的插件（包括 hexo-melody-theme 依赖的渲染插件）：

```bash
npm install hexo-deployer-git #安装部署插件
npm install hexo-renderer-jade hexo-renderer-stylus --save #安装 pug stylus 渲染插件
```

![hexo-plugin](https://s2.ax1x.com/2019/07/03/ZY6BDJ.png)

这样本地的博客写作环境便部署完成了。

### 写作步骤

- `git pull` 拉取最新的 hexo 分支
- `hexo new post 'title'` 新建文章
- `hexo clean` 清理之前的静态站文件
- `hexo g` 重新生成静态站文件
- `git add .`
- `git commit -m "message"`
- `git push` 推送到 hexo 分支
- `hexo d` 推送到 master 分支

## 参考

**感谢他们的付出和贡献**

[手把手教你从0开始搭建自己的个人博客 |无坑版视频教程| hexo](https://www.bilibili.com/video/av44544186?from=search&seid=7561138139683177649)
[Hexo多台电脑更新博客](mcbill.cn/2018/06/22/Hexo多台电脑更新博客/)
[【持续更新】最全Hexo博客搭建+主题优化+插件配置+常用操作+错误分析](https://juejin.im/post/5bebfe51e51d45332a456de0)
[为 win10 打造 Linux 终端（非 wsl）](https://juejin.im/post/5bd5a08cf265da0add520772)
[快速、简洁且高效的博客框架](https://hexo.io/zh-cn/)
[hexo-theme-melody](https://molunerfinn.com/hexo-theme-melody-doc/)