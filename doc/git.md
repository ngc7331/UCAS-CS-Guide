# Git

前置条件：对 Linux 以及命令行有一定基本的了解

## 目录
- [Git](#git)
  - [目录](#目录)
  - [是什么 & 为什么](#是什么--为什么)
  - [一个比喻](#一个比喻)
  - [基本概念](#基本概念)
    - [仓库 repository](#仓库-repository)
    - [提交 commit](#提交-commit)
  - [具体使用](#具体使用)
    - [创建仓库](#创建仓库)
      - [初始化`git init`](#初始化git-init)
      - [克隆远程仓库 `git clone`](#克隆远程仓库-git-clone)
    - [追踪 `git add`](#追踪-git-add)
    - [提交 `git commit`](#提交-git-commit)
    - [标签 `git tag`](#标签-git-tag)
  - [.gitignore](#gitignore)
  - [参考](#参考)



---
## 是什么 & 为什么
Git（读音为/gɪt/）是一个开源的分布式**版本控制系统**。由 Linux 之父 Linus Torvalds 开发。

- [什么是版本控制](https://www.git-tower.com/learn/git/ebook/cn/command-line/basics/what-is-version-control#start)

至于为什么要用 Git，一言以蔽之，它可以帮助你更好地**回溯到过去的版本**、**与同伴协作**、**辅助代码审计和调试**

- [为什么要使用版本控制系统？](https://www.git-tower.com/learn/git/ebook/cn/command-line/basics/why-use-version-control/)

另外强烈推荐一个在线可视化练习 Git 的页面 -> [Learn Git Branching](https://learngitbranching.js.org/?locale=zh_CN)，推荐的关卡有：
- 主要
  - 基础篇（全部）
  - 高级篇（全部）
  - 杂项
    - 4: Git tag
- 远程（全部）



---
## 一个比喻
尽管不太严谨，但在下文中，您可以使用一系列比喻来辅助理解 Git 相关的概念：
概念 | 比喻
--: | :--
用户（您）| 代码的生产者，一个工厂
追踪（add）| 告诉仓库有一个新的代码需要被存储
本地仓库（local）| 工厂的附属仓库
提交（commit）| 将生产好的代码存入本地仓库
远程仓库（remote）| 销售网点的仓库
远程上游仓库（upstream）| 用户生产代码过程中，所需工具的生产者仓库
推送到远程仓库（push）| 将生产的代码发送到网点
拉取远程上游仓库（pull）| 将更新的工具运输到本地仓库以供使用


---
## 基本概念
- [版本控制的基本工作流程](https://www.git-tower.com/learn/git/ebook/cn/command-line/basics/basic-workflow#start)
### 仓库 repository
有时简写作 repo

一个仓库是一个目录，包含了一个项目中所有可能需要被管理的文件和相应的元数据

仓库的根路径下有一个`.git`目录，其中包含了仓库的配置文件、版本控制信息等必要信息

### 提交 commit
提交时需要填写 commit message，默认第一行为标题，后续为详情

**每次提交都会生成一个40位的唯一 id，称为 commit hash**

当需要引用某一次 commit 时，例如针对某一次 commit 打标签时，都是使用 commit hash 来唯一地确定指定的 commit

例如在[本仓库的 commit 页面](https://github.com/ngc7331/UCAS-CS-Guide/commits/main)有这样的一次提交：

![image.png](https://s2.loli.net/2022/05/02/WcMIvyU1HPB8FLb.png)

蓝色箭头为标题，黄色箭头为详情，红色箭头为 commit hash 的前7位

它是通过执行
```
$ git commit -m "docs: 2022.04.01.1

- add more hint..."
```
产生的，注意这里用双引号包裹了整个 commit message，因此其中可以包含换行

- [工作在你的项目上](https://www.git-tower.com/learn/git/ebook/cn/command-line/basics/working-on-your-project#start)

---
## 具体使用
### 创建仓库
#### 初始化`git init`
在某一目录下使用`git init`便可将该目录初始化为一个仓库

示例
```
$ pwd
/home/ucas/workspace
$ ls -a
stupid_program.c
$ git init
$ ls -a
.git/       stupid_program.c
```
#### 克隆远程仓库 `git clone`
使用`git clone <url> [<path>]`可将 url 地址处的仓库克隆到本地的 path 路径下

path必须为一个空目录，若缺省，则为当前路径下与远端仓库同名的目录

示例：
克隆本仓库
```
$ git clone https://github.com/ngc7331/UCAS-CS-Guide
$ ls -a
UCAS-CS-Guide
$ cd UCAS-CS-Guide
$ ls -a
.git/       .github/        doc/        _config.yml     ......
```
计算机组成原理
```
$ git clone https://gitlab.<hidden>/ucas-cod-2022/<学号>.git COD-Lab
$ ls -a
COD-Lab
```

### 追踪 `git add`
使用`git add <path>`便可追踪 path，若 path 是一个目录，则追踪该目录下的**所有文件**

每次 commit 前都需要针对修改了的文件执行`git add`

一个比较偷懒的用法是在**仓库根目录**下执行`git add .`，它意味着追踪仓库内所有文件

亦可在执行`git commit`时使用`-a`选项，它将自动追踪仓库内所有文件

### 提交 `git commit`
使用`git commit -m "<commit message>"`便可以将当前仓库中所有**被追踪**的文件变动提交到仓库。

- [git commit 编写指南 \| 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)

### 标签 `git tag`
使用`git tag <tag name> [<commit hash>]`来为某一次commit打tag

当 commit hash 缺省时，默认为 HEAD，即当前分支上最新一次提交



---
## .gitignore
TODO



---
## 参考
- \*[Git 官方网站](https://git-scm.com/)
- [Learn Version Control with Git（有中文）](https://www.git-tower.com/learn/git/ebook/cn/command-line/introduction)
- [git 命令速查表](https://www.runoob.com/note/56524)
