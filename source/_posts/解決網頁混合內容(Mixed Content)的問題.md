---
title: '解決網頁混合內容(Mixed Content)的問題'
date: 2017-11-14 09:06:00
tags: [http, web, CSP]
---

寫過前端的都知道，http跟https在同一個頁面上不能混用

否則就會造成[混合內容(Mixed Content)](https://developer.mozilla.org/zh-TW/docs/Security/MixedContent)的問題

這個問題說好解很好解，因為只要把網頁全部Request改成http或https其中一個就好了

但是說不好解也很難解

像我遇到的問題就是，我的api endpoint是別人丟在npm上的

他是用http存取[NBA](http://stats.nba.com)的api

我雖然可以改code再重build(該套件基於[MIT License](https://opensource.org/licenses/MIT))，但一來要花時間了解，二來如果npm上套件更新我又要手動再弄一次

那把deploy那端改成http呢？

也沒辦法，因為想要發在[Github page](https://pages.github.com/)上

所以需要其他的解法

找了很久終於找到正確的解法了...

只能說是基礎功不夠，因為是個很基本的東西: [Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)

CSP簡介： CSP是額外的安全層，用以防護特定攻擊，像是跨頁腳本[(XSS)](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting)和內容注入攻擊(data injection attacks)，從資料盜取、網頁內容汙染到散布惡意軟體，這都是主要的攻擊手段

在CSP裡面，有一個[upgrade insecure requests](https://developers.google.com/web/fundamentals/security/prevent-mixed-content/fixing-mixed-content#upgrading_insecure_requests)

就是把你全部網頁中的Request全部升級成https

只要在`<head>`裡面加一個`<meta>` tag

    <meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">


就能立馬解掉混合內容的問題

CSP裡面還有其他的東西可以用，可以參照:

+ [Preventing Mixed Content | Web Fundamentals | Google Developers](https://developers.google.com/web/fundamentals/security/prevent-mixed-content/fixing-mixed-content#upgrading_insecure_requests)
+ [Content Security Policy (CSP) - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
