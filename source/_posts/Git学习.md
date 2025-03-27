---
title: git 常用命令
categories: 计算机技术    
---

列举了一些常用的 git 命令和自己遇到的一些问题

<!---more--->

## 本地修改了，但是无法推送到远程

明明本地修改了，但是远程仓库却提示是干净的。需要使用

```shell
git rm -r --cached [你的项目]
```

强制删除 git 缓存后就好了

## git 分支管理

### 创建分支

```shell
git branch [new_branch]
```

### 切换分支

```shell
git checkout [new_branch]
```

### 合并分支

先切换到主分支上,执行

```shell
git merge [your_want_branch]
```

这样你选的分支就会合并到你当前所在的分支上了

### 合并分支的另一种方式 rebase

这个也是一种方式,效果是把你的修改"复制"到另一个分支上,看起来代码是线性提交的(实际上是平行工作的)

先切换到你的其他分支比如`bugFix`,执行下面的命令,你在`bugFix`上的修改就会到`main`分支上了

```shell
git rebase main
```
