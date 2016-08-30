---
title: "动态绑定浏览器插件弹出窗口内容"
date: "2016-02-26 15:54:24"
tags: ["chrome","javascript"]
---


最近在重构插件，把之前和现在遇到的问题都记录一下吧，抛砖引玉，不对的地方，欢迎指正，:)。

## 诉求起源

在编写的插件的时候，我们经常会出现仅允许在某些情况下才可以弹出插件窗口的需求，如：用户打开了非插件页面、书签页面甚至是其他的插件页面。 chrome允许我们在manifest指定browseraction和pageaction，但是假如我们直接在manifest文件中指定了这两个json字段的内容，那么弹出窗的内容将会被『锁死』。

## 解决方案

browseraction和pageaction都包含一个叫做setPopup的API，允许我们动态的设置插件的弹出窗内容，解决问题的关键点之一也就是这个API了。 而在何时执行setPopup，怎么执行setPopup是另外一个关键点，执行过早，插件交互一样会如同诉求起源的问题一样，『锁死』弹出内容。 下面分别以两种类型的插件为例，这两种类型的插件互斥，manifest仅允许存在其中之一（仅简单描述插件的形式区别，暂不展开描述）：

### browser_action

这种类型的插件的表现形式是在插件栏会多一个图标，适用于各种场景。

#### manifest 配置

关键点在于不设置默认的popup等信息。

```
"browser_action": {
    "default_icon": "image/icon/app-128.png",
    "default_title": "字段配置从package.json中读取"
}
```

#### 示例代码

在插件的background.js中合适位置声明每个tab的行为，即可以做到动态展示插件内容。 个人倾向在tabs.onUpdated事件中每个tab的loading状态进行弹出内容绑定，简单可靠：即可以解决新的tab的事件绑定，又能防止同一个tab内容切换后，插件弹出内容没有变化的问题。

```javascript
function checkCurrentTabIsCorrect (tab) {
    return helper.isStandardLink(tab.url, ['http', 'https']);
}

function init () {
    try {
        //初始化事件绑定
        chrome.tabs.onUpdated.addListener(function (tabId, changeInfo, tab) {
            switch (changeInfo.status) {
                case 'loading':
                    chrome.browserAction.enable(tab.id);
                    //浏览器标签未初始化完毕时初始化插件图标
                    if (checkCurrentTabIsCorrect(tab)) {
                        // 绑定弹出窗内容A
                        chrome.browserAction.setPopup({'tabId' : tab.id, 'popup' : 'page/popup/main.html'});
                    } else {
                        // 用户打开其他的插件页面或者chrome内置页面
                        if (tab.url.indexOf('chrome-extension://' + chrome.runtime.id) !== 0 || tab.url.indexOf('chrome://')) {
                            // 绑定弹出窗内容B
                            chrome.browserAction.setPopup({
                                'tabId' : tab.id,
                                'popup' : knowledge.showTopic('error', 'not-allow-page')
                            });
                        } else {
                            // 如果是自己的插件页面不允许弹出（示例）
                            chrome.browserAction.disable(tab.id);
                        }
                    }
                    break;
                case 'complete':
                    //标签完成更新
                    break;
                default :
                    break;
            }
        });
    } catch (e) {
        //插件加载出现错误，正在重载
        location.reload();
    }
}

document.addEventListener('DOMContentLoaded', init);
```

### page_action

这种类型的插件的表现形式是在地址栏多一个图标，适用于插件功能和当前页面强关联的场景。

#### manifest 配置

```
"page_action": {
    "default_icon": "image/icon/app-38.png",
    "default_title": "字段配置从package.json中读取"
}
```

#### 示例代码

和browser_action api稍有不同。

```javascript
chrome.tabs.onUpdated.addListener(function (tabId, changeInfo, tab) {
    switch (changeInfo.status) {
        case 'loading':
            //浏览器标签未初始化完毕时初始化插件图标
            chrome.pageAction.show(tab.id);
            if (checkCurrentTabIsCorrect(tab)) {
                // 绑定弹出窗内容A
                chrome.pageAction.setPopup({'tabId' : tab.id, 'popup' : 'page/popup/main.html'});
            } else {
                if (tab.url.indexOf('chrome-extension://' + chrome.runtime.id) !== 0) {
                    // 绑定弹出窗内容B
                    chrome.pageAction.setPopup({
                        'tabId' : tab.id,
                        'popup' : knowledge.showTopic('error', 'not-allow-page')
                    });
                } else {
                    // 如果是自己的插件页面不允许展示图标（示例）
                    chrome.pageAction.hide(tab.id);
                }
            }
            break;
        case 'complete':
            //标签完成更新
            break;
        default :
            break;
    }
});
```
