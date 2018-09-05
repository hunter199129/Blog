---
title: 'Integrate Kcfinder with Ckeditor on Bitnami Testlink stack'
date: 2018-09-05 13:41:00
tags: [testlink, testcase management, bitnami, open source]
categories: DevOps
---

[Testlink](http://testlink.org/) 的內建文字編輯器是 [Ckeditor 4](https://ckeditor.com/) 是 Open source 的，但它沒有上傳圖片的功能

所以要搭配他們公司的一個外掛 [Ckfinder](https://ckeditor.com/ckeditor-4/ckfinder/) ，但這是要錢的，只能試用一段時間

<!--More-->

因此就用了網路上有另一套叫做 [Kcfinder](https://kcfinder.sunhater.com/) ，基本上就是 Open source 的 [Ckfinder](https://ckeditor.com/ckeditor-4/ckfinder/) ...

## Download and Unzip Kcfinder

`<testlink-dir> = Bitnami\testlink-1.9.17-0\apps\testlink\htdocs\`

下載後解壓縮到 `<testlink-dir>\third_party` 的資料夾下 

## Configure ckeditor

### - `<testlink-dir>\cfg\tl_ckeditor_config.js`

在最底加上設定

    config.filebrowserBrowseUrl = '/testlink/third_party/kcfinder/browse.php?type=files';
    config.filebrowserImageBrowseUrl = '/testlink/third_party/kcfinder/browse.php?type=images';
    config.filebrowserFlashBrowseUrl = '/testlink/third_party/kcfinder/browse.php?type=flash';
    config.filebrowserUploadUrl = '/testlink/third_party/kcfinder/upload.php?type=files';
    config.filebrowserImageUploadUrl = '/testlink/third_party/kcfinder/upload.php?type=images';
    config.filebrowserFlashUploadUrl = '/testlink/third_party/kcfinder/upload.php?type=flash';

這邊要注意的是前面要加上 `/testlink` 讓 ckeditor 可以抓到 testlink 的位置

而這部分的設定是已經寫好在 `Bitnami\testlink-1.9.17-0\apps\testlink\conf\httpd-prefix.conf` 裡面的

有一段

    Alias /testlink/ "D:\Bitnami\testlink-1.9.17-0/apps/testlink/htdocs/"
    Alias /testlink "D:\Bitnami\testlink-1.9.17-0/apps/testlink/htdocs"

這個在之後設定 kcfinder 的時候也會用到(不用改，只是需要知道是為什麼要用 `/testlink`)

## Configure kcfinder

### Enable kcfinder

這邊有兩種方法可以做，第一種比較安全

#### 1. `<testlink-dir>/login.php`

更改

    // Login successful, redirect to destination
        logAuditEvent(TLS("audit_login_succeeded",$argsObj->login,
                      $_SERVER['REMOTE_ADDR']),"LOGIN",$_SESSION['currentUser']->dbID,"users");

        if ($argsObj->action == 'ajaxlogin') 

為

    // Login successful, redirect to destination
        logAuditEvent(TLS("audit_login_succeeded",$argsObj->login,
                      $_SERVER['REMOTE_ADDR']),"LOGIN",$_SESSION['currentUser']->dbID,"users");
        $_SESSION['KCFINDER'] = array();
        $_SESSION['KCFINDER']['disabled'] = false;
        if ($argsObj->action == 'ajaxlogin') 

#### 2. `<testlink-dir>/third_party/kcfinder/config.php`

將 `'disabled' => true` 改為 `'disabled' => false`

### - `<testlink-dir>/third_party/kcfinder/config.php`

這邊是設定更改 upload 的位置

    'uploadURL' => "/testlink/upload_area/",
    'uploadDir' => "../../upload_area",

### - `<testlink-dir>/conf/htaccess.conf`

最後要設定 apache 允許 client 存取圖片的權限

    </Directory>
    <Directory "D:/Bitnami/testlink-1.9.17-0/apps/testlink/htdocs/upload_area">
    ## no access to this folder
    order allow,deny
    deny from all

    </Directory>

把這邊的 `deny from all` 改為 `allow from all`

這樣就大功告成了

中間 `/testlink` 的位置和最後的權限設定花了不少時間在 debug

不過整體還是算整合的很好，所以也很快就可以架完了
