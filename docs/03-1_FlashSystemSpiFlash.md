# 烧写固件至SPI NAND

**注意此方式烧录进的文件系统是ubifs文件系统，如果操作 需要网络文件系统挂载或者使用TF卡，不推荐使用。**

## 准备工作

1. 东山哪吒STU开发板主板 x1
2. 下载全志线刷工具AllwinnertechPhoeniSuit： https://gitlab.com/dongshanpi/tools/-/raw/main/AllwinnertechPhoeniSuit.zip
3. TypeC线 X2
4. 下载SPI NAND最小系统镜像：https://gitlab.com/dongshanpi/tools/-/raw/main/buildroot_linux_nand_uart3.zip
5. 下载全志USB烧录驱动：https://gitlab.com/dongshanpi/tools/-/raw/main/AllwinnerUSBFlashDeviceDriver.zip

## 连接开发板

* 参考下图所示，将两个TypeC线分别连至 开发板 串口接口 与 OTG烧写接口，另一端 连接至 电脑USB接口，连接成功后，可以将下载好的 烧写工具和 SPI NAND最小系统镜像解压缩 使用。

![T113-Pro_FlashSystem](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/100ask_t113_pro/T113-Pro_FlashSystem.png)



* 使用镊子或者杜邦线短接 核心板 SPI NAND FLASH  5-6脚，也就是 MOSI 与SCLK，短接的同时，可以按下 底板上的 RESET 按键，这个时候开发板会进入到FEL烧写模式，进入烧写模式后，我们就可以继续往下安装 专门的烧录驱动。

![T113-Pro_FlashSystem_002](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/100ask_t113_pro/T113-Pro_FlashSystem_002.png)


## 安装usb驱动

在我们连接好开发板以后，先按住 **FEL** 烧写模式按键，之后按一下 **RESET** 系统复位键，就可以自动进入烧写模式。

这时我们可以看到设备管理器  **通用串行总线控制器** 弹出一个  未知设备 ，这个时候我们就需要把我们提前下载好的 **全志USB烧录驱动** 进行修改，然后将解压缩过的 **全志USB烧录驱动**  压缩包，解压缩，可以看到里面有这么几个文件。

```bash
InstallUSBDrv.exe
drvinstaller_IA64.exe
drvinstaller_X86.exe
UsbDriver/          
drvinstaller_X64.exe   
install.bat
```

对于wind7系统的同学，只需要以管理员 打开   `install.bat` 脚本，等待安装，在弹出的 是否安装驱动的对话框里面，点击安装即可。

对于wind10/wind11系统的同学，需要在设备管理器里面进行手动安装驱动。

如下图所示，在第一次插入OTG设备，进入烧写模式设备管理器会弹出一个未知设备。

![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_001.png)

接下来鼠标右键点击这个未知设备，在弹出的对话框里， 点击浏览我计算机以查找驱动程序软件。

![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_002.png)

之后在弹出新的对话框里，点击浏览找到我们之前下载好的 usb烧录驱动文件夹内，找到 `UsbDriver/` 这个目录，并进入，之后点击确定即可。

![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_007.png)

注意进入到  `UsbDriver/`  文件夹，然后点击确定，如下图所示。

![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_003.png)

此时，我们继续点击 **下一页** 按钮，这时系统就会提示安装一个驱动程序。 
在弹出的对话框里，我们点击 始终安装此驱动程序软件 等待安装完成。

![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_004.png)

安装完成后，会提示，Windows已成功更新你的驱动程序。

![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_005.png)

最后我们可以看到，设备管理器 里面的未知设备 变成了一个 `USB Device(VID_1f3a_efe8)`的设备，这时就表明设备驱动已经安装成功。

![Windows_FlashDevice_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Windows_FlashDevice_006.png)


## 运行软件烧写

将下载下来的全志线刷工具 **AllwinnertechPhoeniSuit** 解压缩，同时将**SPI NAND最小系统镜像**下载下来也进行解压缩。

解压后，得到一个 **buildroot_linux_nand_uart3.img** 镜像，是用于烧录到SPI NAND镜像得。另一个是**AllwinnertechPhoeniSuit**文件夹。

首先我们进入到 **AllwinnertechPhoeniSuit\AllwinnertechPhoeniSuitRelease20201225** 目录下 找到 **PhoenixSuit.exe** 双击运行。

打开软件后 软件主界面如下图所示

![PhoenixSuit_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/PhoenixSuit_001.png)


​	接下来 我们需要切换到 **一键刷机**窗口，如下图所示，点击红框标号1，在弹出的新窗口内，我们点击 红框2 **浏览** 找到我们刚才解压过的 SPI NAND 最小系统镜像  **buildroot_linux_nand_uart3.img**，选中镜像后，点击红框3 **全盘擦除升级** ，最后点击红框4  **立即升级**。

​	点击完成后，不需要理会 弹出的信息，这时 我们拿起已经连接好的开发板，先按住 **FEL** 烧写模式按键，之后按一下 **RESET** 系统复位键，就可以自动进入烧写模式并开始烧写。

![PhoenixSuit_002](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/PhoenixSuit_002.png)


​	烧写时会提示烧写进度条，烧写完成后 开发板会自己重启。

![PhoenixSuit_003](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/PhoenixSuit_003.png)


## 启动系统

一般情况下，烧写成功后 都会自动重启 启动系统，此时我们进入到 串口终端，可以看到它的启动信息，等所有启动信息加载完成，输入 root 用户名即可登录烧写好的系统内。

![spinand-flashsystem_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/spinand-flashsystem_001.png)

**系统的登录用户名 root 密码为空**

**系统的登录用户名 root 密码为空**

**系统的登录用户名 root 密码为空**

