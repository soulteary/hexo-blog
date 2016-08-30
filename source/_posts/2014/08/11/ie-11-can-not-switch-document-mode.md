---
title: "解决IE11开发工具无法切换文档模式的问题"
date: "2014-08-11 15:18:26"
tags: ["ie11"]
---


这个问题比较简单，但是目测网上居然没有人写，记录下来，以免其他人也踩坑。

最近测试页面比较频繁，总是登录公司的远程测试服务器或者来回切换虚拟机也不是办法，当然，最后上线前还是得挨着虚拟机逐个测，力求无误，但是开发的时候，使用IE11的话，还是能省很多事情的，可是，莫名其妙的发现我的虚拟机里的IE11的文档模式被锁定成了edge，无法修改。

[![ie11-default-document](http://attachment.soulteary.com/wp/2014/08/ie11-default-document-300x147.jpg)](http://attachment.soulteary.com/wp/2014/08/ie11-default-document.jpg) 

然后看了一下大微软的文档中心：

http://technet.microsoft.com/zh-cn/library/dn321432.aspx

看到支持从IE5~IE10这些文档模式，且最新的文档模式是IE10。 怀疑是浏览器版本低，默默的查看了一下当前浏览器默认的版本号，如图：

[![ie11-default-version](http://attachment.soulteary.com/wp/2014/08/ie11-default-version-300x242.jpg)](http://attachment.soulteary.com/wp/2014/08/ie11-default-version.jpg) 

尝试系统自己升级发现没有可以更新的。

http://windows.microsoft.com/zh-cn/internet-explorer/download-ie 访问微软的下载链接，居然给了我一个很逗比的提示：

[![ie-ms-joke](http://attachment.soulteary.com/wp/2014/08/ie-ms-joke-300x115.jpg)](http://attachment.soulteary.com/wp/2014/08/ie-ms-joke.jpg) 

下载辅助的打补丁工具，如QQ管家OR360安全管家，查找&&修复补丁，（如果你不习惯使用他们，可以尝试稍后关闭或者删除）

当然，如果你有耐心，也可以到这里来查找你需要的补丁，并升级。 http://www.microsoft.com/zh-cn/download/ 升级过后，果然可以选择文档模式了。

[![ie-11-new-version](http://attachment.soulteary.com/wp/2014/08/ie-11-new-version-300x238.jpg)](http://attachment.soulteary.com/wp/2014/08/ie-11-new-version.jpg)

[![ie-11-new](http://attachment.soulteary.com/wp/2014/08/ie-11-new-300x117.jpg)](http://attachment.soulteary.com/wp/2014/08/ie-11-new.jpg)

