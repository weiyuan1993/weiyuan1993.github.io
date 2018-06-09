---
layout: post
title:  "WATICS維德斯實習-第七周(4/11~4/15)"
date:   2016-04-18 21:54:26 +0800
categories: Intern
tags:
-  Intern
---

上一周，因為碰到棘手的網站meta tag問題，於是Fuhua希望我能利用假日在家裡加班盡快把這問題解決，看我要用Node.js、PHP，
如果不行就下周讓Ken教我用JSP來寫，我自己是感到壓力滿大的，因為都是不熟悉的工具，要重新建構這個文章網站應該不是輕鬆事情。

所以禮拜六就起了個大早開始工作，我決定採用Node.js，因為之前寫聊天室時稍微有碰過，且同樣是javascript語言，要上手應該能比較快。PHP自己是不太想學，JSP好像是JAVA那類的後端語言，還是javascript對我最親切。

### Node.js

當然，一開始果然碰到了一些問題，由於對Node.js的不熟悉，我連AJAX都不太會使用，一開始只知道裝了一些感覺會用到的套件，但真正要寫實卻不知如何下手，上禮拜五Fuhua有跟我講過，先不要裝套件，用最原始的方法就好了。於是我對著空白的資料夾新增了一個app.js檔來寫寫看，稍微摸了一陣子終於搞懂Node.js的原理，Node.js算是一個javascript的平台，你可以用它來執行JS檔，不必透過瀏覽器，也就是說你可以在這平台上面執行任何你在瀏覽器跑的javascript。變數運算、迴圈等都可以，算是以javascript單一語言統一了前後端，讓只熟悉網頁設計的人可以也快速上手。

### Request

我要改到Node.js建構網站的唯一目的就是我需要在後端server就先處理ajax，因此必須使用到Node.js的一個套件*Request*，它讓我可以使用如同jQuery的ajax功能，連接遠端API，取得資料回傳。而Request比較不同的地方在於，他是取得整份網頁的HTML碼，用來寫爬蟲程式非常好用，比如你可以爬一些天氣網站的頁面資料回來。

然而我不需要整份HTML檔，那些多餘的HTML TAG不是我主要的目的，我只需要標題、圖片、內容即可，所以我使用Javascript的`JSON.parse`來剖析HTML碼，轉換成可用的JSON物件格式。

### Express.js

搞定Request之後，終於可以重新開始實作這個網站，我依舊使用了Node.js最熱門的Express.js網頁框架，用來快速構建網站非常方便，之後我應該也會寫一些Express的使用心得以及基本方法，

#### Jade模板
Express初始提供往非常良好的環境，他幫你建構了後端伺服器、路由等基本功能，而我會用到的主要是路由器的功能，Router可以幫我處理不同的文章query id，
輸出不一樣的頁面給使用者，而跟Express搭配的頁面模版是Jade，一個超級簡潔的縮排式HTML template，寫法跟原生生HTML差不多，但是簡潔許多，不需要結尾TAG包覆以節省略括弧，並可以鑲嵌後端傳來的變數於內。 而他最大特點是可以重複利用，
我可以利用他先寫好一個共用的Layout，若有共同會使用到的界面都寫到這裡，之後可以利用`extends`來繼承他的特性。 

由於之前已經有利用React寫過一次頁面架構了，所以這次寫起來非常迅速，Bootstrap套一套，之前的CSS也繼續使用，很快就把整個網站頁面給建構起來了。

再Router部分，我必須處理query id的問題，我分為`/zh/article`以及`/en/article`，如此就可區分中英文不同的路徑，並且在這裡處理ajax取得JSON資料，再respond回去，也就是說使用者訪問這個路徑時，express會先執行我寫在裡面的ajax，取得資料後把JSON資料鑲嵌在Jade模板上，再回應此HTML檔給使用者，因此每一篇文章都有自己專屬的meta tag。 

使用Router在建構中英文版時也有很方便的地方，可以讓我把中英文的jade檔區分出來，在開發維護上也方便許多。

#### Query id
忘記講的很重要的一點是，express router可以處理URL的query id，因為公司的文章資料都在後端API裡，它使用`?id=XXXXXX`來區分不同的文章，每一篇文章都有專屬的id，而這串query id會接到網址的後面，我們可以使用`req.query.id`來取得這串數字，這樣就可以針對不同文章的id來做不同URL的ajax取得對應的文章資料。

大致上這個網站就差不多完成，主管也答應給我加班費，算是對我的一點回報吧(畢竟公司目前沒人寫這個東西)。

目前網站的樣子:

![Fitobe 文章網站](https://lh3.googleusercontent.com/-mAIK9km98gI/VxTyKh9cnnI/AAAAAAAAT30/TimefTQfCsYJIxYxajPTJfpLJmiYFtpygCLcB/s0/%25E6%2593%25B7%25E5%258F%2596.PNG "文章擷取")

後來又在主頁做了一個搜尋頁面(圖書館)，可以關鍵字搜尋所有文章。

![Fitobe 圖書館](https://lh3.googleusercontent.com/-xpc7T_KiEZ4/VxTyoRbxhZI/AAAAAAAAT38/DcqRACRNM0obuocrWTYF7PHNoOdaR1r3gCLcB/s0/%25E6%2593%25B7%25E5%258F%2596.PNG "圖書館")

這周差不多就到一個段落囉~

