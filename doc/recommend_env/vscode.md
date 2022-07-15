# VSCode
VSCode 全称为 Visual Studio Code，是微软开发的开源、轻量、全功能的编辑器

VSCode 与 Visual Studio 不同，后者

- [官网](https://code.visualstudio.com/)
- [VSCode 教程 \| 极客教程](https://geek-docs.com/vscode/vscode-tutorials/what-is-vscode.html)

## 目录
- [VSCode](#vscode)
  - [目录](#目录)
  - [推荐理由](#推荐理由)
    - [通用](#通用)
    - [轻量化](#轻量化)
    - [上手容易](#上手容易)
    - [可扩展性强](#可扩展性强)
  - [安装](#安装)
    - [Windows](#windows)
    - [Ubuntu / Debian](#ubuntu--debian)
  - [扩展](#扩展)
  - [文件夹与工作区](#文件夹与工作区)
  - [SSH / WSL](#ssh--wsl)
  - [Git](#git)



---
## 推荐理由
### 通用
其一，VSCode 是一个全平台的编辑器，你可以在 Windows / Linux / MacOS 上使用它。甚至有组织开发了可以自部署的[浏览器版 VSCode](https://github.com/coder/code-server)，以供手机和平板使用。多设备间可通过微软账户自动同步设置和插件，非常方便。

其二，得益于丰富的插件，VSCode 几乎可以用于任意一种语言的开发，只需要准备好相应的后端开发环境、安装插件并进行简单的配置。

其三，利用 WSL 和 SSH 插件，VSCode 可以连接到远程主机进行开发，当需要在多台设备上开发同一个项目时，只需配置好远程连接，而无需重复配置虚拟机等后端环境。

### 轻量化
VSCode 对硬件的要求不高。[系统需求](https://code.visualstudio.com/docs/supporting/requirements)中要求仅有：
- 磁盘 > 500MB
- 内存 ~ 1GB
- CPU ~ 1.6GHz

在实际体验中，安装了33个插件以后，VSCode 比起 Visual Studio 和 Jetbrain 全家桶等 IDE，在启动、响应速度和电脑资源占用方面仍然好上不少，即使是我手边这台8年前的 Surface Pro 3 (i5) 也能无压力运行，实际内存占用~200MB

### 上手容易
VSCode 的界面和最基本的编辑功能就和我们熟悉的 Word 或者记事本相似，要开始使用并进行简单的编程是非常容易的。

### 可扩展性强
如前所述，VSCode 有丰富的插件，不仅可以对各种语言提供基本的语法高亮、语法检查等支持，还可以提供括号配对（新版本已内置）、Docker 容器可视化管理、http 服务器（辅助 web 开发）、Github Copilot 人工智能辅助编程等等诸多个性化功能。

在熟悉以后，用户甚至可以自行开发插件并发布到插件市场供人使用。



---
## 安装
### Windows
在[官网](https://code.visualstudio.com/)下载`.exe`安装包后安装即可

### Ubuntu / Debian
注意：直接在 Linux 系统下使用图形化的 vscode 并不是推荐的做法，见[图形化与命令行](../linux/linux.md#图形化与命令行)。更推荐的使用方式是使用 Windows 下的 vscode 通过 SSH 连接到 Linux 系统，见 [SSH](#ssh--wsl) 一节。

在[官网](https://code.visualstudio.com/)下载`.deb`安装包，在终端中找到安装包所在路径，随后用`dpkg -i <filename>.deb`安装即可

参考[安装程序-dpkg](../linux/install_program.md#dpkg)一节

参考命令如下

```
$ wget "https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64" -O vscode.deb
$ sudo dpkg -i vscode.deb
```



---
## 扩展
如上面提到的，vscode 的一个强项就是丰富的扩展系统。

打开vscode，可以在左侧找到扩展选单👉![扩展界面图标](https://s2.loli.net/2022/07/10/pCM1hRsQ2djc6Go.png)

也可以按下 `Ctrl + Shift + X`开启

推荐安装的通用扩展列表如下：（如有更多推荐，欢迎 issue / PR）
- [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) 套件：包括 SSH、WSL和容器扩展
- [简体中文](https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-zh-hans) 语言扩展
- [Trailing Spaces](https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-spaces)：高亮行尾多余空格
- ...

语言类的插件如下：
- [C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
- [Go](https://marketplace.visualstudio.com/items?itemName=golang.Go)
- [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  * [Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance)
- [Verilog](https://marketplace.visualstudio.com/items?itemName=mshr-h.VerilogHDL)
- ...

注意：这些语言类扩展只是在编辑器的层面上提供语法高亮、语法检查、调试等功能支持，不能替代实际的开发环境、编译器。例如要使用 vscode 进行 c 语言开发，首先需[通过 apt 安装](../linux/install_program.md#apt) `gcc`和`gdb`。



---
## 文件夹与工作区
vscode 的一个重要概念：工作区 (workspace)。

利用工作区，可以为不同的项目进行不同的配置，便于管理和使用

- [关于VSCode中工作区的讲解与使用工作区 \| 知乎](https://zhuanlan.zhihu.com/p/54770077)

即使不使用工作区，也建议使用“打开文件夹”来替代打开单独的文件

这样做的好处有：
- 可以使用左侧的“资源管理器”菜单更加方便地管理该目录下的文件👉![image.png](https://s2.loli.net/2022/07/10/kWIcKP2JdBXV7LG.png)
- 告诉 vscode 现在的工作路径，在 vscode 中打开终端或执行与路径有关的指令（例如调试 python 脚本或编译 c 代码）或默认在该文件夹中进行，而不是使用用户目录。[工作目录](../linux/linux.md#工作目录)
- 若该文件夹下是一个 [git 仓库](../git.md)，可以在左侧的“源代码管理”菜单直接查看和使用 git👉![image.png](https://s2.loli.net/2022/07/10/8zlrthMknXcATqU.png)

![image.png](https://s2.loli.net/2022/07/10/ENga3eVx4wCjmJK.png)
> 上边是打开了名为“汇编语言”的文件夹，下图是一个普通窗口



---
## SSH / WSL
在安装了[Remote Development 扩展](#扩展)后，可以使用左侧的“远程资源管理器”配置和连接 SSH 或 WSL👉![image.png](https://s2.loli.net/2022/07/10/VsjRDLUgF5NQn8q.png)

![image.png](https://s2.loli.net/2022/07/10/8F4PKqyzI7ciVXO.png)
> 可以在菜单顶部切换 SSH 与 WSL

对于 WSL，只要[安装了 WSL](./wsl.md)，即可在菜单中看到资源

对于 SSH，可以点击`SSH TARGETS`右侧的加号，只要在弹出窗口中输入 [ssh 连接的命令](../ssh.md#使用-ssh-连接到远程主机)即可添加资源。亦可点击右侧的齿轮，选择一个配置文件后[手动配置](../ssh.md#sshconfig-文件及多身份)

![image.png](https://s2.loli.net/2022/07/10/8wMYUAza95ZfJ2y.png)
> 弹出窗口

添加完成后，可以看到这样的资源，点击右侧的图标，在弹出窗口输入密码即可连接。
![image.png](https://s2.loli.net/2022/07/10/FPMVDQa4OwbLySj.png)

若需要免密登录，请[配置 ssh-key](../ssh.md#设置-ssh-key-实现免密登陆)



---
## Git
- [VS Code里使用Git和关联GitHub \| Bilibili](https://www.bilibili.com/video/BV1r3411F7kn) 17'54"

TODO
