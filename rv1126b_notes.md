# 目录

[video节点](#video节点)

## SDK配置

### 板级配置(通过menuconfig修改后需要savedefconfig保存)

device/rockchip/rv1126b/invision_ipc_defconfig

### 分区表

device/rockchip/rv1126b/parameter-invision.txt

### 设备树

kernel/arch/arm64/boot/dts/rockchip/invision-ipc.dts

kernel/arch/arm64/boot/dts/rockchip/invision-ipc.dts. bak

### kernel

kernel/arch/arm64/configs/invision_ipc_defconfig

### buildroot

buildroot/configs/rockchip_invision_ipc_defconfig

### overlay(buildroot-config)

buildroot/board/rockchip/rv1126b/fs-overlay-invision-ipc/

### extra-parts overlay

device/rockchip/common/extra-parts/

## video节点

具体节点与entity关系可由media-ctl -p查看

### ISP video节点

| aEntityName  | name           | ChnNum | max output              | support output fmt                    |
|:------------:|:--------------:|:------:|:-----------------------:| ------------------------------------- |
| /dev/video13 | rkisp_mainpath | 0      | 支持无极缩放            | NV12/NV16/UYVY                        |
| /dev/video14 | rkisp_selfpath | 1      | 支持缩放，width最大1920 | NV12/NV16/UYUV/ARGB888/ RGB888/RGB565 |

### VPSS video节点

| aEntityName  | name          | ChnNum | max output                    | support output fmt                                                                                                          |
|:------------:|:-------------:|:------:|:-----------------------------:|:---------------------------------------------------------------------------------------------------------------------------:|
| /dev/video22 | rkvpss_scale0 | 3      | 32倍放大或缩小                | Either Line YUV422/YUV420/YUV400 SP 8bit or Tile4x4 YUV422/YUV420 8bit or RFBC64x4 YUV422/YUV420 8bit                       |
| /dev/video23 | rkvpss_scale1 | 2      | 32倍放大或缩小                | Either Line RGB888/ARGB888/RGB565/YUV422/YUV420/YUV400 SP 8bit or Tile4x4 YUV422/YUV420 8bit or RFBC64x4 YUV422/YUV420 8bit |
| /dev/video24 | rkvpss_scale2 | 4      | 32倍放大或缩小，width最大1920 | Line YUV422/YUV420/YUV400 SP 8bit                                                                                           |
| /dev/video25 | rkvpss_scale3 | 5      | 32倍放大或缩小，width最大1920 | Line YUV422/YUV420/YUV400 SP 8bit                                                                                           |

### buildroot仅安装

buildroot重新生成target和staging，还需要取消buildroot不需要的配置和删除build目录下不需要的包

```shell
#!/bin/bash
# clean_target_only.sh
set -e
echo "=== Removing target ==="
rm -rf target
rm -rf staging
echo "=== Delete all .stamp_target_installed stamp files ==="
find build -name ".stamp_target_installed" -delete

echo "=== Optional: also clean staging stamp ==="
find build -name ".stamp_staging_installed" -delete
echo "Done. Now run: make"
```

### SDK v1.2.1版本问题

- IPC_64_V12版本蓝牙不工作，可参考UVC_32_V12版本修改内核蓝牙配置，打开SERIAL_DEV_BUS和BT_HCIUART_ROCKCHIP，BT_HCIUART改为M

- 编译lua-nginx-module-0.10.31需要设置环境变量，否则找不到 LuaJIT with FFI

  ```shell
  export LUAJIT_LIB=/home/zmq/rv1126b/rockchip/1.2.1/1126B_Linux/buildroot/\output/rockchip_invision_ipc/rockchip_invision_ipc/staging/usr/lib
  ```
