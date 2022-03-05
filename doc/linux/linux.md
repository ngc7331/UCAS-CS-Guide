# linux
想必大家都不乐意阅读冗长而没什么用的介绍。因此本文将尽可能精简，更多的资料将以

- [这样的补充阅读链接]()

的形式附在每节正文的末尾。**其中标有\*的内容为英文资料。**

## 目录
- [linux](#linux)
  - [目录](#目录)
  - [Linux 系统、发行版简要介绍](#linux-系统发行版简要介绍)
    - [什么是 Linux](#什么是-linux)
    - [什么是发行版](#什么是发行版)
    - [Ubuntu](#ubuntu)
  - [重要概念](#重要概念)
    - [权限](#权限)
      - [用户权限](#用户权限)
      - [文件权限](#文件权限)
    - [文件系统](#文件系统)
      - [绝对路径](#绝对路径)
      - [相对路径](#相对路径)
      - [工作目录](#工作目录)
      - [用户目录](#用户目录)
    - [回显](#回显)
  - [图形化与命令行](#图形化与命令行)
    - [读懂终端](#读懂终端)
  - [与 windows 的不同](#与-windows-的不同)
    - [快捷键](#快捷键)
  - [常用指令](#常用指令)
    - [细节](#细节)

## Linux 系统、发行版简要介绍
### 什么是 Linux
与常见的 Windows、MacOS 相同，Linux 也是一个操作系统，一般用于开发和服务器领域。

- \*[What is Linux?](https://www.linux.com/what-is-linux/)
- [Linux 简介 | 菜鸟教程](https://www.runoob.com/linux/linux-intro.html)

### 什么是发行版
Linux 的内核非常小，功能也不多。为了便于人们使用，有许多组织在它的基础上修改和新增了大量软件包并重新发布。这些版本仍属于 Linux，称为 Linux 发行版（Distribution）。

亦有些发行版是基于其它发行版进一步修改得到的。

类比安卓手机，谷歌推出了基础的 Android 系统，而各大手机厂家为了满足本地化的用户需求对它进行了修改。产生了诸如小米 MIUI，华为 EMUI，OPPO ColorOS等衍生版本。

常见的发行版有 Debian、Ubuntu、Arch、Mint、RedHat 等
![常见发行版汇总图](https://www.runoob.com/wp-content/uploads/2014/06/wKioL1bvVPWAu7hqAAEyirVUn3c446.jpg-wh_651x-s_3197843091.jpg)
> 图源：菜鸟教程

而在我们UCAS的课程中，最常使用的发行版为 Ubuntu，因此本指南将以它作为基础。

不同发行版之间可能存在差异，例如 Debian/Ubuntu 采用`apt`作为软件包管理器，而 RedHat 采用`rpm`。因此在使用别人的代码时应留意发行版是否一致，在向他人求助时也请主动提供您的发行版信息。

- \*[Distrowatch](https://distrowatch.com/)：发行版列表及榜单
- \*[Debian is the rock on which Ubuntu is built](https://ubuntu.com/community/debian)：Ubuntu 官网对 Ubuntu 和 Debian 关系的介绍

### Ubuntu
Ubuntu 是由 Canonical Ltd. 构建的 Linux 发行版，拥有桌面版、服务器版、物联网版等分支。

Ubuntu 每六个月（每年的4月和10月）发布一个新版本，每两年发布一个长期支持（LTS）版，版本号标识为
```
Ubuntu yy.mm.x
```
例如`Ubuntu 20.04.3 LTS`表示为2020年04月发布的 LTS 版本的第三次小更新

不同版本间的 Ubuntu 亦可能存在差异，请留意。

- [Ubuntu 中文官网](https://cn.ubuntu.com/)
- \*[What is an Ubuntu LTS release?](https://ubuntu.com/blog/what-is-an-ubuntu-lts-release)



## 重要概念
### 权限
简单来说，Linux 下有两类权限：
#### 用户权限
- root 权限：系统最高权限，甚至可以把系统本身删掉。当做出某些敏感操作（安装软件，修改系统配置等）时需要 root 权限
- root 用户：任何 Linux 系统中都有且仅有一个 root 用户，具有 root 权限，处于安全考虑，请避免直接使用该用户
- 普通用户：平时不具有 root 权限，需要使用时可通过`sudo`临时获取，详见[常用指令](#常用指令)
- 用户组：为方便管理，把需要相同权限的多个用户加入一个组

#### 文件权限
文件有3类权限：读、写、执行，一般以1位8进制数或者包含{w, r, x, -}的字符串表示

Note：这三个权限彼此独立，可以做到一个用户能写入和执行而不能读取

一个文件针对用户、用户组、任何人可以有不同的权限，因此总共需要3位8进制数或者三组{w, r, x, -}表示权限

此外还需要标注文件是否为目录，用1位2进制表示

例如，通过`ls -al`查看详细文件列表时：
```
$ ls -al
total 12
drwxrwxr-x 3 ubuntu ubuntu 4096 3月   5 15:16 ./
drwxr-xr-x 3 ubuntu ubuntu 4096 3月   5 14:45 ../
drwxrwxr-x 2 ubuntu ubuntu 4096 3月   5 15:15 some-dir/
-rw-rw-r-- 1 ubuntu ubuntu    0 3月   5 15:16 some-file.txt
-rwxrwxr-x 1 ubuntu ubuntu    0 3月   5 15:16 some-script.sh*
-rw------- 1 ubuntu ubuntu    0 3月   5 15:16 some-secret-file.txt
```
可以看到每行开头有一串字符串：
- 第三行`drwxrwxr-x`开头的`d`表示这是一个目录，随后的第一组`rwx`表示本用户具有读、写、执行权限，第二组`rwx`表示本**用户组**也具有这三个权限，第三组`r-x`表示任何人只具有读、执行权限而不具有写权限
- 最后一行`-rw-------`开头的`-`表示这不是目录，随后的`rw-`表示本用户具有读、写权限，后面的两组`---`表示本用户组和任何人不具有任何权限

再比如，通过`chmod`变更文件权限：
```
$ ls -al
-rw-rw-r-- 1 ubuntu ubuntu    0 3月   5 15:16 some-script.sh
$ chmod +x some-script.sh
$ ls -al
-rwxrwxr-x 1 ubuntu ubuntu    0 3月   5 15:16 some-script.sh*
$ chmod 750 some-script.sh
-rwxr-x--- 1 ubuntu ubuntu    0 3月   5 15:16 some-script.sh*
```
- `chmod +x`表示为该文件增加可执行权限，类似的有`+r`、`+w`表示增加读、写权限，也有`-r`、`-w`、`-x`表示撤销这三个权限
- `chmod 750`则是使用了3位8进制数表达，让我们写成二进制：`111 101 000`，于是与三组`rwx`的记法一一对应：本用户`111=rwx`，本用户组`101=r-x`，任何人`000=---`

### 文件系统
#### 绝对路径
以根目录`/`开始的路径

Note：`/`有两种含义，一是根目录（必须是最开头的字符），二是表示层级关系，例如
```
/home/ubuntu/some-file.txt
```
中的第一个`/`表示根目录，后面的两个`/`则表示`ubuntu`在`home`中、`some-file.txt`在`ubuntu`中

#### 相对路径
假设当前工作目录的绝对路径为`/home/ubuntu`
1. `some-file.txt`为当前目录下叫`some-file.txt`的文件，其绝对路径为`/home/ubuntu/some-file.txt`
2. `.`表示当前目录，因此`./some-file.txt`同上
3. `..`表示当前目录的上级目录，因此`../ubuntu/some-file.txt`同上
4. 没有`.../`的表示方法！如果要表示上级的上级，应该使用`../../`

#### 工作目录
Linux 下有些命令与目录无关，如`apt update`，但大部分命令均与目录有关，因此请务必留心您所处的工作目录。

查看当前工作目录有两个方法：
1. 直接从终端信息中得到，参考[读懂终端](#读懂终端)一节
2. 使用`pwd`指令获得

#### 用户目录
每个用户有一个默认的登录目录，称为用户目录，一般是`/home/<用户名>`，例如`ubuntu`用户的用户目录为`/home/ubuntu`

用户目录用`~`表示，因此在上例中`~/some-file.txt`和`/home/ubuntu/some-file.txt`是同一个文件

Review：`~/`也是一种相对路径记法

### 回显
当我们进行输入时，通常屏幕上会即时显示出我们输入的内容，称为回显

即使是输入密码等敏感字段，也会有诸如`***`和`···`的回显证明确实输入进去了

而在 Linux 终端中输入密码等敏感字段时，通常屏幕上**不会有任何回显**，按下回车后才能看到相应的结果（成功执行或密码错误等）

请放心大胆地完成输入


## 图形化与命令行
尽管大部分 Linux 发行版都可以像 Windows 一样使用支持鼠标的图形化界面，主流的在 Linux 上进行开发的方式还是使用纯键盘的命令行模式。主要理由如下：
- 图形化界面比较占用资源
- 做开发时大部分时间只需要打开编辑器、使用键盘输入代码，没有必要使用图形化界面
- 有些软件的图形化界面支持并不稳定，会产生预期以外的错误
- 有时开发是在远程服务器上进行的，传输图形化界面对网络要求高

我校课程对于 Linux 的使用也通常只需要命令行即可完成，例如程序设计基础与实验（C 语言）课程的武成岗老师班，要在 Linux 中完成的操作有：
1. 编辑代码
2. 使用`gcc`编译
3. 运行并查看结果

后两步都是纯命令行操作，第一步大家的做法不尽相同，有使用`gedit`/`vscode`这类图形化界面的，亦有使用`vi`/`nano`的，关于这些编辑器更详细的介绍和对比请看[编辑器](editor.md)页面。

特别的，`vscode`虽然是一个图形化程序，但可以使用 Windows 下的 `vscode` 通过插件和`ssh`连接到 Linux 系统，从而避免使用 Linux 的图形化界面。个人也是**强烈推荐**这个方式，详见[编辑器](editor.md)页面。

综上，本指南在大多数情况下也只会涉及到命令行（终端）的部分，希望大家能适应这种模式。

### 读懂终端
打开终端后，您可能会见到类似下面这样的字符：
```
ubuntu@server-7anw4ytc:~$
```
其格式为
```
用户名@主机名:工作目录$
```
下面逐项解释：
- 用户名：当前登录到系统使用的用户名称
- `@`：和邮箱中的`user@site.domain`作用相似，某个主机下的某个用户
- 主机名：当前系统的主机名（hostname）
- 工作目录：当前所处的目录，记法请见[文件系统](#文件系统)一节
- `$`/`#`：前者表示当前用户**不是** root，后者则表示是 root



## 与 windows 的不同
### 快捷键
非常重要的一件事，Windows 下的快捷键大多数对 Linux（终端）不起作用：
- 在`vi`中`Ctrl + S`不能保存文件，`:w`才可以
- 在`nano`中`Ctrl + S`也不能保存文件，`Ctrl + W`才可以

有些甚至会造成事故：
- 复制的快捷键`Ctrl + C`在终端中表示发送一个`SIGINT`信号，简单来说代表强制终止当前执行的命令。

此外，Linux 下的快捷键与终端软件、发行版等很多因素有关，因此请自行查找您使用的 Linux 环境下快捷键的说明。

建议您在熟悉 Linux 环境前适当减少快捷键的使用。

- [Linux 中的信号](http://c.biancheng.net/view/3482.html)



## 常用指令
**TODO 未完成**
| 指令 | 记忆 | 释义 | 用法 | 输出 |
| --- | ---- | ---- | ---- | --- |
| `cd` | **C**hange **D**irectory | 变更当前目录到指定目录 | `cd <目录名>` | 无 |
| `pwd` | **P**rint **W**ork **D**irectory | 输出当前工作目录 | `pwd` | 当前工作目录 |
| `sudo` | | 以系统管理者权限执行指令 | `sudo <指令>` | 即指令的输出|

- [Linux 命令大全](https://www.runoob.com/linux/linux-command-manual.html)

### 细节
- 短期内初次使用`sudo`时会要求输入**当前用户**的密码，输入无[回显](#回显)