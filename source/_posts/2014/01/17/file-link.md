---
title: "创建文件链接和联接"
date: "2014-01-17 12:09:06"
tags: ["git","jfunctions","link","WINDOWS"]
---


之前把VPS折腾回WIN2K8,并添加了GIT自动部署,但是没有折腾符号链接,致使提交的时候需要写全路径,出于美观的原因决定再小折腾一下。 

如果你对之前的东西有兴趣,可以翻阅文末的链接. 

**2014.1.18更新，之前写的时候没有注意，如果要实现GIT HOOK UPDATE，请使用JFUNCTION创建，即使用/J参数。**

## 开始折腾

网上的某种解决方案是使用UNIX工具创建链接,但是WIN2K8下默认就有一个名为MKLINK的工具可以使用,先看一下用法. 

注意,这个特性是WIN7以上的版本针对NTFS格式搞的,所以用FAT的童鞋可以退散了..

```sh
D:\>mklink
创建符号链接。

MKLINK [[/D] | [/H] | [/J]] Link Target

        /D      创建目录符号链接。默认为文件
                符号链接。
        /H      创建硬链接，而不是符号链接。
        /J      创建目录联接。
        Link    指定新的符号链接名称。
        Target  指定新链接引用的路径
                (相对或绝对)。
```

关于这个家伙,有蛮多人进行过总结了,我就不过分赘语了,有兴趣的可以扩展阅读一下文末的"玩转WIN7的MKLINK". 

之前我们配置完GIT环境后,提交地址或许是这个样子:

```user@111.222.111.222:D:\data\website.git```

后面的绝对地址是不是看着时间久了会越来越碍事,没关系,我们干掉它.

```D:\>mklink /J website.git \data\website.git```

接着我们用来链接服务器的GIT地址就变为了:

```ssh://user@111.222.111.222/website.git```

注意:这里有一个隐藏条件,就是CopSSH和要提交的仓库在同一个分区中,如果不是,请使用绝对地址进行mklink操作. 

当然,你也可以使用这个工具:http://schinagl.priv.at/nt/hardlinkshellext/hardlinkshellext.html

## 相关资源

- 配置WIN主机细节1:http://www.soulteary.com/2013/11/16/details-for-win-host.html 
- 配置WIN主机细节2:http://www.soulteary.com/2013/11/27/details-for-win-host2-html.html 
- NTFS 5.0 JFUNCTIONS: http://technet.microsoft.com/en-us/sysinternals/bb896768 
- 玩转WIN7的MKLINK:http://www.cnblogs.com/asion/archive/2011/03/10/1979282.html

