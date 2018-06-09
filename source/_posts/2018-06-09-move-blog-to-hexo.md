---
title: 將部落格搬移到HEXO
date: 2018-06-09 22:11:11
categories: Blog
tags: HEXO
---

開始工作以後，常常覺得自己越來越局限於工作上的學習，下班後也沒動力進修，最近決定好好改變這個情況，首先就從翻新部落格來開始，有漂亮好寫的部落格應該會更有動力學習紀錄吧！

這篇將完整記錄我從 jekyll 搬移至 HEXO 的過程與遇到的坑。

<!-- more -->
## HEXO 安裝與啟用

```bash
$ npm install hexo-cli -g
$ hexo init weiyuan1993.github.io
$ cd weiyuan1993.github.io
$ npm install
$ hexo server
```

## 部署到 GitHub 遇到的問題

HEXO 的部署跟 jekyll 比較不一樣是透過 HEXO 自己的部署工具
```bash
$ npm install hexo-deployer-git --save
$ hexo deploy
```
而有個很大不同是，透過此法部署僅會將 HEXO 生成好的靜態檔案推送到 GitHub master，其餘檔案包括設定檔`_config.yml`，文章 markdown 檔、主題設定等全部都不會推送，對於日後如果更換電腦或是想在其他地方寫作會造成很大麻煩。

於是在網路上找到了還不錯的解法:
https://devtian.me/2015/03/17/blog-sync-solution/

簡單來說就是將想要保存的 source (HEXO 站點資料)，開一個分支
```bash
$ git init
$ git checkout -b source
$ git add .
$ git commit -m "first commit" 
 //這邊我沒用 https 的形式會報錯說 permission denied
$ git remote add origin https://github.com/weiyuan1993/weiyuan1993.github.io.git
$ git push origin source
```
之後如果有寫新文章，更動設定檔就推倒 source 分支即可。
在`_config.yml`需設定：
```
deploy:
  type: git
  repo: https://github.com/weiyuan1993/weiyuan1993.github.io.git
  branch: master
  # 下面這行一定要加不然無法 generate
  message: "{{ now('YYYY-MM-DD HH:mm:ss') }} "
```
這樣執行`hexo deploy`就能將靜態檔案推送到`master`了，因為 GitHub 默認使用 master 當作 GitHub Page

## 主題管理
原本我是直接 clone Next 主題到 themes/next ，好處是隨時可以 pull 更新，但此法會生成一個 git repo ，這樣我對於主題設定檔的變更及客製化無法推送 GitHub，查了原因是說，查了原因是說用了第三方的 repo ，是無法直接 push 的。
解法是用`fork`的方式到站點資料夾

```bash
$ git submodule add https://github.com/weiyuan1993/hexo-theme-next.git themes/ next
```

此法會建立 submodule ，之後有修改主題資料夾內的東西，就可以推送到 source 分支

```bash
$ git add .
$ git commit -m "modify theme"
$ git push origin source
```
[參考資料](https://git-scm.com/book/zh-tw/v1/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E7%B5%84-Submodules
)


## 新增 categories、about、tags 等頁面

```bash
$ hexo new page categories
$ hexo new page about
$ hexo new page tags
```
執行完會在站點 source 新增三個 .md，修改如下
```
---
title: categories
date: 2018-06-09 21:56:22
type: "categories"
comments: false
---
```
以此類推

## 新增 DISQUS 留言板功能
先去 [DISQUS](https://disqus.com/) 官網辦帳號，設定 shortname，並在 Next 主題的 `_config.yml`設定：
```
# Disqus
disqus:
  enable: true
  shortname: weiyuan1993
  count: true
  lazyload: false
```


## Read more 功能
在 Next 主題的 `_config.yml`設定：
```
auto_excerpt:
  enable: true
  length: 150
```
此法如果遇到 code highlight 會造成首頁不正常顯示，解決方法如下：

在要隱藏的文章內容前加上
`<!-- more -->`

## 常用指令與寫文章流程

### 一般寫文章
```
$ hexo new post 標題
$ hexo server // or hexo s

```

### 推送
```
$ hexo generate //or hexo g
$ hexo deploy //or hexo d

//生成完推送
$ hexo generate -d
```

### 清快取

```
$ hexo clean
```
###  推送文章原始檔及網站
```
$ hexo new post 標題
$ git add .
$ git commit -m "new post"
$ git push origin source
$ hexo generate -d
```


## 待處理的事項
目前 google search 搜尋不到本站，需解決 SEO 問題。

折騰了一天，終於大功告成，中間還裝了兩次ＱＱ
