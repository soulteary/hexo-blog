---
title: "家庭网络环境折腾笔记二"
date: "2014-12-05 02:41:59"
tags: ["MY CLOUD","NAS","家用网络"]
---


订的屌丝版NAS到了，于是开始继续折腾，目前效果还算良好，记录一些细节，或许会帮到相同硬件的人。

购入设备是WD MY CLOUD 3T，这个NAS有个毛病，是总是只自作主张的扫描磁盘上的数据进行分类，那么，我们要先关了它：

```zsh
update-rc.d wdphotodbmergerd disable
update-rc.d wdmcserverd disable
```

因为目前没有备份的NAS，不想用手头的做实验，虽然降级了，安装了aria，但是考虑数据安全，果断又升级回去了...

升级降级最简方法(ref:yunnas论坛yoogo)

先按照下面的命令修改系统版本号，然后WEB界面上传旧固件即可，但是如果固件版本高于**04.00.00-xxx**，那么需要操作两次，第一次先降级到**sq-040000-607-20140630.deb**，第二次再降级到**sq-030401-230-20140415.deb**。

具体操作：

1.先修改版本号，登录WEB界面打开SSH开关。

```zsh# 直接终端连就好
ssh root@路由分配给NAS的IP地址
# 默认密码是welc0me

# 查看当前系统版本
WDMyCloud:~# cat /etc/version
04.00.01-623

# 系统降级
WDMyCloud:~# echo "02.02.02-111">/etc/version
# 查看版本是否修改ok
WDMyCloud:~# cat /etc/version
02.02.02-111
```

2.到web控制台，设置-固件-手动更新，选择文件：**sq-030401-230-20140415.deb**。

如果需要两次降级，那么再次ssh到机器上的时候，如果非windows，记得删除**用户目录/.ssh/known_hosts**中的之前的旧的key。

这两个固件的下载地址：

*   http://download.wdc.com/nas/sq-040000-607-20140630.deb.zip
*   http://download.wdc.com/nas/sq-030401-230-20140415.deb.zip

相关测试数据更新在Github上了，稍后更新一些虚拟机构建脚本。

*   仓库地址：https://github.com/soulteary/CodesPlan
*   有线使用：https://github.com/soulteary/CodesPlan/blob/master/lan.md
*   无线使用：https://github.com/soulteary/CodesPlan/blob/master/wifi.md
