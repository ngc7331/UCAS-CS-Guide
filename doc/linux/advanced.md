# 进阶内容

## 目录
- [进阶内容](#进阶内容)
  - [目录](#目录)
  - [terminal & shell](#terminal--shell)
    - [shell 脚本](#shell-脚本)
  - [dotfiles](#dotfiles)
    - [.bashrc](#bashrc)



---
## terminal & shell
在此之前，我们刻意忽略了终端（terminal）和 shell 的关系，将它们统称为“终端”、“命令行”

但事实上，终端最初是一种硬件，用于和计算机交换信息的输入输出设备，例如电传打印机。而随着计算机系统硬件一体化水平提高，如今的终端通常指“终端模拟器”，是一类用于人机交互的软件，例如
- vscode 下方菜单中的终端
- Ubuntu 自带 Gnome 桌面环境中的 Gnome-terminal（右键文件夹）
- Xshell、MobaXterm 等
- ...

不同终端之间可能有界面、字体、字符集等不同，但若已经到了可以输入命令使用系统的阶段，终端间的差异基本就影响不大了

而 shell 则是实质去解释指令、执行指令、返回执行结果的软件，常见的有
- sh
- ash
- bash（最常见，是 Ubuntu、Debian 等许多发行版的默认 shell）
- zsh
- ...

不同 shell 间可能有内建命令、默认配置文件等不同，因此需要留意使用的 shell

可以通过`SHELL`环境变量来检查当前运行的 shell，即`echo $SHELL`

关于物理设备、终端（模拟器）、shell 的一个不太严谨的关系示意图如下

![terminal--shell-1.drawio.png](https://s2.loli.net/2022/08/03/3RLJc6QAIwMZz7u.png)

### shell 脚本
TODO

---
## dotfiles
在如今很多 Linux 发行版中，用户主目录`~`下经常存在`.profile`、`.bashrc`（bash 环境）、`.zshrc`（zsh 环境）一类的文件

这些文件以`.`开头（表示为 Linux 系统中的隐藏文件，在`ls`时需使用`-a`参数才能列出），因而得名 "dotfile" （直译为“点文件”）

它们的作用是保存一些开发环境和工具的配置信息，由对应的环境或工具在启动时自动读取或执行

### .bashrc
以`.bashrc`为例，它是 shell 程序 bash 的配置文件，或者说是启动脚本（即程序启动时自动执行的 shell 脚本），主要功能有：
- 定义 prompt（提示信息，例如每行开头的`user@host:path$`）的格式
- 定义环境变量
- 定义别名
- ...

对这类 shell 配置文件而言，最常见的用途便是[定义别名](command.md#别名-alias)，如 Ubuntu 20.04 LTS 系统自带的`.bashrc`中
```
$ cat ~/.bashrc
...
# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
...
```
就定义了三个与列出目录文件`ls`命令有关的别名，简化了参数的使用

若需定义更多别名，也只需这样在文件任意位置（建议放在一起便于管理）添加一行`alias name="cmd"`即可

- [什么是 .bashrc，为什么要编辑 .bashrc？ \| Linux 中国](https://zhuanlan.zhihu.com/p/33546077)
