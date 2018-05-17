---
title: 使用Travis CI隨時隨地發Blog文章(Hexo to Github)實作篇
date: 2017-03-13 13:39:00
tags: [hexo, Travis CI, Github]
categories: Blog
---

先上參考網址: <https://levirve.github.io/2016/hexo-deploy-through-travisci/>

幾乎全部都是照這篇文章做的。

<!--More-->

## New Branch

一開始要先到source folder建一個branch(這邊branch的名稱用raw)

    $ git init
    $ git checkout -b raw #切換branch
    $ git add .
    $ git commit -m "Push blog source code to github"
    $ git push origin raw


## Generator Github's Public Access Tokens

進到[Github](github.com) > Settings > Personal Access Token

**scope**只要選**public_repo**就好

![Public Access Tokens setting](http://imgur.com/I8fsFrV.png)

- 這個token是用來給第三方使用者的權限，不要任意公布


## Travis CI Web Setting

這邊就是利要用剛才在Github拿到的Public Access Token去給Travis CI一個可以**access public repo**的權限

進到[Travis CI](https://travis-ci.org/)，到對應的repo > Settings

設定**Services**和**Environment Variables**

這邊設定的Variable Name等等會用到，要記下來

![Travis CI console setting](http://imgur.com/3N1p0KA.png)

## package.json Setting

在**package.json**中加上

	"scripts": {
		"build": "hexo generate",
		"deploy": "hexo deploy"
	}

這個node，等等要讓Travis CI的script去做build的動作。


## Travis CI Script File (.travis.yml) Configuration

這邊我是用上面文章的設定

> 整個建構流程分三步：取得歷史（fetch old commits history），產生檔案（hexo generate static files），發布結果（git push changes of static files to master）

要寫文章時仔細看才發現，這邊是用`hexo g`建立好之後自己push到master branch，而不是用`hexo d`去做發佈。我是覺得讓script去自己做`hexo d`應該也可以，不過沒有測試過。

- `before_script`: 先把master branch的東西clone到`./public`資料夾底下

- `script`: 跑`hexo g`

- `after_success`: 成功build之後，把產生出來的靜態網頁push回master branch

```
.travis.yml

language: node_js
  node_js:
    - "6"
cache:
  directories:
    - node_modules
before_script:
  - git clone --branch master https://github.com/hunter199129/blog.git public
script:
  - npm run build
after_success:
  - cd public
  - git config user.name "hunter199129"
  - git config user.email "hunter199129@gmail.com"
  - git add --all .
  - git commit -m "Travis CI Auto Builder"
  - git push https://$DEPLOY_TOKEN@github.com/hunter199129/blog.git master
Variable
  branches:
    only:
      - raw
```

這樣設定完就完成了，可以push一次後到[Travis CI](https://travis-ci.org/)上看看log，看有沒有成功！

![build success](http://imgur.com/Edyb5lZ.png)

## 安全性相關

安全性的東西在上面的Ref中有相關的概念，我是覺得目前我的Blog沒什麼重要的東西要注意的啦

不過其中有一點是Personal Access Token是提供給**全repo**的權限，這是最需要注意的。

## 後記

有了這個方式之後，只要能Access到[Github](https://github.com/)網頁就可以發文啦！用手機平板都能，所以之後可以多寫很多廢文了(誤。

另外就是因為在網頁發文不能new出一個template，所以可以放個template的file在source底下，這樣發文就更方便了。

而且也概念上理解了一些[Travis CI](https://travis-ci.org/)的用法和操作，也算是個不錯的練習。
