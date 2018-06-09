---
layout: post
title:  "React開發環境建置以及webpack的使用"
date:   2016-03-23 20:48:26 +0800
categories: React
tags:
-  React
-  Webpack
---

最近想要學習React這個火紅的javascript libarary來運用到工作上面，因為對於他的設計理念很感興趣，無奈於網路上找到的環境建置流程都殘缺不全，沒有一個完整的教學。

React官方網站有建議使用npm來安裝並且搭配module system(webpack、 browserify)環境，但一開始接觸完全搞不懂那些是什麼東西，就先使用Starter kit來用，只是學習一樣新東西，如果連安裝環境都不會建置，那也甭學了!

後來終於在國外網站友人分享[Setting up React for ES6 with Webpack and Babel](https://www.twilio.com/blog/2015/08/setting-up-react-for-es6-with-webpack-and-babel-2.html)
完全是我要的東西啊! 他這篇教學教我透過npm來安裝React，並且透過Babel轉譯ES6語法，真是一舉兩得，廢話不多說，來看一下流程吧。

### 開始之前

必要工具:Node.js、npm

在Win 10系統上的Node.js是自帶npm的，不用特別去下載

---

### 建置資料夾
在喜歡的地方新增資料夾，並且透過npm初始化

	npm init

這樣會在資料夾內產生package.json，可以讓我們紀錄需要哪些node_modules。

### 安裝React
	npm install --save react
	npm install --save react-dom

這樣會在node_modules資料夾內安裝react的檔案

### 安裝Webpack
我們還需要安裝Webpack來把檔案打捆(bundle)起來，以及Webpack development server來建置本地開發伺服器

	npm install --save-dev webpack
	npm install webpack-dev-server -g
--save

會只會安裝在資料夾內，也可`npm install webpack -g`安裝到全域

### 安裝Babel
Babel是用來幫助我們轉譯ES6語法以及React會用到的JSX語法，因為這些瀏覽器都還不支援，所以要透過轉譯器來轉換成舊的語法讓瀏覽器讀取。

	npm install --save-dev babel-loader
	npm install --save-dev babel-core
	npm install --save-dev babel-preset-es2015
	npm install --save-dev babel-preset-react

### 建立第一個component
上述基本上就結束react的安裝了，現在動手寫第一個元件(components)測試看看吧。

在React，components是獨立的building blocks，用來呈現View，這也就是react官網說他們是MVC架構裡的V(view)，react透過元件的狀態改變來自動渲染。

在資料夾內用編輯器**建立一個新檔案hello.jsx**

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
 
class Hello extends React.Component {
  render() {
    return <h1>Hello</h1>
  }
}
ReactDOM.render(<Hello/>, document.getElementById('hello'));
```
此為ES6寫法，也可以用ES5的`React.createClass`
再建立另一個檔案**world.jsx**到資料夾

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
 
class World extends React.Component {
  render() {
    return <h1>World</h1>
  }
}
 
ReactDOM.render(<World/>, document.getElementById('world'));
```

### 建立index.html

我們需要一個html來呈現網站，並且含有兩個div(hello、world)

```html
<!doctype html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Hello React</title>
  </head>
  <body>
    <div id="hello"></div>
    <div id="world"></div>
    <script src="bundle.js"></script>
  </body>
</html>
```
等等會提到為何要載入bundle.js這個檔案

### Bundling everything with Webpack!

#### 先建立一個新檔案**main.js**

```javascript
import Hello from './hello.jsx';
import World from './world.jsx';
```
import剛剛寫的兩個元件到main.js

#### 設定webpack

建立webpack.config.js

```javascript
var path = require('path');
var webpack = require('webpack');
 
module.exports = {
  entry: './main.js',
  output: { path: __dirname, filename: 'bundle.js' },
  module: {
    loaders: [
      {
        test: /.jsx?$/,
        loader: 'babel-loader',
        exclude: /node_modules/,
        query: {
          presets: ['es2015', 'react']
        }
      }
    ]
  },
};
```
entry是進入點，webpack會進入這個檔案把所有元件bundle起來output出bundle.js，index.html再載入這個bundle.js來取用所有元件。

loaders用來設定如何轉譯檔案，這個例子裡面我們設定轉譯ES6和React的JSX。

### 啟動webpack server

webpack最常用的指令

	webpack-dev-server --progress --colors

執行後webpack會build你所有的code，並啟動server，啟動後所有檔案的改動也會即時編譯讓你在瀏覽器馬上看到成果不須重新整理。

現在進入[http://localhost:8080/webpack-dev-server/](http://localhost:8080/webpack-dev-server/)就可以進入webpack開發者模式囉

今天就到這邊，之後再來研究Redux framework來結合react囉!