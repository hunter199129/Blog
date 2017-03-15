---
title: 'Youtube api practice: nbahl'
date: 2016-11-29 14:38:51
tags: [Youtube, Google api, api, html, javascript]
---
## [Google API Console](https://console.developers.google.com)
要使用Google的API之前，都要先去[Google API Console](https://console.developers.google.com)做啟用的動作。啟用之後就是看每個api給的配額及需求去做調整，基本上Google都有一個免費使用的額度，而call的次數不多基本上都是不用錢的，也算是方便開發者做測試。

進去啟用之後會有一個憑證，憑證方式主要是分成oath跟api-key兩種，不用做身份認證的api就用api-key的方式就好了，就直接建立新的api-key憑證。

建好之後就可以開始寫code，或是去找sample code看怎樣用這個key去操作。

## [Youtube Data API v3](https://developers.google.com/youtube/v3/)
我要做的事很簡單，就是做Youtube影片搜尋，只是是用按鈕的方式去做固定的搜尋，因為NBA有30隊，每次要找精華的時候都要重下關鍵字，我只是想省去我連結到Youtube每次一直做重新下關鍵字的動作。

而我在[codecademy](https://www.codecademy.com/)上有找到類似的練習跟sample，所以我就把上面的大部分程式碼抓來用了，哈。

而Google的Document上也有[Sample Code](https://developers.google.com/youtube/v3/code_samples/)可以參考，但寫的比較複雜一點，但是各個支援的語言都有，所以也比較general。

因為想要直接可以掛在[github](https://www.github.com)上，所以選擇用javascript去做，讓他光在前端就可以做搜尋，因為不是很複雜，也懶的找其他地方掛後端的code。

基本上做的事情非常簡單，就是用特定關鍵字做按鈕找出影片，然後抓下來的影片資訊是JSON，解析之後再用Youtube iframe的api做播放。Sample code上就已經做好搜尋的部份，接下來就是解析出我要的東西，跟掛到Youtube播放的api。

## [Youtube Iframe API](https://developers.google.com/youtube/iframe_api_reference)
比起Data API，這東西簡單多了。因為就只是給他一個iframe創造Youtube播放器，然後塞video id給它就好了，而且這個也不需要用到api-key，全部由Google提供，因為沒有利益上的用途。

當然也可以對影片做很多不同的動作，就像Google Document裡面的這個[範例](https://developers.google.com/youtube/iframe_api_reference#Examples)，影片播放時外框是綠色，暫停是紅色，結束是黃色等等。

所以如果要做一個連續播放Youtube playlist的播放器，只要加上oath，照理來講也不是件難事。

## [Code](https://github.com/hunter199129/nbahl)
HTML的code就是把iframe塞進去，然後剛開始隱藏，用placehold的圖片先留住頁面，順便連結到espn的賽程表(本來是想直接撈賽程，但是還沒有想法要怎麼撈)，只要點了任何一個連結，再把它顯示，把placehold的圖隱藏。

再來js。我做的就是把原本找到亂糟糟的資訊parse出來，截出我要的title, vid, channel名稱, 發佈時間，用innerHTML印出來。不過目前不知道為什麼只能抓到五筆，我要再去找找原因，但因為他是照上傳時間排序，所以目前也沒有造成很大的困擾。

` parent.player.loadVideoById(vId, 0, "hd720");`

這是Youtube iframe的api，可以即時更換影片，很好用！因為iframe是下一層，所以要用parent。

`part: 'snippet'`

這邊可以抓出你要的part，詳細的內容要參照[api document](https://developers.google.com/youtube/v3/getting-started#part)。原本還想再抓片長的，但因為片長需要先把影片找出來之後再call一次api service，想想也不是很必要，就先放著，先做出主要功能重要！

## api-key configuration
因為我的api-key是公開的，所以要做第access的調整，不然被別人拿去玩到沒credit是很麻煩的。Google也有說要好好保護自己的api-key([參照](https://support.google.com/googleapi/answer/6310037?hl=zh-Hant))，因為很多都是商業用途，不小心被拿去亂用損失會很大！

其實這樣公開是很不好，因為裡面也有說最好放到另外的資料夾，因為主要都是存在後端，所以隱藏的方法也比較簡單。前端當然也有方法，就是放到個人的雲端空間中，像是google drive然後用oath去開，但就是繞了一圈。

會公開也是覺得因為我用超過就超過了，目前也沒什麼利益的考量，反正也不是一定要用，加上我覺得Google的機制不會這麼容易被破，也不會有人這麼無聊來駭我一個小小的api-key。

講這麼多廢話還沒講到重點，在[console](https://console.developers.google.com)裡面有一個只能透過http存取。
![Api-Key setting](https://i.imgur.com/iCTF6qX.png)
剛好很符合我的需求，加上可以配合網址，這也是為什麼我說我相信Google的機制不會這麼容易被破解的原因，哈哈。打上網址，就只有透過那個網址的api call會通過，其他都會被隔絕，所以我才會直接公開api-key，不過也是因為懶的繞啦。

## 後續
之後想到什麼會盡量加，因為現在就是功能取向，還很陽春，可能也會試著把頁面弄的fancy一點。目前主要會弄前面講的
+ 搜尋功能：把只能搜尋五筆弄多一點，然後加個可以打字搜尋的。
+ 撈賽程：不確定到底能不能做到，可以去找找相關的資料。

畢竟前端能做的事還是有限，所以盡量看看可以塞點什麼，哈。
