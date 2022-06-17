# Buildoot For 100ask-t113-pro board.

* 运行环境，此套构建系统基于全志T113-S3 芯片，适配了buildroot 2022lts主线版本，兼容了百问网的项目课程以及相关组件，真正做到了低耦合，高可用，使用不同的buildroot external tree规格，讲不同的项目 不同的组件分别管理，来实现更容易上手 也更容易学习理解。

## 使用说明

* 运行环境配置： 此系统基于ubuntu18.04进行验证，需要安装如下依赖。

```bash
 sudo apt-get install -y which sed make binutils build-essential  gcc g++ bash patch gzip bzip2 perl  tar cpio unzip rsync file  bc wget python ncurses5  bazaar cvs git mercurial rsync scp subversion android-tools-mkbootimg
```

* 安装完成后，执行如下命令进行开始编译操作。
  * 如下示例编译的为 SD Card系统镜像，如果需要编译 spi nand flash镜像，请修改配置文件为 `100ask_t113-pro_spinand_core_defconfig`


```bash
git clone  https://github.com/DongshanPI/buildroot-100ask_t113-pro
cd buildroot-100ask_t113-pro/
git submodule update --init --recursive
git submodule update --recursive --remote
cd  buildroot/
make  BR2_EXTERNAL="../br2t113pro ../br2lvgl "  100ask_t113-pro_sdcard_core_defconfig
```

* 对于国内无法访问github的同学，可以使用如下命令。

```bash
git clone  https://gitee.com/weidongshan/buildroot-100ask_t113-pro
cd buildroot-100ask_t113-pro/
git submodule update --init --recursive
git submodule update --recursive --remote
cd  buildroot/
make  BR2_DL_DIR=../Download  BR2_EXTERNAL="../br2t113pro  ../br2lvgl"  100ask_t113-pro_sdcard_core_defconfig
```

编译完成后会在 output/images目录下输出 sdcard.img文件，将文件拷贝到Windows系统下使用 wind32diskimage烧写，或者使用dd if 烧录到tf卡内，

之后插到开发板上，即可启动，注意这套系统默认使用的是 PB6 PB7 也就是UART3为默认的串口。



如果您编译的是 SPI NAND FLASH镜像，请使用 全志官方的  AllwinnertechPhoeniSuit 进行烧写。

