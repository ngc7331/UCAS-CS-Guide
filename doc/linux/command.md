# 常用指令
下面只列出最基础的用法，如需了解指令的更多参数和用法请自行查找资料

用中括号`[]`包裹的参数为可选参数

一般而言，可以用`man cmd`或`cmd --help`或`cmd -h`查看关于命令`cmd`的说明和帮助

俗话说：“没有消息就是最好的消息”，Linux 下很多指令**成功执行时都没有输出**，只要确认输入正确，就无需担心运行结果是否正确。如仍需校验结果，常常需要使用不同指令辅助。

- [Linux 命令大全 \| 菜鸟教程](https://www.runoob.com/linux/linux-command-manual.html)



## 目录
- [常用指令](#常用指令)
  - [目录](#目录)
  - [切换当前工作目录 `cd`](#切换当前工作目录-cd)
  - [输出当前工作目录 `pwd`](#输出当前工作目录-pwd)
  - [列出目录文件](#列出目录文件)
    - [`ls`](#ls)
    - [`tree`](#tree)
  - [创建文件 `touch`、创建目录 `mkdir`](#创建文件-touch创建目录-mkdir)
  - [编辑文件](#编辑文件)
  - [移动 `mv`、复制`cp`](#移动-mv复制cp)
  - [删除](#删除)
    - [删除空目录 `rmdir`](#删除空目录-rmdir)
    - [删除文件 `rm`](#删除文件-rm)
  - [原样输出 `echo`](#原样输出-echo)
  - [输出文件](#输出文件)
    - [输出全文 `cat`](#输出全文-cat)
    - [输出头行 `head`、输出尾行 `tail`](#输出头行-head输出尾行-tail)
    - [输出差异 `diff`](#输出差异-diff)
  - [文本匹配和筛选 `grep`](#文本匹配和筛选-grep)
  - [更改文件权限 `chmod`](#更改文件权限-chmod)
  - [更改文件所有者 `chown`](#更改文件所有者-chown)
  - [用户手册 `man`](#用户手册-man)
  - [切换用户 `su`](#切换用户-su)
  - [`sudo`](#sudo)
  - [特殊符号](#特殊符号)
    - [与 `&&`、或 `||`、`;`](#与-或-)
    - [管道符 `|`](#管道符-)
    - [输出重定向符 `>`、 `>>`](#输出重定向符--)
    - [多行输入` \ `](#多行输入)
  - [安装软件](#安装软件)
  - [`ssh`](#ssh)
  - [`git`](#git)
  - [退出 `exit`](#退出-exit)
  - [电源管理](#电源管理)
    - [`shutdown`](#shutdown)
    - [`reboot`](#reboot)



---
## 切换当前工作目录 `cd`
- 用法：`cd [<path>]`
- Change Directory，切换当前工作目录至`path`，或者说移至（打开）`path`
- `path`可以使用绝对路径或相对路径表示
- 特殊用法：
  * `path`缺省时默认为用户目录`~`，即`cd`等价于`cd ~`
  * `cd -`：回到上个访问的目录

## 输出当前工作目录 `pwd`
- 用法：`pwd`
- Print Work Directory，输出当前工作目录的绝对路径

## 列出目录文件
### `ls`
- 用法：`ls [-a] [-l] [<path>]`
- 列出`path`下的所有文件（夹）
- `path`缺省时默认为当前目录`.`，即`ls`等价于`ls .`
- 使用`-a`时列出包括隐藏文件（`.`开头的文件）
- 使用`-l`时列出包括文件权限、大小、创建时间在内的详细信息（类比 Windows 资源管理器从列表模式改成详细信息模式）

### `tree`
- 用法：`tree <path>`
- 列出`path`下的所有文件（夹）和所有子文件（夹），用文件树的形式输出
- `path`默认为当前目录`.`，即`ls`等价于`ls .`

## 创建文件 `touch`、创建目录 `mkdir`
- 用法：`touch <name>`、`mkdir <name>`
- 创建一个叫`name`的空文件/空目录

## 编辑文件
- 见[编辑器](editor.md)一节

## 移动 `mv`、复制`cp`
- 用法：`mv <src> <dst>`、`cp <src> <dst>`
- 将文件从`src`移动/复制到`dst`
- `dst`可以为
  * 一个已存在的目录，此时会把`src`移动/复制到该目录下
  * 一个不存在的文件/目录，此时`src`必须为一个文件/目录
- `src`可以是多个文件，此时`dst`必须为一个已存在的目录

## 删除
### 删除空目录 `rmdir`
- 用法：`rmdir <dir>`
- 将空目录`dir`删除，若非空则报错
### 删除文件 `rm`
- 用法：`rm [-R] <path>`
- 删除`path`指定的文件
- 使用`-R`时采用递归删除的方式，删除`path`指定的目录和其中所有的子目录/文件。不使用`-R`而`path`为目录时报错

## 原样输出 `echo`
- 用法：`echo <text>`
- 输出`text`
- 常用于脚本中（类比 c 语言 printf 使用）或配合[输出重定向符](#输出重定向符)使用

## 输出文件
### 输出全文 `cat`
- 用法：`cat <file>`
- 输出`file`的内容
- Note：当文件过大时可能造成卡顿，请考虑使用`head`或`tail`

### 输出头行 `head`、输出尾行 `tail`
- 用法：`head <file> [-n <lines>]`、`tail <file> [-n <lines>]`
- 输出`file`头部/尾部`lines`行的内容，默认为10行

### 输出差异 `diff`
- 用法：`diff <file1> <file2>`
- 逐行比较两个文件的差异并输出不同的行
- 如果两个文件没有差异则**无输出**

## 文本匹配和筛选 `grep`
- 用法：`grep [-i] <pattern> <file>`
- 在文件`file`中查找带有`pattern`的行并输出
- `pattern`可以是一个正则表达式
- 使用`-i`时忽略大小写
- 注：常与[管道符](#管道符)联合使用
- 注：`grep`，`awk`和`sed`有时并称“Linux 三剑客”，它们有着比此处所提及到更强大的功能，但由于课程一般使用不到，请有兴趣的同学自行了解
  * [grep,sed,awk笔记 \| AJ丶cheng](https://blog.51cto.com/u_12554680/2307019)
- 示例：
```
$ cat config.json
{
...
  "USERNAME": "some-user",
  "password": "this_is_a_passwd",
...
}
```
```
$ grep -i username config.json
  "USERNAME": "some-user",
$ grep username config.json
// 无输出（未指定-i，config.json文档中没有匹配小写username的行）
```

## 更改文件权限 `chmod`
- 用法：`chmod <permission> <path>`
- 将`path`指定的文件（夹）赋予权限`permission`，它可以是
  * `+r`，`+w`，`+x`，表示授予读、写、执行权限
  * `-r`，`-w`，`-x`，表示撤销读、写、执行权限
  * 上面写法可以连用，例如`+rw`表示授予读写权限
  * 3位8进制数，见[文件权限](linux.md/#文件权限)一节

## 更改文件所有者 `chown`
- 用法：`chown <user>:<group> <path>`
- 将`path`指定的文件（夹）的所有者更改为`user:group`

## 用户手册 `man`
- 用法：`man <指令>`
- 查看指令的用户手册
- 使用[阅读模式](linux.md/#阅读模式)

## 切换用户 `su`
- 用法：`su [<user>]`
- 切换至`user`，需要输入`user`的密码
- `user`缺省时默认为当前用户，一般来讲没有作用，但考虑下面这种做法：
```
user1@host:~$ sudo -u user2 su
// 由于使用了 sudo -u，该命令被认为是以 user2 身份执行
// su 后面缺省了参数，故会将用户切换为 user2
// 而 sudo 是 user1 执行的，故需要输入的是 user1 的密码
```
- 常见特殊用法：直接`su root`需要使用`root`的密码，该密码一般是用户未知、或者出于安全考虑被禁用的；而`sudo su`可以切换至`root`用户，只需输入自己的密码
```
user1@host:~$ su root
Password: // 应输入 root 的密码，未知或禁用
user1@host:~$ sudo su
[sudo] Password of user1: // 应输入 user1 的密码，已知
// sudo su 等价于 sudo -u root su root，请参考这两个命令的缺省值理解
```

## `sudo`
- 用法：`sudo [-u <user>] <指令>`
- 以其它用户的身份执行指令
- 不使用`[-u <user>]`时默认为`root`用户
- 短期内首次使用时会要求输入**当前用户**的密码
- 示例：
```
$ sudo apt update
$ sudo reboot
```

## 特殊符号
### 与 `&&`、或 `||`、`;`
- 用法：`<指令1> && <指令2>`、`<指令1> || <指令2>`、`<指令1> ; <指令2>`
- 前一个指令**成功**、**失败**、**无条件**执行后一个指令
- 这三个用法可以类比 c 语言程序：
  * 当指令**成功**时返回值为0，因此与类似于`if (func1()==0 && func2()==0) ;`
  * `||`同理
  * `;`则与 c 几乎完全相同，类似于`func1(); func2();`
- 注意：`sudo <指令>`将被当作一个指令整体，即`sudo <指令1> && <指令2>`只有指令1获得了超级用户权限
- 示例：
```
$ gcc hw1.c -o hw1 && ./hw1   // 编译 hw1.c 到 hw1 并在编译成功时运行它
$ cd ~/COD-Lab && git pull    // 移动到 ~/COD-Lab 路径下并执行 git pull 拉取仓库
$ sudo apt update && apt upgrade      // 错误：后一个 apt upgrade 指令没有 sudo 权限
$ sudo apt update && sudo apt upgrade // 正确：先执行更新软件源，随后更新所有软件包
```

### 管道符 `|`
- 用法：`<指令1> | <指令2>`
- 简单来说，它是把指令1的输出作为指令2的输入进行执行
- 常见用于`grep`指令，例如`cat <file> | grep <text>`等价于`grep <text> <file>`
- 示例：
```
$ cat config.json | grep -i username
  "USERNAME": "some-user",
$ cat config.json | grep password
  "password": "this_is_a_passwd",
```

### 输出重定向符 `>`、 `>>`
- 用法：`<指令> > <file>`、`<指令> >> <file>`
- 将指令的输出放入`file`，前者会覆盖文件原本的内容，后者则增添到尾部
- 示例：
```
$ echo 123 > test
$ cat test
123
```
```
$ echo 456 >> test
$ cat test
123
456
```
```
$ echo 789 > test
$ cat test
789
```

### 多行输入` \ `
- 当一个指令需要多行输入时（通常是为了人读起来更方便），在除了最后一行外每行结尾增加` \ `
- 后续行的标识符将变成`>`，而不再是`$`或`#`
- 示例：
```
$ echo 123\
> 456 \            // 注意本行的 \ 前有一个空格
> 789
123456 789
// 完全等价于 echo 123456 789
```
```
$ docker run -p 80:80 \
> -v /data:/data \
> -d nginx:latest
// 完全等价于 docker run -p 80:80 -v /data:/data -d nginx:latest
// 用于分割参数时必须在 \ 前带一个空格，否则↓
```
```
$ docker run -p 80:80\
> -d nginx:latest
docker: Invalid containerPort: 80-d
// 由于缺少空格，这行命令实际等价于 docker run -p 80:80-d nginx:latest，注意到 80-d 连在了一起，而它不是一个有效的端口号，故报错
```


## 安装软件
- `apt` / `dpkg` / `make`
- 见[安装软件](install_program.md)一节

## `ssh`
- 见[ssh](../ssh.md)一节

## `git`
- 见[git](../git.md)一节

## 退出 `exit`
- 用法：`exit`
- 退出当前终端

## 电源管理
### `shutdown`
- 用法：`shutdown [<-r>] [<-h>] <time> [<message>]`
- 关机或重启
- 使用`-r`时重启
- 使用`-h`时关机
- `<time>`为倒计时，默认单位为分钟。可以是`now`
- `[message]`为关机信息
- 示例：
```
$ shutdown -r now // 立即重启
$ shutdown -h 10  // 10分钟后关机
```

### `reboot`
- 用法：`reboot`
- 重启电脑