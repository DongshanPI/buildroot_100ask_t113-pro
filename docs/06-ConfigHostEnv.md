# 安装并配置开发环境


## 获取虚拟机系统

### 下载vmware虚拟机工具

使用浏览器打开网址    https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html   参考下图箭头所示，点击下载安装 Windows版本的VMware Workstation ，点击 **DOWNLOAD NOW**  即可开始下载。

![vmwareworkstation_download_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/vmwareworkstation_download_001.png)

下载完成后全部使用默认配置一步步安装即可。



### 获取Ubuntu系统镜像

* 使用浏览器打开  https://www.linuxvmimages.com/images/ubuntu-1804/     找到如下箭头所示位置，点击 **VMware Image** 下载。

![linuxvmimage_downlaod_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/linuxvmimage_downlaod_001.png)

下载过程可能会持续 10 到 30 分钟，具体要依据网速而定。



## 运行虚拟机系统

1. 解压缩 虚拟机系统镜像压缩包，解压缩完成后，可以看到里面有如下两个文件，接下来，我们会使用 后缀名为 .vmx 这个 配置文件。

![ConfigHost_003](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/ConfigHost_003.png)

2.  打开已经安装好的 vmware workstation 软件 点击左上角的 **文件** --> **打开** 找到上面的 Ubuntu_18.04.6_VM_LinuxVMImages.COM.vmx  文件，之后会弹出新的虚拟机对话框页面。

![ConfigHost_004](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/ConfigHost_004.png)

3. 如下图所示为  为我们已经虚拟机的配置界面，那面我们可以 点击 红框 2 编辑虚拟机设置 里面 去调正 我们虚拟机的 内存 大小 和处理器个数，建议 最好 内存为 4GB 及以上，处理器至少4 个。 调整好以后，就可以 点击 **开启此虚拟机** 来运行此虚拟机了

![ConfigHost_005](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/ConfigHost_005.png)

第一次打开会提示  一个 虚拟机已经复制的 对话框，我们这时，只需要 点击  我已复制虚拟机  就可以继续启动虚拟机系统了。

![ConfigHost_006](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/ConfigHost_006.png)

等待数秒，系统就会自动启动了，启动以后 鼠标点击   **Ubuntu**  字样，就可以进入登录对话框，输入  密码 ubuntu 即可登录进入ubuntu系统内。

注意： 

**Ubuntu默认的用户名密码分别为 ubuntu ubuntu** 

**Ubuntu默认的用户名密码分别为 ubuntu ubuntu** 

**Ubuntu默认的用户名密码分别为 ubuntu ubuntu** 

**ubuntu默认需要联网，如果你的 Windows电脑已经可以访问Internet 互联网，ubuntu系统后就会自动共享 Windows电脑的网络 进行连接internet 网络。**



### 配置开发环境



* 安装必要软件包, 鼠标点击进入 ubuntu界面内，键盘同时 按下 **ctrl + alt + t** 三个按键会快速唤起，终端界面，唤起成功后，在终端里面执行如下命令进行安装必要依赖包。

```bash
sudo apt-get install -y  sed make binutils build-essential  gcc g++ bash patch gzip bzip2 perl  tar cpio unzip rsync file  bc wget python  cvs git mercurial rsync  subversion android-tools-mkbootimg vim  libssl-dev  android-tools-fastboot
```

如果你发现你的ubuntu虚拟机 第一次启动 无法 通过 windows下复制 命令 粘贴到 ubuntu内，则需要先手敲 执行如下命令 安装一个 用于 虚拟机和 windows共享剪切板的工具包。

```bash
sudo apt install open-vm-tools-desktop 
```

![ConfigHost_007](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/ConfigHost_007.png)

安装完成后，点击右上角的 电源按钮，重启ubuntu系统，或者 直接输入 sudo reboot 命令进行重启。

这时就可以 通过windows端向ubuntu内粘贴文件，或者拷贝拷出文件了。

![ConfigHost_008](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/ConfigHost_008.png)

做完这一步以后，就可以继续往下，获取源码 开始RISC-V 东山哪吒STU开发板的开发之旅了。
