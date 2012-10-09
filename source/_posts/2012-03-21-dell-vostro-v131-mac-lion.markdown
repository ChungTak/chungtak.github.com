---
layout: post
title: "Dell Vostro v131 mac lion 完美安装"
date: 2012-03-21 20:29
comments: true
categories: 
---

安装黑苹果真的很折腾，近来同事在v131上完美安装了lion，足足花了一天，不断的重启不断滴尝试。建议想用苹果的还是直接买白苹果省心。
###安装情况###
CPU:Core i3 （完美）
主板: Intel HM 55 （完美）
显卡:HD Graphics 3000  （QE/CI完美）
声卡  （完美无爆音）
集成 摄像头/Mic  （完美）
2x USB 3.0 （完美）
1 USB 2.0  （完美）
触摸板：（两点触摸 完美）
SD Card reader(完美)
蓝牙(无驱动)
WiFi(无驱动) 这款卡太老，没驱动的，建议买个usb网卡替代。[EDUP EP-N8513 隐藏型无线网卡](http://www.360buy.com/product/389343.html)
<!--more-->
###安装注意事项###
安装基础教程这里就不说了，pcbeta上有写得非常好的 。[lion图文详细安装教程](http://bbs.pcbeta.com/viewthread-920341-1-1.html)

这里指出这款机器安装的特别要注意的地方(10.8支持很好，不需以下步骤)
1.  保证所有硬盘卷轴不能有中文，不然会导致安装界面鼠标键盘不能用
2.  如果安装界面usb鼠标键盘仍然不能用，把/System/Library/Extensions/AppleIntel** 、 Ati* 、Geforce*** 和 NV** 驱动都删掉
3.  安装的时候usb要用左边的usb2.0接口，右面的usb3.0接口要进入系统后安装相应驱动才能使用
4.  使用-f -v -x进入

_经同事的不懈努力测试，按上面方法，系统绝对能正常安装。_

安装好后第一件事就是把AppleIntel驱动放到/System/Library/Extensions，**但严重注意这些驱动不是安装盘的AppleIntel**
去pcbeta上搜索一下lion的原版驱动，必须包含AppleIntelHD3000Graphics**开头的驱动（安装盘上没这些，正常不删文件安装是有这些驱动的）


###必备软件###
1.  [10.7.3 Combo Update](http://support.apple.com/kb/DL1484)   (升级包)
2.  [MultiBeast-4.3.1 for lion](http://www.tonymacx86.com/C:Dq-OCPpT/MultiBeast-4.3.1.zip)           （驱动包）
3.  Kext Utility          (驱动权限重建MultiBeast自带)
4.  SleepEnabler.kext     解决睡眠问题
5.  VoodooBattery.kext    电池显示

###驱动安装###
+   先安装_10.7.3 Combo Update_ ，安装完后**不要重启!!**
+   运行MultiBeast,选择下面提到的驱动
``` tex
System Utilities
Drivers & Bootloaders -> Kexts & Enablers -> Miscellaneous -> ElliottForceLegacyRTC
Drivers & Bootloaders -> Kexts & Enablers -> Miscellaneous -> EvOreboot
Drivers & Bootloaders -> Kexts & Enablers -> Miscellaneous -> FakeSMC
Drivers & Bootloaders -> Kexts & Enablers -> Miscellaneous -> IOUSBFamily Rollback
Drivers & Bootloaders -> Kexts & Enablers -> Miscellaneous -> NullCPUPowerManagement
Drivers & Bootloaders -> Kexts & Enablers -> Miscellaneous -> USB 3.0
Drivers & Bootloaders -> Kexts & Enablers -> Miscellaneous -> PS/2 Keyboard/Mice...
Drivers & Bootloaders -> Kexts & Enablers -> Network - > Lnx2Mac's Realtek...
Customization -> System Definitions -> MacBook Pro - > MacBook Pro 8,1
OSx86 Software -> 全选
```
+   点击下一步、继续继续。安装完后，**不要重启**。

+   在运行一次MultiBeast，只选择声卡驱动
``` tex
Drivers & Bootloaders -> Kexts & Enablers -> Audio -> Universal -> VoodooHDA .2.7.3
```
删除自带的AppleHDA

``` bash
rm -r AppleHDA.kext   (在/System/Library/Extensions)
```

+   安装睡眠和电池驱动
SleepEnabler.kext和VoodooBattery.kext

+   使用Kext Utility重建缓存
+   重启，使用-f参数

按上面的步骤重启后，系统声卡、显卡、网卡、usb3.0都能驱动的了。

现在右侧的USB3.0接口可以完美使用了，但是左侧的USB2.0却不能使用。
要驱动左侧的USB2.0和SD读卡器，需要删除AppleHPET.kext（**要备份**，这个在我机器上有效，其他机器不一定）

###显示器亮度###
安装好的系统不能调节亮度，需要在dsdt上添加几行代码
推荐使用dsdt editor 导出dsdt，该软件能自动fix错误和编译。
在dsdt中搜索PWRB ，添加以下这个Device (PNLF)方法，原来的内容不要删

```
Device (PWRB)
{
         Device (PNLF)
         {
                  Name (_HID, EisaId ("APP0002"))
                  Name (_CID, "backlight")
                  Name (_UID, 0x0A)
                  Name (_STA, 0x0B)
         }
}
```
编译后把dsdt.aml放到/Extra下，重启就可以在显示器里调节亮度了,不过每次重启后又要重新调节。
不要用ACPIBacklight.kext，用了反而调节不了。不知道为何·~求解~~
