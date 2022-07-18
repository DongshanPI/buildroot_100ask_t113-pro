# 运行输出hello word



## 配置开发环境

首先我们需要获取 交叉编译工具链。

## 获取交叉编译工具链

我们的源码都存放在不同的git仓库内，其中以github为主要托管，也是最新的状态，同时也会使用 gitee作为备用站点，根据大家的实际情况，来进行选择。

* 对于可以访问github的同学 请使用如下命令获取源码

```bash
git clone https://github.com/DongshanPI/eLinuxCore_100ask-t113-pro.git
cd  eLinuxCore_100ask-t113-pro
git submodule update  --init --recursive
```



* 对于无法访问GitHub的同学 请使用如下命令获取源码。

```bash
git clone https://gitee.com/weidongshan/eLinuxCore_100ask-t113-pro.git
cd  eLinuxCore_100ask-t113-pro
git submodule update  --init --recursive
```



获取完成源码后，需要进入到交叉编译工具链路径到 内，用于验证是否可用。

```bash
book@virtual-machine:~/eLinuxCore_100ask-t113-pro/toolchain/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi/bin$ ./arm-linux-gnueabi-gcc -v
Using built-in specs.
COLLECT_GCC=./arm-linux-gnueabi-gcc
COLLECT_LTO_WRAPPER=/home/book/eLinuxCore_100ask-t113-pro/toolchain/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi/bin/../libexec/gcc/arm-linux-gnueabi/7.2.1/lto-wrapper
Target: arm-linux-gnueabi
Configured with: '/home/tcwg-buildslave/workspace/tcwg-make-release/builder_arch/amd64/label/tcwg-x86_64-build/target/arm-linux-gnueabi/snapshots/gcc.git~linaro-7.2-2017.11/configure' SHELL=/bin/bash --with-mpc=/home/tcwg-buildslave/workspace/tcwg-make-release/builder_arch/amd64/label/tcwg-x86_64-build/target/arm-linux-gnueabi/_build/builds/destdir/x86_64-unknown-linux-gnu --with-mpfr=/home/tcwg-buildslave/workspace/tcwg-make-release/builder_arch/amd64/label/tcwg-x86_64-build/target/arm-linux-gnueabi/_build/builds/destdir/x86_64-unknown-linux-gnu --with-gmp=/home/tcwg-buildslave/workspace/tcwg-make-release/builder_arch/amd64/label/tcwg-x86_64-build/target/arm-linux-gnueabi/_build/builds/destdir/x86_64-unknown-linux-gnu --with-gnu-as --with-gnu-ld --disable-libmudflap --enable-lto --enable-shared --without-included-gettext --enable-nls --disable-sjlj-exceptions --enable-gnu-unique-object --enable-linker-build-id --disable-libstdcxx-pch --enable-c99 --enable-clocale=gnu --enable-libstdcxx-debug --enable-long-long --with-cloog=no --with-ppl=no --with-isl=no --disable-multilib --with-float=soft --with-mode=thumb --with-tune=cortex-a9 --with-arch=armv7-a --enable-threads=posix --enable-multiarch --enable-libstdcxx-time=yes --enable-gnu-indirect-function --with-build-sysroot=/home/tcwg-buildslave/workspace/tcwg-make-release/builder_arch/amd64/label/tcwg-x86_64-build/target/arm-linux-gnueabi/_build/sysroots/arm-linux-gnueabi --with-sysroot=/home/tcwg-buildslave/workspace/tcwg-make-release/builder_arch/amd64/label/tcwg-x86_64-build/target/arm-linux-gnueabi/_build/builds/destdir/x86_64-unknown-linux-gnu/arm-linux-gnueabi/libc --enable-checking=release --disable-bootstrap --enable-languages=c,c++,fortran,lto --build=x86_64-unknown-linux-gnu --host=x86_64-unknown-linux-gnu --target=arm-linux-gnueabi --prefix=/home/tcwg-buildslave/workspace/tcwg-make-release/builder_arch/amd64/label/tcwg-x86_64-build/target/arm-linux-gnueabi/_build/builds/destdir/x86_64-unknown-linux-gnu
Thread model: posix
gcc version 7.2.1 20171011 (Linaro GCC 7.2-2017.11)

```

完成以后 我们就可以加入到 系统的 PATH环境变量内。

首先 需要获取 交叉编译工具链 所在的绝对路径，进入到  <code>eLinuxCore_100ask-t113-pro/toolchain/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi/bin</code>目录下执行 **pwd** 命令，即可得到绝对路径 ` /home/book/eLinuxCore_100ask-t113-pro/toolchain/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi/bin` 。

```bash
book@virtual-machine:~/eLinuxCore_100ask-t113-pro/toolchain/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi/bin$ pwd
/home/book/eLinuxCore_100ask-t113-pro/toolchain/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi/bin
```

接下来，可以在终端下执行如下命令，讲这个加入到系统 环境变量内，这样就可以在任意位置执行  交叉编译工具链了。

```bash
export PATH=$PATH:/home/book/eLinuxCore_100ask-t113-pro/toolchain/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi/bin
```

注意：此方式只针对当前的终端有效，如果你关闭了这个终端，再次开启终端 需要重新执行才可以。

还有另一种永久生效的方式 就是写入到 系统环境变量里面，需要修改  **/etc/environment** 在末尾加上 你获取到的交叉编译工具链绝对路径,注意修改需要使用 sudo 命令。

```bash
book@virtual-machine:~$ cat /etc/environment
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/book/eLinuxCore_100ask-t113-pro/toolchain/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi/bin"
```



## 编写Hello word程序

* 配置好交叉编译工具链以后，就可以开始编写我们的应用程序了，如下为一个最简单的 hello word打印示例程序。

```c
#include <stdio.h>
int main (void)
{
    printf("hello word!\n");
    return 0;
}    
```

编写完成后，保存到 helloword.c

之后我们执行 如下编译命令进行编译 

```bash
book@virtual-machine:~$ vim helloword.c 
book@virtual-machine:~$ arm-linux-gnueabi-gcc -o helloword helloword.c
```

## 拷贝到开发板

怎么拷贝文件到开发板上？ 有U盘  ADB 网络 串口等等。

那么我们优先推进使用 网络方式，网络也有很多，有TFTP传输，有nfs传输，有SFTP传输，其中nfs传输需要内核支持 nfs文件系统，SFTP需要根文件系统支持 openssh组件服务，那么最终我们还是选用tftp服务。



### 使用usb adb方式

* 首先将开发板OTG线连接，系统内默认启动会自动启动一个 usb adb服务，这时电脑会弹出一个设备，进入到我们的VMware虚拟机讲弹出来的设备，连接到ubuntu内。
* 这时我可以使用 adb push命令来上传文件,开始上传之前可以使用 adb devices 命令来查看开发板是否连接到系统上。
* 如下示例，使用adb命令 上传 helloword到 开发板的 /root下。

```shell
adb push helloword /root
```



## 运行

下载好程序以后，需要使用chmod +x 命令来给程序添加可执行权限，之后 我们就可以执行 这个helloword应用了。

```bash
# chmod +x helloword
# ./helloword
hello word!
#
```

