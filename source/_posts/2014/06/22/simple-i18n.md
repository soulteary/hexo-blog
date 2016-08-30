---
title: "[JS]简单的i18N脚本"
date: "2014-06-22 00:42:16"
tags: ["i18n","js"]
---


一个简单的i18N脚本，如果在单页面展示页面中你有切换语言的需求，那么可以尝试使用它。

如果有任何建议或者意见，欢迎反馈。

GITHUB仓库:[https://github.com/soulteary/simple-i18N](https://github.com/soulteary/simple-i18N)

在线DEMO:[http://soulteary.github.io/simple-i18N/](http://soulteary.github.io/simple-i18N/)

代码没几行，去掉注释的话...

```js
(function (cfg) {
    "use strict";
    /**
     * @file:   i18N.js
     * @desc:   一个简单的i18N.js
     * @author: [soulteary](soulteary@qq.com)
     * @date:   2014.06.21
     * @useage:
     *
     *  --default HTML structure:
     *      <body>
     *        <h1 data-lang-en="英文">中文</h1>
     *        ...
     *      </body>
     *
     *
     *  --default out side config:
     *
     *      {"ver": "2014.06.21", "win": window, "doc": document, "api": "lang"}
     *
     *
     *  --default inner config:
     *
     *      var config = {
     *          "version": cfg.ver,         //版本号
     *          "key": "data-lang-",        //DOM节点保存文本的命名空间前缀
     *          "tag": "data-lang",         //保存到父节点上的当前语言标志
     *          "rule": {                   //语言规则
     *              "zh": "zh-CN",
     *              "en": "en-US"
     *          },
     *          "default": "zh"             //默认语言
     *      };
     *
     *
     *  --default open api:
     *
     *      window.lang('zh');
     *      window.lang('en');
     *
     */

    /**
     * 缓存全局对象
     */
    var win = cfg.win,
        doc = cfg.doc;

    /**
     * 要进行翻译的对象
     * @type {NodeList}
     */
    var wrap = doc.getElementsByTagName('body')[0];
    var elems = wrap.getElementsByTagName('*');

    /**
     * 声明全局配置
     * @type {{version: string, key: string}}
     */
    var config = {
        "version": cfg.ver,         //版本号
        "key": "data-lang-",       //DOM节点保存文本的命名空间前缀
        "tag": "data-lang",         //保存到父节点上的当前语言标志
        "rule": {                   //语言规则
            "zh": "zh-CN",
            "en": "en-US"
        },
        "default": "zh"             //默认语言
    };

    /**
     * 获取默认的i18N设置
     * @param short
     * @returns {string}
     */
    var getLang = function (short) {
        var sets = doc.cookie;
        if (sets) {
            return short ? (JSON.parse(sets)).lang : config.rule[(JSON.parse(sets)).lang];
        } else {
            return short ? config.default : config.rule[config.default];
        }
    };

    /**
     * 初始化页面的hash
     * 优先从cookie中获取，如果获取不到，那么使用默认配置
     */
    win.location.hash = "#!/" + getLang();

    /**
     * 切换页面语言
     * @param input
     * @returns {boolean}
     */
    var switchLang = function (input) {
        if (!input) {
            return false;
        }
        try {
            for (var lang in config.rule) {
                //如果请求切换语言存在于配置中，并且要切换的语言不是当前语言
                if (input === lang && getLang() !== config.rule[lang]) {
                    //切换翻译
                    translate(lang);
                    //更新uri hash
                    win.location.hash = "#!/" + config.rule[lang];
                    //更新cookie
                    doc.cookie = '{"lang":"' + lang + '"}';
                    break;
                }
            }
            return true;
        } catch (e) {
            return false;
        }
    };

    /**
     * 翻译所有元素
     * @returns {boolean}
     */
    var translate = function (to) {

        var toLang = config.key + to,
            curLang = config.key + (wrap.hasAttribute(config.tag) ? wrap.getAttribute(config.tag) : config.default);

        /**
         * 目标语言不相等的时候进行翻译
         */
        if (toLang === curLang) {
            return false;
        }

        /**
         * 翻译每一个元素
         * @param elem
         * @returns {boolean}
         */
        var trans = function (elem) {
            var toText;
            try {
                if (elem.hasAttribute(toLang)) {
                    toText = elem.getAttribute(toLang);
                    elem.setAttribute(curLang, elem.innerHTML);
                    elem.innerHTML = toText;
                }
                return true;
            } catch (e) {
                return false;
            }
        };

        /**
         * translate each element.
         */
        try {
            for (var i = 0, j = elems.length; i < j; i++) {
                trans(elems[i]);
            }
            //更新当前语言到wrap dom
            wrap.setAttribute(config.tag, to);
            return true;
        } catch (e) {
            return false;
        }
    };

    /**
     * 公开函数到全局
     */
    window[cfg.api] = switchLang;

    if (getLang() !== config.default) {
        translate(getLang(true))
    }

})({"ver": "2014.06.21", "win": window, "doc": document, "api": "lang"});
```
