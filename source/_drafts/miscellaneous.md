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

## VS Code

### code runner

> command 'code-runner.run' not found

看看 vscode 的默认终端配置，或者 update 一下，不需要注销掉 `codeManger.js` 中的第 12 行代码。

贴一下我工位上的 win10 vscode 的 `settings.json` 文件：

```json
{
    "workbench.colorTheme": "Monokai Dimmed",
    "workbench.iconTheme": "material-icon-theme",
    "editor.renderIndentGuides": false,
    "editor.fontFamily": "'Sarasa Term SC',Consolas, 'Courier New', monospace",
    "python.pythonPath": "python3",
    "markdown-preview-enhanced.mermaidTheme": "default",
    "markdown-preview-enhanced.codeBlockTheme": "atom-material.css",
    "terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe",
    "terminal.integrated.shellArgs.windows": ["--login", "-i"],
    "code-runner.executorMap": {
        "python": "set PYTHONIOENCODING=utf-8 && python"
    }
}
```

其中，我将 git bash 设置为了默认终端：

```json
"terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe",
"terminal.integrated.shellArgs.windows": ["--login", "-i"],
```

code runner 执行 python 脚本出现中文乱码的解决办法：

- 使用 utf-8 编码
  ```json
  "code-runner.executorMap": {
        "python": "set PYTHONIOENCODING=utf-8 && python"
    }
  ```

- 输出到设置好的终端

  在 `settings.json` 中加入下面这句：
  ```json
  "code-runner.runInTerminal": true,
  "code-runner.executorMap": {
        "python": "python"
    }
  ```
