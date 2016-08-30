---
title: "再说按键触发的彩蛋"
date: "2014-01-21 00:55:40"
tags: ["js","彩蛋","按键"]
---


不知道还有多少人对“上上下下左左右右ABBA”记忆犹新。 

再次想起这个秘籍，第一反应居然是QQ音乐，好吧，就用他的代码来说事吧。 

原始代码如下：

```js
MUSIC.channel.top.egg_contra = function() {
    var _konamiCode = [], _jsLoader = null;
    function init(doc) {
        MUSIC.event.on(doc || document, "keyup", _trick)
    }
    function _reset() {
        _konamiCode = [65, 66, 39, 37, 39, 37, 40, 40, 38, 38]
    }
    function _trick(evt) {
        evt = MUSIC.event.getEvent(evt);
        var t = _konamiCode.pop();
        try {
            if (evt.keyCode - t == 0) {
                if (_konamiCode.length == 0) {
                    if (!_jsLoader) {
                        _jsLoader = new MUSIC.JsLoader;
                        _jsLoader.onload = function() {
                            try {
                                g_eggContraEx.start()
                            } catch (e) {
                            }
                        };
                        _jsLoader.load("http://imgcache.gtimg.cn/music/portal_v3/extend/egg_contra.js",
                            null, "utf-8")
                    } else
                        try {
                            g_eggContraEx.start()
                        } catch (e) {
                        }
                    _reset()
                }
            } else
                _reset()
        } catch (e) {
        }
    }
    _reset();
    return {init: init}
}();
```

经典的揭示模块的写法，对外提供了一个简单的可调的api:init，我们来精简一下代码：

```js
var egg = function() {
    //
    var _konamiCode = [], _jsLoader = null;
    //初始化，用于注册事件
    function init(doc) {
        //如果其他事件中已经获取过document并且传递进来
        //那么用之前缓存好的，否则直接绑定document元素的按键事件
        doc = doc || document;
        doc.addEventListener("keyup", _trick);
    }
    //重置按键序列
    function _reset() {
        _konamiCode = [65, 66, 39, 37, 39, 37, 40, 40, 38, 38]
    }
    //判断是否秘籍合法
    function _trick(e) {
        if(!e||!e.keyCode){
            return false;
        }
        //依次取序列最后的数值
        var t = _konamiCode.pop();
        if(0 == e.keyCode - t){
            if(0==_konamiCode.length){
                //执行你想要的东西
                alert('excute code.');
                //重置按键序列或者也可以加标志，或者移除事件
                _reset();
            }
        }else{
            _reset()
        }
    }
    //初始化按键序列
    _reset();
    return {init: init}
}();
//初始化，并调用公开API绑定事件。
egg.init()
```

大概就是这样了吧，睡醒再说。

