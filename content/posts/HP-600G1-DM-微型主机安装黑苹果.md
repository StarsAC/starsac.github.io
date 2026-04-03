+++
date = 2024-05-14T19:18:11+08:00
title = "HP 600G1 DM 微型主机安装黑苹果"
description = "HP 600G1 DM 微型主机安装黑苹果"
slug = "HP600G1DM-install-Hackintosh"
tags = ["PC","Hardware"]
+++

![HP600G1](https://resources.starsac.cn/2026/04/hp600g1.jpg)

## 0.硬件选择

### 01.准系统选择

在2024年3月节点, 200元左右价位可选的小主机准系统大概有以下几种型号 :

* 联想 : m73, m73p, m93p, m7300hq

  * 其中m7300hq为m73的国内版本

  > m73的黑苹果算是比较成熟的, 网上的教程也很多, 但是这台机器由于BIOS有网卡白名单机制, 如果更换黑苹果免驱网卡需要刷BIOS, 较为麻烦且风险较大, 如果不动BIOS在开机的时候会有报警提示音, 因此只能拔掉喇叭使用. 此外m73没有m.2固态硬盘位, 拓展性稍差.

* 戴尔 :

  * Optiplex 3020m

  > 戴尔的机器价格比较贵, 支持4代CPU的大概只有这一个选择, 且这个机器存量较少, 性价比低.

* 惠普 : 

  * [HP ProDesk 400 G1 微型台式电脑](https://support.hp.com/cn-zh/product/details/hp-prodesk-400-g1-desktop-mini-pc/7519860)
  * [HP ProDesk 600 G1 微型台式电脑](https://support.hp.com/cn-zh/product/product-specs/hp-prodesk-600-g1-desktop-mini-pc/6595197)
  * [HP EliteDesk 800 G1 微型台式电脑](https://support.hp.com/cn-zh/product/details/hp-elitedesk-800-g1-desktop-mini-pc/6595205)

  > 三款机型差别如下, 其他配置参照官网文档

|   |芯片组|USB数量|DP接口数量|
|---|---|---|---|
|400G1|H81|2\*3.0 + 4\*2.0|1|
|600G1|Q85|4\*3.0 + 2\*2.0|2|
|800G1|Q87|6\*3.0|2|

三款机型中400G1和600G1为同一价位, 800G1稍贵, 综合来看, 600G1性价比更高一些. 400G1支持TDP最高35W的四代CPU, 上TDP更高的CPU会开机报错, 不过可以按F1键跳过报错继续开机. 三款机型均支持nvme硬盘, 但不支持开机引导, 后期可以通过魔改BIOS来实现nvme硬盘引导.

综合考虑, 600G1性价比较高. 最终选择600G1准系统 + 原装65W电源.

### 02.CPU选择

当前时间节点四代i3的T后缀处理器价格为25-50元不等, 四代i5的T后缀处理器价格要高一倍不止, 遂最终选择i3-4360T, 主机原生支持, 不用修改BIOS.

### 03.内存选择

当前时间节点DDR3代笔记本内存已经相当便宜, 最终选择单条8GB三星DDR3笔记本内存.

### 04.硬盘选择

小主机仅原生支持AHCI协议的PCIe固态硬盘, 这种硬盘现已被淘汰且存货量稀少,二手价格高昂. 支持NVMe引导需要修改BIOS较为麻烦, 遂最终选择120GB三星SATA固态.

### 05.网卡选择

由于要装黑苹果系统, 网卡选择范围很小, 基本只有Broadcom的几款黑苹果免驱显卡. 最终选择BCM943224 + NGFF转接板 + 内置天线.

## 1.前期准备

### 11.下载写盘工具

[BalenaEtcher](https://etcher.balena.io/)

> 如果官网下载缓慢可以到[Github](https://github.com/balena-io/etcher)或其镜像站点下载

### 12.下载恢复镜像

* [黑果小兵](https://blog.daliansky.net/)上有很多懒人镜像, 赞助作者后即可下载.

* 根据[OpenCore](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/)的官方指南(英文)自行下载恢复镜像进行配置.

  > 注意: 此方法安装时需要联网下载完整版安装镜像, 对网络环境要求较高.

### 13.刷写U盘

> ***准备一个16GB以上容量的U盘, 确保U盘中所有资料已备份后再进行操作!!!***

打开BalenaEtcher, 选择USB闪存盘和下载好的镜像, 点击开始烧录即可.

### 14.制作EFI

* [RapidEFI](https://github.com/JeoJay127/RapidEFI-Tool)项目可使我们通过简单明了的图形化界面一键配置EFI, 只需选择对应的CPU平台, 处理器代数, 一些对应硬件参数即可一键生成EFI文件.

* 参考官方的[OpenCore配置文档](https://dortania.github.io/OpenCore-Install-Guide/)进行配置.

* 一些优质的参考资源

  * <https://github.com/Road-tech/Hackintosh_HP-800G1_I7-4770hq_OC>
  * <https://github.com/LemoFire/OpenCore-HP-800G1-DM-HD4600>
  * <https://github.com/shidongfei2/800g1>

  * <https://github.com/vikingcabin/Hackintosh-HP-600G1-DM-BigSur-OC>
  * <https://github.com/LomotHo/Hackintosh-600g1-DM-4670t>
  * <https://github.com/nidemimihihi/600G1-DM-BigSur-OC> 

  > 如果安装BigSur级以上系统, 请通过USBToolBox定制USB.


## 2.系统安装

### 21.BIOS设置

首先将其更新到HP支持官网提供的最新版本(2019年发布的那版), 可先安装Win10系统, 在系统中对BIOS进行更新

> ***注意 : 强烈建议更新完BIOS后手动按主板上的的CMOS清除按键进行重置***

这个机器的BIOS设置项目非常非常少, 基本上保持默认即可, 记得修改U盘启动为第一启动项. 

修改BIOS使其支持NVMe硬盘引导启动:<https://juejin.cn/post/7166643141179605022>

### 22.进入OpenCore引导

如果配置无误, 现在开机应该已经可以进入OpenCore的引导界面了.

> 如果没有配置过主题, 那么界面应该是黑色的命令行界面, 通过键盘上下键来选择启动项.
>
> 如果配置过主题, 那么界面应该是有背景的图形界面, 可以用鼠标选择启动项.

> 没有MacOS相关的启动选项? 可以先尝试按空格呼出隐藏选项(`____`之类的选项也可能是安装选项), 如果还没有, 可能需要重新烧录安装镜像.

选择与MacOS有关的选项进入.

> 如果遇到问题, 请首先尝试更换EFI, 如果仍然无法解决, 请尝试硬重置BIOS(按主板按键), 如果都卡在某一个地方代码不动, 那么可以参考官方的OpenCore文档, 里面有各种错误的排错指南.

### 23.安装macOS

如果一切顺利, 在经过一段时间的跑码之后就可以进入macOS的恢复菜单了.

这里可以在菜单栏处找到磁盘工具, 使用APFS格式抹掉磁盘. 然后就可以在安装器窗口看到你刚刚抹掉的磁盘了.

> ***注意: 请务必备份硬盘里的重要资料后再进行操作.***

接着就是选择刚刚创建的磁盘, 开始正式安装macOS.

> 如果你和我一样安装的时候闲着无聊, 可以从左上角打开安装器日志, 体验看跑码的快乐(*^▽^*).

在安装过程中, 电脑会经历数次重启, 在重启时启动项会发生变化, 请***务必不要***在显示OpenCore引导菜单时更改默认启动项, 否则将会导致安装失败.

如果一切顺利, 安装完成之后应该进入到了选择语言的界面, 之后的流程(OOBE)可以按照自己的习惯进行设置, 但请务必注意以下几点:

* OOBE时可以选择联网, 也可以不联网, 但在此时请不要登录iCloud及其相关在线服务, 跳过就好.
* 设置用户名时请***务必不要***设置中文, 且尽量设置一个自己将会长期使用的名字, 如果后期再修改用户名将会非常麻烦.
* 保存好自己设置的密码.

在进入桌面后, 你可以检查各项硬件功能是否驱动成功, 如USB, 声卡, 显卡, 蓝牙, WIFI等是否正常工作, 如果硬件工作有问题, 请使用`OpenCore Configurator`挂载EFI分区, 并更换EFI文件.

### 24.安装OpenCore Configurator

在[Github页面](https://github.com/HackintoshFans/OpenCoreConfigurator)下载软件, 并在macOS安装.

在OpenCore Configurator里可以查看各个硬件的工作状态, 修改EFI文件, 挂载EFI分区, 功能十分强大.

### 25.补全三码

在刚装好系统的时候, 如果你尝试登录iCloud服务, 会发现是登不上去的, 究其原因就是我们还没有补全三码, 苹果服务器不信任我们的设备.

打开OpenCore Configurator, 在右上角菜单栏选择挂载EFI分区

> 强烈建议将此时的EFI分区备份到桌面, 以防止EFI在编辑过程中损坏等情况.

使用OpenCore Configurator打开`config.plist`文件, 在PlatformInfo选项下选择机型并点击Generate生成UUID, ROM(选择Mac,from System),序列号等信息, 点击保存即可. 删除原EFI分区中的文件, 并将修改好的EFI放入分区中.

> 在生成序列号时, 可以前往苹果官网[查看设备的保障信息](https://checkcoverage.apple.com/)查询序列号有效性, 一般来说, **有效且未被使用的序列号 > 无效的序列号 > 已经被使用的序列号**, 对于较老的机型, 刷到有效且未被使用的序列号可能性很小, 一般情况下使用无效序列号即可.

接下来请重启, 此时可以移除安装U盘, **在OpenCore引导菜单里按空格并选择CleanNVRAM**, 之后回到引导主菜单再次启动即可.

进入系统后可以尝试登录iCloud, 如果还是提示`ICLOUD_UNSUPPORTED_DEVICE`之类的报错信息, 那么可能说明你的AppleID不太好, 请尝试更换符合下面要求的AppleID账号:

* 至少绑定一个苹果设备(iPhone, iPad, mac)且正常使用至少一个月时间.
* 账号最近有iCloud使用记录, 最近在AppStore下载过软件(最好有购买记录)
* 绑定了至少一种支付方式(微信, 支付宝, 银联等)

经过以上调整, 基本上就可以正常登录使用iCloud服务了.

## 3.系统完善

恭喜你, 经过以上的操作, 你的黑苹果主机已经可以正常使用了, 但是如果想要更加接近于白苹果的体验, 你还可以进行下面的操作.

* 关闭"啰嗦模式", 显示原汁原味的苹果进度条

  和之前修改三码的步骤相同, 打开`config.plist`文件, 在NVRAM选项卡里点击UUID查找里面的`boot-args`键, 将值里面的`-v`删除, 覆盖EFI分区, 重启清理NVRAM之后就可以看到跑代码的界面已经消失.

* 添加`Audio.dxe`文件, 让黑苹果小主机开机时发出"Duang"的苹果开机音效.

​	这部分请参考[OpenCore开机音频设置教程！_opencore configurator hdmi声音-CSDN博客](https://blog.csdn.net/imacosx_cn/article/details/128883830)

* 开启HiDPi, 让黑苹果在高分屏下的体验更好

  这部分请参考[黑苹果 - 国光的黑苹果安装教程：手把手教你配置 OpenCore (sqlsec.com)](https://apple.sqlsec.com/7-完美黑果/7-1/#hidpi)

* 解决睡眠睡死问题, 优化体验

  简单粗暴的方法: 在**系统偏好设置-节能**里面设置显示器关闭时间, 并设置永不休眠, 可以达到类似睡眠的效果.

  如果想彻底解决, 请参考[Fixing Sleep | OpenCore Post-Install (dortania.github.io)](https://dortania.github.io/OpenCore-Post-Install/universal/sleep.html)

还有更多玩法, 期待你的探索.

## 4.附录

黑苹果的实现要归功于开源社区的无数开发者们的共同努力, 是他们让我们每一个人不用付昂贵的价钱购买苹果设备也可以低成本体验精彩的macOS和苹果生态.

2019年, 苹果发布了macOS11 BigSur系统和M1芯片并宣布转向ARM架构. 这个里程碑不仅标志着有15年历史的OSX的落幕, 同时也意味着黑苹果的生命进入了倒计时. 截至目前, macOS14 Sonoma依然提供x86架构的版本, 2019年Intel Xeon W处理器的MacPro依然在生命周期内, 但是再过几年呢? 苹果完全转向ARM之日, 就是黑苹果灭亡之时.

虽然黑苹果的技术终究会在历史中消亡, 但开源精神和"生命不息, 折腾不止"的意志定会激励一代代开发者们在计算机领域做出更大的贡献. 

向OpenCore, Clover的开发人员,以及国内外为这项开源技术做出不懈贡献的伟大开发者们致敬 !

一些高质量的教程和参考资料:

[主页 - 国光的黑苹果安装教程：手把手教你配置 OpenCore (sqlsec.com)](https://apple.sqlsec.com/)

[OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/)

[黑果小兵的部落阁 (daliansky.net)](https://blog.daliansky.net/)

平台:

[GitHub: Let’s build from here · GitHub](https://github.com/)

[哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://www.bilibili.com/)
