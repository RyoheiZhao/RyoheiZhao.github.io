---
title: git 学习笔记 & 常见问题
categories:
    - 笔记总结
tags:
    - git
---


## 常见问题

### git pull 提示错误

**情景**：你与他人同时编辑了一个文件，且对方已经提交了修改，此时你执行 `git pull` 进行拉取时，git 给出提示：

> your local changes to the following files would be overwritten by merge ...

**解决办法**：

- 两个人的修改都要保留，那么采取合并的办法：
    ```bash
    git stash                   #暂存你当前正在进行的编辑
    git pull origin <分支名>    #拉取远端代码
    git stash pop               #将暂存代码合并进去
    ```
- 覆盖你的代码，直接拉取：
    ```bash
    git reset --hard            #你本地的git库回滚到上一个版本
    git pull origin <分支名>    #拉取远端代码
    ```