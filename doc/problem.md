# 应对错误
## 目录
- [应对错误](#应对错误)
  - [目录](#目录)
  - [常见错误及解决方案](#常见错误及解决方案)
    - [环境配置问题](#环境配置问题)
      - [未找到命令](#未找到命令)
        - [软件包未安装，但可以通过包管理器安装](#软件包未安装但可以通过包管理器安装)
        - [软件包未安装，且无法通过包管理器安装](#软件包未安装且无法通过包管理器安装)
    - [权限问题](#权限问题)
      - [用户没有权限](#用户没有权限)
      - [文件没有可执行权限](#文件没有可执行权限)
    - [网络问题](#网络问题)
      - [DNS 解析错误](#dns-解析错误)
    - [磁盘问题](#磁盘问题)
      - [文件系统故障](#文件系统故障)
  - [如何初步读懂报错](#如何初步读懂报错)
    - [定位报错信息](#定位报错信息)
    - [检查日志文件](#检查日志文件)
    - [关于 Warning](#关于-warning)
  - [如何有效向他人提问](#如何有效向他人提问)
    - [避免重复提出问题](#避免重复提出问题)
    - [避免提出过于宽泛的问题](#避免提出过于宽泛的问题)
  - [参考资料](#参考资料)



## 常见错误及解决方案
### 环境配置问题
#### 未找到命令
遇到这类错误，请先检查拼写是否正确。
##### 软件包未安装，但可以通过包管理器安装
```
Command 'xx' not found, but can be installed with:

sudo apt install xx   # version xxx
```
按提示输入`sudo apt install xx`安装即可。

##### 软件包未安装，且无法通过包管理器安装
```
xx: command not found
```
请先确定这个命令由什么软件包提供，随后去查找这个软件包的来源，最后参考[安装软件](linux/install_program.md)页面进行安装。

常见的来源有：
1. 可以由 apt 管理，但不在默认源中，如 docker
2. `.deb` 包，如 vscode
3. 需要从源代码编译安装

### 权限问题
各种各样的`Permission Denied`
#### 用户没有权限
例如在普通用户下执行`apt update`出现，
```
$ apt update
Reading package lists... Done
E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)
E: Unable to lock directory /var/lib/apt/lists/
W: Problem unlinking the file /var/cache/apt/pkgcache.bin - RemoveCaches (13: Permission denied)
W: Problem unlinking the file /var/cache/apt/srcpkgcache.bin - RemoveCaches (13: Permission denied)
```
或者访问没有权限的文件或目录。
```
$ ls /root
ls: cannot open directory '/root': Permission denied
```
解决方法很多，最简单的是以 sudo 权限执行，即在命令前加上`sudo `。
```
$ sudo apt update
$ sudo ls /root
```
或者直接使用 root 用户执行。**可能会造成安全问题**。
```
# apt update
```

#### 文件没有可执行权限
```
$ ./some_script.sh
bash: ./some_script.sh: Permission denied
```
为文件赋予权限即可。
```
$ chmod +x ./some_script
```

### 网络问题
网络是个很复杂的东西，很难在这里枚举出各种情况和应对方案，请适时求助他人。
#### DNS 解析错误
例如
```
Could not resolve host: xx
Temporary failure in name resolution
```
关键词是“解析” (resolve / resolution)

错误原因通常为：
- 你网络寄了：
  * 虚拟机请检查虚拟网卡是否开启
  * 请检查 Wifi 或有线网络连接是否正常
  * 请检查校园网是否登录
- DNS 服务器寄了：请向他人询问是否发生相同错误，以便确认确实是 DNS 服务器寄了。一般等待即可恢复。



### 磁盘问题
#### 文件系统故障
虚拟机系统正常启动，读取正常，但是写入、修改、删除文件时出现类似如下报错：
```
fatal: unable to create xxx: Read-only file system
rm: cannot remove xxx: Read-only file system
```
如果在`sudo`权限下依然出现这样的报错，基本可以排除[权限问题](#权限问题)。此时需要考虑是否出现文件系统故障。

首先查看系统的文件系统挂载情况
```
$ cat /proc/mounts
...
devpts /dev/pts devpts rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000 0 0
/dev/sda1 / ext4 ro,relatime,errors=remount-ro 0 0
tmpfs /dev/shm tmpfs rw,nosuid,nodev,inode64 0 0
...
```
这里面每一行代表一个挂载条目，格式如
```
设备名 挂载点 文件系统格式 flags
```
- 设备名包括物理磁盘`/dev/sda`、`/dev/sdb`等，也包括临时文件系统`tmpfs`等
- 挂载点表示该设备挂载的路径
- 文件系统格式包括`ext4`、`tmpfs`等

需要关注的是根目录`/`对应的物理设备，如上例中的`/dev/sda1`，后面是否出现`ro`(Read-only)、`errors=remount-ro`等字样。

若出现，则很可能是文件系统故障导致系统自动将其挂载为只读模式。

应对方式如下：
1. 关闭虚拟机并备份，避免修复出错导致文件无法读取
2. 使用`sudo fsck -f <device>`，其中device为实际物理设备名，如上例中`/dev/sda1`，进行自动修复，该过程可能持续数十分钟
3. 重启虚拟机，再次检查文件系统是否恢复`rw`(Read-Write)模式

Note:
- 请避免使用强制关机虚拟机，普通的关机方式虽然慢，但可以减少故障
- 请适时备份虚拟机，见[虚拟机](VM.md)一节
- 如果该故障多次出现，请留意主机硬盘是否存在/将要故障
  * 若了解，尝试使用 Crystal Disk Info 等工具读取 S.M.A.R.T. 信息检查
  * 否则，请适时向他人求助



## 如何初步读懂报错
### 定位报错信息
这一步的关键时找到关键词，例如
- `E:`、`Err`、`Error`
- `Fatal`
- `Critical`
- `Failed to xxx`

有些情况下，报错信息会以红色显示，请务必留意。

### 检查日志文件
很多程序都会在运行时写入日志文件，出错时相关细节也会记录在日志中。这类文件的常见后缀名为`.log`。

请留意报错时是否提示了某一日志文件，如果有，请使用`cat`或`tail`指令查看该文件以便更好的定位错误。

### 关于 Warning
大部分的 Warning 都是**不影响程序执行**的轻微错误，因此无需太过在意。

但当以下情况发生时仍需注意 Warning 提供的信息：
- 大量 Warning 同时出现
- 程序没有终止但行为与预期不符时
- 没有 Error 但程序终止



## 如何有效向他人提问
墙裂建议阅读[提问的智慧](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/main/README-zh_CN.md)，这上面的准则也有助于您未来在 StackOverflow 等技术论坛向他人求助。下文为我个人大致总结的观点，可能与它有所重复。

### 避免重复提出问题
在 QQ、微信群或是论坛上提问时，请善用搜索功能，检查是否有人提出过相同或类似的问题。

如果有人提出过并且得到了（对 ta）有效的解答，那么这个解答一定会有助于你：
- 对你有效，恭喜你解决了问题
- 对你无效，在提问时指出：“我尝试了上面提到的xx方法，但它对我无效，具体表现是xx”。这将使他人可以更快且有效地提供帮助

### 避免提出过于宽泛的问题
这类问题通常有两种情况：
1. 问题背后可能的影响因素过多，难以解答：
> 我的虚拟机打不开怎么办？
2. 问题实际上是由不熟悉工具引起的，只需查找相关资料即可：
> vim 如何输入字符？

对于前者，请参考[如何初步读懂报错](#如何初步读懂报错)一节，先尽可能把问题细化：
> 我的虚拟机打不开，报错提示说"Failed to load R0 module..."，请问如何解决？

对于后者，请先自行搜索相关资料，如果仍然无法解决再进行提问：
> 我的 vim 无法输入字符，百度告诉我按 i 键即可进入编辑模式，但我按了没有效果，请问怎么办？



## 参考资料
- [提问的智慧](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/main/README-zh_CN.md)
