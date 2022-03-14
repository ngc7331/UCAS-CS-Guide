# 安装软件
`apt`和`dpkg`都是 Debian 系发行版（包括 Ubuntu 等）特有的命令

## 目录
- [安装软件](#安装软件)
  - [目录](#目录)
  - [包管理器与软件源](#包管理器与软件源)
    - [包管理器是什么](#包管理器是什么)
    - [软件源是什么](#软件源是什么)
    - [软件源镜像](#软件源镜像)
    - [默认软件源与附加软件源](#默认软件源与附加软件源)
  - [`apt`](#apt)
    - [`apt update`](#apt-update)
    - [`apt install <pkg1> [<pkg2>...]`](#apt-install-pkg1-pkg2)
    - [`apt list --upgradable`](#apt-list---upgradable)
    - [`apt upgrade`](#apt-upgrade)
    - [`apt remove <pkg1> [<pkg2>...]`](#apt-remove-pkg1-pkg2)
    - [`apt autoremove`](#apt-autoremove)
    - [示例](#示例)
  - [`dpkg`](#dpkg)
  - [编译安装](#编译安装)



---
## 包管理器与软件源
### 包管理器是什么
像是安卓系统上的应用商店或是 IOS 系统的 AppStore，为了方便人们使用，Linux 可以使用**包管理器**便捷地进行软件包的下载、安装、初始配置、升级和卸载等动作，并自动化处理软件依赖关系。

在以 Ubuntu 为例的 Debian 系[发行版](linux.md)中，这个包管理器是 `apt`，而在 RedHat 系发行版中，一般使用的是`yum`。

### 软件源是什么
软件源是实际为包管理器**提供软件包的服务器**，一般在`/etc/apt/sources.list`文件中记录

当执行`apt`指令时看到的
```
$ apt update
Hit:1 https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal InRelease
...
```
中`https://mirrors.tuna.tsinghua.edu.cn/ubuntu`即为我的软件源

### 软件源镜像
为了加速软件源在特定地区的访问、减轻源服务器的压力，很多组织建立了镜像——即从源服务器上下载所有文件，并定期更新，以供他人访问使用的服务器

例如上面的`https://mirrors.tuna.tsinghua.edu.cn/ubuntu`就是清华大学 TUNA 协会维护的`http://archive.ubuntu.com/ubuntu`的镜像

有时国内访问源服务器较慢，换成国内镜像将显著改善软件包的下载速度

- [清华源](https://mirrors.tuna.tsinghua.edu.cn/)：[使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)，[一键配置脚本](https://tuna.moe/oh-my-tuna/)
- [科大源](https://mirrors.ustc.edu.cn/)：[使用帮助](https://mirrors.ustc.edu.cn/help/ubuntu.html)

### 默认软件源与附加软件源
系统自带的软件源在大多数时候足够使用，但有些软件包——例如 docker、vscode——并没有包含在默认软件源中，此时需手动添加**附加软件源**

软件包的官方文档里一般会有提示，例如 [docker](https://docs.docker.com/engine/install/ubuntu/)

```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
// Note: 以上指令并不完整，仅截取以作示例，请见官方文档。
```
其中
- 第一步是添加了一个 GPG-key，这是一种安全通信技术，暂时无需了解，有些软件源不需要这步。
- 第二步则是将 GPG-Key、发行版信息（`lsb_release -cs`）、软件源地址`https://download.docker.com/linux/ubuntu`等信息配置到了`/etc/apt/sources.list.d/docker.list`文件中

这样就配置好了一个**附加软件源**，并可以使用`apt`安装该源提供的软件包


---
## `apt`
实际到了指令的部分就非常简单了

与软件包相关的操作大部分都是敏感操作，需要 [sudo 权限](linux.md/#用户权限)
### `apt update`
从更新软件源信息，在执行 `install` / `upgrade` 等操作前，或添加软件源后，先执行这一操作

相当于下载和更新软件源索引列表，告诉本地的包管理器“我这个软件源有哪些软件包”

### `apt install <pkg1> [<pkg2>...]`
安装名为 pkg1 的软件包，可以一次安装多个软件包

尖括号表示参数名无需输入

### `apt list --upgradable`
列出系统上所有已安装且可升级的软件包

### `apt upgrade`
升级系统上所有已安装的软件包

### `apt remove <pkg1> [<pkg2>...]`
卸载名为 pkg1 的软件包，可以一次卸载多个软件包

### `apt autoremove`
自动卸载不需要的包（通常是为了解决依赖关系自动安装，而现在不再被依赖的包）

### 示例
```
$ sudo apt update
[sudo] password for <username>:
Hit:1 https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal InRelease
Hit:2 https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-updates InRelease
Hit:3 https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-security InRelease
Hit:4 https://mirrors.tuna.tsinghua.edu.cn/ubuntu focal-backports InRelease
Hit:5 https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu focal InRelease
Reading package lists... Done
Building dependency tree
Reading state information... Done
53 packages can be upgraded. Run 'apt list --upgradable' to see them.
```
执行源更新时，系统自动提示存在可升级的包，通过`apt list --upgradable`列出
```
$ apt list --upgradable
...
g++-9/focal-updates,focal-security 9.4.0-1ubuntu1~20.04 amd64 [upgradable from: 9.3.0-17ubuntu1~20.04]
gcc-9-base/focal-updates,focal-security 9.4.0-1ubuntu1~20.04 amd64 [upgradable from: 9.3.0-17ubuntu1~20.04]
gcc-9/focal-updates,focal-security 9.4.0-1ubuntu1~20.04 amd64 [upgradable from: 9.3.0-17ubuntu1~20.04]
...
```
格式如：`包名/分支 版本号 架构 [可从旧版本升级]`
```
$ sudo apt upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following package was automatically installed and is no longer required:
  ...
Use 'sudo apt autoremove' to remove it.
The following NEW packages will be installed:
  ...
The following packages will be upgraded:
  ...
53 upgraded, 9 newly installed, 0 to remove and 0 not upgraded.
37 standard security updates
Need to get 219 MB of archives.
After this operation, 26.1 MB of additional disk space will be used.
Do you want to continue? [Y/n]
```
提示了三类包：
1. 为了解决某依赖关系自动安装，而现在不再被依赖的包。提示可以使用`sudo apt autoremove`自动移除
2. 为了解决升级以后新的依赖关系，需要被安装的包
3. 将被升级的包

[输入y确认](linux.md/#输入y确认)操作



---
## `dpkg`
当一个软件包以`.deb`格式发布，而它不在任何已知 apt 源中时，需要手动下载软件包并用`dpkg`安装

`cd`移动到下载的包所在的目录随后执行：
```
$ dpkg -i <pkgname>.deb
```

此外，由于`apt`底层实际上也是调用`dpkg`进行安装，当使用`apt`安装或升级软件包异常中断时，需要使用
```
$ dpkg --configure -a
```
尝试恢复



---
## 编译安装
有的软件包既不提供 apt 源，亦无 deb 包，只提供了源代码（或`.tar` / `.tar.gz`）格式的源代码压缩包。此时需要编译安装。

情况较为复杂，请查询具体的软件包构建说明

比较常见的流程是
```
$ tar -xvf <pkgname>.tar    // 解压缩
$ cd <pkgname>              // 进入解压缩得到的文件夹
$ ./configure               // 执行配置脚本
$ make                      // 编译
$ make install              // 编译安装
```

这一过程通常需要`gcc`和`make`作为工具，可通过`apt`安装它们
```
$ sudo apt install gcc make
```