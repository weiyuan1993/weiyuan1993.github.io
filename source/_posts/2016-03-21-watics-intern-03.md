---
layout: post
title:  "WATICS維德斯實習-第三周(3/14~3/18)"
date:   2016-03-20 23:18:26 +0800
categories: Intern
tags:
-  Intern
---
很快的，到維德斯實習已經滿三個禮拜，認識了公司裡的夥伴，
雖然我只是個小小實習生，但他們對待我也如一般同事一樣，  
公司內部我們都互稱英文名字，這對我也是挺新鮮的事，  
只好把好多年未用的英文名字拿出來用啦! 現在起，請叫我Vic。  
  
由於上一周我已經把聊天室的基本功能給寫好了，  
但是還有一個最重要的問題還沒解決，離線訊息。  
市面上的即時通訊軟體，都支援傳訊息給離線的人，  
這樣雙方都不用同時在線上也能傳訊，  
想當然爾，我也必須做出此項功能。  
  
ejabberd伺服器其實是默認支援離線訊息的，當朋友不再線上時傳訊息給他，能夠將離線訊息儲存在ejabberd server，一旦朋友上線，server再將離線訊息傳送給他。

但這有一點BUG，有時候離線時並不會馬上把離線狀態傳給server，server會判定你還在線上，所以就不會幫你儲存離線訊息。

後來想想這不是辦法，畢竟只有離線訊息也沒用，還有有**歷史訊息**啊! 只好回去尋找Strophe.js的plugin。

我使用的方法是[Strophe.js MAM plugin](https://github.com/metajack/strophejs-plugins/tree/master/mam)，這個plugin能夠讓我取出server儲存的訊息DATA，我再將這些DATA做處理，讓用戶點開朋友的對話視窗時，載入歷史訊息，有了這項功能，我當然也不須理會離線訊息啦，因為所有的訊息都被我輸出出去了。(但是速度有點慢，因此我先暫時限定最近20則訊息，往後再想想看辦法)

聊天視窗的畫面
![enter image description here](https://lh3.googleusercontent.com/-Chb4ocJ-TEk/Vu7J2x9BXqI/AAAAAAAATgs/_U_MUcqziysl6VnXxCXh8rrJetFa8TbgQ/s0/xmpp_client.PNG "xmpp_client.PNG")
  


----------


就這樣，我的第一份任務暫時告一段落了。

緊接著下一項被指派的工作是寫文章分享的網頁，

因為對於HTML LAYOUT還滿熟練的，很快我就把版型給建立起來，

閒暇的時候還摸了一點React.js，也用他建立了一份Layout，

版面搞定後，就跟Ken要API來串接，設定好header(中間折騰了好久，原來雙方的header必須完全一樣)，利用jQuery ajax，利馬把文章的DATA 接到我的網頁啦。目前ALAN給我的版型在電腦版上看有一點窄，再看之後怎麼調囉。

![enter image description here](https://lh3.googleusercontent.com/-ZttjQgqJfJg/Vu7JPCFCPKI/AAAAAAAATgU/jibqjkRrqBER3OQa2FnaOtQSa5GhRx58w/s0/fitobe_img.PNG "fitobe_img.PNG")

