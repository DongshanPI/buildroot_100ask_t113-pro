# 使用buildroot-SDK编译构建rootfs

## 单独编译配置busybox



``` shell
book@virtual-machine:~/Neza-D1/buildroot-2021$ make  busybox-menuconfig
```

### 清理无效缓存 重新打包

``` shell
rm -rf  output/target;    find output/build/ -name .stamp_target_installed | xargs rm ; make
```