---
layout: post
title:  "React-Router學習筆記"
date:   2016-06-21 00:49:26 +0800
categories: React
tags:
-  React
-  React-Router
---

### React Router

> 官方的話: React Router 是完整的React 路由解決方案，React Router 保持UI 與URL 同步。它擁有簡單的API 與強大的功能例如代碼緩衝加載、動態路由匹配、以及建立正確的位置過渡處理。你第一個念頭想到的應該是URL，而不是事後再想起。

### 前言
由於最近練習React時寫了個部落格系統，部落格系統有一點很重要就是需要URL匹配每一篇文章路徑，這時使用React-Router就能很輕鬆地分配不同路徑囉


### 使用方法 
新增一個`route.js`檔設定路由器，
引入react-router函式庫中的`Route`、`IndexRoute`  
簡易設定如下:

```javascript
import React from 'react';
import { Route, IndexRoute } from 'react-router';

import App from './components/app';
import PostsIndex from './components/posts_index';
import PostsNew from './components/posts_new';
import PostsShow from './components/posts_show';

const Page1 = () => {
  return <div>I'm page One.</div>
}
const Page2 = () => {
  return <div>I'm page Two.</div>
}
const Page3 = () => {
  return <div>I'm page Three.</div>
}

export default(
  <Route path="/" component={App} >
    <IndexRoute component={PostsIndex} /> {/*IndexRoute用來顯示首頁內容*/}
    <Route path="posts/new" component={PostsNew} />
    <Route path="posts/:id" component={PostsShow} />
    <Route path="page1" component={Page1} />
    <Route path="page2" component={Page2} />
    <Route path="page3" component={Page3} />
  </Route>
);

```
主程式`index.js`裡設定(範例搭配`Redux`、`Material-UI`):  

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore, applyMiddleware } from 'redux';
import { Router, browserHistory} from 'react-router';
import reducers from './reducers';
import routes from './route';
import promise from 'redux-promise';
const createStoreWithMiddleware = applyMiddleware(promise)(createStore);
import getMuiTheme from 'material-ui/styles/getMuiTheme';
import MuiThemeProvider from 'material-ui/styles/MuiThemeProvider';
ReactDOM.render(
  <Provider store={createStoreWithMiddleware(reducers)}>
    <MuiThemeProvider muiTheme={getMuiTheme()}>
      <Router history={browserHistory} routes={routes} />
    </MuiThemeProvider>
  </Provider>
  , document.querySelector('.container'));
```


