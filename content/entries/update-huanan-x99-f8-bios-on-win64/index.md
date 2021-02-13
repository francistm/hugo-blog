+++
title = "华南金牌 X99 F8 BIOS 升级"
slug = "update-huanan-x99-f8-bios-on-win64"
date = 2021-02-13T14:00:00+08:00
lastmod = 2021-02-13T14:00:00+08:00
publishDate = 2021-02-13T14:00:00+08:00
draft = true
+++

一年前因为种种原因，购入的华南金牌 X99-F8 + E5-2690v3，替换了用了也不是很久的 ASUS B150 + i5 6500。
换了之后总的来讲，只在少数场景，如同时运行多个 Hyper-V 虚拟机时有明显性能提升，其他时候并没有太明显的感受。
第一次尝试捡洋垃圾，还算没翻车。不过这捡垃圾门槛还是略高。


一直觉得这主板兼容性挺差，比如内存条位置调换后就无法开机，偶尔也会因为其他硬件重新插拔导致无法开机。看到官网有新版的 BIOS ，所以就特别想尝试一下。


官网上下载下来的 BIOS 升级包中，主要包含了两个文件，一个 fpt.exe ，一个 X99-F8.bin 文件。可是这 fpt.exe 是一个纯 DOS 的应用，看来还得想法进纯 DOS 系统才行。


一开始用企图用 CloneZilla 中的 FreeDOS 进行升级，可怎么也没找到官网截图中所示的 FreeDOS 入口。原来不能以 UEFI 模式，而是要以 MBR 模式进行引导就能看见了。偶然的机会看见 [Ventoy](https://www.ventoy.net/en/index.html) ,可以同时支持 MBR + UEFI 启动。终于成功进入 FreeDOS 。可是U盘好像因为USB驱动问题没法读取，硬盘又因为 MBR 形式无法读取 GPT 分区也无法加载。

找了一圈，发现了 [Intel_ME_System_Tools](https://github.com/mostav02/Remove_IntelME_FPT/tree/master/Intel_ME_System_Tools) 这个工具。看起来可以在 Windows 下更新了。

- X79用`v8r3`
- X99用`v9.1r7`

1. 用管理员系统Powershell
1. 运行 `.\fptw64.exe -I` 查看可更新设备的信息。如果出现异常则可能软件版本错误
1. 运行 `.\fptw64.exe -D .\OLD_BACKUP.bin` 备份当前设备固件
1. 运行 `.\fptw64.exe -F .\X99-F8.bin` 更新新的设备固件

---

可更新的Intel ME设备信息
```
Intel (R) Flash Programming Tool. Version:  9.1.10.1000
Copyright (c) 2007 - 2014, Intel Corporation. All rights reserved.

Platform: Intel(R) C610 Series Chipset
Reading HSFSTS register... Flash Descriptor: Valid

    --- Flash Devices Found ---
    W25Q128BV    ID:0xEF4018    Size: 16384KB (131072Kb)

    --- Flash Image Information --
    Signature: VALID
    Number of Flash Components: 1
        Component 1 - 16384KB (131072Kb)
    Regions:
        Descriptor - Base: 0x000000, Limit: 0x000FFF
        BIOS       - Base: 0x800000, Limit: 0xFFFFFF
        ME         - Base: 0x001000, Limit: 0x7FFFFF
        GbE        - Not present
        PDR        - Not present
    Master Region Access:
        CPU/BIOS - ID: 0x0000, Read: 0xFF, Write: 0xFF
        ME       - ID: 0x0000, Read: 0xFF, Write: 0xFF
        GbE      - ID: 0x0118, Read: 0xFF, Write: 0xFF

Total Accessable SPI Memory: 16384KB, Total Installed SPI Memory : 16384KB

FPT Operation Passed
```
