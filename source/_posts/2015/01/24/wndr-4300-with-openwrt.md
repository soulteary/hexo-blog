---
title: "WNDR 4300刷机及使用建议"
date: "2015-01-24 17:20:42"
tags: ["openwrt","WNDR4300"]
---


双十一的时候，JD活动，入了WNDR4300，已经有两个多月了，入手后因为后门问题以及使用舒适状态，不自觉的就刷成了open-wrt，目前使用稳定，简单说一些建议，以免大家盲目去安装软件，当然，如果你的带宽小于百M，组网环境也小于百M，没有过多的5G设备，且没有使用NAS作为家用储存，那么请随意折腾，发挥其余热。

年末的时候因为电信的问题，把3台路由都重启了，目前的运行状态如下：

（无下载，正常浏览文本内容，有一些网络应用，手机联网）

```
型号	NETGEAR WNDR4300
固件版本	OpenWrt Barrier Breaker 14.07 / LuCI 0.12 Branch (0.12+git-222cc9c)
内核版本	3.10.49
本地时间	Sat Jan 24 16:26:12 2015
运行时间	16d 20h 32m 44s
平均负载	0.84, 0.50, 0.28
```

最近24小时内使用的主机（不包含虚拟主机），明天还能来一台XC2，想想暗爽。

```
主机名 IPv4-地址 
gameboy 10.10.1.129
iPhone  10.10.1.201
XiaoMi3 10.10.1.200
Y485    10.10.1.101
WorkMBP 10.10.1.100
NAS     10.10.1.99
```

目前的无线设置：

```
# Generic 802.11bgn Wireless Controller (radio0)  
信号: -44 dBm / 噪声: -92 dBm
94% SSID: XXXXXXXX-2G
模式: Master
信道: 11 (2.462 GHz)
传输速率: 52 Mbit/s
BSSID: XX:XX:XX:XX:XX:XX
加密: WPA2 PSK (CCMP)

# Generic 802.11an Wireless Controller (radio1)   
信号: -41 dBm / 噪声: -92 dBm
98% SSID: XXXXXXXX-5G
模式: Master
信道: 36 (5.180 GHz)
传输速率: 300 Mbit/s
BSSID: XX:XX:XX:XX:XX:XX
加密: WPA2 PSK (CCMP)
```

其实如果你不担心信息被滥用，不加密也可以，直接限定MAC LIST，不过LZ距离公司1公里出头，楼上楼下都是厂里同事，还是以防万一吧。

另外，因为对2G信号强制使用CCMP加密，会使得前几年的老设备无法访问，诸如NDS，PSP，NOKIA，老电脑（3DS没问题），Kindle，如果你遇到这个问题，可以考虑网线，或者外接子路由解决问题。

关于MAC LIST，Luci对于WNDR 4300似乎有小bug，GUI界面是不可以设置的，不过我们可以通过命令来改。

比如我将我的路由分别绑定了`[0-3].route`这4个地址。

```
ssh root@2.route
vim /etc/config/wireless
# 找到wifi-iface，在下面添加你允许的地址即可，一行一个，
# 如果有5G，会有两个，可以选择性填写，也可以复制粘贴，小数量不影响效率。
config wifi-iface
        option device 'radio0'
        option network 'lan'
        option mode 'ap'
        option encryption 'psk2+ccmp'
        option ssid 'xxxxxxxx-2G'
        option key 'console.log'
        option 'macfilter' 'allow'
        list 'maclist' 'XX:XX:XX:XX:XX:XX'
        list 'maclist' 'XX:XX:XX:XX:XX:XX'
        list 'maclist' 'XX:XX:XX:XX:XX:XX'
        list 'maclist' 'XX:XX:XX:XX:XX:XX'
        list 'maclist' 'XX:XX:XX:XX:XX:XX'
```

关于开头为何我不建议大家太折腾这个路由，原因如下：

```
root@Route:~# cat /proc/cpuinfo 
system type		: Atheros AR9344 rev 2
machine			: NETGEAR WNDR4300
processor		: 0
cpu model		: MIPS 74Kc V4.12
BogoMIPS		: 278.93
wait instruction	: yes
microsecond timers	: yes
tlb_entries		: 32
extra interrupt vector	: yes
hardware watchpoint	: yes, count: 4, address/irw mask: [0x0000, 0x0ff8, 0x0ff8, 0x0ff8]
isa			: mips1 mips2 mips32r1 mips32r2
ASEs implemented	: mips16 dsp dsp2
shadow register sets	: 1
kscratch registers	: 0
core			: 0
VCED exceptions		: not available
VCEI exceptions		: not available
```

这款CPU看似很不错了，但是，对于大流量下的复杂计算，真的捉襟见肘。

在个人内网千兆环境下，NAS通过网线拷贝到主机SSD，最大速度100MB/s（猜测存在buffer），从SSD拷贝至NAS（最大速度70~80MB/s），5G互拷（24MB/s左右），长时间（几个小时）外网满速下载(10MB/s+)时，系统负载在3~5之间，计算负荷已经很重，长时间的话，不利于设备稳定性（当然，土豪任性请无视，不过土豪为啥要买这款，而不是R系列，或其他macmini做路由...）。

另外，目前市面上的小米路由，迅雷路由等，无一不是使用luci改造后的UI来博人好感，如果你有兴趣，可以也玩一下。

最后附加一段刷机小攻略：

```
1.关闭路由电源，移除其连接的设备。
2.修改计算机IP为固定IP（192.168.1.2/255.255.255.0）。
3.将计算机连接至路由器的LAN口。
4.用牙签/曲别针等类似东西按住路由的复位键，给路由痛点，直到电源指示灯规律闪烁时松开。
5.使用tftp上传固件，服务器地址为192.168.1.1.
6.耐心等待一段时间（耐心）。
7.将电脑IP获取方式改回为DHCP（如果你没有额外设定的话）。
8.个别机器需要连同按着WPS按钮，如果无法上传固件，可以尝试。```

如果你是windows笔记本，可以考虑在windows控制面板中找到程序，再打开启动或关闭windnows功能，开启tftp客户端。 然后可以使用如下命令来更新固件，例子中先使用官方固件重置固件，在使用openwrt固件覆写。

```
D:\w4300>tftp -i 192.168.1.1 put openwrt-ar71xx-nand-wndr4300-ubi-factory.img
传输成功: 3 秒 5505153 字节，1835051 字节/秒

D:\w4300>tftp -i 192.168.1.1 put WNDR4300-V1.0.1.64PRRU.img
传输成功: 6 秒 12845189 字节，2140864 字节/秒
```

刷机完毕后，请重启，以规避首次启动，无线初始化有问题的问题（好绕口）。 重启比较慢，请耐心等待。