---
title: '用Travis CI自動發佈Angular app到gh-pages'
date: 2017-11-06 09:11:00
tags: [Travis CI, Github, Github pages, Angular]
categories: DevOps
---

持續整合(continuous integrations)在現代軟體開發中是不可或缺的技術，而也有很多公司提供這方面的服務，其中跟Github最緊密的就是[Travis CI](travis-ci.org)了。

<!--More-->

Travis CI提供了在Github上只要是open-source的專案就可以免費使用的方案，所以也很適合拿來做CI的config練習。CI在這些平台的支援之下，也只是做configuration的調整而已。

## Preconditions

+ Angular專案
+ Github repo

## Github Settings

這邊要先設定Personal access token讓Travis CI可以發佈新的branch

`Account > Settings > Personal access tokens`

新增一個token，權限控制選public_repo就好，建好之後把token先存下來，之後要丟給Travis CI

## Travis CI Settings

這邊的話要先連到自己的Github account，找到要整合的repo，勾選之後進到Settings裡面

把`General > Build only if .travis.yml is present`選到*on*

`Environment Variables`這邊新增一個變數叫*GITHUB_TOKEN*，value就是剛剛從github取到的token

這樣這邊就算設定好了

## Project Settings

接下來就是要設置專案的CI設定

### package.json

找到專案中的`package.json`，新增`build.prod`指令如下

    {
        ...
        "scripts": {
            "ng": "ng",
            "start": "ng serve",
            "build": "ng build",
            "test": "ng test",
            "lint": "ng lint",
            "e2e": "ng e2e",
            "build.prod": "ng build --aot --prod --progress false --base-href \"https://<user-name>.github.io/<repository-name>/\""
        },
        ...
    }

這邊要一定要設定`base-href`的參數，因為Angular app的原始設定會預設編成`https://<user-name>.github.io/`而不是`https://<user-name>.github.io/<repository-name>/`

### .travis.yml

在專案的根目錄要新增一個`.travis.yml`的檔案，是給Travis CI的設定

這邊參照[Travis docs](https://docs.travis-ci.com/user/deployment/pages/#Setting-the-GitHub-token)的設定，改成如下

    language: node_js
    node_js:
    - "6"
    
    branches:
    only:
        - master
    
    cache:
    directories:
        - node_modules
    
    script:
    - npm run build.prod
    
    deploy:
    provider: pages
    skip_cleanup: true
    github_token: $GITHUB_TOKEN
    local_dir: dist
    on:
        branch: master

## Push to Github

全部設定完就可以先推到Github上，接下來Travis CI就會自動編譯Angular app到gh-pages的branch上。

最後只要再到repo settings開啟Github pages，設定到gh-pages的branch，這樣就全部都設定完成了。之後只要code有變動push到Github上，Github pages上的內容也會跟著改動，這樣就不用每次編譯都要重推兩份code了。
