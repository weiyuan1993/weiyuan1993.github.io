---
layout: post
title:  "[Git]為專案開Branch"
date:   2016-06-28 00:28:26 +0800
categories: Git
tags:
-  Git
-  Git Branch
---

最近在公司有比較多的時間自主學習，常常想在專案上開發不同版本或測試新功能，但是一直複製資料夾也不是個長遠的解決辦法，於是卯起來學習Git Branch。
話說回來，開Branch應該是git 版本控制最重要的功能了，我居然拖到現在才去學習...

## Git Branch
要為專案開發新功能、重構程式碼、或想測試某的東西，又不想動到原本的程式碼時，Git的Branch功能就派上用場了。而且新的Branch寫好以後，還可以合併回去原本的master branch，真的超方便，也不用為了命名一個新的資料夾而傷透腦筋。

## 如何使用


```
git branch //確認目前branch
git branch test-branch //新增一個branch
git checkout test-branch //切換到test-branch
git merge test-branch //與test-branch merge

```
這是最基本的是個方法，還有很多更深入的方法我還沒研究，但只要會了這個，其實對工作上幫助就很大啦~

`git checkout`過去新的branch會馬上把工作目錄所有檔案切換成那個branch最新commit的狀態，但是要記得原本master branch一定要先commit過才切換哦，不然之前寫的就前功盡棄了。 

另外，輸入`gitk --all` 可以開啟圖像化來檢視所有的commit內容，以及各branch目前所在狀態。