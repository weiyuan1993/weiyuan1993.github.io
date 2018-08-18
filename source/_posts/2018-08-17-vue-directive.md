---
title: "[Vue] 使用 Directive 來註冊客製化指令"
date: 2018-08-17 17:43:32
categories: Vue.js
tags:
  - Vue.js
---

# Vue Directive

最近在上 [Udemy](https://www.udemy.com/) 上開設的 Vue.js 課程 - [Vue JS 2 - The Complete Guide (incl. Vue Router & Vuex)](https://www.udemy.com/vuejs-2-the-complete-guide/)，講述了 `Directive` 的利用，覺得滿有趣的。

除了 Vue 自身內建的 `v-model`， `v-show` 等等指令，也可以自行註冊客製化的指令。


[Source code](https://github.com/weiyuan1993/vue-complete-guide/tree/master/homework-Directives)

## 註冊方式

**全局註冊**
```javascript
Vue.directive('hightlight', {
 //do something
})
```
**組件內註冊**
```javascript
directives: {
    'highlight': {
        //do something
    }
},
```

## hook 函數

Vue 提共幾個鉤子函數來使用(摘錄自官方文檔)):

- bind：只調用一次，指令第一次綁定到元素時調用。在這裡可以進行一次性的初始化設置。

- inserted：被綁定元素插入父節點時調用(僅保證父節點存在，但不一定已被插入文檔中)。

- update：所在組件的VNode更新時調用，但是可能發生在其子VNode更新之前。指令的值可能發生了改變，也可能沒有。但是你可以通過比較更新前後的值來忽略不必要的模板更新(詳細的鉤子函數參數見下)。

- componentUpdated：指令所在組件的VNode 及其子VNode全部更新後調用。

- unbind：只調用一次，指令與元素解綁時調用。

## 自定義一個閃爍效果的指令

課程裡只有利用到 `bind` 的 hook 函數，因此這邊只記錄 `bind` 的使用方式。

```javascript
directives: {
    'flash-highlight': {
        bind(el, binding, vnode) {
            const flashTime = binding.value.flashTime || 500
            const color1 = binding.value.color1 || 'yellow'
            const color2 = binding.value.color2 || 'pink'
            let currentColor = color1
            if (binding.modifiers['big']) {
                el.style.fontSize = '2em'
            } else if (binding.modifiers['small']) {
                el.style.fontSize = '0.8em'
            }
            setInterval(() => {
                currentColor == color2 ? (currentColor = color1) : (currentColor = color2)

                if (binding.arg === 'backgrond') {
                el.style.backgroundColor = currentColor
                } else {
                el.style.color = currentColor
                }
            }, flashTime)
        }   
    }
},

```

### binding.value
透過 hook 函式 `bind()` 裡的參數 `binding`，可以使用 `binding.value` 來取得指令的綁定值，這邊使用物件包起來來傳多個綁定值，設定好閃爍秒數及兩種顏色，利用 `||` 來讓指令更彈性，不一定都要填值。

### binding.modifiers
修飾符的使用，`v-model.trim` 的 `trim` 就是一個修飾符的運用，可以方便的在指令內直接套用刪除前後空白的效果。
透過 `binding.modifiers` 可取得修飾符陣列，這邊就可設定客製化的修飾符 `big`與 `small`，來讓被綁定的元素作放大或縮小的效果。

### binding.arg
`binding.arg` 為要傳給指令的參數，這邊傳入 background，來讓閃爍的顏色設定於背景，如果不傳入則是設定於元素本身。

### 使用於 template
```html
<!-- 不做任何設定與綁定 -->
<p v-flash-highlight>Crazy Directive</p>

<!-- 讓文字背景本身閃爍與縮小 -->
<p v-flash-highlight:background.big="{flashTime:700,color1:'orange',color2:'purple'}">Crazy Directive</p>

<!-- 讓文字本身閃爍與縮小 -->
<p v-flash-highlight.small="{flashTime:300,color1:'aqua',color2:'green'}">Crazy Directive</p>

```

玩過之後就發現 directive 可以做的變化很多，且可以精簡程式碼，讓各個組件都能使用到。

## 自訂義監聽

課程功課是自己實作 directive 練習，我自己練習了閃爍效果的指令，講者則是使用 directive 製作客製化的監聽指令。

有時候如果想要監聽到實際的 DOM，這時實作一個客製化的指令還滿方便的，就不需要使用到 `$ref` 來做操作(必須要在 template 上綁定 `ref` 屬性才能使用)。

### 使用方式

template

```html
<div v-customOn:click="click" v-customOn:mouseenter="mouseEnter" v-customOn:mouseleave="mouseLeave" class="square">
```

script
```javascript
directives:{
    customOn: {
        bind(el, binding, vnode) {
        // custom event
        const event = binding.arg
        const fn = binding.value
        el.addEventListener(event, fn)
        }
    }
}

methods:{
    click(e) {
      console.log('click', e, this)
    },
    mouseEnter() {
      console.log('mouse enter')
    },
    mouseLeave() {
      console.log('mouse left')
    }
}
```









