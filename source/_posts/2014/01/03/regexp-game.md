---
title: "正则表达式游戏"
date: "2014-01-03 02:27:32"
tags: ["js"]
---


看到微博有人转了一个正则表达式测试题，于是果断拿来玩玩，记录一下解题思路。 

地址：http://regex.alf.nu/

## 第一题：

|Match all of these|and none of these…|
| --- | --- |
|*   afoot |*   Atlas|
|*   catfoot|*   Aymoro|
|*   dogfoot|*   Iberic|
|*   fanfoot|*   Mahran|
|*   foody|*   Ormazd|
|*   foolery|*   Silipan|
|*   foolish|*   altared|
|*   fooster|*   chandoo|
|*   footage|*   crenel|
|*   foothot|*   crooked|
|*   footle|*   fardo|
|*   footpad|*   folksy|
|*   footway|*   forest|
|*   hotfoot|*   hebamic|
|*   jawfoot|*   idgah|
|*   mafoo|*   manlike|
|*   nonfood|*   marly|
|*   padfoot|*   palazzi|
|*   prefool|*   sixfold|
|*   sfoot|*   tarrock|
|*   unfool|*   unfold |

根据习惯，脑补了第一个答案是：

```^\w*foo\w*$```

原因很简单，想写严谨，第一列元素都是小写字母开头，以及小写字母结束，中间包含foo的字符串。不过这个复杂度略高，得分只有199，我们来简化的玩。

```\w*foo\w*```

去掉匹配开头和结尾，得分高了2分，变为201，看来得分和复杂度果然是正相关啊。继续简化的玩。

```foo```

这个得分207，应该不会有更高了吧...当然你如果为了优雅，可以写成下面的，损失一点分数。

```fo{2,}```

这个204分，不过看起来有条理些。  

## 第二题：

思路和之前差不多的就不写了，浪费篇幅。 看到又是同样两个list：

|Match all of these|and none of these…|
| --- | --- |
|*   Mick|*   Kickapoo|
|*   Rick|*   Nickneven|
|*   allocochick|*   Rickettsiales|
|*   backtrick|*   billsticker|
|*   bestick|*   borickite|
|*   candlestick|*   chickell|
|*   counterprick|*   fickleness|
|*   heartsick|*   finickily|
|*   lampwick|*   kilbrickenite|
|*   lick|*   lickpenny|
|*   lungsick|*   mispickel|
|*   potstick|*   quickfoot|
|*   quick|*   quickhatch|
|*   rampick|*   ricksha|
|*   rebrick|*   rollicking|
|*   relick|*   slapsticky|
|*   seasick|*   snickdrawing|
|*   slick|*   sunstricken|
|*   tick|*   tricklingly|
|*   unsick|*   unlicked|
|*   upstick|*   unnickeled|

``` ick$```

这个是不是一眼看出，但是等等，直接写下面这个是不是更简单。

```k$```

208分，继续下一题。

## 第三题：

|Match all of these|and none of these…|
| --- | --- |
|*   abac|*   beam|
|*   accede|*   buoy|
|*   adead|*   canjac|
|*   babe|*   chymia|
|*   bead|*   corah|
|*   bebed|*   cupula|
|*   bedad|*   griece|
|*   bedded|*   hafter|
|*   bedead|*   idic|
|*   bedeaf|*   lucy|
|*   caba|*   martyr|
|*   caffa|*   matron|
|*   dace|*   messrs|
|*   dade|*   mucose|
|*   daff|*   relose|
|*   dead|*   sonly|
|*   deed|*   tegua|
|*   deface|*   threap|
|*   faded|*   towned|
|*   faff|*   widish|
|*   feed|*   yite|


```^[a-f][^g-z]*[a-f]$```

不是太好看，分数也不高，只有191，想想办法，把分数搞高一点吧。

```^[a-f][^g-z]*$```

何必全部匹配呢，只要中间有不对的就排斥掉嘛...这下有196了，将就一下，下一题吧。

## 第四题：

|Match all of these|and none of these…|
| --- | --- |
|*   allochirally|*   anticker|
|*   anticovenanting|*   corundum|
|*   barbary|*   crabcatcher|
|*   calelectrical|*   damnably|
|*   entablement|*   foxtailed|
|*   ethanethiol|*   galvanotactic|
|*   froufrou|*   gummage|
|*   furfuryl|*   gurniad|
|*   galagala|*   hypergoddess|
|*   heavyheaded|*   kashga|
|*   linguatuline|*   nonimitative|
|*   mathematic|*   parsonage|
|*   monoammonium|*   pouchlike|
|*   perpera|*   presumptuously|
|*   photophonic|*   pylar|
|*   purpuraceous|*   rachioparalysis|
|*   salpingonasal|*   scherzando|
|*   testes|*   swayed|
|*   trisectrix|*   unbridledness|
|*   undergrounder|*   unupbraidingly|
|*   untaunted|*   wellside|


```(\w{2,3})(?:\w+)\1```

偷懒的写法貌似只有182分，继续偷偷懒，把分数提高。

```(.{2})(?:.+)\1```

把范围精简一下，186分，继续下一题吧。

## 第五题：


|Match all of these|and none of these…|
| --- | --- |
|*   acritan|*   abba|
|*   aesthophysiology|*   anallagmatic|
|*   amphimictical|*   bassarisk|
|*   baruria|*   chorioallantois|
|*   calomorphic|*   coccomyces|
|*   disarmature|*   commotive|
|*   effusive|*   engrammatic|
|*   fluted|*   glossoscopia|
|*   fusoid|*   hexacoralla|
|*   goblinize|*   hippogriffin|
|*   nihilistic|*   inflammableness|
|*   noisefully|*   otto|
|*   picrorhiza|*   overattached|
|*   postarytenoid|*   saffarid|
|*   revolutionize|*   sarraceniaceae|
|*   suprasphanoidal|*   scillipicrin|
|*   suspenseful|*   tlapallan|
|*   tapachula|*   trillion|
|*   transmit|*   unclassably|
|*   unversatile|*   unfitting|
|*   vibetoite|*   unsmelled|
|  |*   warrandice|


``` ^(?!.*(.)(.)\2\1)```

这个和上面的一样，只不过要排除，193分。

## 第六题：

|Match all of these|and none of these…|
| --- | --- |
|*   civic|*   arrogatingly|
|*   deedeed|*   camshach|
|*   degged|*   cinnabar|
|*   hallah|*   defendress|
|*   kakkak|*   derivedly|
|*   kook|*   gourmet|
|*   level|*   hamleteer|
|*   murdrum|*   hydroaviation|
|*   noon|*   lophine|
|*   redder|*   nonalcohol|
|*   repaper|*   outslink|
|*   retter|*   pretest|
|*   reviver|*   psalterium|
|*   rotator|*   psorosperm|
|*   sexes|*   scrummage|
|*   sooloos|*   sporous|
|*   tebbet|*   springer|
|*   tenet|*   sunburn|
|*   terret|*   teleoptile|
| |*   unstuttering|
| |*   womanways|


```^((.+)(.*){1}(.+)\3\2)$```

中规中矩，157分，那么开始精简吧。

```^(.).*\1$```

这样就提高到171分了。

## 第七题：

|Match all of these|and none of these…|
| --- | --- |
|*   xx|*   xxxx|
|*   xxx|*   xxxxxx|
|*   xxxxx|*   xxxxxxxx|
|*   xxxxxxx|*   xxxxxxxxx|
|*   xxxxxxxxxxx|*   xxxxxxxxxx|
|*   xxxxxxxxxxxxx|*   xxxxxxxxxxxx|
|*   xxxxxxxxxxxxxxxxx|*   xxxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxxxxxxx|*   xxxxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxxxxxxxxxxx|*   xxxxxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxxxxxxxxxxxxxxxxx|*   xxxxxxxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxx... [31 chars]|*   xxxxxxxxxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxx... [37 chars]|*   xxxxxxxxxxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxx... [41 chars]|*   xxxxxxxxxxxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxx... [43 chars]|*   xxxxxxxxxxxxxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxx... [47 chars]|*   xxxxxxxxxxxxxxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxx... [53 chars]|*   xxxxxxxxxxxxxxxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxx... [59 chars]|*   xxxxxxxxxxxxxxxxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxx... [61 chars]|*   xxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxx... [67 chars]|*   xxxxxxxxxxxxxx... [30 chars]|
|*   xxxxxxxxxxxxxx... [71 chars]|*   xxxxxxxxxxxxxx... [32 chars]|


这道题真卡住了，因为想用重复来实现....其实不必，网站的思路是这个284分：

```^(?!(..+)(\1)+$)```

还是分组排除，不难发现，绝对不可以被2整除，那么起点就是两个字符（以上），然后double一下，再排除掉。

## 第八题：

|Match all of these|and none of these…|
| --- | --- |
|*   Makaraka|*   Ludgate|
|*   Wasagara|*   Mitsukurinidae|
|*   degenerescence|*   Ternstroemiaceae|
|*   desilicification|*   arrhythmical|
|*   elevener|*   bleater|
|*   hipponosological|*   energetics|
|*   homoeomorphy|*   inthrow|
|*   homologous|*   mecopterous|
|*   ileocolotomy|*   multum|
|*   intervisibility|*   naphthalene|
|*   jararaca|*   nullibicity|
|*   locomotory|*   observancy|
|*   micropoikilitic|*   overpunishment|
|*   odontonosology|*   overregularly|
|*   parhomologous|*   overwilily|
|*   pogonotomy|*   participator|
|*   promonopolist|*   predisable|
|*   protohomo|*   reyield|
|*   pseudoprimitivism|*   rubeola|
|*   tocororo|*   traitorlike|
|*   unintelligibility|*   unregainable|

 间隔字符四次重复，那么就跟着题意写一下吧，198分：

```(.).\1.\1.\1```

貌似合并一下，可以提高一分，到199了:

```(.)(.\1){3}```

## 第九题：

|Match all of these|and none of these…|
| --- | --- |
|*   access|*   analyse|
|*   accloy|*   balanism|
|*   adeem|*   baronet|
|*   aflow|*   biddable|
|*   aglow|*   griefless|
|*   beefin|*   harebrain|
|*   befist|*   jestword|
|*   billot|*   laicize|
|*   bossy|*   marvelry|
|*   certy|*   oriole|
|*   chintz|*   pickietar|
|*   chips|*   preferee|
|*   chort|*   primness|
|*   cloop|*   pulghere|
|*   coost|*   rebirth|
|*   demos|*   scupper|
|*   fitty|*   serigraph|
|*   flory|*   sororize|
|*   flossy|*   theowman|
|*   ghost|*   unfrayed|
|*   mopsy|*   wagonman|


这道题的副标题提示为cheat，然后题目为order，先写一个搞笑的吧，188分。

```^[a-m][c-o](?!st|a|dd)```

但是很容易发现左边是不是短了点，基本都是五位的吧，把六位的那几个没选中的扔编辑器中，列编辑去掉前五位，结果是s, y, n, t, t, z, y, 去重就是synzt，分数提到到196分。

```^.{5}[synzt]*$```

##  第十题：

|Match all of these|and none of these…|
| --- | --- |
|*   000000000|*   000000005|
|*   000000003|*   000000008|
|*   000000006|*   000000010|
|*   000000009|*   000000011|
|*   000000012|*   000000014|
|*   000000015|*   018990130|
|*   066990060|*   112057285|
|*   140091876|*   159747125|
|*   173655750|*   176950268|
|*   312440187|*   259108903|
|*   321769005|*   333162608|
|*   368542278|*   388401457|
|*   390259104|*   477848777|
|*   402223947|*   478621693|
|*   443512431|*   531683939|
|*   714541758|*   704168662|
|*   747289572|*   759282218|
|*   819148602|*   769340942|
|*   878531775|*   851936815|
|*   905586303|*   973816159|
|*   953734824|*   979204403|

作者在这题写了一句，少年们你们做完这道题去尝试解一下7倍的吧！卧槽，这个作者的喜好好变态。 我觉得这道题应该用重复做，但是感觉实现起来好长，那么用偷懒的方法吧，写段脚本辅助分析一下：

```
//把左边的数据存一下
var data = [
    '000000000',
    '000000003',
    '000000006',
    '000000009',
    '000000012',
    '000000015',
    '066990060',
    '140091876',
    '173655750',
    '312440187',
    '321769005',
    '368542278',
    '390259104',
    '402223947',
    '443512431',
    '714541758',
    '747289572',
    '819148602',
    '878531775',
    '905586303',
    '953734824'
];
//把字符串切片，算出现次数
var ret = {};
for (var i = 0, j = data.length; i < j; i++) {
    var item = data[i];
    for (var m = 0, n = data[i].length; m < n; m++) {
        for (var p = m + 1, q = data[i].length; p < q; p++) {
            var key = item.slice(m, p);
            if (ret[key]) {
                ret[key] += 1;
            } else {
                ret[key] = 1;
            }
        }
    }
}
//把出现一次的屌丝们干掉
data.length=0;
for (var i in ret) {
    if (ret[i] === 1) {
        delete ret[i]
    }
}

console.log(JSON.stringify(ret))
```

把结果超过10次的还有共性太大的01系干掉后，结果如下：

```
{
    "12": 2,
    "14": 3,
    "17": 4,
    "18": 2,
    "22": 3,
    "24": 2,
    "31": 2,
    "36": 2,
    "39": 2,
    "40": 3,
    "43": 2,
    "44": 2,
    "48": 2,
    "53": 2,
    "54": 2,
    "55": 2,
    "57": 2,
    "69": 2,
    "73": 2,
    "75": 2,
    "85": 2,
    "86": 2,
    "87": 2,
    "90": 4,
    "91": 3,
    "95": 2,
    "124": 2,
    "900": 2,
    "06": 2,
    "02": 2
}
```

经过尝试，貌似不妥，换一个思路，统计间隔字符出现频率。 大概实现如下：

```js
var data = [
    '000000000',
    '000000003',
    '000000006',
    '000000009',
    '000000012',
    '000000015',
    '066990060',
    '140091876',
    '173655750',
    '312440187',
    '321769005',
    '368542278',
    '390259104',
    '402223947',
    '443512431',
    '714541758',
    '747289572',
    '819148602',
    '878531775',
    '905586303',
    '953734824'
];
//把字符串切片，算出现次数
var ret = {};
for (var i = 0, j = data.length; i < j; i++) {
    var item = data[i];
    ret[item] = [];
    for (var m = 0, n = 9; m < n; m++) {
        for (var p = 0, q = 9; p < q; p++) {
            var left = item.indexOf(m);
            var right = item.lastIndexOf(p);

            if(left==-1||right==-1||left==right){
                break;
            }
            console.log(m,p,item)
            var tmp = {}
            tmp[m] = p;
            ret[item].push(tmp)
        }
    }
}

console.log(JSON.stringify(ret))
```

上面两种比较笨的思路行不通,那么我们用观察的好了，开始的一部分以及第七个,第八个其实都有包含00,所以或许我们把前八个抽象更好.

```00($|3|6|9|12|15)```

接着我们砍掉前八个,就只匹配后面的几个试一试.

``` 00($|3|6|9|12|15)|36.5|3.?2|[79]14|8.3|02.*4+|3.*4[38]|9572```

这条写的稀烂，但是分数是571...应该有更多更好的组合方式吧。

## 十一题：

|Match all of these|and none of these…|
| --- | --- |
|*   *err* matches superreform|*   *anapaestical* matches anapaestically|
|*   *falle*ess matches unfallenness|*   *chegonio matches archegoniophore|
|*   *il*log* matches unphilological|*   *dissoluti* matches dissolutional|
|*   *plen*tud* matches overplenitude|*   *domestica matches domesticality|
|*   *taiodi* matches pentaiodide|*   *expedition matches expedition|
|*   *viceberry matches serviceberry|*   *hormog matches hormogonium|
|*   bowdl* matches bowdlerism|*   *stipular* matches infrastipular|
|*   bron*hopleur*sy matches bronchopleurisy|*   *strabis matches strabismal|
|*   chromatophobia matches chromatophobia|*   cathartica matches cathartically|
|*   cockneyla* matches cockneyland|*   di matches gerundively|
|*   colorlessly matches colorlessly|*   hacean matches zoanthacean|
|*   cretefaction matches cretefaction|*   headmist matches headmistress|
|*   downrightly matches downrightly|*   herwi matches trencherwise|
|*   leather* matches leatherbark|*   iemphraxia matches cardiemphraxia|
|*   mitogenet* matches mitogenetic|*   kmak matches packmaking|
|*   palindrom* matches palindromic|*   mbable* matches unclimbable|
|*   parallelepiped matches parallelepiped|*   nspi*tor matches inspirator|
|*   primigenial matches primigenial|*   ocumidi matches pseudocumidine|
|*   puppe* matches puppetlike|*   raretinal* matches intraretinal|
|*   resurrender matches resurrender|*   tte matches whitterick|
|*   wreathwi* matches wreathwise|*   uefoliate matches quinquefoliate|


ctrl+f一下*，发现两边都有没有*的字符串，并且数量都不一定成双，所以这道题虽然名曰glob，但是却不是考察*，如果非要说考察，看的是转义了。 我们来实现一下他的规则吧，首先是完整匹配：

```^(.*)(?!\*)\sm.*\s\1$```

接着是单词尾部带*：

```^(.*)\*\sm.*\s\1.*$```

然后是两边都带*的:

```^\*(.+)\*\sm.*\s.+\1.+$```

中间包含多个*的：

```^\*(.+)\*(.+)\*\sm.*\s.+\1.+\2.+$```

首尾才有*的：

```^\*(.*)\*\sm.*\s.+\1.+$```

上面几条规则简化以后大概是这样的：

```
^(.+)(?!\*).+\s\1$
^(.+)\*\s.+\s\1.+$
^\*(.+)\*\s.+\s.+\1.+$
^\*(.+)\*(.+)\*\s.*\s.+\1.+\2.+$
^\*(.+)\*\s.*\s.+\1.+$
```

去重和化简合并后,检查有没有漏的后，就是这个样子了,322分：

```^((.+)(?!\*).+\s\2$|(.+)\*\s.+\s\3.+$|\*(.+)\*.+\s.+\4.+$|\*(.+)(?!\*)\s.+\s.+\5$|(?!\*)(.*\*){2})```

##  十二题：

|Match all of these|and none of these…|
| --- | --- |
|*   <<<<<>><<>>><<... [62 chars]|*   <|
|*   <<<<<>><>><<><... [110 chars]|*   <<<<<<>>><<><>>>>>><<>|
|*   <<<<<>><>><>><... [102 chars]|*   <<<<<>>><>>><<<>>>><>>|
|*   <<<<<>><>>>><<... [88 chars]|*   <<<<<>>>>>>|
|*   <<<<<>>><<<>><... [58 chars]|*   <<<<>><<<<<><<>><><<<<|
|*   <<<<<>>><<><>>... [152 chars]|*   <<<>><<<<><><><><|
|*   <<<<<>>><<>><<... [42 chars]|*   <<<>>>><><<<><>|
|*   <<<<<>>><>><<<>>>><<>>|*   <<><<<<><<><<>>><<|
|*   <<<<<>>>><<<<>... [102 chars]|*   <<><<<>>>>><<|
|*   <<<<<>>>><<<><... [30 chars]|*   <<>>><<<>>|
|*   <<<<<>>>><><<<... [66 chars]|*   <><<<>><<>>><<>|
|*   <<<<<>>>><><<<... [124 chars]|*   <><<>>><<<><>><<<>>><<>>>><|
|*   <<<<<>>>><>><<>>|*   <><<>>><><<<>|
|*   <<<<><<>>><<<>... [34 chars]|*   <><>><>>><><<<... [36 chars]|
|*   <<<<>><<<>>>><... [92 chars]|*   <>><><<<><>|
|*   <<<<>>><<<<>><>><<<>>>>>|*   <>>>>>><<<>><<>><><|
|*   <<<<>>><<<><<>>><><<>>>><<>>|*   <>>>>>>><<<|
|*   <<<<>>><<><<<>... [84 chars]|*   >|
|*   <<<<>>>><<<><<... [52 chars]|*   ><|
|*   <<<><<<>>>><<<... [50 chars]|*   ><<<>><><<<><<|
|*   <<<><<><>>>>|*   ><<<>>>><><<<<><>>><<><><<|
|*   <<<><>><<<>>>>|*   ><<><<<<><<<<>>>><|
|*   <<<>><<<><<>>>... [44 chars]|*   ><><><<<>>>>>|
|*   <<<>><><<<><>>... [48 chars]|*   ><><>>><>><>|
|*   <<<>>><<><<<<>>>><<><<<>>>>>|*   ><><>>>><>>>>>>><>>><>>|
|*   <<><<<<>><>>>>... [60 chars]|*   ><>><<<<<>>|
|*   <<>>|*   ><>><><><<>><<>>><<|
|*   <<>><<<<<>>>>>... [54 chars]|*   ><>>><>>>>><<><<<><>><>><<<|
|*   <<>><<<<>><<<>... [74 chars]|*   >><<<><<<<<<><>><<|
|*   <>|*   >><>>><<<><>>><><<>><<><><<|
|*   <><>|*   >>>><>><>>>><>>><>><><|
| |*   >>>>><<<>>> |


老外君给了一句提示:虽然看起来不可能，但是数量有限。那么就一定是写死一定数量的某东西了... 通过观察发现，复杂的包含简单的，所以我们匹配到简单的就成： 这里继续是参考老外童鞋的思路，外层写死7层，为什么是7，因为基数是2层，就是最里面的是2，然后右边用*忽略多出来的层数，这个分数是286。

```^(<(<(<(<(<(<<>>)*>)*>)*>)*>)*>)*$```

##  十三题：

|Match all of these|and none of these…|
| --- | --- |
|*   x|*   xxx|
|*   xx|*   xxxxx|
|*   xxxx|*   xxxxxxx|
|*   xxxxxxxx|*   xxxxxxxxxxx|
|*   xxxxxxxxxxxxxxxx|*   xxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxx... [32 chars]|*   xxxxxxxxxxxxxxxxxxxxxxxxxxxx|
|*   xxxxxxxxxxxxxx... [64 chars]|*   xxxxxxxxxxxxxx... [48 chars]|
|*   xxxxxxxxxxxxxx... [128 chars]|*   xxxxxxxxxxxxxx... [160 chars]|
|*   xxxxxxxxxxxxxx... [256 chars]|*   xxxxxxxxxxxxxx... [401 chars]|
|*   xxxxxxxxxxxxxx... [512 chars]|*   xxxxxxxxxxxxxx... [600 chars]|
|*   xxxxxxxxxxxxxx... [1024 chars]|*   xxxxxxxxxxxxxx... [1025 chars]|


先写一个最笨的方法：

```^(x|xx|x{4}|x{8}|x{16}|x{32}|x{64}|x{128}|x{256}|x{512}|x{1024})$```

分数太低了，我们改进一下（初中的分段函数），61分：

```^((x{32}){6,32}|(x{32}){1,4}|(x{4}){1,6}|x{1,2})$```

##  十四题：

|Match all of these|and none of these…|
| --- | --- |
| *   0000 0001 0010 0011 0100 0101 0110 0111 1000 1001 1010 1011 1100 1101 1110 1111 |*   0000 0001 0000 0011 0100 0101 0110 0110 1000 1001 1010 1011 1100 1101 1110 1111|
| |*   0000 0001 0010 0010 0100 0101 0110 0111 1000 1001 1010 1011 1100 1101 1110 1111|
| |*   0000 0001 0010 0011 0000 0101 0110 0111 1000 1001 1010 1011 1100 1101 1110 1111|
| |*   0000 0001 0010 0011 0100 0101 0110 0011 1000 1001 1010 1011 1100 1101 1110 1111|
| |*   0000 0001 0010 0011 0100 0101 0110 0111 1000 0001 1010 1011 1100 1101 1110 1111|
| |*   0000 0001 0010 0011 0100 0101 0110 0111 1000 1000 1010 1011 1100 1101 1110 1111|
| |*   0000 0001 0010 0011 0100 0101 0110 0111 1000 1001 1010 1001 1100 1101 1110 1111|
| |*   0000 0001 0010 0011 0100 0101 0110 0111 1000 1001 101011011 1100 1101 1110 1111|
| |*   0000 0001 0010 0011 0100 0101 0110 0111 1000 1001 1110 1011 1100 1101 1110 1111|
| |*   0000 0001 0010 0011 0100 0101 0110 0111 1010 1001 1010 1011 1100 1101 1110 1111|
| |*   0000 0001 0010 0011 0100 0101 0110 011111000 1001 1010 1011 1100 1101 1110 1111|
| |*   0000 0001 0010 0011 0100 0101 0111 0111 1000 1001 1010 1011 1100 1101 1110 1111|
| |*   0000 0001 0010 0011 0100 0101 1110 0111 1000 1001 1010 1011 1100 1101 1110 1111|
| |*   0000 0001 0010 0011 0100 0111 0110 0111 1000 1001 1010 1011 1100 1101 1110 1111|
| |*   0000 0001 0010 0011 010010101 0110 0111 1000 1001 1010 1011 1100 1101 1110 1111|
| |*   0000 0001 0010 1011 0100 0101 0110 0111 1000 1001 1010 1011 1100 1101 1110 1111|
| |*   0000 000110010 0011 0100 0101 0110 0111 1000 1000 1010 1011 1100 1101 1110 1111|
| |*   0000 0101 0010 0011 0100 0101 0110 0111 1000 1001 1010 1010 1100 1101 1110 1111|
| |*   0001 0001 0010 0011 0100 0101 0110 0111 1000 1001 1010 1011 1100 1100 1110 1111|
| |*   1000 0001 0010 0011 0100 0101 0110 0111 1000 1001 1010 1011 1100 1101 1110 1110|


 这道题老外疏忽了，我们可以这样（191分）：

```js
0000 0001 0010 0011 0100 0101 0110 0111 1000 1001 1010 1011 1100 1101 1110 1111
```

是不是很无赖，仔细看以下，发现是二进制进位，先写了一个排重的语句，发现这货其实并不是所有的不能通过的例子都是重复的，还是要考虑顺序，于是：

