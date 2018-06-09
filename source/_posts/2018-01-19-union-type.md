---
layout: post
title:  "[TypeScript]Union Type & Type Guard"
date:   2018-01-19 20:25:26 +0800
categories: JavaScript
tags:
-  JavaScript
-  TypeScript
---

最近寫公司專案時，常常遇到需要接收不同資料格式的情況，因為不想在另寫功能相仿的 function，導致重複的程式碼，這時 TypeScript 的 `Union Type` 與 `Type Guard` 就派上用場了。

原本 TS 的型別檢查是像這樣子的：

``` javascript
interface Param {
    value:string;
    index:number;
}
const getValue = (param: Param) => {
    return param.value;
}
```

如果想用同一函式，然而傳來的 param 資料格式不一樣，TS 會報錯。這時就可以採用 `Union Type` 來定義多種資料格式。

```javascript
interface Param1 {
    value:string;
    index:number;
}
interface Param2 {
    content:{
        value:string;
        index:number;
    }
    getValueOfProduct:()=>{};
}
const getValue = (param: Param1 | Param2) => {
    return param.value || param.content.value;
}
```
不過這時會產生另一個問題，TS 並不知道你傳進來的 param 是哪一個格式，一樣會報錯(Param1 沒有 Param2 的參數，Param2 沒有 Param1 的參數)，這時必須再增加 Type Guard 函式來檢查。

Type Guard
``` javascript

function checkIfParam1(param: Param1 | Param2):param is Param1 => {
    return (<Param1>param).value !== undefined;
}
const getValue = (param: Param1 | Param2 ) =>{
    if(checkIfParam1(param)){
        return param.value;
    }else{
        return param.content.value;
    }
}
```
使用 `checkIfParam1()` 來驗證 param 是屬於哪一個 interface ，如此一來編輯器也能知道當前的 param 類型，就可以取用 param.value。
