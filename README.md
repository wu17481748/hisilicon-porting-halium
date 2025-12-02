Hisilicon Droidian and Ubuntu touch 移植指南
======================

Droidian and Ubuntu touch 是一个基于 halium and libhybris 驱动硬件的 GNU/Linux 发行版，专为移动设备设计。

目标是使 linux 能在下游内核的 Android 手机上运行。

这一目标通过使用一些知名的技术实现，例如 [libhybris](https://github.com/libhybris/libhybris) 和 [halium](https://halium.org)。


您的设备最好处于 Android 9 及以上系统并有对应的安卓内核源码
------------
简单介绍一下Droidian and Ubuntu touch 移植过程
1.编译内核（使用安卓内核源码开启启动halium所需要的配置和patch内核修复）
2.打包内核镜像（打包使用安卓内核打包的各偏移参数然后使用halium的initramfs一起打包,cmdline参数droidian和ubuntu touch各有差别具体情况具体分析）
3.使用安卓内核启动对应linux系统（首次移植请修改rootfs先停用halium并放入udev规则）
4.linux系统启动后测试启动halium（halium相当于一个最简的安卓gsi系统,启动halium的目的是驱动硬件）（halium是gsi系统相当于droidian和ubuntu touch系统对应安卓版本的rootfs也是gsi除非自己编译的halium）
5.修复声音 wifi 蓝牙可以看droidian和ubuntu touch调试（文件覆盖式修复法主要用于halium系统的system vendor）
------------

内容
--------
* Droidian Porting:
  * [Droidian 移植页面](https://github.com/droidian/porting-guide/tree/zh_CN?tab=readme-ov-file)
* 当前已知的工作和支持设备
  * [Droidian 设备页面](https://devices.droidian.org)
* 移植指南
  * [内核编译](https://github.com/droidian/porting-guide/blob/zh_CN/kernel-compilation.md)
  * [调试技巧](https://github.com/droidian/porting-guide/blob/zh_CN/debugging-tips.md)
  * [Rootfs 创建](https://github.com/droidian/porting-guide/blob/zh_CN/rootfs-creation.md)
  * [软件包仓库](https://github.com/droidian/porting-guide/blob/zh_CN/host-package-repo.md)
  
--------
Ubuntu touch Porting:
 * [Ubuntu touch 旧版移植页面](https://github.com/ubports/porting-notes/wiki/Generic-system-image-(GSI))
 * [Ubuntu touch 20移植页面](https://docs.ubports.com/zh-cn/latest/about/introduction.html)
  * [Ubuntu touch 新版移植页面](https://gitlab.com/ubports/porting/community-ports)
----------------------


常规设备移植halium内核
开启halium内核配置
initramfs启动补丁
更新AppArmor补丁
android binder补丁


华为Hisilicon上的移植具有很多怪癖
1.内核源码过度定制化需要关闭冲突配置
2.不能使用initramfs启动（跳过打initramfs启动补丁）
3.bootloader传递cmdline参数  root=路径  init=/init 寻找system分区并启动系统分区根目录下的init（我们启动droidian和ubuntu必须把rootfs刷到system并放入halium的init脚本到rootfs的根目录才能正常启动系统，cmdline参数我们也可以使用内核参数强制覆盖以达到把rootfs刷到userdata启动系统)
4.usb网络rndis共享必须在init脚本阶段启动且关机充电状态下自动启动系统才有效正常启动系统无效
6.触摸屏在关机充电下自动启动系统下你会失去它（正常开机有触摸没有rndis，关机自动充电开机有rndis没有触摸）
5.需要更改wpa_supplicant服务屏蔽p2p，NetworkManager连接wifi异常，只能手动连接wifi


----------------------
其他非gki设备移植常规操作就可以了
1.编译内核开配置, 打initramfs启动补丁，打更新AppArmor补丁，打android binder补丁

----------------------


gki设备内核自行参考其他gki设备移植补丁
 *[Ubuntu touch gki移植机型页面] （https://gitlab.com/ubports/porting/community-ports/android13/oneplus-11）