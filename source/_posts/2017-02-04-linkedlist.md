---
layout: post
title:  "[JS]實作LinkedList鏈結串列"
date:   2017-02-04 00:28:26 +0800
categories: JavaScript
tags:
-  Data Structure
-  JavaScript
---

由於自身資料結構的基礎薄弱，買了一本*JavaScript資料結構與演算法實作*的書來看，重新把LinkedList鏈結串列學習了一遍，並用JS實作出來。

## LinkedList鏈結串列
要存放多個元素，最常用的資料結構可能是陣列，但是在大多數語言中，陣列的大小是**固定的**，要從陣列中插入項目或移除項目的成本很高，因為必須移動其他元素。
**LinkedList** 就解決了這個情況，它存放有順序的元素集合，但不同於陣列，鏈結串列的元素在記憶體中並不是連續放置，每個元素由一個存放元素本身的節點和一個指向下一個元素的指位器組成。

## 實作


``` javascript
function LinkedList(){
  //要加入串列的元素 Node輔助類別
  var Node = function(element){
     this.element = element;
     this.next = null;
  }
  var length = 0;
  var head = null;
  //尾部追加元素
  this.append = function(element){
    var node = new Node(element),
        current;
    if(head === null){
      head = node;
    }else{
      current = head;
      while(current.next){
        current = current.next;
      }
      current.next = node;
    }
    length++;
  }
  //移除特定位置元素
  this.removeAt = function(position){
    //檢查有無越界
    if(position > -1 && position < length){
      var current = head,
          previous,
          index = 0;
      //移除第一項
      if(position === 0){
        head = current.next;
      }else{
        while(index++ < position ){
          previous = current;
          current = current.next;
        }
      }
      length--;
      return current.element;
    }else{
      return null;
    }
  }
  //插入特定位置元素
  this.insert = function(position, element){
    if(position >=0 && position <= length){
      var node = new Node(element),
          current = head,
          previous,
          index = 0;
      //在第一個位置添加
      if(position === 0){
        node.next = current;
        head = node;
      }else{
        while(index++ < position){
          previous = current;
          current = current.next;
        }
        node.next = current;
        previous.next = node;
      }
      length++;
      return true;
    }else{
      return false;
    }
  }
  //將list輸出成String
  this.toString = function(){
    var current = head,
        string = '';
    while(current){
      string += current.element + (current.next ? ', ' : '');
      current = current.next;
    }
    return string;
  }
  //查詢元素在第幾項
  this.indexOf = function(element){
    var current = head,
        index = 0;
    while(current){
      if(element === current.element){
        return index;
      }
      index++;
      current = current.next;
    }
    return -1;
  }
  //移除指定元素
  this.remove = function(element){
      var index = this.indexOf(element);
      return this.removeAt(index);
  }
  this.isEmpty = function(){
    return length === 0;
  }
  this.size = function(){
    return length;
  }
  this.getHead = function(){
    return head;
  }
}


```
