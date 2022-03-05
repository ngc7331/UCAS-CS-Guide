一份面向UCAS本科计算机科学与技术专业同学的基础指南

（应该也适用于通识课程的计算机科学导论以及网络空间安全的同学）

Made with ❤️ by an ordinary UCASer

# 关于
## IDEA
~~你是否也曾烦恼于计算机科学导论、C 语言、计算机组成原理等课程？~~

~~你是否需要 Linux\Git\虚拟机等课上要用老师却不教的内容的指导？~~

~~快来阅读本指南吧！5毛一条，括号删掉~~

总而言之，由于我校的专业课没有正式讲授关于 Linux、虚拟机等内容，却要频繁使用它们完成课堂学习或者课后作业。同学们经常会遇到各种各样的问题，需要四处查找资料或向他人请教。

我作为一个有几年 Linux 使用经验的先飞笨鸟，希望能通过某些手段帮助同学们初步熟悉 Linux 系统、理解老师提供的命令、读懂报错并更有效地提问，因此斗胆创建了该仓库。

## 说明
- 因个人能力有限，本指南**不可避免的会存在疏漏乃至错误**，希望各位大佬包涵
- ***如果您发现了问题，或者有希望添加到指南中的内容，欢迎发送Discussion, Issue以及PR***
  * 什么是 Discussion / Issue / PR？请参考 [git](doc/git.md) 页面
- 本指南会大量结合我——作为一个 UCAS 计系学生——在课程中的实际经验进行编写。侧重于解释和解决实际同学产生的困惑和遇到的麻烦，不一定具有普适性
- 个人主要使用 Debian 系的发行版，对其它发行版不算熟悉，本文将基于 Ubuntu 进行编写，如果您熟悉其他发行版，欢迎补充。
  * 什么是 发行版 / Debian / Ubuntu？请参考 [linux](doc/linux/linux.md) 页面
- 本指南仍在施工中，请见[Project](https://github.com/ngc7331/UCAS-CS-Guide/projects/1)页面

## 如何使用
- 本指南可以在以下页面阅读：
  * 当前 [Github](https://github.com/ngc7331/UCAS-CS-Guide) 仓库
  * [Github Pages](https://ngc7331.github.io/ucas-cs-guide/)
  * [Gitee](https://gitee.com/xu_zh/UCAS-CS-Guide) 镜像仓库：国内可能访问较快，但更新可能滞后于 Github，采用 Github Actions 自动同步
- 请善用 `Ctrl+F` 搜索页面中的内容
- 参考资料会在各页面的底部给出，如有需要查阅更加详细的文档，请前往原始页面
- 代码块的含义：本文中可能会出现下面这样的“代码块”，其中
  * 以`$`开头的行表示用户输入的指令（不含`$`字符）
  * 以`#`开头的行表示用户以 root 用户输入的指令（不含`#`字符）
  * 以`//`后表示注释
  * 单独一行`...`表示省略一些行
  * 其它行表示计算机给出的输出
```
$ sudo apt update              // 执行 apt update 指令
Reading package lists... Done  // 计算机给出的输出
...                            // 这里行数很多，省略
# apt upgrade -y               // 以 root 用户执行 apt upgrade -y 指令
...
```
- 欢迎您把本指南分享给更多有需要的同学
- 希望这些信息能对您有所帮助

## 本指南不包含...
- 课程上正式讲授的内容，例如 C 语言、Verilog 教程
- 不常用的命令及参数

## 知识共享
- 本文采用[CC-BY-SA-4.0 协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)共享
- 本文开源在 Github 上

# 目录
## [Linux](doc/linux.md)
- Linux 系统、发行版简要介绍
- 图形化与命令行
- 与 windows 的不同
- 常用指令

### [安装软件](doc/linux/install_program.md)
- apt
- dpkg

### [编辑器](doc/editor.md)
- vi、vim
- nano
- gedit
- vscode

## [SSH](doc/ssh.md)
- 简介
- 使用 ssh 连接到虚拟机
- 在 vscode 中使用 ssh
- ssh-key

## [Git](doc/git.md)
- 简介
- 常用指令和参数
- Github

## [虚拟机](doc/VM.md)

## 课程实例
- [说明](doc/course_example/info.md)
- [程序设计基础与实验（C语言）](doc/course_example/c.md)
- [计算机组成原理](doc/course_example/COD.md)

## [应对错误](doc/problem.md)
- 常见错误及解决方案
- 如何初步读懂报错
- 如何有效向他人提问

## [学习更多](doc/more.md)
- 参考书单&在线文章&工具列表...

## 待续
