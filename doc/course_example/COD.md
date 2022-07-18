# 课程实例-计算机组成原理（研讨课）
计组研讨课应该是第一个强制要求使用 Linux + Git 的课程。在本章中，我们会对课件中出现的命令进行解释，但仍建议您抽空阅读 [Linux 基础](../linux/linux.md)一节和 [Git](../git.md) 一节，以便对它们有更全面的了解。

## 目录
- [课程实例-计算机组成原理（研讨课）](#课程实例-计算机组成原理研讨课)
  - [目录](#目录)
  - [命令解释](#命令解释)
    - [P01-实验课及平台开发流程简介-v6.pdf](#p01-实验课及平台开发流程简介-v6pdf)
    - [...](#)
    - [其余注意事项](#其余注意事项)
      - [简化命令](#简化命令)
  - [环境搭建](#环境搭建)
  - [FAQ](#faq)



---
## 命令解释
以2021-2022学年春季学期课件为准的命令解释，以后可能会有所不同，欢迎 Issue / PR。

注意：课件上很多引号`"`、短横线`-`由于 PPT 默认设置变成了中文全角字符，建议手动输入命令，不要复制粘贴以免出现问题

### P01-实验课及平台开发流程简介-v6.pdf
- `git config --global user.name "Your_last_name<空格>Your_first_name"`
  * 用于配置 git 用户名
  * `git`为命令，使用`git --help`查看帮助
  * `config`为`git`的子命令，修改 git 配置，使用`git config --help`查看帮助
  * `--global`是一个 flag，表示该修改是**用户全局**的，即对当前用户的所有仓库生效。常用的还有`--local`，表示对当前仓库生效
  * `user.name`为配置的键，表示要修改的配置是用户名
  * `"Your_last_name<空格>Your_first_name"`为配置的值，表示要修改成什么。两端的引号用于包裹含空格的参数，见[空格与引号](../linux/linux.md#空格与引号)一节
  * 成功时，命令无回显，见[回显](../linux/linux.md#回显)一节
  * 若需确保配置正确，可使用`--get` flag 获取配置值，即`git config --get user.name`
- `git config --global user.email "GitLab注册邮箱"`
  * 用于配置 git 用户邮箱
  * 大部分与上一条相同，只是配置键变为了`user.email`，不再赘述
- `git config --global core.editor vim`
  * 这一配置会指定 git 使用的默认[编辑器](../linux/editor.md)为 vim。亦可使用 nano 等您熟悉的编辑器代替
  * 大部分与上一条相同，只是配置键变为了`core.editor`
- `cd && ssh-keygen –t rsa –C "GitLab注册邮箱"`
  * 用于生成 ssh 密钥对，进而用于 git 远程仓库（GitLab）免密码身份认证
  * `cd`为切换目录命令，缺省参数时切换到[用户主目录](../linux/linux.md#用户目录主目录)。**已经处于主目录时可以省去，下同**。见[常见命令-cd](../linux/command.md#切换当前工作目录-cd)
  * `&&`为与运算符，表示当前一条指令执行成功时执行后一条指令，见[常见命令-与](../linux/command.md#与-或)
  * `ssh-keygen`为生成 ssh 密钥对，使用`ssh-keygen --help`查看帮助，见 [ssh](../ssh.md#设置-ssh-key-实现免密登陆) 一节
  * `-t rsa`表示指定密钥类型为`rsa`，个人认为这块可以不写（使用默认密钥类型，通常是`rsa`或`ed25519`，现在常用的 git 平台和 ssh 服务器都支持使用这两者，前者兼容性更好，后者性能更好且安全性更高，[参考](https://www.cnblogs.com/librarookie/p/15389876.html)）
  * `-C "GitLab注册邮箱"`表示为密钥对添加注释，注释内容为 GitLab 注册邮箱，个人认为可以不写
  * 也就是说，如果已经处在用户主目录，并且不指定密钥类型和注释，整条命令可以简化为`ssh-keygen`
- `gedit ~/.ssh/id_rsa.pub`
  * 使用 gedit 编辑器打开 ssh 公钥文件以便复制
  * 文件路径为`~/.ssh/id_rsa.pub`，`~`表示用户主目录，见[目录与路径](../linux/linux.md#目录与路径)一节
  * 个人不推荐使用 gedit，见[图形化与命令行](../linux/linux.md#图形化与命令行)一节
  * 相对的，建议使用`cat ~/.ssh/id_rsa.pub`代替，见[常见命令-cat](../linux/command.md#输出全文-cat)
- `cd && gedit .gitlab.token`
  * 使用 gedit 编辑器打开`~/.gitlab.token`文件以便写入
  * 同上不推荐使用 gedit，建议用`vim` / `nano`代替
  * 亦推荐使用`echo <your_token> > ~/.gitlab.token`代替，见[常见命令-echo](../linux/command.md#原样输出-echo)，`<your_token>`处替代为从网页上复制的 PAT (personal access token)
- `sudo apt install -f vim jq curl`
  * 使用`apt`安装 vim / jq / curl
  * `sudo`为使用超级用户权限执行，由于安装软件是敏感行为，必须具有超级用户权限，见[常用命令-sudo](../linux/command.md#sudo)
  * `apt`见[安装程序-apt](../linux/install_program.md#apt)
  * `install`为`apt`的子命令，表示要执行安装
  * `-f`存疑，强制执行？
  * 后续的参数为要安装的软件包名
- `cd && git clone https://<gitlab_url>/<GitLab用户名> COD-Lab`
  * 将远程仓库克隆到本地
  * `clone`为`git`的子命令，克隆远程仓库，使用`git clone --help`查看帮助
  * 随后的参数是远程仓库地址
  * 最后是要保存到的本地仓库路径，默认为当前目录下与远程仓库同名的子目录
  * 个人认为这里有误，因为使用 https 协议不支持 ssh 密钥对方式的身份验证，在没有身份管理器的情况下每次操作远程仓库，都会要求输入用户名与密码，十分麻烦
  * 相对的，应该使用`git clone ssh://<gitlab_url>/<GitLab用户名> COD-Lab`代替，需要注意的是 ssh 协议使用的端口号与 https 协议不同
- `cd ~/COD-Lab && git remote add upstream https://<gitlab_url>/ucas-cod-2021-dev/cod-lab`
  * 添加远端上游仓库
  * `remote`为`git`的子命令，修改本地仓库的远端配置，使用`git remote --help`查看帮助
  * `add`表示操作的类型是添加远端配置
  * `upstream`是新远端配置的名称，可以自由指定，只要保证与下一条配套即可
  * 随后的参数是远端的地址，同上我认为应改为 ssh 协议
- `cd ~/COD-Lab && git pull upstream master`
  * 拉取远端上游仓库
  * `pull`为`git`的子命令，拉取远端仓库，使用`git pull --help`查看帮助
  * 随后的第一个参数为远端配置的名称。默认为`origin`，即最初仓库初始化/克隆下来的远端。而这里指定的是上一步设置的上游仓库（即老师所写实验框架的仓库），因此不可缺省
  * 随后的第二个参数为本地分支的名称。默认为当前所处分支，因此若没有使用过`git branch`切换分支，可以缺省不写
- `cd ~/COD-Lab && vim fpga/design/ucas-cod/hardware/sources/example/adder.v`
  * 使用 vim 编辑实验代码
  * 建议用 vscode 等替代，更容易上手
- `cd ~/COD-Lab && git add fpga/design/ucas-cod/hardware/sources/example/adder.v`
  * git 追踪实验代码文件
  * `add`为`git`的子命令，追踪文件，使用`git add --help`查看帮助
  * 随后的参数为要追踪的路径
  * 比较偷懒的方法是追踪整个仓库`cd ~/COD-Lab && git add .`或`cd ~/COD-Lab && git add --all`，或在`commit`时使用`-a`参数（即`git commit -a`）。但这样使用需要注意，不要追踪不该追踪的文件（例如日志文件`*.log`等）
- `cd ~/COD-Lab && git commit`
  * git 提交 commit
  * `commit`为`git`的子命令，提交 commit，使用`git commit --help`查看帮助
  * 这样会采用交互式的方法使用之前设置的`core.editor`填写 commit message
  * 更推荐的方法是直接使用`-m`参数把 commit message 写在命令里，如下所示。由于标题和内容是一整个字符串，中间包含换行符，所以两端要用引号包裹，若信息中带有引号，请使用转义符。习惯上标题和信息间空一行
```
$ cd ~/COD-Lab && git commit -m "<commit_title>

<commit_message>"
```
- `cd ~/COD-Lab && git log`
  * git 查看提交记录
  * `log`为`git`的子命令，查看 commit 提交记录，使用`git log --help`查看帮助
  * 采用[阅读模式](../linux/linux.md#阅读模式)，按`q`退出
- `cd ~/COD-Lab && git push origin master`
  * git 推送本地修改到远程
  * `push`为`git`的子命令，追踪文件，使用`git push --help`查看帮助
  * 和`git pull`是一对，参数格式相同，即
  * 随后的第一个参数为远端配置的名称。默认为`origin`，即最初仓库初始化/克隆下来的远端，这里可以缺省不写
  * 随后的第二个参数为本地分支的名称。默认为当前所处分支，因此若没有使用过`git branch`切换分支，可以缺省不写
- `cd ~/COD-Lab && make FPGA_PRJ=ucas-cod FPGA_BD=nf SIM_TARGET=example wav_chk`
  * 查看波形
  * `make`是一个构建工具，[参考](https://zhuanlan.zhihu.com/p/92589781)
  * 随后的几个`key=value`的键值对用来设置`make`的环境变量
  * 最后的`wav_chk`为构建目标
  * 用在这里是为了简化我们学生的操作，本质上执行了：
    + 利用之前保存的`.gitlab.token`文件登录到云平台
    + 获取流水线运行情况
    + 下载流水线运行结果（artifact）
    + 使用`gtkwave`打开波形文件
    + ...
  * 相对而言属于进阶内容，请自行学习`make`或死记硬背
  * 可以查看各目录下的`Makefile`和`*.mk`文件尝试解读

### ...
TODO

### 其余注意事项
#### 简化命令
~~偷懒大法~~

经过上面的解释，可以注意到老师提供的命令有一些只是为了提高鲁棒性（避免因个人配置不同出错），例如
- 几乎每个命令前都存在的`cd &&`
- 可以使用默认值的参数

随着对 Linux 熟练度的提高，例如
- 养成了关注当前工作目录的习惯
- 熟悉了参数默认值

可以试着简化老师的命令，这可以有效的提高对 Linux 各种机制的理解，同时节省时间



---
## 环境搭建
~~从此开始抛弃老师提供的环境吧~~
TODO



---
## FAQ
建议首先阅读[应对错误](../problem.md)一章
TODO
