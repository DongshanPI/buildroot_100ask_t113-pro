# 使用buildroot-SDK编译构建Bootloader

* 东山哪吒STU开发板，Bootloader由4部分组成， 第一部分是 boot0 阶段，用于初始化CPU DDR UART 时钟等一些必要外设和引脚分配，之后进入第二部分，第二部分是 uboot  board.dtb 这三部分组成，为一个 boot_package.fex 文件。
* 所以Bootloader的整体的启动流程是，boot0-->u-boot-->board.dtb

## 单独编译打包第一部分

## 单独编译打包第二部分

### 单独编译 uboot

* 单独编译 uboot阶段
``` shell
book@virtual-machine:~/Neza-D1/buildroot-2021$  make uboot-rebuild V=1
```

### 单独编译 board.dtb
