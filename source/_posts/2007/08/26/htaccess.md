---
title: "设置.htaccess"
date: "2007-08-26 16:22:06"
tags: ["apache","htaccess"]
---


![](http://attachment.soulteary.com/wp/2007/08/htaccess.gif "htaccess")

梦游的收费用户不是都有CP权限么，我们可以使用CP里面的目录转向来实现对某个人或者站点的屏蔽~

此实现方式归根结底是通过可视化的.htaccess来做到的! 所以为了更加彻底的解决问题~ 我们直接编辑.htaccess来实现保护我们的资源吧!

**实现原理:**

1.用户/爬虫访问网站
2.先读取网站根目录下.htaccess文件
3.服务器判断用户请求地址,使用正则来替换请求地址
4.服务器替换地址之后，访问地址被转向到设定的地址

创建一个文本，比如promiseforever.txt

写入:

```apache
RewriteEngine on
RewriteCond %{HTTP_REFERER} !^http://promiseforever.com/.*$ [NC]
RewriteCond %{HTTP_REFERER} !^http://promiseforever.com$ [NC]
RewriteCond %{HTTP_REFERER} !^http://www.promiseforever.com/.*$ [NC]
RewriteCond %{HTTP_REFERER} !^http://www.promiseforever.com$ [NC]
RewriteRule .*.(jpg|jpeg|gif|png|bmp|rar|zip|exe|mp3|wma)$ http://promiseforever.com/fuck.png [R,NC]
```

注释:其中,promiseforever.com是我们要保护的地址,http://promiseforever.com/fuck.png是转向以后的地址! http://promiseforever.com/fuck.png 可以是警告信息~ 例如: ！@#￥%，给我go away quickly！ 然后保存文本，上传到ftp，把名字改成.htaccess

**补充资料**

1 .htacess文件是系统默认文件，通过访问.htaccess文件，可以查看用户是否拥有访问互联网或本地网的当前目录下所有文件的权限。.htaccess文件被放在当前目录下，是一个可配置文件。
2 .htaccess利用分层目录结构，采用鉴权方式，结合获取互联网页面内容的原始程序——类似于超文本链接协议代理这种程序。当用户输入URL地址时，会在URL地址前自动输入HTTP://，这样HTTPd就可以识别这条指令。（HTTPd是一种后台处理程序，当其他应用程序对它发出消息时被激活）。
3 HTTPd采用的主要访问权限控制文件是全局配置文件，全局配置文件通常放在HTTPd服务器的根目录下。.htaccess是一个附加文件，通过.htaccess文件可以控制用户对目录的访问权限。
4 当HTTPd服务器接收到用户试图访问某文档的要求时，服务器首先查找文档所在目录以及其上的所有父目录。如果找到了.htaccess文件，那么就查看此文件，了解用户权限，根据用户被授权程度决定是否应该向用户询问帐号密码或允许用户直接进入。
5 .htaccess文件是HTTPd服务器采用的默认配置文件名。不过也可以更改此文件名称，在 srm.conf文件中的AccessFileName 一行可以看到，名称应该是.htaccess，可以改成其他名字。在网景公司的服务器系统中，的名字一般是.nsconfig。

**采用.htaccess文件的优缺点**

1 通常网络管理员采用.htaccess文件来进行用户组的目录权限访问控制。没有必要将所有的HTTPd服务器、配置文件以及目录访问权限全部授权给管理员。利用当前目录的.htaccess文件可以允许管理员灵活的随时按需改变目录访问策略。
2 采用.htaccess的缺点在于：当系统有成百上千个目录，每个目录下都有对应的.htaccess文件时，网络管理员将会对如何配置全局访问策略无从下手。同时，由于.htaccess文件十分被容易覆盖，很容易造成用户上一时段能访问目录，而下一时段又访问不了的情况发生。最后，.htaccess文件也很容易被非授权用户得到，安全性不高。

