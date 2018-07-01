---
title: JavaScript30挑戰-Day7
date: 2018-07-01 18:24:57
categories: JavaScript30
tags:
-  JavaScript30
-  JavaScript
---

# 07 - Array Cardio Day 2

[DEMO](https://weiyuan1993.github.io/JavaScript30/07-Array-Cardio-Day2/)
[GitHub](https://github.com/weiyuan1993/JavaScript30/tree/master/07-Array-Cardio-Day2)

## 筆記

陣列操作練習 2，`some()`,`every()`,`find()`,`findIndex`

<!--More-->

### some()

只要有一個元素通過驗證函式，則回傳 true。

```javascript
const isSomePersonOver19 = people.some(
  person => new Date().getFullYear() - person.year >= 19
);
```

### every()

所有元素都要滿足驗證函式才回傳 true。

```javascript
const isEveryPersonOver19 = people.every(
  person => new Date().getFullYear() - person.year >= 19
);
```

### find()

找到符合的第一個元素即停止並回傳。

```javascript
const commentWith823423 = comments.find(comment => comment.id === 823423);
```

### findIndex()

找到符合的第一個元素的索引值

```javascript
const indexOfThisId = comments.findIndex(comment => comment.id === 823423);
```

### 運用展開語法

更動陣列內容時，如果不想破壞原始陣列，可以使用展開語法重組陣列。

```javascript
//刪除元素的index:indexOfThisId
const afterDelete = [
  ...comments.slice(0, indexOfThisId),
  ...comments.slice(indexOfThisId + 1)
];
```
