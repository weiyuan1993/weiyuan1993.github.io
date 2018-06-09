---
layout: post
title:  "[DB]MongoDB筆記"
date:   2017-05-06 12:12:26 +0800
categories: Database
tags:
-  Database
-  NoSQL
---


## mLab
這次學習NoSQL的資料庫MongoDB，由於不想在電腦上浪費空間，便轉往尋找雲端方案[**mLab**](https://mlab.com/)  
mLab提供500MB免費的儲存空間，我想對於學習的使用者來說非常足夠。


### 如何連線
先於mLab網站註冊免費帳戶，並新增database使用者
採用第三方node.js模組 **moongoose** 來方便操作mongoDB  

``` javascript
mongoose.connect('mongodb://使用者名稱:使用者密碼@dsxxxxx.mlab.com:xxxxx/資料庫名稱');
var db = mongoose.connection;
//監聽連接錯誤
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function() {
  console.log('Connection on DB successful');
});
```
### 設計Schema
雖然說NoSQL資料庫是不需要預先設置好Schema，但有了Schema可以讓我們的資料更有系統性，
這邊用一個Place schema來做說明
```javascript
var PlaceSchema = mongoose.Schema({
  user:String,
  name:String,
  location:String,
  rating:Number,
  place_id:String,
  day:Number
});
var Place = mongoose.model('Place' , PlaceSchema);
```
PlaceSchma定義了地點的使用者、名稱、地址、評分等，之後我們就可以用這個規格來新增Place
### 新增
```javascript
Place.create(req.body, function (err, post) {
  if (err) return next(err);
  console.log("receive post!",req.body);
  res.json(req.body);
});
```
Moongoose的create方法讓我們使用Place這個model來新增一筆資料，第一個參數為post傳來的body
### 查詢
```javascript
//搜尋指定property的項目
Place.findOne({欲查找的項目屬性},function(err,place){
  if(err)return next(err);
  res.json(place);
});
//欲查找的項目屬性意思就像一個過濾器，篩選符合的資料，例如我要查找叫做火車站的地方，
//我可以填入{name:"火車站"}，這樣就會回傳符合火車站的所有Place資料

//搜尋此Schema全部資料
Place.find(function(err,places){
  if(err)return next(err);
  res.json(places);
});
```
### 刪除
```javascript
Place.findOneAndRemove({欲刪除的項目屬性},function(err){
  if(err)return next(err);
});
```
### 更新

```javascript
Place.findOneAndUpdate({欲更新的項目屬性},req.body,function(err,place){
  if(err)return next(err);
  res.json(place);
});
```

>注意，這邊的操作方法有簡化過程，正確使用方式還請依照官方
