---
title: "构建易于扩展的前端统计脚本"
date: "2015-09-21 17:56:21"
---


抛砖引玉，简单聊聊前端统计的方案。

## 场景

随着一个公司规模的扩大，业务越来越繁荣，公司对于数据的依赖也会越来越明显。随便拍脑门定产品策略和『意淫』用户需要的产品需求无异于浪费公司和团队的资源，所以产品或多或少都进行了一些数据上的统计，来验证方向是否正确。用前一阵流行的话可以这么讲：『大数据下用数据驱动的方式来做事情。』 常规的Web应用中，我们可以通过后端接口的请求日志来进行用户行为分析，得出相对准确的数据，但是随着Web应用越来越『前端化』，SPA页面（Single Page Application）和增量更新的Hybird App越来越多，交互行为越来越多的停留在了当前页面：

*   当页面中有一个轮播图组件的时候，用户对其中的新闻/商品图片感兴趣，通过点击或者鼠标悬停(Hover)反复浏览了其中的几张。
*   当页面中有一个筛选工具的时候，用户随意选择，将页面中已经显示出来的内容进行了多次组合排序。
*   当页面中存在切换皮肤或者展示模式工具的时候，用户对界面进行了修改。
*   页面比较长，用户滚动了页面。
*   SPA应用中，用户切换了许多虚拟的『页面』。
*   一个页面中有许多搜索框，但是它们属于不同的业务线或者不同的模块。
*   异步从CDN上加载的图片，有没有加载失败，或者重新加载的。
*   某个页面由多个业务和应用组成，同一用户同时间访问，服务器产生日志比较分散。
*   ...

如果我们能够知道百分之多少的主要用户对于『某种颜色』更喜欢，对『某类交互组件』使用率更高，对『某种商品推荐』更感兴趣，不管是对于衡量产品健康程度，还是对产品的未来发展做出相对正确的决策，都是十分重要的。 可惜的是在这些时候，如果没有和后端接口进行主动交互，那么将无法记录用户的行为；或者说，至少无法精确的区分行为数据发生的位置。 所以前端统计应运而生，但是我们该用什么样的统计方案呢？ 看到这里，你一定会说直接用大家都在用的现成方案不就好了：

*   Google/Baidu/Tencent Analytics
*   CNZZ
*   51.la

如果公司规模尚小、流量不大、需求简单、没有历史包袱、用户在商业活动中产生的数据不是那么敏感的话，使用它们，没什么问题，倘若牵扯到上述问题，或许就是时候『造』或者『用』个符合自己需求的『轮子』了。 互联网公司中，崇尚榜样的力量。那我们就来看看业界其他公司使用中(公开)的方案吧：

*   Google
    *   [Google Analytics](https://www.google.com/analytics/)
*   Alibaba:
    *   [SPM && SCM](http://open.taobao.com/doc/detail.htm?id=959)
    *   [黄金令箭](http://www.cnblogs.com/a-jian/archive/2012/09/12/2681420.html)
    *   [量子恒道(原雅虎统计)](http://www.linezing.com/)
*   Baidu:
    *   [Alog](https://github.com/fex-team/alogs)
    *   [百度统计](http://tongji.baidu.com/web/welcome/login)
*   Tencent:
    *   [腾讯分析](http://ta.qq.com/)

上述方案无一不是使用传统的『前端打点』实现的，这种方案历史悠久中规中矩，可以参考几篇老文：

*   [统计日志打点方案的权衡via 奇虎irideas](http://www.75team.com/archives/170)
*   [Beforeunload打点丢失原因分析及解决方案via 1688朱铁根](http://ued.iliunian.com/?p=547)
*   [beforeunload丢失率统计via 淘宝鱼相](http://ued.taobao.org/blog/2012/11/beforeunload%E4%B8%A2%E5%A4%B1%E7%8E%87%E7%BB%9F%E8%AE%A1/)

看过这些，你或许会想，那么按照这种玩法，照猫画虎的来一发，不就好了么。 『没有银弹。』 其实我也希望能够使用这种简单的方案，但是看着这种方案带来的问题：

*   业务代码和统计代码高度耦合，不利于统计方案的变更；
*   浏览器额外请求一堆，网络质量不佳时，阻塞正常浏览；
*   上报url参数繁多，后续维护成本较高；
*   直接使用，需要业务方对老代码进行较多的修改，落地周期相对较长；

所以，搞起！

## 任务

实现兼容已有统计方案，对业务无副作用、稳定、准确、可剥离的统计代码。

## 行动

### 方案调研

常规前端统计方案有哪些呢：

*   发送get请求。
*   发送post请求。
*   基于flash建立flash socket长连接的上报。
*   基于websocket长连接的上报。

随着客户端对HTML5标准的支持度的完善，和Flash的衰落，Flash相关方案沦为了客户端中的下策：iOS不支持Flash控件、Google和Mozilla的浏览器中要逐步干掉Flash，已经干掉了基于Flash的广告、用户需要在设备上安装Flash控件、低版本IE浏览器无法获取origin... WebSocket方案先不论设备支持程度（虽然势头良好），对于许多成长中的公司，还需要投入额外的机器和人力资源。如果没有强烈的实时统计的需求，是否值得投入建设有待商榷。 传统使用Get请求的方案可以相对实时的将业务所需的数据统计上来，且技术点简单明晰，天然支持跨域，纵观Google analytics、百度统计（&&厂内诸多小实现方案）、淘宝SPM && 黄金令箭等无一不是使用这个方案。但是它的缺点也很明显：

*   没有完全剥离业务和统计工具，对于有关联的上下游页面，必须使用URL参数来进行维护关系，久而久之URL上添加了一堆不明觉厉的额外参数，参数有跨业务被覆盖或丢失的风险。
*   发送统计数据上报请求相对频繁，网络质量差的情况下对用户体验有影响。
*   策略相对固定，发送数据量有限且数据格式不易调整修改，容易遭到数据污染，影响准确性。
*   提交统计数据后，不易根据服务端响应实时调整策略。如果使用get图片等非可执行资源的方式，那么无法获取服务端下发策略；如果使用getJSON类似的方案，有安全风险。

所幸，天无绝人之路，还留下了`POST`方案。先说缺点：

*   IE6 POST跨域比较麻烦。（我司抛弃了这个化石浏览器，所以不是问题）
*   IE7/8跨协议无法提交数据。（公司目前正在从HTTP过度HTTPS，所幸IE7 && IE8用户量较少，接受降级使用业务页面的协议进行数据上报。）
*   需要等待服务器有效响应，相比较GET请求数据交换时间长。

暂时无法跨过的门槛只有以上三个，如果你可以接受以上三点，并且也不想使用Flash和WebSocket作为主要上报数据方案的话，我们来看看这个方案的优势:

*   极适合配合本地储存一起使用，不污染URL。
*   提交数据尺寸基本不受限制，同样数据量，发送频率相比较Get方式可以降低不少，利于弱网访问。
*   支持上报数据后获取服务端响应，以确保数据是否正确上报，以及根据服务器返回来动态更新策略。
*   使用XHR/XDR组件直接跨域&&跨协议提交，方案简单粗暴。
    *   对于Safari和Blink/较新的webkit浏览器对于POST行为不一致，一行代码解决问题。
*   关于跨域的问题，可以围观MDN的:[HTTP access control (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
*   关于IE的XDR方案，可以查看MSDN:[XDomainRequest object](https://msdn.microsoft.com/en-us/library/cc288060(v=vs.85).aspx)
*   关于FLASH的问题和八卦，可以围观知乎这篇问题的回复:[未来是HTML5还是Flash的时代?](http://www.zhihu.com/question/19728465/answer/63865048)

### 使用痛点

业务方常规使用统计代码的方式有：

1.  代码直接进入业务代码仓库，成为业务的一部分。
2.  代码使用标签的方式异步引入页面，业务方使用需要等待统计脚本初始化好；代码使用同步方式引入页面，优先初始化，如果不能统一页面共有引入部分（存在多个业务方），代码升级需要依次通知引入的业务方，重复部署安装代码。
3.  数据来源希望有白名单控制，尽可能减少恶意提交。

解决方案：

1.  对于特殊业务，诸如hybird中包含离线更新包的应用，提供支持各种模块化使用的UMD包。
2.  提供最小化的种子安装代码，让业务方可以忽略异步的问题，直接同步使用代码；使用种子安装代码来动态加载统计脚本，解决文件版本更新的问题，且非大版本更新和原则性冲突，对承诺的API的行为进行保障和外观模式的升级。
3.  除了清洗恶意提交流量之外，在上报接口处的前端机上进行CORS设置。

UMD模块示意：

```javascript
;(function (root, factory) {
    if (typeof define === 'function') {
        define(['analytics'], factory);
    } else if (typeof exports === 'object') {
        module.exports = factory();
    } else {
        factory();
    }
}(this, function () {
    'use strict';
/* global define */
var MA = function(){
    // 自动转换的代码
    return MA;
}();
    return MA;
}));
```

种子代码设计示例:

```javascript
/**
 * 页面使用加载代码
 * @version {{$version}}
 */
(function(config) {

    /**
     * 常量
     * @type {string}
     */
    var STR_EMPTY = '';
    // 和异步代码一致的DOM Id
    var STR_DOM_ID = 'abc-analytics';

    /**
     * 私有注册事件id
     * @notice 保存前100个私有频道
     * @type {number}
     * @private
     */
    var _instanceId = 100;

    /**
     * 注册频道cache
     * @type {{}}
     * @private
     */
    var _channelCache = {};

    /**
     * 保存实例对象
     * @type {null}
     * @private
     */
    var _instance = null;

    /**
     * 简化的统计对象
     * @private
     */
    var _MA = {
        // 保存配置信息
        config     : config,
        // 保存所有调用信息
        data       : {},
        // 初始化时保存初始化配置
        init       : function(config) {
            var useStrCfg = false;
            var data = config || {};
            if (Object.prototype.toString.call(config).slice(8, -1).toLowerCase() === 'string') {
                useStrCfg = true;
            }
            if (useStrCfg) {
                if (_channelCache[useStrCfg]) {
                    return _channelCache[useStrCfg];
                }
                data = {};
            }
            _instanceId++;
            this.data[_instanceId] = {};
            this.data[_instanceId].init = data;
            if (useStrCfg) {
                _channelCache[useStrCfg] = _instanceId;
                this.data[_instanceId].channelName = config;
            }
            return _instanceId;
        },
        // 未选择实例
        selectInst : false,
        // 当前状态未就绪
        status     : false,
        // 选择实例
        use        : function(id) {
            // 尝试选择当前序号的实例
            _instance = _MA.data[id];
            // 创建当前对象的浅copy
            var shadow = _clone(_MA);
            shadow.selectInst = true;
            return shadow;
        }
    };

    // 获取数值没有初始化一律返回为空
    _MA.get = _MA.getEnv = _MA.getEvs = _MA.getTag = function() {
        return;
    };
    // 上报返回`false`
    _MA.report = _MA.reload = function() {
        return false;
    };

    /**
     * 储存参数
     * @returns {_storage}
     * @private
     */
    function _storage() {
        var params = Array.prototype.slice.call(arguments, 0);
        var actName = params.shift();
        _instance[actName] = _instance[actName] || [];
        _instance[actName].push(params);
        return this;
    }

    /**
     * 检查调用写入相关API是否选择了实例
     * @returns {*}
     * @private
     */
    function _checker() {
        var shadow = _clone(_MA);
        var error = {
            func : arguments.callee.name,
            argv : arguments
        };
        if (!_instance) {
            error.desc = '未初始化实例';
            shadow.error = shadow.error || error;
            return shadow;
        }
        if (!this.selectInst) {
            error.desc = '未使用`use`初始化';
            shadow.error = shadow.error || error;
            return shadow;
        }
        var argv = Array.prototype.slice.call(arguments, 0);
        argv.unshift(arguments.callee.caller.name);
        return _storage.apply(this, argv);
    }

    /**
     * 浅拷贝对象
     * @param obj
     * @returns {{}}
     * @private
     */
    function _clone(obj) {
        // 省略实现
    }

    /**
     * 更新环境信息
     * @since 2.1.0
     * @returns {*}
     */
    function updateEnv() {
        return _checker.apply(this, arguments);
    }

    _MA.updateEnv = updateEnv;

    /**
     * 更新事件信息
     * @returns {*}
     */
    function updateEvs() {
        var argv = arguments;
        // 省略业务实现
        return _checker.apply(this, argv);
    }

    _MA.updateEvs = updateEvs;

    /**
     * 保存Tag信息
     * @returns {*}
     */
    function updateTag() {
        return _checker.apply(this, arguments);
    }

    _MA.updateTag = updateTag;

    /**
     * 将新对象挂载全局
     */
    window.Analytics = _MA;

    /**
     * 当前加载代码版本
     * @type {string}
     */
    var version = '{{$version}}';

    /**
     * 使用SDK的客户端类型
     * @notice 暂时可选 web/mobile
     * @since  0.1.0
     * @type {string}
     */
    var client = config.client || 'web';

    /**
     * 使用的统计脚本的发布类型
     *
     * @notice 暂时可选 stable
     * @since 0.1.0
     * @type {string}
     */
    var category = config.category || 'stable';

    /**
     * 客户端脚本更新频率
     * @type {*|{}|cache|number}
     */
    var cache = config.cache || 4;

    /**
     * 客户端当前时间
     * @type {Date}
     */
    var date = new Date();

    /**
     * 是否自动运行
     * @since 2.1.0
     * @type {string|string}
     */
    var autoRun = config.run !== 'manual';

    /**
     * 要加载的脚本资源的根域名
     *
     * @todo 后期需要移动到https CDN
     * @private
     * @since 0.1.0
     * @type {host|string}
     */
    var host = config.host || location.hostname;
    // 域名容错
    if (!host.match(/(domainA|domainB).com/)) {
        host = 'www.domainA.com';
    }
    host = host.replace(/(\w+\.)*?(\w+\.(com|com.cn))/, 'analytics.$2');
    /**
     * 计算URL中需要使用的时间戳
     * @private
     */
    var _timestamp = (function() {
        function prefix(str) {
            if (typeof str === 'number') {
                str = str.toString();
            }
            return ('00' + str).substr(-2);
        }

        if (cache === -1) {
            return (date - 0) + STR_EMPTY;
        }

        return [
            date.getFullYear() + prefix(date.getMonth() + 1),
            prefix(date.getDate()),
            prefix(date.getHours()),
            prefix(date.getMinutes())
        ].slice(0, cache).join(STR_EMPTY);
    }());

    /**
     * 约束加载脚本逻辑为启动函数
     * @private
     * @since 2.1.0
     */
    var launch = function() {
        var headElem = window.head;
        /* jshint -W085,-W020 */
        if (!headElem) {
            headElem = document.getElementsByTagName('head')[0];
        }
        with (document)with (headElem)with (insertBefore(createElement('script'), firstChild))setAttribute('data-start', (date - 0), id = STR_DOM_ID, src = ['/', host, category, _timestamp, client, 'index.js'].join('/').toLowerCase(), ver = version, defer = 'defer');
        /* jshint +W085, +W020 */
    };

    // 插入脚本到当前页面中
    if (autoRun) {
        launch();
    } else {
        window.Analytics.launch = launch;
    }
})({config : {'client' : 'web', 'category' : 'stable', 'run' : 'auto'}});
```

上报接口的CORS简单白名单设置：

```
location ~* / {
    gzip on;

    default_type "application/json; charset=utf-8";
    add_header "Cache-Control" "no-cache";

    if ($http_origin ~* (https?://.*\.whitelistA\.com(:[0-9]+)?)) {
        set $cors "true";
    }
    if ($http_origin ~* (https?://.*\.whitelistB\.com(:[0-9]+)?)) {
        set $cors "true";
    }
    if ($http_origin ~* (https?://.*\.whitelistC\.com(:[0-9]+)?)) {
        set $cors "true";
    }

    if ($request_method = 'GET') {
        set $cors "${cors}get";
    }
    if ($request_method = 'POST') {
        set $cors "${cors}post";
    }
    if ($cors = "trueget") {
        add_header 'Access-Control-Allow-Origin' "$http_origin";
        add_header 'Access-Control-Allow-Credentials' 'true';
    }
    if ($cors = "truepost") {
        add_header 'Access-Control-Allow-Origin' "$http_origin";
        add_header 'Access-Control-Allow-Credentials' 'true';
    }

    echo "{\"status\":200}";
    # 如果后面的接口是动态的，可以直接重定向过去
    #rewrite ^.*$ /index.php last;
    # 如果后面的接口是静态的，nginx需要忽略405错误
    error_page 405 = $uri;
}

```

### 开发痛点

不同的业务方的用户的群体和数量不同，对于需求也不同，反映到技术上的问题会有：

1.  上报时间的个性化需求。
    1.  尽可能及时，放弃POST可以提交大量数据的优势。
    2.  尽可能慢，因为数据量太大，频率太高，抽样即可。
2.  客户端终端类型稍有差异的客户端和业务，比如：
    1.  需要兼容离线储存、和JSON等功能的业务（IE7/IE8兼容模式/套壳浏览器/Safari低版本存在JSON无法正常使用的情况）。
    2.  尽可能减少统计脚本尺寸的移动端业务。
    3.  不需要脚本进行HTTP(S)数据请求，数据通信一律由native客户端程序处理的hybird业务。
3.  线上出现测试没有覆盖到的问题，调试不便；预发布版本，进行用户灰度测试。

解决方案：

1.  提供手动调用上报接口的方法，以及手动设置调用频率的方法，并在合理时间范围内做校验，防止数据被恶意/错误的设置的过长或者过短，影响数据的采集。
2.  尽可能使用localStorage为数据持久化方案，做好polyfill；做好JSON的polyfill，谢绝使用eval，防止带来安全隐患。
    *   关于localStorage的简单polyfill，可以参考[修改自jStorage的xStorage](https://github.com/soulteary/xStorage)，至于用cookie来做localStorage的shim，如果是站群的场景，个人认为弊大于利，故不推荐。

面对1&&2，除了提供给业务方混合到业务中使用的UMD模块写法的插件形式的代码外（不推荐，因为涉及到业务方维护和升级），结合自定义的构建模式、自建模块化方案，支持差异化打包，生成适配多套终端代码，而非仅仅生成一套代码去适配所有客户端，白白增大代码体积。

1.  配合服务器简单设置，进行差异化调试，和灰度设置。

代码示例，配置文件中的版本号由脚本构建工具自动生成，无须人工干预:

```
http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile            on;
    tcp_nopush          on;
    keepalive_timeout  110;
    gzip                on;
    gzip_proxied any;
    gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/javascript;

    upstream mc_server {
        server 127.0.0.1:11211;
        keepalive 512 ;
    }

    server {
        listen       8080;
        server_name  localhost;
        access_log   off;

        root   /your/webroot;
        index  index.html index.htm;

        location ~* ^/favicon.ico$ {
            access_log off;
            proxy_pass http://www.domainA.com;
            break;
        }

        location /stable {
            if (!-f $request_filename) {
                set $cache_key $request_uri;
                srcache_fetch GET /cache $cache_key;
                srcache_store PUT /cache $cache_key;
                add_header X-Cached-From $srcache_fetch_status;
                rewrite ".*\d+/(\w+)/.*.js" /release/2.1.0/$1/index.js break;
            }
        }

        location /dev {
            if (!-f $request_filename) {
                set $cache_key $request_uri;
                srcache_fetch GET /cache $cache_key;
                srcache_store PUT /cache $cache_key;
                add_header X-Cached-From $srcache_fetch_status;
                rewrite ".*\d+/(\w+)/.*.js" /release/2.1.1/$1/index.js break;
            }
        }

        # 如果需要根目录调试，需要将下面的内容同样添加到根目录
        location ~* /release {
            concat on;
            concat_unique on;
            concat_max_files 20;
        }

        location /cache {
            internal;
            memc_connect_timeout 100ms;
            memc_send_timeout 100ms;
            memc_read_timeout 100ms;
            set $memc_key $query_string;
            set $memc_exptime 30;
            memc_pass data_assets_mc_server;
        }

        location /flush {
            set $memc_cmd flush_all;
            memc_pass data_assets_mc_server;
        }

        location /stats {
            set $memc_cmd stats;
            memc_pass data_assets_mc_server;
        }

        location / {
            proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header Accept-Encoding "";
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For  $proxy_add_x_forwarded_for;
            try_files $uri $uri/ /index.php?q=$uri&$args;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /your/webroot;
        }

   }
}

```

不同业务线会因为用户特点（回头客多寡、移动端多寡、老设备多寡、是否实时...）而需要稍有不同的release版本，目前我们使用的方案是： 不同业务版本，使用不同的入口文件，尽可能共享模块文件。 项目结构如下：

```
.
├── Gulpfile.js
├── README.md
├── benchmark
├── demo
├── deploy
│   ├── README.md
│   ├── nginx.conf
│   ├── post-deploy.sh
│   └── pre-deploy.sh
├── dist
├── package.json
├── release
│   ├── 1.0.0
│   │   ├── file.xxx.js
│   │   └── ...
│   ...
│   ├── 2.1.3
│   │   ├── file.xxx.js
│   │   └── ...
│   └── README.md
├── server-mock
│   └── ...
└── src
    ├── app
    │   ├── file.xxx.js
    │   └── ...
    ├── conf
    │   ├── conf.xxx.js
    │   └── ...
    ├── core
    │   ├── file.xxx.js
    │   └── ...
    ├── demo
    │   ├── demo.xxx.html
    │   └── ...
    └── module
        ├── module.xxx.js
        └── ...
```

如果是Web通用版本的统计脚本，那么入口文件是`src/web/main.js`，如果是mobile通用版本，那么入口文件就是`src/web/mobile.js`。 它们私有配置会存放在`conf/*.js`中； 它们共同依赖会存放在`core/*.js`中； 它们私有依赖会存放在`module/localstorage.web(mobile).js`中。 如果有一个Web版本需要支持实时提交数据，可以创建`src/web-realtime/main.js`、`conf/web-realtime.js`、`module/socket.web-realtime.js`，简单实现socket相关功能后，修改下文件引用，打包就可以使用了。 同样，如果对于数据格式有了修改，或者数据通讯方式有了修改，可以直接在`core/network.js/report.js`模块中进行修改。 接下来我们会尝试将文件使用模块shim（同AMD具名模块）的方式，进行打包，达到修改模块只修改一份文件，防止漏改的目的（另外一个项目中已经稳定使用），但是要求业务逻辑必须尽可能相同。

### 线上调试

最后，来简单说说线上调试。 上面目录中的`release`脚本将会被发布到线上，上文提到的种子代码将会自动加载脚本，引入的资源地址形如(时间戳根据配置进行不同程序的浏览器端的缓存。)：

```
http://analytics.domainA.com/stable/时间戳/web/index.js
```

同时它等价于:

```
https://analytics.domainA.com/{{release category}}/{{release version}}/??moduleA,moduleB,...
```

如果现在线上的stable版本为2.1.0，那么实际加载的路径是：

```
https://analytics.domainA.com/stable/2.1.0/??moduleA,moduleB,...
```

预发布想进行灰度的版本是2.1.1，并且改动只有moduleA一个文件的话，那么可以进行这样的访问：

```
https://analytics.domainA.com/??stable/2.1.0/moduleB,stable/2.1.0/moduleC,dev/2.1.1/moduleA
```

然后把这个地址写入上面的nginx.conf中，使用请求参数或者IP段的方式使之生效即可。 当然，也可以直接用前端代理工具，把线上的URL和这个手动拼合出来的URL进行代理替换。

## 结果

上述方案和想法已经迭代了几个版本，并在公司中进行推广使用，数据没有太大的差异。