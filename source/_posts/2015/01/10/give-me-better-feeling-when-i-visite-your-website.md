---
title: "天下武功，唯快不破"
date: "2015-01-10 10:16:37"
tags: ["前端优化","服务器优化","网站速度"]
---


不知道从什么时候开始，不管是写独立博客，还是网络应用，甚至写托管博客的人都会朝着“大”网站看齐，去追求网站的响应速度，通俗点说，就是白屏时间，因为据各种报告说，网站打开速度更快一些，带来的用户体验就更好一些，从而带来更多的附加利益。但是对于用户来说，快，并不是简简单单请求数目尽可能少，和服务器吞吐能力尽可能大。那么，怎么快？ 

由于本人水平有限，内容可能有误，欢迎拍砖斧正，和帮助补充。 

谈到**速度**（参考物和例子稍后一起提），我们首先能想到的事物有：

*   服务器机器性能
*   服务器机房带宽资源
*   服务器软件性能
*   DNS查询速度
*   页面资源压缩（服务端+客户端）
*   页面提供资源数量
*   页面资源加载时机
*   用户终端某时刻性能
*   用户终端浏览器性能
*   用户直观感受
*   [附加]异常流量情况

如果有兴趣，不妨和我一起逐一聊聊吧：

## 服务器性能

说到服务器性能，可能多数人会停留在几核几G几百G这种概念上，但是对于网站服务器，我们关注的应该是单机/VPS的数字运算能力和IO读写能力，如果不是单机服务器，那么请关注自己实际能使用的资源数量，尤其是高峰时刻，具体请参考VPS虚拟化常见方案:OpenVZ/Xen/KVM/VMWare/Hyper-V等方案在其他实例占用CPU过高的时候，对其他实例的影响（部分虚拟化方案，会因为某些实例锁死时间片而使用过高影响其他实例）。

就博客/网站主来说，我们应该使用尽可能更好的资源，但是非土豪的话，资源好到什么程度呢，答曰：够用。

够用是什么程度呢，满足最大的调用程度，且有余力。

这个“有余力”是你对网站/应用的访问量有评估后，并进行压测，观察机器负载得到的。

如果你的网站有大量文件IO/数据库读写操作，那么为了保证最佳性能，不妨尝试使用SSD，或者进行内存缓存等操作，一旦你使用内存缓存，那么整体的性能瓶颈多数情况下会从机器整体性能变为网卡/带宽，是不是可喜可贺。

在正确设置服务器软件配置以满足自己需求场景后，如果你对运行程序优化得当，那么最佳体验应该是内存有30%cache，swap占用极少，或不占用，负载0.5以下。（以防突发流量）

说到压测，不得不继续说下面的话题了。

## 机房带宽资源

带宽资源或许是除了高端存储设备外，价格最贵的资源之一了。所以，评估带宽是否满足你的站点，是特别重要的事情。

一般来说小站点，1~2M的带宽绝对够用。如果不知道你的机器的带宽能力，不妨登录机器后台观察流量图峰值，或者机器安装speedtest-cli，来进行数据收集。

诸如我用过的机器，突发状况下能力为：

```Linode 测速
Testing from Linode (X.X.X.X)...

Selecting best server based on latency...

Hosted by T-Mobile (West Norriton, PA) [104.63 km]: 79.475 ms

Testing download speed........................................

Download: 53.68 Mbits/s

Testing upload speed..................................................

Upload: 32.51 Mbits/s
```


```HK 机房测速
Testing from HK DataCenter (X.X.X.X)...

Selecting best server based on latency...

Hosted by FPT Telecom (Hong Kong) [5.98 km]: 28.453 ms

Testing download speed........................................

Download: 77.30 Mbits/s

Testing upload speed..................................................

Upload: 4.45 Mbits/s
```

观察两者，会发现前者上传比较大，嗯，没错，服务器的上行带宽，即是我们常说的网站带宽，一般而言，此数值越大，提供的访问能力就越强。

但是，综合现实因素，诸如政策和地理位置的原因，网站响应速度和机房有重大关系，比如，BGP机房线路智能负载后的响应时间可以达到10ms甚至以下，或者你可以尝试ping一下自己的本地服务器，响应时间感人，但是在祖国大地，如果你直接访问一些美帝国或者欧洲大陆机房则可能速度会增大几十到几百倍，甚至出现访问不能的状况。

基于这个状况，如果你的网站的受众包括国外友人，那么你的选择：

*   国内无所谓，自己能访问就好，国外快快的：
    *   美帝机房/加拿大机房/日本机房/韩国机房/欧洲机房
*   国内国外速度相对快的机房
    *   港澳台机房/日本机房/韩国机房/欧洲机房
*   国内快速，国外可能访问不了的机房
    *   各省市机房/学校机房/港澳台机房

当然，你也可以根据自己情况选择CDN加速方案：

*   国内机房+国内外CDN
*   国外机房+国内外CDN

当然，如果你和我的网站一样，是一个自己的无事写几笔的小站，那么选择国内的阿里云，或者香港机房都是不错的选择。 硬件说完了，我们聊聊第一节中提到的软件。

## 服务器软件性能

“尺有所短，寸有所长”，软件也是一样，小站点，资源有限的情况下：

*   如果你以前使用apache，且没有使用一些三方模块，或者不需要使用apache软件套装里的高级功能，或者没有软件必须依赖apache，以及三方模块能在nginx中找到替代的，可以考虑替换为nginx。
*   如果你的程序允许实现数据库缓存/站点内容缓存，但是没有使用缓存的，请开启缓存功能。
*   如果你的程序使用了文件缓存，在内存资源有富裕的情况下，请使用内存缓存（自己考虑缓存策略）。
*   如果你的程序原来的运行环境执行速度不够快，那么请考虑升级或运行环境，诸如php5.2->php.5.6+，或者php5.6->hhvm 3.x，asp/php->nodejs。
*   如果你的程序中多数功能你用不到，考虑使用更轻便的小程序。
*   如果你启用了缓存，且数据库（关系数据库）读取热数据频率高于冷数据，且访问量不是特别大，不需要考虑数据库效率，否则需要考虑数据库进行分库分表和建立适当的索引，以提高数据库吞吐能力。
*   根据自己情况适当调整nginx/mysql/redis/memcache等软件的数据分块大小。
*   优化程序关键逻辑的流程，尽可能让程序始终遵循最短路径结束任务。
*   尽可能让TCP链接重用，或者适当调整持久链接的时间和数量（Keep-Alive），以及考虑使用SPDY。
*   防火墙/服务器代理软件/程序对访客限制流量。
*   过滤或者禁止能力范围内的异常流量。

## DNS查询速度

DNS对于站点首次打开速度至关重要，所以请尽可能选择靠谱的DNS提供商来解决DNS查询问题。 国内有两家DNS提供商比较不错：

*   DNSPOD
*   DNSLA

除此之外，对于webkit支持DNS预缓存的浏览器，可以在页面头部尽少和尽合理的添加要缓存的DNS，以加快页面展示速度。 比如，我的页面中可能存在资源域名和附件域名。如果页面在加载的时候，同时进行这两个域名的DNS缓存操作，接下来请求这两个域名的资源的速度会更加的快。

```xhtml
<link rel="dns-prefetch" href="//attachment.soulteary.com">
<link rel="dns-prefetch" href="//assets.soulteary.com">
```

但是是否分离域名，请根据自己情况来。因为分离域名之后，不可否认的是，会带来额外的查询操作（查询本地缓存也算）。但是分离资源，可以使得程序更容易维护，以及对于程序整体安全性带来提高。

解释一下最后一句话，当初还在新浪的时候，翻看现在已然是大boss的ppt，其中提过这么一句话“执行不可写，可写不执行”。如果你的目录允许上传，那么上传目录的文件一定要限定不可执行，以及可以执行的目录，不可以进行写入操作，以免网页木马的上传，提高系统安全性。

另外，如果你将网站数据分离，那么网站的迁移操作，将会变的十分简单。比如，我将网站在国内国外迁移了若干次，只是需要先同步附件和页面资源域名，然后改变这两个域名的指向，然后同步程序文件，改版域名指向即可。

接下来，我们说说节约带宽和提高速度的一个大杀器：压缩。

## 页面资源压缩（服务端+客户端）

提到压缩，正常用户会想到的是rar/zip/7z，媒体爱好者想到的会是ABR/CBR/VBR/H.264，我们这类代码爱好者恐怕是gzip，如果你也是前端，可能你还会想到js compressor。

*   服务器根据CPU能力，尽可能输出gzip后的资源。

为什么是尽可能输出gzip呢，因为可能你的页面需要支持古老手机端浏览器，一些古老的MID或者性能不太好的MID设备，或者是古老的IE6？如果不需要，那么请一律输出GZIP后的页面。

*   替换或者提高压缩算法和策略。

如果你有特别的客户端，可以考虑使用自定义的更高压缩比的压缩方式，这个做手机应用的童鞋或许接触过，和十年前大家压缩MP3以及做软件压缩包一样，使用自己软件算法和策略替代市面上已有的算法和策略。如果没有特别的客户端，那么我们不妨对图片和视频使用更好的压缩格式，比如webp和webm，以及适当情况下的gif替代png等。

*   对静态资源内容适当排序。

如对最后生成的css文件进行排序可以提高gzip压缩比。

*   适当添加页面额外内容，提高压缩比。

可以将页面的通用样式或者脚本混合在页面里，提高页面压缩比。

*   使用脚本去掉多余的空格和换行。

虽然对于gzip效果甚微，但是对于缓存读取和写入有特别大的差异。

*   使用缩略图/响应式图片来替代页面中展示的原始图片。

传统模式下，我们可以使用服务端脚本thumb类库/CDN提供的图片缩略服务来进行资源的缩略图，来替换原始图片，并增加一些交互文本，对用户实现降级访问。如果用户浏览器支持CSS3，那么我们可以使用Media Queries特性来对内容图片进行切换。如果用户浏览器支持HTML5，我们可以使用image-set标签来进行图片响应式输出。 

接下来我们来说说另外一种变相的数据压缩，减少请求。

## 页面提供资源数量

*   尽可能减少同一时间的资源请求数量。

对于静态样式和脚本，使用合并策略。针对单页面程序，你可以将所有样式或者脚本都合并为一个单独的文件。但是针对多页面，以及带有皮肤策略的站点，则考虑抽象基础的Base内容和额外的内容，并通过前后端脚本进行策略加载。对于图片和视频资源，在交互允许的情况下，使用延时加载，跨屏预加载一定数量，来取代页面文档加载完成后就加载全部的策略。

*   对不同浏览器使用不同的脚本。

差异对待浏览器，对古老浏览器不使用一些功能，以及差异对待浏览器使用的基础脚本库。如果你使用下一节提到的JS加载器，那么这个很容易做到。

*   页面增量更新。

如果你的内容支持异步增量更新，那么使用接口更新增量内容的模式，来替换打开新页面的模式。曾几何时，这个被称作ajax页面局部刷新。

*   客户端缓存

此处深坑，稍后留写一篇详叙，简单的说，尽可能给所有资源使用最长时间的缓存，对于不支持200 cache的客户端提供304 Modified缓存（前者不需要额外HTTP请求）。

*   客户端本地缓存。

对于变化不大的站点，配合脚本，对支持使用本地缓存的客户端进行适当的数据缓存，这个是深坑，且有一定的安全风险，稍后写篇具体的内容来描述。 上面提到的内容，多数属于道，现在我们来聊聊术。

## 页面资源加载时机

做网站时间比较久的童鞋或许还记得yahoo slow总结出的几条“规则”：

*   把css放在文档顶部。
*   把js放在文档底部。
*   减少inline脚本的存在。

这里或许应该为：

*   将页面主要样式尽可能放在文档顶部。
*   将三方不可合并脚本尽可能放置页面底部。
*   将页面inline脚本尽可能替换为配置内容。

将基础样式放置于文档顶部，可以让页面渲染基础内容更快，如果前几点你都做到了，或者做到大多数，项目复杂度不高的话，那么把所有的样式打包合并放在此处也无关系。

将三方不可合并的内容放在页面底部，一方面是出于维护的考虑，一方面是因为我们要使用JS加载器来控制资源的加载（这里需要将原本页面中的脚本替换为具体执行脚本所需要的inline脚本配置）。 

做到如此，页面将会首屏渲染极快，以及页面卡顿大幅减少（大量动画情况另说）。

## 用户终端某时刻性能

这个不是我们所能控制的，因为受限于客户端宿主机性能以宿主机网络环境。和最开始提到的服务器性能一样，CPU时间片被其他程序占用时，或者硬件古老，以及网络被其他程序占用的时候，会带来浏览的不畅。

如果你对网站的一般访问速度有信心（通过收集到的数据的反馈），且网站属于内容展示类的，可以在适当的位置加诸如以下的提示（程序打底提示）：

*   页面加载过慢，不妨检查网络环境是否有其他软件占用（下载工具/在线视频），并刷新页面。
*   资源加载失败，请刷新重试。

待页面加载完成，干掉以上提示。但是请权衡此内容的存储位置和脚本执行时机，考虑搜索引擎将提示和内容都缓存的情况。

## 用户终端浏览器性能

如果你的用户使用者古老的浏览器，软件性能成为页面数据下载和渲染瓶颈，那么不妨给其一个提示，或者强制其使用新版本的浏览器进行访问：

*   请更新浏览器以获得更加体验。
*   本站仅支持新的浏览器：A,B,C。
*   为了您的访问速度和安全考虑，我们推荐您安装：X,Y,Z。
*   您是不是打开太多页面了，请考虑关闭无用的页面，加快本页面打开速度（这招请考虑道德问题）。

当然，在页面资源数量一节中，有提到一些，这里补充一条，对于支持HTML5video标签的客户端，不妨使用其来替换flash，减少客户端CPU使用率。

## 用户直观感受

终于写到这里了，本节内容，其实上面的小节都有提到一些。

一句话以蔽之，用上面的方法，不要放过任何可以加快数据展示的方法，给用户尽可能最快的体验。 

当然，这里有个偷懒的方式，你只需要尽可能战胜同类型网站就好。

## [附加]异常流量情况

异常流量可能存在以下状况：

*   搜索引擎蜘蛛不约而同的来采集你的网站内容。

适当干掉一些你不喜欢和需要的蜘蛛，诸如俄罗斯的一堆等，或者小众浏览器搜索引擎。在sitemap中增加访问时间间隔，或者考虑对不同蜘蛛输出不同时间。内容添加缓存。

*   内容引发热点，真实访问量大增。

内容确实有趣/有争议/有实用价值，用户访问量增加，如果你是盈利的，那么加机器吧，如果你是非盈利的，兴趣驱动，无广告的，诸如我这类小博客的，加缓存，或者加免费CDN，或者使用DNS进行多机负载。

*   三方无聊的恶作剧/利益相关的恶意攻击/错误的域名指向

无聊的恶作剧，包括扫描，这个避免不了，但是你可以在fail2ban、iptables、nginx/apache过滤掉一些机器人和恶作剧。

如果是SYN的话，瓶颈在带宽资源/机器资源/机器流量限制，可以考虑切换DNS（前提是有备份机器）。 

如果是最近的错误域名指向的问题，比如国外最大视频站点突然IP指向到你的机器，那么请毫不犹豫的去换IP吧。 

如果是恶意攻击的话，这里区分两种状况：

*   你是盈利的，对用户承诺SLA，那么请考虑加硬件，加CDN，过滤IP。
*   你是非盈利的，诸如我这类blog，加内容缓存就好了，自己实测，压满了带宽，机器负载还是0.5以下，当然如果带宽大的话，那么机器估计压挂。如果你有备份机器，那么切换下DNS，或者除了当前域名外，使用三方托管页面进行博客备份，诸如Github Page/Issue、新浪博客之类的BSP。

暂时先写到这里，稍后把客户端缓存/参考物和优化后的对比的坑填了。

---EOF---

晓白，2015.1.10