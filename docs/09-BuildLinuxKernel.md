# 使用buildroot-SDK编译打包Linux Kernel
### 单独编译各个部分





* 单独编译 kernel阶段
``` shell
book@virtual-machine:~/Neza-D1/buildroot-2021$  make linux-rebuild V=1
```

* 单独编译文件系统
  * 指定完成工具链 系统配置 需要安装的包 以及所需的格式 执行如下命令，最后生成的镜像在 output/image目录下。
``` shell
book@virtual-machine:~/Neza-D1/buildroot-2021$ make  all //完整编译系统
```
