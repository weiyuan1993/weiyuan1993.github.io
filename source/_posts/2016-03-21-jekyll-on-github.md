---
layout: post
title:  "[jekyll]架設部落格及部屬到 Github"
date:   2016-03-21 18:53:26 +0800
categories: Blog 
tags:
- jekyll
- Blog
- GitHub
---

最近這陣子花了些時間研究Github page的用法，它可以讓我們部屬靜態網站，容量似乎有1GB，而且免費。

常用的靜態網站部屬工具有jekyll、HEXO、Octopress等，我採用jekyll因為他是Github推薦。

以下是安裝流程，系統為Windows 10  


### 下載[Ruby](https://www.ruby-lang.org/zh_tw/documentation/installation/) 
  
  
  
我使用微軟系統，所以直接下載Ruby安裝包，省時又省力

### 安裝[jekyll](https://jekyllrb.com/)

因為jekyll是由ruby語言編寫而成，所以必須要有Ruby環境，接著啟動終端機輸入:

    $ gem install jekyll

安裝好後在你想要建立專案的地方，輸入:

    $ jekyll new jekyll-blog
    $ cd jekyll-blog 
    /jekyll-blog $ jekyll serve
 
這樣就會在資料夾內安裝jekyll所有檔案，啟動server後，在127.0.0.1:4000的localhost即可觀看部落格。

基本上jekyll安裝就到這邊，非常簡單。

### 部屬至Github

使用jekyll部屬至Github非常簡單，只要將資料夾push到repo即可。
首先，先在github建立一個新repo，名稱為

    GithubID.github.io
    
記住GithubID必須為你的github帳號名稱，這樣github就會幫你建立一個可瀏覽的repo，網址就是GithubID.github.io

建立好後請在jekyll資料夾初始化git，並新增remote到github repo:

     $ git init
     $ git add .
     $ git commit -m 'init git'
     git remote add origin https://github.com/GithubID/GithubID.github.io.git
     git push -u origin master
如此一來，就可將jekyll資料夾push上github，以後有新增文章或是更改什麼設定，一樣push即可更新部落格。

### 寫一篇文章

新增文章也很簡單，在專案資料夾底下有個**_posts**資料夾，只要在裡面新增符合格式的Markdown檔案，啟動jekyll的同時就會自動幫你生成對應的html檔，非常容易。

檔案名稱格式:

    年-月-日-文章名稱(英文).md
    ex:2016-03-21-my-first-post.md

### 文章檔案開頭必須設定文章資訊

	---
	layout: post
	title:  "第一篇文章"
	date:   2016-03-21 20:50:26 +0800
	categories: jekyll test
	tags:
	-  jekyll
	- post_article
	
	---
	嗨!我是第一篇文章~
	...
	...
	...
	
categories會讓jekyll在產生的html檔自動分類資料夾，日後若有新增category功能，也會依據這個分類，tags也一樣。

存檔好之後，若你有啟動server他也會即時更新，到Localhost就能馬上看到結果囉。

最後要更新至Github，同樣的步驟在做一次就行了

	git add .
	git commit -m "new post"
	git push -u origin master

之後再來研究一下theme的設定，因為預設的主題太樸素了。








