# 课程实例-智能计算系统

## 目录
- [课程实例-智能计算系统](#课程实例-智能计算系统)
  - [目录](#目录)
  - [踩坑](#踩坑)
    - [通用](#通用)
      - [python 运行报错`SyntaxError: Non-ASCII character`](#python-运行报错syntaxerror-non-ascii-character)
    - [exp4](#exp4)
      - [exp4.4 bazel 报错`corrupt installation`](#exp44-bazel-报错corrupt-installation)
    - [exp4.4 tensorflow 编译时报错`server terminated abruptly`](#exp44-tensorflow-编译时报错server-terminated-abruptly)



---
## 踩坑
### 通用
#### python 运行报错`SyntaxError: Non-ASCII character`
```
SyntaxError: Non-ASCII character '\xe6' in file xxx.py on line xxx, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details
```

在文件头上加上`# encoding: utf-8`即可

### exp4
#### exp4.4 bazel 报错`corrupt installation`
错误信息如：
```
$ bazel version
FATAL: corrupt installation: file '/<workspace>/.cache/bazel/_bazel_root/install/<hash>/A-server.jar' is missing or modified. Please remove '/<workspace>/.cache/bazel/_bazel_root/install/<hash>' and try again.
```

简单的解决方法是删除缓存文件后重新运行
```
$ sudo rm -rf /<workspace>/.cache/bazel/_bazel_root/install/<hash>
$ bazel version
```
需要注意将`<workspace>`和`<hash>`换成自己的目录

或者可以重新安装 bazel，参考[官方文档](https://docs.bazel.build/versions/0.24.0/install-ubuntu.html)
```
$ wget https://releases.bazel.build/0.24.0/rc9/bazel-0.24.0rc9-installer-linux-x86_64.sh
...
$ chmod +x bazel-0.24.0rc9-installer-linux-x86_64.sh
$ ./bazel-0.24.0rc9-installer-linux-x86_64.sh
...
$ export PATH=/usr/local/bin:$PATH
$ bazel version
Build label: 0.24.0rc9
Build target: bazel-out/k8-opt/bin/src/main/java/com/google/devtools/build/lib/bazel/BazelServer_deploy.jar
Build time: Thu Mar 21 19:49:51 2019 (1553197791)
Build timestamp: 1553197791
Build timestamp as int: 1553197791
```

需要注意安装的版本应在`0.24.0`到`0.25`，过旧或过新的版本无法编译课程使用的 tensorflow 版本

### exp4.4 tensorflow 编译时报错`server terminated abruptly`
修改编译脚本`build_tensorflow-v1.10_mlu.sh`，将`jobs_num`从 32 改为 16 即可（更小理论可行，但编译速度会慢；更大未经测试）

```
$ vi build_tensorflow-v1.10_mlu.sh
...
jobs_num=32 // 改为 jobs_num=16
...
```

