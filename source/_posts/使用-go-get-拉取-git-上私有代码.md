---
title: 使用 go get 拉取 git 上私有代码
date: 2018-09-02 16:07:06
tags:
- go
- git
categories: 踩的坑
---

> 在 **go** 项目开发过程中，我们经常使用 **go get** 命令拉取 git 上的代码。
>
> **go get** 实际上也是使用 **git clone**，但会根据项目在 **git** 的目录，在本机**GOPATH** 路径下生成一样的目录。对于使用内部 **git** 的同学，**go get** 可以保持本地仓库与远程仓库 **import** 的绝对路径一致*（相对 **GOPATH** 的路径）*，方便管理 。
>
> **go get** 的默认方式是 **https** ，当我们需要 **go get** 一个私有项目的时候，无法输入账号密码。这时是不能成功拉取到代码的。



## 将 **http** 变为 **ssh**

首先想到的解决方法是将 **https** 变为 **ssh**。

以 **github** 为例。打开 *~/.gitconfig* 目录，添加：

```
[url "git@github.com"]

insteadOf = https://github.com
```

保存后，执行 **go get** 时会将 **git clone** 地址的 https://github.com 部分变为 git@github.com 。

对于 **github** 这种默认 **ssh** 端口为22的仓库，这种方法是可行的；但对于 **ssh** 端口非22的仓库，这种方法是行不通的。



## 保存 **https** 账号密码

于是还是回到原点，选择 **https** 方式。

**https** 方式每次都要输入密码，可以执行以下命令来长期储存账号密码：

```
git config --global credential.helper store
```

这里以 **github** 为例。

设置完成后，我们只需要执行一次下面的命令，拉取你的仓库中任意项目。

```
git clone http://yourname:password@github.com/any/repo/path.git
```

之后再使用 **go get** 就可以无需账号密码了。



*参考：https://gitee.com/oschina/git-osc/issues/2586*