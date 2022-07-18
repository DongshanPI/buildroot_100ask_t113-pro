# 烧写固件至TF卡

## 准备工作
1. T113开发板主板 x1
2. Type-C线 X1
3. TF卡读卡器  x1
4. 8GB以上的 micro TF卡 x1
5. win32diskimage工具 : https://gitlab.com/dongshanpi/tools/-/raw/main/win32diskimager-1.0.0-install.exe
6. SDcard专用格式化工具：https://gitlab.com/dongshanpi/tools/-/raw/main/SDCardFormatter5.0.1Setup.exe
7. TF卡最小系统镜像：https://gitlab.com/dongshanpi/tools/-/raw/main/100ask-t113-pro_sdcard.zip

## 运行烧写软件烧写

首先需要下载  **win32diskimage  SDcard专用格式化** 这两个烧写TF卡的工具，然后获取到TF卡系统镜像文件**100ask-t113-pro_sdcard.zip**，获取到以后，先安装 **win32diskimage  SDcard专用格式化**  这两个工具，同时可以解压 一下TF卡系统的镜像文件 **dongshannezhastu-sdcard.zip**，可以得到一个  **100ask-t113-pro_sdcard.img**文件 这个文件就是我们要烧写的镜像。



* 使用SD CatFormat格式化TF卡，注意备份卡内数据。参考下图所示，点击刷新找到TF卡，然后点击 Format 在弹出的 对话框 点击 **是(Yes)**等待格式完成即可。

![SDCardFormat_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/SDCardFormat_001.png)

* 格式化完成后，使用**Win32diskimage**工具来烧录镜像，参考下属步骤，找到自己的TF卡盘符，然后点击2 箭头 文件夹的符号 找到 刚才解压的 TF卡镜像文件  **dongshannezhastu-sdcard.img** 最后 点击 写入，等待写入完成即可。

![wind32diskimage_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/wind32diskimage_001.png)


完成以后，就可以弹出TF卡，并将其插到 东山哪吒STU 最小板背面的TF卡槽位置处，此时连接 串口线 并使用 串口工具打开串口设备，按下开发板的 **RESET**复位按键就可以重启进入TF卡系统内了。

## 启动系统
![sdcard-flashsystem_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/sdcard-flashsystem_001.png)

**系统的登录用户名 root 密码为空**

**系统的登录用户名 root 密码为空**

**系统的登录用户名 root 密码为空**

