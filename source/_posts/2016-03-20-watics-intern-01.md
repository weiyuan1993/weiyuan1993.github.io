---
layout: post
title:  "WATICS維德斯實習-第一周(3/1~3/4)"
date:   2016-03-20 21:48:26 +0800
categories: Intern
tags:
-  Intern
---
在大四上學期的時候，有感於自身能力的不足以及未清楚了解自我的興趣領域，決定利用下學期去實際的公司實習，一方面提升自己的專業能力，另一方面也提早了解業界生態。

寒假的時候面試了幾家公司，最後上了這家WATICS Global Corporation 維德斯全球股份有限公司，工作內容是負責網頁端的軟體開發(Web& Server)，主管說前後端都會碰到，讓我有很大的興趣。

公司雖然規模小，員工不超過10人，但公司內部環境還不錯，且位於交通方便的科技大樓站。而我是裡面目前唯一的實習生，職位是Software Developer Intern，一週三天，工作時間早上九點至晚上六點。
第一周主管要我研究XMPP Server，並且把它架在Debian系統上。*註:XMPP（Extensible Messaging and Presence Protocol，前稱Jabber）是一種以XML為基礎的開放式即時通訊協定，是經由網際網路工程工作小組（IETF）通過的網際網路標準。XMPP因為被Google Talk應用而被廣大網友所接觸。*

目前市面上有很多符合XMPP標準的Server可以使用，ejabberd、openfire、prosody等，說實在一開始學習這完全沒接觸過的東西，實在讓我很頭大，好在拜Google大神所賜，大部分的問題都能找到解答。
一開始先嘗試了Openfire架設，因為他有Web的 設定介面，比較容易使用，但Linux Debian系統是沒有GUI介面，無法使用瀏覽器，所以帶我的工程師跟我說用ejabberd架設，ejabberd的架設就有點難度了，有分安裝包安裝和Source code安裝，Linux指令還不太熟悉的我只能一邊照著網路上的指令操作一邊研究。

ejabberd的設定方式其實也滿簡單的，主要設定只有在ejabberd.yml這個檔案，我必須要設定好domain，admin user，http連線等設定。約莫在第二天的時後終於把Server端弄好，基本操作也都摸得滿熟練了。接下來主管要我寫一個Web的Client端，實作一個聊天室，這又是另一項挑戰了。

第二周待續...