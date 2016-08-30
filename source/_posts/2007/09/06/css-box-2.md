---
title: "[css]彻底弄懂CSS盒子模式(DIV布局快速入门)"
date: "2007-09-06 13:06:27"
tags: ["CSS","盒模型"]
---


![](http://attachment.soulteary.com/wp/2007/09/css.gif "css")

转载唐大大的经典文章。 

彻底弄懂CSS盒子模式(DIV布局快速入门) 

作者：唐国辉 

实例网页网址：http://www.hsptc.com/css2.html

前言

如果你想尝试一下不用表格来排版网页，而是用CSS来排版你的网页，也就是常听的用DIV来编排你的网页结构，又或者说你想学习网页标准设计，再或者说你的上司要你改变传统的表格排版方式，提高企业竞争力，那么你一定要接触到的一个知识点就是CSS的盒子模式，这就是DIV排版的核心所在，传统的表格排版是通过大小不一的表格和表格嵌套来定位排版网页内容，改用CSS排版后，就是通过由CSS定义的大小不一的盒子和盒子嵌套来编排网页。因为用这种方式排版的网页代码简洁，更新方便，能兼容更多的浏览器，比如PDA设备也能正常浏览，所以放弃自己之前钟爱的表格排版也是值得的，更重要的是CSS排版网页的优势远远不只这些，本人在这里就不多说，自己可以去查找相关信息。  

理解CSS盒子模型 什么是CSS的盒子模式呢？为什么叫它是盒子？先说说我们在网页设计中常听的属性名：内容(content)、填充(padding)、边框(border)、边界(margin)， CSS盒子模式都具备这些属性。

CSS盒子模式

这些属性我们可以把它转移到我们日常生活中的盒子（箱子）上来理解，日常生活中所见的盒子也具有这些属性，所以叫它盒子模式。那么内容就是盒子里装的东西；而填充就是怕盒子里装的东西（贵重的）损坏而添加的泡沫或者其它抗震的辅料；边框就是盒子本身了；至于边界则说明盒子摆放的时候的不能全部堆在一起，要留一定空隙保持通风，同时也为了方便取出嘛。在网页设计上，内容常指文字、图片等元素，但是也可以是小盒子（DIV嵌套），与现实生活中盒子不同的是，现实生活中的东西一般不能大于盒子，否则盒子会被撑坏的，而CSS盒子具有弹性，里面的东西大过盒子本身最多把它撑大，但它不会损坏的。填充只有宽度属性，可以理解为生活中盒子里的抗震辅料厚度，而边框有大小和颜色之分，我们又可以理解为生活中所见盒子的厚度以及这个盒子是用什么颜色材料做成的，边界就是该盒子与其它东西要保留多大距离。在现实生活中，假设我们在一个广场上，把不同大小和颜色的盒子，以一定的间隙和顺序摆放好，最后从广场上空往下看，看到的图形和结构就类似我们要做的网页版面设计了，如下图。 由“盒子”堆出来的网页版面 现在对CSS盒子模式理解多少了，如果还不够透彻，继续往下看，我会在后面举例，并延用盒子的概念来解释它。 转变我们的思路 　　传统的前台网页设计是这样进行的：根据要求，先考虑好主色调，要用什么类型的图片，用什么字体、颜色等等，然后再用Photoshop这类软件自由的画出来，最后再切成小图，再不自由的通过设计HTML生成页面，改用CSS排版后，我们要转变这个思想，此时我们主要考虑的是页面内容的语义和结构，因为一个强CSS控制的网页，等做好网页后，你还可以轻松的调你想要的网页风格，况且CSS排版的另外一个目的是让代码易读，区块分明，强化代码重用，所以结构很重要。如果你想说我的网页设计的很复杂，到后来能不能实现那样的效果？我要告诉你的是，如果用CSS实现不了的效果，一般用表格也是很难实现的，因为CSS的控制能力实在是太强大了，顺便说一点的是用CSS排版有一个很实用的好处是，如果你是接单做网站的，如果你用了CSS排版网页，做到后来客户有什么不满意，特别是色调的话，那么改起来就相当容易，甚至你还可以定制几种风格的CSS文件供客户选择，又或者写一个程序实现动态调用，让网站具有动态改变风格的功能。 实现结构与表现分离 　　在真正开始布局实践之前，再来认识一件事——结构和表现相分离，这也用CSS布局的特色所在，结构与表现分离后，代码才简洁，更新才方便，这不正是我们学习CSS的目的所在吗？举个例来说P是结构化标签，有P标签的地方表示这是一个段落区块，margin是表现属性，我要让一个段落右缩进2字高，有些人会想到加空格，然后不断地加空格，但现在可以给P标签指定一个CSS样式：P {text-indent: 2em;}，这样结果body内容部分就如下，这没有外加任何表现控制的标签：


>加进天涯社区有一段时间了，但一直没有时间写点东西，今天写了一篇有关CSS布局的文章，并力求通过一种通俗的语言来说明知识点，还配以实例和图片，相信对初学CSS布局的人会带来一定的帮助。

如果还要对这个段落加上字体、字号、背景、行距等修饰，直接把对应的CSS加进P样式里就行了，不用像这样来写了：

<pre lang="html4strict">

<font color="#FF0000" face="宋体">段落内容</font>

</pre>

这个是结构和表现混合一起写的，如果很多段落有统一结构和表现的话，再这样累加写下去代码就繁冗了。 再直接列一段代码加深理解结构和表现相分离： 使用CSS排版

<pre lang="html4strict"> <style type="text/css"><!--   
#photoList img{
	height:80;
	width:100;
	margin:5px auto;
	}
--></style> 

<div id="photoList">
![](01.jpg)
![](02.jpg)
![](03.jpg)
![](04.jpg)
![](05.jpg)
</div>

</pre>

不用CSS排版

<pre lang="html4strict">![](01.jpg)
![](02.jpg)
![](03.jpg)
![](04.jpg)
![](05.jpg)
</pre>

　　第一种方法是结构表现相分离，内容部分代码简单吧，如果还有更多的图片列表的话，那么第一种CSS布局方法就更有优势，我打个比喻你好理解：我在BODY向你介绍一个人，我只对你说他是一个人，至于他是一个什么样的人，有多高，是男是女，你去CSS那里查下就知道。这样我在BODY的工作就简单了，也就是说BODY的代码就简单了。如果BODY有一个团队人在那里，我在CSS记录一项就行了，这有点像Flash软件里的元件和实例的概念，不同的实例共享同一个元件，这样动画文件就不大了，把这种想法移到CSS网页设计中，就是代码不复杂，网页文件体积小能较快被客户端下载了。 演示地址：http://www.hsptc.com/css1.html 用CSS排版减小网页文件体积 　　像上面我做的那个版面，一共分为四个区块，每个区块的框架是一样的，这个框架就是用CSS写出来的，样式写一次，就可以被无数次调用了(用 class调用，而不是ID)，只要改变其中的文字内容就可以生成风格统一的众多板块了，它的样式和结构代码是（请不要直接复制生成网页，把下面代码分别粘贴到网页中它们应在的位置）：

<pre lang="html4strict">
 <style type="text/css"><!--
* {margin:0px; padding:0px;}
body {
	font-size: 12px;
	margin: 0px auto;
	height: auto;
	width: 805px;
	}
.mainBox {
	border: 1px dashed #0099CC;
	margin: 3px;
	padding: 0px;
	float: left;
	height: 300px;
	width: 192px;
	}
.mainBox h3 {
	float: left;
	height: 20px;
	width: 179px;
	color: #FFFFFF;
	padding: 6px 3px 3px 10px;
	background-color: #0099CC;
	font-size: 16px;
	}
.mainBox p {
	line-height: 1.5em;
	text-indent: 2em;
	margin: 35px 5px 5px 5px;
	}
--></style> 

<div class="mainBox">

### 前言

正文内容

</div>

<div class="mainBox">

### CSS盒子模式

正文内容 

</div>

<div class="mainBox">

### 转变思想

正文内容 

</div>

<div class="mainBox">

### 熟悉步骤

正文内容 

</div>

</pre>

熟悉工作流程 　　在真正开始工作之前我们脑海中要形成这样一种思想：表格是什么我不知道，在内容部分我不能让它再出现表现控制标签，如：font、color、 height、width、align等标签不能再出现，（简单说工作前先洗脑，忘掉以前的一惯做法，去接受和使用全新的方法），我不是单纯的用DIV来实现排版的嵌套，DIV是块级元素，而像P也是块级元素，例如要分出几个文字内容块，不是一定要用DIV才叫DIV排版，不是“

<div>文字块一</div>

<div>文字块二</div>

<div>文字块三</div>

”，而用“

文字块一

文字块二

文字块三

” 更合适。 　　用DIV+CSS设计思路是这样的: 1.用div来定义语义结构；2.然后用CSS来美化网页，如加入背景、线条边框、对齐属性等；3.最后在这个CSS定义的盒子内加上内容，如文字、图片等（没有表现属性的标签），下面大家跟我一起来做一个实例加深对这个步骤的理解。先看结果图： 演示地址：http://www.hsptc.com/css2.html CSS排版结果图 用div来定义语义结构 　　现在我要给大家演示的是一个典型的版面分栏结构，即页头、导航栏、内容、版权（如下图）， 典型版面分栏结构 其结构代码如下：上面我们定义了四个盒子，按照我们想要的结果是，我们要让这些盒子等宽，并从下到下整齐排列，然后在整个页面中居中对齐，为了方便控制，我们再把这四个盒子装进一个更大的盒子，这个盒子就是BODY，这样代码就变成:最外边的大盒子（装着小盒子的大盒子）我们要让它在页面居中，并重定义其宽度为760像素，同时加上边框，那么它的样式是：

<pre lang="html4strict">
body {
	font-family: Arial, Helvetica, sans-serif;
	font-size: 12px;
	margin: 0px auto;
	height: auto;
	width: 760px;
	border: 1px solid #006633;
	}

</pre>

页头为了简单起见，我们这里只要让它整个区块应用一幅背景图就行了,并在其下边界设计定一定间隙，目的是让页头的图像不要和下面要做的导航栏连在一起，这样也是为了美观。其样式代码为：

<pre lang="html4strict">
#header {
	height: 100px;
	width: 760px;
	background-image: url(headPic.gif);
	background-repeat: no-repeat;
	margin:0px 0px 3px 0px;
	}

</pre>

导航栏我做成像一个个小按钮，鼠标移上去会改变按钮背景色和字体色，那么这些小小的按钮我们又可以理解为小盒子，如此一来这是一个盒子嵌套问题了，样式代码如下：

<pre lang="html4strict">
#nav {
	height: 25px;
	width: 760px;
	font-size: 14px;
	list-style-type: none;
	}
#nav li {
	float:left;
	}
#nav li a {
	color:#000000;
	text-decoration:none;
	padding-top:4px;
	display:block;
	width:97px;
	height:22px;
	text-align:center;
	background-color: #009966;
	margin-left:2px;
	}
#nav li a:hover{
	background-color:#006633;
	color:#FFFFFF;
	}

</pre>

　　内容部分主要放入文章内容，有标题和段落，标题加粗，为了规范化，我用H标签，段落要自动实现首行缩进2个字,同时所有内容看起来要和外层大盒子边框有一定距离，这里用填充。内容区块样式代码为：

<pre lang="html4strict">
#content {
	height:auto;
	width: 740px;
	line-height: 1.5em;
	padding: 10px;
	}
#content p {
	text-indent: 2em;
	}
#content h3 {
	font-size: 16px;
	margin: 10px;

</pre>

　　版权栏，给它加个背景，与页头相映，里面文字要自动居中对齐，有多行内容时，行间距合适，这里的链接样式也可以单独指定，我这里就不做了。其样式代码如下：

<pre lang="html4strict">
#footer {
	height: 50px;
	width: 740px;
	line-height: 2em;
	text-align: center;
	background-color: #009966;
	padding: 10px;
	}

</pre>

最后回到样式开头大家会看到这样的样式代码：

<pre lang="html4strict">
* {
	margin: 0px;
	padding: 0px;
	}

</pre>

这是用了通配符初始化各标签边界和填充，（因为有部分标签默认会有一定的边界，如Form标签）那么接下来就不用对每个标签再加以这样的控制，这可以在一定程度上简化代码。最终完成全部样式代码是这样的：

<pre lang="html4strict">
 1\. <style type="text/css">2\. <!-- 
 3\. * {
 4\. margin: 0px;
 5\. padding: 0px;
 6\. }
 7\. body {
 8\. font-family: Arial, Helvetica, sans-serif;
 9\. font-size: 12px;
10\. margin: 0px auto;
11\. height: auto;
12\. width: 760px;
13\. border: 1px solid #006633;
14\. }
15\. #header {
16\. height: 100px;
17\. width: 760px;
18\. background-image: url(headPic.gif);
19\. background-repeat: no-repeat;
20\. margin:0px 0px 3px 0px;
21\. }
22\. #nav {
23\. height: 25px;
24\. width: 760px;
25\. font-size: 14px;
26\. list-style-type: none;
27\. }
28\. #nav li {
29\. float:left;
30\. }
31\. #nav li a{
32\. color:#000000;
33\. text-decoration:none;
34\. padding-top:4px;
35\. display:block;
36\. width:97px;
37\. height:22px;
38\. text-align:center;
39\. background-color: #009966;
40\. margin-left:2px;
41\. }
42\. #nav li a:hover{
43\. background-color:#006633;
44\. color:#FFFFFF;
45\. }
46\. #content {
47\. height:auto;
48\. width: 740px;
49\. line-height: 1.5em;
50\. padding: 10px;
51\. }
52\. #content p {
53\. text-indent: 2em;
54\. }
55\. #content h3 {
56\. font-size: 16px;
57\. margin: 10px;
58\. }
59\. #footer {
60\. height: 50px;
61\. width: 740px;
62\. line-height: 2em;
63\. text-align: center;
64\. background-color: #009966;
65\. padding: 10px;
66\. }
67\. --> 
68\.</style> 
</pre>

结构代码是这样的：

<pre lang="html4strict">
1\. 
 2\. 
 3\. 

*   [首 页](#)

*   [文 章](#)

*   [相册](#)

*   [Blog](#)

*   [论 坛](#)

*   [帮助](#)

11\. 

<div id="content">
12\. 

### 前言

13\. 

第一段内容

14\. 

### 理解CSS盒子模式

15\. 

第二段内容

16\. </div>

17\. 

<div id="footer">
18\. 

关于我们 | 广告服务 | 客服中心 | Q Q留言 | 网站管理 

Copyright ?2006 - 2008 Tang Guohui. All Rights Reserved

19\. </div>

20\. 

</pre>

结束语 好了，此文到此结束，更多内容，如：CSS中的盒子宽度计算，浏览器兼容问题，XHTML规范化写法等请大家去参考其它资料。如果觉得此文还可以，看过之后记得跟帖，你的鼓励是我不断出新文章的动力^-^ 本文完全为个人原创作品，转摘请注明作者，作者：唐国辉。感谢经典论坛网页标准化专栏斑竹blankzheng指点优化几处

