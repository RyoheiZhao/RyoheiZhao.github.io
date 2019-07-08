---
title: 杂七杂八的问题
categories:
    - 随笔记录
tags:
    - 杂项
---

## 前提环境

### 硬件环境

在公司：win10 + wsl(ubuntu)

在家：MacOS

## 杂七杂八的问题汇总

### vim 打开文件出现 ^M 符号

起因是我在 win10 的 git 仓库下 `git pull`, 切换到 wsl 下对应的目录显示有文件被修改需要提交，问题是我并没有对任何文件做修改啊。

最后发现原因，由于 Windows 的换行符是 `\n\r`,Linux 换行符是 `\n`, 当你在 Linux 下用 vim 打开在 Windows 编辑过的文件时会出现这种情况，虽然实际编辑运行不受影响，但多少会影响文件阅读，或是 git 总是提示文件被修改这样的状况，如何解决该问题呢？

列出几个办法，其原理都是将 ^M 替换掉

- ubuntu 安装tofrdos
    ```bash
    sudo apt-get install tofrdos      #安装 tofrdos
    todos filename                    #转化为 dos
    fromdos filename                  #转化为 unix
    ```
- vi 正则表达式替换
    ```bash
    vi filename
    g/^M/s/^M//
    %s/^M//g
    #以上两条命令都可以实现
    ```