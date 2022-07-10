# SSH
注：下文中有时混用“远程主机”与“服务器”的概念，指代被连接的主机

注：“远程主机”虽有“远程”两字，但也可以是本地的虚拟机或 WSL 环境

## 目录
- [SSH](#ssh)
  - [目录](#目录)
  - [SSH 是什么](#ssh-是什么)
  - [为什么需要 SSH](#为什么需要-ssh)
  - [SSH 的安装](#ssh-的安装)
    - [客户端](#客户端)
    - [服务端](#服务端)
  - [使用 SSH 连接到远程主机](#使用-ssh-连接到远程主机)
  - [设置 SSH-key 实现免密登陆](#设置-ssh-key-实现免密登陆)
    - [使用 XShell 进行配置](#使用-xshell-进行配置)
  - [在 vscode 中使用 ssh](#在-vscode-中使用-ssh)
  - [进阶内容](#进阶内容)
    - [.ssh/config 文件及多身份](#sshconfig-文件及多身份)

## SSH 是什么
SSH（Secure Shell，安全外壳协议）是一种加密网络传输协议，常用于从客户端远程登录服务器。

## 为什么需要 SSH
对于很多计系课程来说，配置本地 linux 环境具有以下缺点：

- **步骤繁琐，从而出错概率较大**。好多意想不到的报错都是因为不经意间敲漏了若干命令或是空格。
- **占用空间大**。虚拟机的 OVA 文件往往有几十个 GB，会挤占好多好多的游戏空间。
- **对计算机性能有一定要求**。如果内存过小，容易卡成 PPT。
- ……

因此，不少课程配备了相应的云端服务，来缓解大家的配置压力。

例如，2021春季学期开设的**计算机科学导论**课程就为大家准备了 Web 版的 VS Code，将其作为一种编译运行 Go 语言的备选方案；**计算机组成原理**研讨课也提供了云端虚拟机，为有需要的同学提供实验设计、代码编写、提交与推送、错误仿真波形查看等功能。

SSH作为一种远程登陆的手段，可以令本地客户端直接连接远程服务器，并允许用户通过命令行界面远程执行命令。

此外，即使在本地运行虚拟机，也常常使用 SSH 连接到虚拟机中，避免了使用 Linux 图形化界面。

## SSH 的安装
### 客户端
如果想远程登陆服务器，只需安装客户端。（Ubuntu 默认安装了客户端）

在 Debian 系[发行版](linux/linux.md)中，可以直接使用`apt`进行安装：
```
$ sudo apt install openssh-client
```

在 Windows 和 MacOS 环境下，可以类似地直接安装 openssh 并使用命令行
- [安装 OpenSSH \| Windows 官方文档](https://docs.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_install_firstuse)

也推荐使用终端软件，便于管理终端、凭据和SSH-Key，个人使用过并推荐使用的终端有：
- [putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/)：开源免费，可以使用 SSH、Telnet 和串口协议。
- [Termius](https://www.termius.com/)：收费，但可以凭学校邮箱免费申请 [Github 的学生认证](https://education.github.com/pack)，随后在学生期间免费使用。支持全平台（含 Android 和 iOS）和多设备同步。界面美观。
- [XShell](https://www.xshell.com/zh/)：收费，但对[个人和教育用途免费](https://www.xshell.com/zh/free-for-home-school/)。同时配有 XFTP 便于传输文件。见 [XShell](./recommend_env/xshell.md) 一节

### 服务端
如果想把你的计算机（虚拟机或 WSL）作为服务器，让其他计算机连接，需要安装服务端。
```
$ sudo apt install openssh-server
```

## 使用 SSH 连接到远程主机
登陆一台远程主机，往往需要知道四个参数：

- 远程主机提供的用户名（这里以 ucas 为例）
- 远程主机的 ip 地址（这里以 X.X.X.X 为例）
- 远程主机 SSH 服务的端口号（这里以1234为例；如果不指定`-p 端口号`，默认端口为22）
- 远程主机该用户名相应的密码

基本使用格式为：

```
$ ssh ucas@X.X.X.X -p 1234
```

如果是第一次登陆该远程主机，一般会出现以下确认界面：

```
The authenticity of host '...' can't be established.
ECDSA key fingerprint is SHA256: ...
Are you sure you want to continue connecting (yes/no)?
```

输入`yes`回车即可。

接下来需要输入远程主机的密码，注意输入过程中并不会有任何提示。输入完成后直接回车即可。

此时，远程主机的命令行提示符会取代原来的提示符，说明已成功连接。

可以通过输入`exit`命令回车退出远程主机的连接。

## 设置 SSH-key 实现免密登陆
经过上述步骤，每次登陆远程主机都需要输入密码，较为繁琐。因此，推荐配置通过公钥登陆。

配置的流程大体上是：
1. 生成密钥对
2. 配置私钥
3. 配置公钥
4. （可能不需要）在服务器上启用 SSH-Key

每个步骤都有多种完成的方式。

一个典型流程如下：

首先在本地计算机上生成密钥对：
```
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (~/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```
命令中`-t rsa`表示指定加密算法为 rsa 算法，其他可选项有`dsa | ecdsa | ecdsa-sk | ed25519 | ed25519-sk`，如不了解，可以缺省该项以使用默认的设置

运行中需要输入的分别是：
1. 要保存的私钥的**绝对路径**（默认为`~/.ssh/id_rsa`），公钥将保存到同样路径下的`.pub`文件（默认为`~/.ssh/id_rsa.pub`）
2. 密钥对的密码（默认为无密码）
3. 确认密码

完成后，`~/.ssh`下会出现下列文件：

```
id_rsa			//私钥文件
id_rsa.pub		//公钥文件
```

将公钥文件`id_rsa.pub`内容复制粘贴远程主机`~/.ssh/authorized_keys`文件末尾即可完成配置，可以通过`ssh-copy-id`命令实现：
```
$ ssh-copy-id ucas@X.X.X.X -p 1234
```

根据提示英文操作完成配置。

亦可打开公钥文件，复制全部内容，然后手动粘贴到`~/.ssh/authorized_keys`文件中

由于是在本地计算机上生成的密钥对，私钥已默认配置完毕

最后，在服务器上允许使用 SSH-key 登录。有些环境已默认允许，请检验`/etc/ssh/sshd_config`文件内容
```
$ sudo nano /etc/ssh/sshd_config
...
PubkeyAuthentication yes  // 找到该项并改为yes
...
```

### 使用 XShell 进行配置
见 [XShell](recommend_env/xshell.md) 一节

## 在 vscode 中使用 ssh
见 [vscode](recommend_env/vscode.md) 一节

## 进阶内容
### .ssh/config 文件及多身份
- [How to Use SSH Config File \| Beginner's Guide](https://linuxhandbook.com/ssh-config-file/)

可能有时会有这样的需求：希望在不同的平台（如 Github 和 GitLab ）使用不同的 ssh-key，那么通过在`.ssh/config`配置文件内进行多身份的设置就十分重要

首先，需要生成多个密钥对，在[设置 SSH-key 实现免密登陆](#设置-ssh-key-实现免密登陆)一节中已经提到，使用`ssh-keygen`生成密钥时可以指定一个保存路径。只需再次运行该命令并指定一个不同的文件名，即可在不影响原有密钥对的情况下生成新的密钥对

（建议将所有密钥保存到`~/.ssh/`路径下便于管理）

接下来，修改（或创建）`~/.ssh/config`文件，示例如下
```
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/key-for-github

Host=gitee.com
    HostName=gitee.com
    PreferredAuthentications=publickey
    IdentityFile ~/.ssh/key-for-gitee

Host private-server
    HostName 192.168.123.456
    User ucas
    Port 2222
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa
```
每一行都符合`key value`或`key=value`的形式（为了美观，不建议如该例中混用），其中 key 对大小写不敏感，而 value 敏感

每个 Host 对应一个主机（或者身份）其名称可以自由设置，一个 Host 内部可以有一些可选参数：
- HostName 为主机名，即服务器的域名或 ip 地址
- User 为要使用的用户，可以缺省
- Port 为服务器 ssh 服务的端口，缺省时为默认的22端口
- IdentityFile 为私钥的绝对路径
- ...

在配置完成后，可以使用`ssh <host>`来使用配置文件中的身份，例如
```
$ ssh private-server
```
在上例的配置中就等价于
```
$ ssh ucas@192.168.123.456 -p 2222
```
且优先使用密钥对方式进行身份认证

此外，上例配置后在使用`ssh`协议访问 github 与 gitee 私有仓库时，也会自动使用相应的私钥进行认证，免去重复输入密码的痛苦
