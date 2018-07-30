---
title: 用 JavaScript 操作 CSS 偽元素
date: 2018-07-27 22:56:23
categories: JavaScript
tags:
-  JavaScript
-  CSS
---

若要對一些原生 HTML element (video, input 等等) 修改樣式時，須針對各家瀏覽器前綴字樣的 CSS 偽元素做修改，
而 JS 似乎沒有直接操作偽元素樣式的 API，如果專案是一套 JS library，無法寫額外的 css 樣式檔，這時使用一些技巧還是能將偽元素樣式透過 JS 撰寫。

主要是透過 `createElement()` 來新增 css style 檔案到 head，透過 `createTextNode()` 將所需的樣式 append 到樣式表裡即可。

於是就順手寫了一個方便的 api，無論是偽元素或著是一般樣式皆可方便操作。

```javascript
const addStyle = function(selector, rule, pseudoElement) {
    const sheetId = "customPseudoStyle";
    const head = document.head || document.getElementsByTagName("head")[0];
    const styleSheet =
        document.getElementById(sheetId) || document.createElement("style");
    styleSheet.id = sheetId;

    function getRuleValue(rule) {
        let ruleString = "";
        let result = "";
        for (let key in rule) {
            ruleString += `${key}:${rule[key]};`;
        }
        result = `{${ruleString}}`;
        return result;
    }
    if (pseudoElement) {
        // pseudo element styling
        styleSheet.appendChild(
            document.createTextNode(
                `${selector}${pseudoElement}${getRuleValue(rule)}`
            )
        );
    } else {
        // normal css
        styleSheet.appendChild(
            document.createTextNode(`${selector}${getRuleValue(rule)}`)
        );
    }
    if (!document.getElementById(sheetId)) {
        head.appendChild(styleSheet);
    }
};
```

[GitHub](https://github.com/weiyuan1993/styling-in-js/blob/master/README.md)
