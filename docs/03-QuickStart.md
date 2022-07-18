# 快速开始使用
## 上电启动系统

**注意：开发板默认都有烧录SPI NAND FLASH系统，收到开发板后，只需要两根typec线即可启动**

### 1. 连接串口线电源线
* 连接两根TypeC线分别到下图红框未知，一个是 OTG接口，用来供电和烧录功能，另一根为 串口线，用来调试输出。连接成功后，需要将底板上的电源切换开关拨至靠近 OTG口方向，此时核心板上的绿色电源灯也会亮起，灯亮起表示设备已经可以正常通电工作。
* 对于串口接口， 默认情况下系统会自动安装串口设备驱动，如果没有自动安装，可以使用驱动精灵来自动安装。

![T113-Pro_FlashSystem](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/100ask_t113_pro/T113-Pro_FlashSystem.png)


* 对于Windows系统
此时Windows设备管理器 在 端口(COM和LPT) 处会多出一个串口设备，一般是以 `USB-Enhanced-SERIAL CH9102`开头，您需要留意一下后面的具体COM编号，用于后续连接使用。

![QuickStart-01](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/QuickStart-01.png)

如上图，COM号是96，我们接下来连接所使用的串口号就是96。

* 对于Linux系统
可以查看是否多出一个/dev/tty<> 设备,一般情况设备节点为 /dev/ttyACM0  。

![QuickStart-02](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/QuickStart-02.png)

### 2. 打开串口控制台
#### 获取串口工具
使用Putty或者MobaXterm等串口工具来开发板设备。

* 其中putty工具可以访问页面  https://www.chiark.greenend.org.uk/~sgtatham/putty/  来获取。
* MobaXterm可以通过访问页面 https://mobaxterm.mobatek.net/ 获取 (推荐使用)。

#### 使用putty登录串口

![QuickStart-04](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/QuickStart-04.png)

#### 使用Mobaxterm登录串口
打开MobaXterm，点击左上角的“Session”，在弹出的界面选中“Serial”，如下图所示选择端口号（前面设备管理器显示的端口号COM21）、波特率（Speed 115200）、流控（Flow Control: none）,最后点击“OK”即可。步骤如下图所示。
**注意：流控（Flow Control）一定要选择none，否则你将无法在MobaXterm中向串口输入数据**

![Mobaxterm_serial_set_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/Mobaxterm_serial_set_001.png)




### 3. 进入系统shell
使用串口工具成功打开串口后，可以直接按下 Enter 键 进入shell，当然您也可以按下板子上的 `Reset`复位键，来查看完整的系统信息。
![bootlogs_001](https://cdn.staticaly.com/gh/DongshanPI/Docs-Photos@master/DongshanNezhaSTU/bootlogs_001.png)

**系统默认的 登录 用户名 是 root 没有密码。**

**系统默认的 登录 用户名 是 root 没有密码。**

**系统默认的 登录 用户名 是 root 没有密码。**

