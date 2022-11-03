# 课程实例-操作系统（研讨课）
## 目录
- [课程实例-操作系统（研讨课）](#课程实例-操作系统研讨课)
  - [目录](#目录)
  - [环境搭建](#环境搭建)
    - [**注**](#注)
    - [准备](#准备)
    - [RISC-V 64 交叉编译环境](#risc-v-64-交叉编译环境)
    - [QEMU](#qemu)
    - [minicom](#minicom)
    - [Project0 测试](#project0-测试)



---
## 环境搭建
### **注**
- **[2022.11.03更新] 经过3个 Project 的测试，wsl 环境开发没有任何问题，QEMU 模拟运行正常。但上板仍需使用虚拟机**
- **[2022.08.29更新] 警告：本课程后续需要通过 USB 串口连接 Pynq 开发板，而 WSL 对于 USB 设备的支持不完善，需要参考[官方文档](https://docs.microsoft.com/zh-cn/windows/wsl/connect-usb)、甚至重新编译内核**
- 本节使用的方法理论上与老师给出的虚拟机相同
  - riscv 交叉编译环境：
  ```
  $ riscv64-unknown-linux-gnu-gcc --version
  riscv64-unknown-linux-gnu-gcc (GCC) 9.2.0
  Copyright (C) 2019 Free Software Foundation, Inc.
  This is free software; see the source for copying conditions.  There is NO
  warranty; not even for MERCHANTABILITY or FITNESS FOR A    PARTICULAR PURPOSE.
  ```
  - QEMU：
  ```
  $ ~/OSLab-RISC-V/qemu/riscv64-softmmu/qemu-system-riscv64 --version
  QEMU emulator version 4.1.1.1 DASICS
  Copyright (c) 2003-2019 Fabrice Bellard and the QEMU Project developers
  ```
  - minicom：
  ```
  $ minicom -v
  minicom version 2.8
  Copyright (C) Miquel van Smoorenburg.
  This program is free software; you can redistribute it and/or
  modify it under the terms of the GNU General Public License
  as published by the Free Software Foundation; either version
  2 of the License, or (at your option) any later version.
  ```
- 且 Project0 测试正常
- **但不排除老师还做了其它我尚不清楚的修改、造成环境不一致的可能性**
- 仅适用于有一定折腾能力、因各种原因（老师提供的镜像导入运行失败、希望使用 WSL）自行搭建环境的同学

### 准备
- 一个 Linux 环境（示例使用的环境为 WSL2 Ubuntu 22.04 LTS）
- 老师在预备课上提供的 SD 卡中如下三个文件，拷入 Linux 系统备用：
  - riscv64-linux.tar.gz
  - OSLab-RISC-V.tar.gz
  - minicom.tar.gz
```
$ pwd
/home/user
$ ls
... OSLab-RISC-V.tar.gz  minicom.tar.gz  riscv64-linux.tar.gz ...
```

### RISC-V 64 交叉编译环境
可执行文件已编译好，只需解压、放到系统目录中、修改环境变量即可
1. 解压文件
   ```
   $ tar -xvf riscv64-linux.tar.gz
   ...
   ```
2. 移动到系统目录中，并设置权限
   ```
   $ sudo mv opt/riscv64-linux /opt
   $ sudo chown root:root /opt/riscv64-linux -R
   ```
3. 添加`/opt/riscv64-linux/bin`到环境变量
    - 临时修改（重启终端失效）
      ```
      $ export PATH=/opt/riscv64-linux/bin:$PATH
      ```
    - 长期修改，需要将上面的命令添加到 [shell 启动脚本](../linux/advanced.md#dotfiles)中。以 Ubuntu 默认的 bash 为例，添加到`~/.bashrc`中
      ```
      $ nano ~/.bashrc        // 可使用任意编辑器，这里用 nano
      // 在文件末尾另起一行，写入“export PATH=/opt/riscv64-linux/ bin:$PATH”
      $ source ~/.bashrc      // 保存后使改动生效，亦可重启终端
      ```
      若不确定自己使用的 shell，可以使用`echo $SHELL`查看

4. 验证配置
   ```
   $ riscv64-unknown-linux-gnu-gcc --version
   riscv64-unknown-linux-gnu-gcc (GCC) 9.2.0
   Copyright (C) 2019 Free Software Foundation, Inc.
   This is free software; see the source for copying conditions.  There is NO
   warranty; not even for MERCHANTABILITY or FITNESS FOR A    PARTICULAR PURPOSE.
   ```

### QEMU
要编译，但老师已写好编译脚本，只需解压并运行即可
1. 解压文件
   ```
   $ tar -xvf OSLab-RISC-V.tar.gz
   ...
   ```
2. 安装编译所需环境
   ```
   $ sudo apt install pkg-config libglib2.0-dev libpixman-1-dev
   ...
   ```
3. 运行编译脚本。此步需要时间较长，且有大量 Warning（似乎是不同 Python 版本间`==`和`is`的差异引起，助教老师说无需在意，没有报 Error 就行），请耐心
   ```
   $ cd OSLab-RISC-V
   $ ./rebuild-qemu.sh
   ...
   ```
4. 验证编译
   ```
   $ ./qemu/riscv64-softmmu/qemu-system-riscv64 --version
   QEMU emulator version 4.1.1.1 DASICS
   Copyright (c) 2003-2019 Fabrice Bellard and the QEMU Project developers
   ```

### minicom
要求为 2.8 及以上版本
- Ubuntu 22.04自带的 apt 源已满足要求：
  ```
  $ apt search minicom
  ...
  minicom/jammy 2.8-2 amd64
    Friendly menu driven serial communication program
  ...
  ```
  类似情况，使用包管理器安装即可
  ```
  $ sudo apt install minicom
  ```
- 否则，编译安装：
  1. 解压文件
     ```
     $ tar -xvf minicom.tar.gz
     ...
     ```
  2. 编译安装
     ```
     $ ./configure
     $ make -j4           // 该数字请根据实际 CPU 核心数量调整
     $ make install
     ```
  3. 验证安装
     ```
     $ minicom -v
     minicom version 2.8
     Copyright (C) Miquel van Smoorenburg.
     This program is free software; you can redistribute it and/or
     modify it under the terms of the GNU General Public License
     as published by the Free Software Foundation; either version
     2 of the License, or (at your option) any later version.
     ```

### Project0 测试
克隆老师给出的仓库，进入 Project0 文件夹

检查 MakeFile 中工具路径是否与实际一致
```
$ cat MakeFile
...
# -----------------------------------------------------------------------
# Host Linux Variables
# -----------------------------------------------------------------------

SHELL           = /bin/sh
DIR_OSLAB       = $(HOME)/OSLab-RISC-V
DIR_QEMU        = $(DIR_OSLAB)/qemu

# -----------------------------------------------------------------------
# Build and Debug Tools
# -----------------------------------------------------------------------

CROSS_PREFIX    = riscv64-unknown-linux-gnu-
CC                              = $(CROSS_PREFIX)gcc
GDB                             = $(CROSS_PREFIX)gdb
QEMU                    = $(DIR_QEMU)/riscv64-softmmu/qemu-system-riscv64
...
```
上面为老师给出的配置，即之前编译的 QEMU 位于路径`~/OSLab-RISC-V/qemu/riscv64-softmmu/qemu-system-riscv64`下

只要最初在用户主目录`~`下执行解压命令`tar -xvf OSLab-RISC-V.tar.gz`，路径应该是正确的

检查后，编译
```
$ make
riscv64-unknown-linux-gnu-gcc -O0 -fno-builtin -nostdlib -nostdinc -Wall -mcmodel=medany -ggdb3 -Wl,--defsym=TEXT_START=0x50000000 -T riscv.lds -o  test test.S -e main
```

调试运行
```
$ make debug
/home/user/OSLab-RISC-V/qemu/riscv64-softmmu/qemu-system-riscv64 -nographic -machine virt -m 256M -kernel  test -bios none -s -S
```
另起一终端使用 gdb
```
$ make gdb
riscv64-unknown-linux-gnu-gdb  test -ex "target remote:1234"
...
Reading symbols from test...
Remote debugging using :1234
0x0000000000010000 in ?? ()
(gdb)
```

如上，未报错，说明编译和运行正常，测试成功
