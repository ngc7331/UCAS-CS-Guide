# 编辑器

![镇楼图](https://s2.loli.net/2022/03/11/PDokvlYhx5mtVQ1.png)
> 镇楼图：广为流传的《常见编辑器学习难度曲线》（有调侃的意思，仅作参考）

“编辑器” 和 “IDE”（Integrated Development Environment，集成开发环境）的概念有区别，前者是后者的组成部分。但本节中将忽略这些差异，着重介绍我们最长接触、最需要了解的、前端的编辑器部分。

本节中只会涉及到 Linux 环境下常用编辑器的基本使用。

除 vscode 外，本节中的软件包均可[通过`apt`安装](install_program.md#apt)

一个玩笑：[为什么Word是最好用的编程IDE \| Joma Tech](https://www.bilibili.com/video/BV11R4y1W7Wk)

## 目录
- [编辑器](#编辑器)
  - [目录](#目录)
  - [nano](#nano)
  - [vi / vim](#vi--vim)
  - [gedit](#gedit)
  - [vscode](#vscode)

## nano
- 使用：`nano <path>`
- 若`path`指定的文件不存在，在**保存时**自动创建
- 进入后可直接对文件进行修改
- 页面底部有如下的使用提示，其中`^`代表`Ctrl`键，`M-`代表`Alt`键：
```
^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos     M-U Undo       M-A Mark Text
^X Exit        ^R Read File   ^\ Replace     ^U Paste Text  ^T To Spell    ^_ Go To Line  M-E Redo       M-6 Copy Text
```
- `Ctrl + O`保存文件。会在页面底部要求输入文件名，默认与`path`一致，回车即可保存
- `Ctrl + X`退出。如果有未保存的修改，会在页面底部询问是否保存，按下`N`直接退出
- [nano \| Linux 命令大全](https://ipcmen.com/nano)

## vi / vim
以前`vi`和`vim`有一些差异，但一般涉及不到。现在`vi`似乎是`vim`的别名，故本节不提及。

vim 是一个功能非常强大的 IDE，支持插件等定制化使用，但正如头图展示的那样，它入手难度相对较高，本节中只会涉及最最最最基础的概念，及编辑、保存、退出的方法。

- 使用：`vi <path>`、`vim <path>`
- 若`path`指定的文件不存在，在**保存时**自动创建
- vi 有三个模式：一般模式（命令模式），编辑模式，底线命令模式。刚进入 vi 界面时为一般模式，切换方式如下：
  * 在一般模式下，按`i`键进入编辑模式，此时左下角将变为`-- INSERT --`
  * 在编辑模式下，按`Esc`键进入一般模式
  * 在一般模式下，按`:`键（即`Shift + ;`）进入底线命令模式，此时左下角将出现一个`:`
  * 在底线命令模式下，输入任意命令并按回车后自动回到一般模式，删除掉`:`也会回到一般模式
  * 注：若无法输入`:`请检查输入法是否为中文，只有**英文半角冒号**才能进入底线命令模式
![工作模式切换示意图](https://www.runoob.com/wp-content/uploads/2014/07/vim-vi-workmodel.png)
- 在一般模式下，按`dd`键（即按两次`d`键）删除光标所在行；按`x`键删除光标所在字符
- 在**编辑模式下才可**直接对文件进行修改
- 在底线命令模式下
  * 输入`w`（在左下角显示为`:w`）并按回车保存文件
  * 输入`q`并按回车退出。如果有未保存的修改，将报错`E37: No write since last change (add ! to override)`，使用`q!`强制退出
  * 注：底线命令模式大部分命令可连用，如`wq`表示写入并退出
- [vi / vim \| 菜鸟教程](https://www.runoob.com/linux/linux-vim.html)
- [Learn-Vim(the Smart Way) 中文翻译](https://github.com/wsdjeg/Learn-Vim_zh_cn)

## gedit
**不推荐使用**，见[图形化与命令行](./linux.md#图形化与命令行)
- 使用：`gedit <path>`或在图形化界面中直接启动
- 与 Windows 下记事本类似

## vscode
见 [vscode](../recommend_env/vscode.md) 一节
