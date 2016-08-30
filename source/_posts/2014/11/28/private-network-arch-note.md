---
title: "家用网络环境折腾笔记"
date: "2014-11-28 20:19:54"
tags: ["家用环境","开发环境"]
---


发一篇没有折腾完，但是折腾完对自己蛮有意义的东西。

## 折腾目标：

```
搞定家用学习/娱乐环境。
```

*   满足切换设备无缝写码需求。
*   满足切换设备无缝调试需求。
*   构建自己的发布流程。
*   添加自己想要的持续集成工具。
*   满足高速家用网络传输需求。
*   满足偶尔无聊的游戏需求。

## [设备清单](https://github.com/soulteary/CodesPlan#%E8%AE%BE%E5%A4%87%E6%B8%85%E5%8D%95)

省钱(屌丝)方案:

*   杭州电信100M
*   Lenovo Y485(2012年，U换A10，添双SSD，16G内存)
*   MacBook Pro (2013年，i7 2GHz，8G内存，Retina)
*   NETGEAR WNDR4300(全千兆，双频，双128MB)
*   TL-MR12U
*   2412Hx2(DP+MiniDP+HDMI) + 2312HM(HDMI+VGA)
*   几台可联网的娱乐设备

## [需求分析](https://github.com/soulteary/CodesPlan#%E9%9C%80%E6%B1%82%E5%88%86%E6%9E%90)

*   因为工作环境需求，Mac也依旧是OSx。
*   构建发布流程期望通用且可以方便移植到其他的机器上，那么只有虚拟化了。
    *   虚拟化技术方案：虚拟机 or Docker
    *   虚拟化系统选择：非Win和OSx之外，可选Ubuntu或CentOS
*   因为偶尔娱乐，所以Y485依旧使用Win8作为宿主系统。
*   高速网络利用：
    *   和舍友共用WNDR 3000一只，(固件原厂不折腾)放家中作主路由使用，主路由和4300之间用超五类连接，4300和笔记本之间用超六类连接，以便再扩充设备后，内部可以进行千兆数据交换。
    *   无线利用，详见 #无线使用#
*   持续集成工具功能需求：
    *   版本控制
    *   代码编译
    *   代码构建
    *   代码同步（自用不一定使用打包分发的方式）
    *   代码测试 & Lint
    *   切换发布版本和环境（灰度）
*   发布流程想折腾的需求：
    *   分离环境
        *   日常（本地+虚拟机）
        *   预发（虚拟机二套环境）
        *   线上（远程VPSx3）
    *   分离代码
        *   服务脚本（偏后）
        *   前端资源
        *   数据层代码（DB）
    *   CLI TRIGGER EVENT
*   无缝写码环境 && 无缝测试环境
    *   路由表策略 && DHCP半动态

## [具体实践](https://github.com/soulteary/CodesPlan#%E5%85%B7%E4%BD%93%E5%AE%9E%E8%B7%B5)

*   [基础网络使用](https://github.com/soulteary/CodesPlan/blob/master/network.md)
*   [远程访问及数据交换](https://github.com/soulteary/CodesPlan/blob/master/remote.md)
*   [无线使用](https://github.com/soulteary/CodesPlan/blob/master/wifi.md)
*   [日志收集和查看](https://github.com/soulteary/CodesPlan/blob/master/log.md)
*   [虚拟机使用](https://github.com/soulteary/CodesPlan/blob/master/vm.md)

未完待续...

