# 编译运行Helloword驱动

## 配置开发环境

首先我们需要获取  配套的交叉编译工具链.

由于目前工具链没有提供windows版本，所以只能在 Linux下进行，操作，请先参考上述章节 配置ubuntu 虚拟机章节，进行配置，并配置好。


## 获取kernel源码工程
我们的源码都存放在不同的git仓库内，其中以github为主要托管，也是最新的状态，同时也会使用 gitee作为备用站点，根据大家的实际情况，来进行选择。

* 对于可以访问github的同学 请使用如下命令获取源码

```bash
git clone https://github.com/DongshanPI/eLinuxCore_100ask-t113-pro
cd  eLinuxCore_100ask-t113-pro
git submodule update  --init --recursive
```


* 对于无法访问GitHub的同学 请使用如下命令获取源码。

```bash
git clone https://gitee.com/weidongshan/eLinuxCore_100ask-t113-pro.git
cd  eLinuxCore_100ask-t113-pro
git submodule update  --init --recursive
```


## 配置内核编译环境
```bash
export PATH=$PATH:/home/book/eLinuxCore_100ask-t113-pro/toolchain/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi/bin
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabi-
```
```bash
sun8iw20p1smp_defconfigbook@100ask:~/eLinuxCore_100ask-t113-pro/linux$ export ARCH=arm
book@100ask:~/eLinuxCore_100ask-t113-pro/linux$ export CROSS_COMPILE=arm-linux-gnueabi-
book@100ask:~/eLinuxCore_100ask-t113-pro/linux$ export PATH=$PATH:/home/book/eLinuxCore_100ask-t113-pro/toolchain/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi/bin
book@100ask:~/NezhaSTU/eLinuxCore_100ask-t113-pro/linux$ make sun8iw20p1smp_defconfig
  HOSTCC  scripts/basic/fixdep
  HOSTCC  scripts/kconfig/conf.o
  HOSTCC  scripts/kconfig/confdata.o
  HOSTCC  scripts/kconfig/expr.o
  LEX     scripts/kconfig/lexer.lex.c
  YACC    scripts/kconfig/parser.tab.[ch]
  HOSTCC  scripts/kconfig/lexer.lex.o
  HOSTCC  scripts/kconfig/parser.tab.o
  HOSTCC  scripts/kconfig/preprocess.o
  HOSTCC  scripts/kconfig/symbol.o
  HOSTLD  scripts/kconfig/conf
#
# configuration written to .config
#
book@100ask:~/eLinuxCore_100ask-t113-pro/linux$ make zImage -j8

```



## 编写 helloword驱动

hello_drv.c

```c
#include <linux/module.h>

#include <linux/fs.h>
#include <linux/errno.h>
#include <linux/miscdevice.h>
#include <linux/kernel.h>
#include <linux/major.h>
#include <linux/mutex.h>
#include <linux/proc_fs.h>
#include <linux/seq_file.h>
#include <linux/stat.h>
#include <linux/init.h>
#include <linux/device.h>
#include <linux/tty.h>
#include <linux/kmod.h>
#include <linux/gfp.h>

/* 1. 确定主设备号                                                                 */
static int major = 0;
static char kernel_buf[1024];
static struct class *hello_class;


#define MIN(a, b) (a < b ? a : b)

/* 3. 实现对应的open/read/write等函数，填入file_operations结构体                   */
static ssize_t hello_drv_read (struct file *file, char __user *buf, size_t size, loff_t *offset)
{
	int err;
	printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
	err = copy_to_user(buf, kernel_buf, MIN(1024, size));
	return MIN(1024, size);
}

static ssize_t hello_drv_write (struct file *file, const char __user *buf, size_t size, loff_t *offset)
{
	int err;
	printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
	err = copy_from_user(kernel_buf, buf, MIN(1024, size));
	return MIN(1024, size);
}

static int hello_drv_open (struct inode *node, struct file *file)
{
	printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
	return 0;
}

static int hello_drv_close (struct inode *node, struct file *file)
{
	printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
	return 0;
}

/* 2. 定义自己的file_operations结构体                                              */
static struct file_operations hello_drv = {
	.owner	 = THIS_MODULE,
	.open    = hello_drv_open,
	.read    = hello_drv_read,
	.write   = hello_drv_write,
	.release = hello_drv_close,
};

/* 4. 把file_operations结构体告诉内核：注册驱动程序                                */
/* 5. 谁来注册驱动程序啊？得有一个入口函数：安装驱动程序时，就会去调用这个入口函数 */
static int __init hello_init(void)
{
	int err;
	
	printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
	major = register_chrdev(0, "hello", &hello_drv);  /* /dev/hello */


	hello_class = class_create(THIS_MODULE, "hello_class");
	err = PTR_ERR(hello_class);
	if (IS_ERR(hello_class)) {
		printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
		unregister_chrdev(major, "hello");
		return -1;
	}
	
	device_create(hello_class, NULL, MKDEV(major, 0), NULL, "hello"); /* /dev/hello */
	
	return 0;
}

/* 6. 有入口函数就应该有出口函数：卸载驱动程序时，就会去调用这个出口函数           */
static void __exit hello_exit(void)
{
	printk("%s %s line %d\n", __FILE__, __FUNCTION__, __LINE__);
	device_destroy(hello_class, MKDEV(major, 0));
	class_destroy(hello_class);
	unregister_chrdev(major, "hello");
}


/* 7. 其他完善：提供设备信息，自动创建设备节点                                     */

module_init(hello_init);
module_exit(hello_exit);

MODULE_LICENSE("GPL");
```

```c

#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>

/*
 * ./hello_drv_test -w abc
 * ./hello_drv_test -r
 */
int main(int argc, char **argv)
{
	int fd;
	char buf[1024];
	int len;
	
	/* 1. 判断参数 */
	if (argc < 2) 
	{
		printf("Usage: %s -w <string>\n", argv[0]);
		printf("       %s -r\n", argv[0]);
		return -1;
	}

	/* 2. 打开文件 */
	fd = open("/dev/hello", O_RDWR);
	if (fd == -1)
	{
		printf("can not open file /dev/hello\n");
		return -1;
	}

	/* 3. 写文件或读文件 */
	if ((0 == strcmp(argv[1], "-w")) && (argc == 3))
	{
		len = strlen(argv[2]) + 1;
		len = len < 1024 ? len : 1024;
		write(fd, argv[2], len);
	}
	else
	{
		len = read(fd, buf, 1024);		
		buf[1023] = '\0';
		printf("APP read : %s\n", buf);
	}
	
	close(fd);
	
	return 0;
}

```



Makefile:

```makefile

# 1. 使用不同的开发板内核时, 一定要修改KERN_DIR
# 2. KERN_DIR中的内核要事先配置、编译, 为了能编译内核, 要先设置下列环境变量:
# 2.1 ARCH,          比如: export ARCH=arm64
# 2.2 CROSS_COMPILE, 比如: export CROSS_COMPILE=aarch64-linux-gnu-
# 2.3 PATH,          比如: export PATH=$PATH:/home/book/100ask_roc-rk3399-pc/ToolChain-6.3.1/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin 
# 注意: 不同的开发板不同的编译器上述3个环境变量不一定相同,
#       请参考各开发板的高级用户使用手册

KERN_DIR = /home/book/eLinuxCore_100ask-t113-pro/linux/

all:
	make -C $(KERN_DIR) M=`pwd` modules 
	$(CROSS_COMPILE)gcc -o hello_drv_test hello_drv_test.c 

clean:
	make -C $(KERN_DIR) M=`pwd` modules clean
	rm -rf modules.order
	rm -f hello_drv_test

obj-m	+= hello_drv.o

```

### 编译

```bash
book@100ask:~$ make
make -C /home/book/NezhaSTU/eLinuxCore_100ask-t113-pro/linux/ M=`pwd` modules
make[1]: Entering directory '/home/book/NezhaSTU/eLinuxCore_100ask-t113-pro/linux'
  CC [M]  /home/book/NezhaSTU/hello_drv.o
  Building modules, stage 2.
  MODPOST 1 modules
  CC [M]  /home/book/NezhaSTU/hello_drv.mod.o
  LD [M]  /home/book/NezhaSTU/hello_drv.ko
make[1]: Leaving directory '/home/book/NezhaSTU/eLinuxCore_100ask-t113-pro/linux'
riscv64-unknown-linux-gnu-gcc -o hello_drv_test hello_drv_test.c
book@100ask:~$
```



## 拷贝到开发板

怎么拷贝文件到开发板上？ 有U盘  ADB 网络 串口等等。

那么我们优先推进使用 网络方式，网络也有很多，有TFTP传输，有nfs传输，有SFTP传输，其中nfs传输需要内核支持 nfs文件系统，SFTP需要根文件系统支持 openssh组件服务，那么最终我们还是选用tftp服务。

### 使用tftp网络服务

1. 首先，需要你的ubuntu系统支持 tftp服务，已经配置并且安装好，然后讲编译出来的 helloword程序 拷贝到 tftp目录下。

```bash
book@100ask:~$ cp hello_drv_test hello_drv.ko ~/tftpboot/
```

2. 进入到开发板内，首先让开发板可以获取到IP地址，并且可以和 ubuntu系统ping通(这里指的是编译helloword主机)，之后我们在开发板上 获取 helloword 应用程序，并执行。

```bash
# udhcpc
udhcpc: started, v1.35.0
[  974.154486] libphy: 4500000.eth: probed
[  974.159083] sunxi-gmac 4500000.eth eth0: eth0: Type(8) PHY ID 001cc916 at 0 IRQ poll (4500000.eth-0:00)
udhcpc: broadcasting discover
udhcpc: broadcasting discover
[  979.331180] sunxi-gmac 4500000.eth eth0: Link is Up - 1Gbps/Full - flow control off
[  979.340154] IPv6: ADDRCONF(NETDEV_CHANGE): eth0: link becomes ready
udhcpc: broadcasting discover
udhcpc: broadcasting select for 192.168.1.47, server 192.168.1.1
udhcpc: lease of 192.168.1.47 obtained from 192.168.1.1, lease time 86400
deleting routers
adding dns 192.168.1.1
# [  992.315224] random: crng init done
[  992.319022] random: 2 urandom warning(s) missed due to ratelimiting

# tftp -g -r hello_drv.ko 192.168.1.133
# tftp -g -r hello_drv_test  192.168.1.133
# ls
hello_drv.ko    hello_drv_test  helloword
```

如上所示，我的ubuntu主机IP地址是 192.168.1.133 ，所以使用tftp 从 ubuntu获取helloword 程序，获取速度根据网速而定。

### 使用usb adb方式

* 首先将开发板OTG线连接，系统内默认启动会自动启动一个 usb adb服务，这时电脑会弹出一个设备，进入到我们的VMware虚拟机讲弹出来的设备，连接到ubuntu内。
* 这时我可以使用 adb push命令来上传文件,开始上传之前可以使用 adb devices 命令来查看开发板是否连接到系统上。
* 如下示例，使用adb命令 上传 helloword到 开发板的 /root下。

```shell
adb push helloword /root
```



## 运行

```bash
# insmod hello_drv.ko
[ 1007.072991] hello_drv: loading out-of-tree module taints kernel.
[ 1007.081285] /home/book/NezhaSTU/hello_drv.c hello_init line 70
# chmod +x hello_drv_test
# ls /dev/h
hdmi   hello
# ls /dev/hello
/dev/hello
# ./hello_drv
hello_drv.ko    hello_drv_test
# ./hello_drv_test  -w abc
[ 1060.000621] /home/book/NezhaSTU/hello_drv.c hello_drv_open line 45
[ 1060.007613] /home/book/NezhaSTU/hello_drv.c hello_drv_write line 38
[ 1060.015194] /home/book/NezhaSTU/hello_drv.c hello_drv_close line 51
# ./hello_drv_test  -r
[ 1062.312864] /home/book/NezhaSTU/hello_drv.c hello_drv_open line 45
[ 1062.319853] /home/book/NezhaSTU/hello_drv.c hello_drv_read line 30
APP read : abc[ 1062.327680] /home/book/NezhaSTU/hello_drv.c hello_drv_close line 51

#
```

