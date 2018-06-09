---
layout: post
title:  "WATICS維德斯實習-第四周(3/21~3/25)"
date:   2016-03-25 23:48:26 +0800
categories: Intern
tags:
-  Intern
---

不知不覺實習快滿一個月了，越來越熟悉這裡的步調，也喜歡上了工作的環境，不大的空間，卻有溫暖的裝潢與簡約的布置。

在維德斯裡，我認識了負責後端的Ken，負責UI設計的Alan，很喜歡蘋果的Reed，同時也負責iOS開發，還有開發的領導也是面試我的甫華，負責整個產品的領導，以及公司唯二的女生Viola和Vicky，厚厚還有最近新來的帥哥運動編輯Allen。  
  
---
這個禮拜繼續開發公司的文章網站，上禮拜是用最基本方法，先刻HTML的Layout，再用jQuery的ajax來接後端API的文章資料。這是最傳統最直接的寫法，無須用到任何framework，但是缺點是後期維護、修改程式碼會越來越困難，雖然我只是寫個小專案，但希望自己能學習一些不一樣的工具，所以就邊碰之前就很想學的react邊寫啦!

一開始使用react的starter kit編寫react，但後來發現有**Webpack**套件可以讓我們打包各類js檔案，並且可以安裝轉譯器，react安裝最麻煩的就是他必須轉譯JSX語法給瀏覽器，否則瀏覽器無法讀取，而Weboack要裝這些東西都非常容易，幾個指令就能完成，我也寫了一篇文章來記錄安裝過程。

React最大的特色就是元件化，任何一個有功能的東西都可以把它做成元件，像是我的文章網站劃分為好幾個區域，有導航列、文章圖片和標題、文章內容、作者、熱門文章列、臉書粉絲專業區塊等，那我就可以透過`React.createClass`來創造這些元件區塊，各元件只負責自己的功能，這樣在管理上非常方便且一目了然。

由於一開始React還不太熟練，所以先搭配jQuery來寫(熟練的話，基本上不需要jQuery)，主要是要使用$.ajax來連接後端API的文章資料。每篇文章都有一串id，附加在網址後面，所以必須用javascript取得當前URL，並用javascript的split功能拆分字串，只取得需要的id。接下來透過ajax獲取遠端json檔案，裏頭包括圖片、標題、作者、內容等。

jQuery可以很直覺方便的操作DOM(違反react精神哈哈)，我可以將獲取的資料利用`append()`或是`text()`來輸出到我的介面上，速度也很快。

但是後來覺得自己這樣不行，該來用react的精神下去了，因此我花了些時間研究react元件的互動，將ajax部分移到主要文章顯示內，並在那裏再區分文章圖片和標題、作者、內容等小元件。

	-FullView(整體架構)
		-Nav(導航列)
			-Dropdown(手機版的更多按鈕)
		-Article_view(文章顯示架構)
			-Article-Img(文章圖片)
			-Author(作者)
				-Heart(未來的喜歡功能)
			-Article_Content(文章內文)
		-Article_List(熱門文章列表)
		-Fbfans(FB粉絲專業)

四大區塊，可以一目了然整個網頁的架構，要新增功能或修改都非常方便。React有個最重要的精神就是state，透過監測當前狀態是否有改變來更新DOM，使用`this.setState`就可以改變狀態，而子元件可以透過`this.props`來取得父元件元素。

例如我在Article_view先使用`getInitialState`初始化狀態，return data為空陣列，接者寫`loadArticleData()`來使用ajax連接API，並放入`componentDidMount`裡(表示DOM載入完畢後會執行這項操作)，並在獲得遠端data時，使用`this.setState({data:data.article})`(json的文章資料在article底下)，以此更新狀態，表示目前data有東西進來了，改變了初始狀態，這時，react就會自動重新調用`render`，重新渲染一次有改變狀態的DOM，那麼我就將要顯示的區塊，使用this.state綁定資料，這樣ajax擷取資料一完成，就會將資料顯示上去了。