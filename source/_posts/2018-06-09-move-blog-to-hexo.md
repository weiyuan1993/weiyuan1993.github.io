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
我使用簡約的 NexT 主題，直接透過 git clone 方式安裝到站點
``` 
$ git clone https://github.com/theme-next/hexo-theme-next.git themes/next
```
在 `_config.yml` 設定 :
```
theme: next
```

為了之後可以隨時使用 git pull 更新主題，我採用 HEXO 的 `Data files` feature：

從 `主題的 _config.yml` 文件中複製所有設定到  `HEXO 的 _config.yml` 中，然後在這些參數最上方添加一行 `theme_config :`，並將所有參數縮排兩空格以符合 yml 格式

使用此一方式可以很輕鬆地只要管理一份 HEXO 的`_config.yml` 即可，也可以推送到 GitHub 保存，這樣在 pull 第三方的 NexT repo 才不容易出現問題(如果有更動檔案的話)。

之後換電腦或是在別的電腦想寫文章，直接 clone source 分支，這樣就包含了設定好的 `_config.yml` 和乾淨的主題配置。

## 換電腦寫文章初始設定
經實測發現 clone 下來的 主題會是空的，所以須補上 clone 主題

```
$ git clone -b source https://github.com/weiyuan1993/weiyuan1993.github.io.git
$ npm run init
```

[參考資料](https://github.com/theme-next/hexo-theme-next/blob/master/docs/zh-CN/DATA-FILES.md)

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

## 設定 favicons
新增 `source/favicons` 資料夾，將所需圖檔放入，並在 `_config.yml` 主題設定區設定：
```
  avatar:
    url: /favicons/favicon.png
  favicon:
    small: /favicons/favicon-16x16.png
    medium: /favicons/favicon-32x32.png
    apple_touch_icon: /favicons/apple-icon-180x180.png
    safari_pinned_tab: /favicons/apple-icon-180x180.png
```
## 新增 DISQUS 留言板功能
先去 [DISQUS](https://disqus.com/) 官網辦帳號，設定 shortname，並在 `_config.yml` 主題設定區設定：
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

### 懶人指令
直接用 `package.json` 寫了快速指令
```
  "scripts": {
    "init": "git clone https://github.com/theme-next/hexo-theme-next.git themes/next && npm install",
    "update": "git pull origin source",
    "start": "hexo s",
    "deploy": "hexo generate -d",
    "push_source": "git add . && git commit -m 'update' && git push origin source",
    "deploy_push_source": "git add . && git commit -m 'update' && git push origin source && hexo generate -d"
  }
```
1. `npm run init`: git clone 下來初始化設定
2. `npm run update`: pull 最新的 source 分支
3. `npm run start`: 啟動 server
4. `npm run deploy`: 生成後發佈
5. `npm run push_source`: push 更動後的 source 檔
6. `npm deploy_push_source`: push 更動後的 source 檔，並且發佈到網站



折騰了一天，終於大功告成，中間還裝了兩次ＱＱ 


## SEO優化

### 設定 Google 分析與 search console
為了解決無法搜尋到部落格的問題，先去[Google Webmaster](https://www.google.com/webmasters/)新增網址，選擇其他方式裡使用 html 標記的方式，複製`<meta>`標記的`content`字串，接著在`_config.yml`裡貼上：
```
  google_site_verification: xxxxxxxxxxxxxxxxxxxxxx

  GA則是貼上：
  google_analytics: UA-XXXXXXXX
```

點選驗證網站後，即大功告成


### 新增 sitemap.xml

```bash
$ npm install hexo-generator-sitemap --save
```

在站點的 `_config.yml` 設定：
```
sitemap: 
  path: sitemap.xml 

```
之後重新產生網站時就會多一個 sitemap.xml 檔案，再來到 search console 提交 sitemap.xml 及完成。



