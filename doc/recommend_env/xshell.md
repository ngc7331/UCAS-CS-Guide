# XShell
[XShell](https://www.xshell.com/zh/)是一个付费 SSH 客户端，但对[个人和教育用途免费](https://www.xshell.com/zh/free-for-home-school/)。同时配有 XFTP 便于传输文件。

其它在 Windows 和 MacOS 下的 SSH 图形化客户端与 XShell 基本大同小异，可作为参考。

## 目录
- [XShell](#xshell)
  - [目录](#目录)
  - [配置 SSH-Key](#配置-ssh-key)
    - [生成密钥对](#生成密钥对)
    - [导入私钥](#导入私钥)
    - [配置公钥](#配置公钥)
    - [配置私钥](#配置私钥)
      - [在 XShell 中使用](#在-xshell-中使用)
      - [在 OpenSSH 中使用](#在-openssh-中使用)



---
## 配置 SSH-Key
### 生成密钥对
1. 打开 XShell->工具->新建用户密钥生成向导

![image.png](https://s2.loli.net/2022/03/23/OfIjAUEdl9Hpys5.png)

2. 选择算法和长度，默认即可

![image.png](https://s2.loli.net/2022/03/23/ewA6NvTfjE3WoXP.png)

3. 设置密钥名称和密码，默认即可（密码留空即可，但注意不要泄露生成的文件）

![image.png](https://s2.loli.net/2022/03/23/X7oxNFYs982bkHK.png)

4. 得到公钥，复制下来备用

![image.png](https://s2.loli.net/2022/03/23/4STjmgdGosL3c12.png)

5. （可选）导出私钥备用，可以在 XShell->工具->用户密钥管理器 打开界面，导出格式可以选择 OpenSSH New Format Private Keys

![image.png](https://s2.loli.net/2022/03/23/jmy1EVa5pFd2Nft.png)

![image.png](https://s2.loli.net/2022/03/23/ig7TxwOlmt8r5MK.png)



### 导入私钥
若您使用 XShell 生成密钥对，则无需进行导入

将其它方式生成的**私钥内容**粘贴到任意文件中，随后在 XShell->工具->用户密钥管理器 中选择导入该文件即可
![image.png](https://s2.loli.net/2022/03/23/RjIilByCMwGSAnO.png)

例如，通过`ssh-keygen`生成的密钥对，首先获取私钥内容
```
$ cat ~/.ssh/id_rsa
-----BEGIN OPENSSH PRIVATE KEY-----
...
...
...
-----END OPENSSH PRIVATE KEY-----
```
将全部内容复制，到 Windows 下用记事本或任意文本编辑软件新建文件，文件名任意，建议与私钥名统一，即上例中`id_rsa`

在该文件中粘贴入所有内容，注意结尾应有一个空行
![image.png](https://s2.loli.net/2022/03/23/kDqXry3Uu1KaORb.png)

保存时注意不要有如`.txt`的后缀名



### 配置公钥
将[生成密钥对第4步](#生成密钥对)得到的公钥内容复制，连接到服务器上，将内容粘贴到`~/.ssh/authorized_keys`文件中
```
$ nano ~/.ssh/authorized_keys
// 粘贴入公钥内容，使用 Ctrl + O 保存，使用 Ctrl + X 退出
```



### 配置私钥
#### 在 XShell 中使用
编辑主机设置，在用户身份验证中勾选 Public Key，并选择刚刚生成的密钥
    ![image.png](https://s2.loli.net/2022/03/23/EkegzXDTNslCj7P.png)

#### 在 OpenSSH 中使用
为了允许 vscode 使用该密钥连接至主机，需要将[生成密钥对第5步](#生成密钥对)导出的私钥复制到`C:\Users\<username>\.ssh`目录下