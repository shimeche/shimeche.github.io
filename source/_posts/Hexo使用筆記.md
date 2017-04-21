---
title: Hexo使用筆記
date: 2016-05-12 18:02:25
tags:
---

### 發表文章

``` bash
$ hexo n [type] [title]
or
$ hexo new [type] [title]
```
[type]可以是post, page or draft, 預設是post

### 發表文章 Example
``` bash
$ hexo n 這是一篇文章
or
$ hexo new 這是一篇文章
```
<!-- more -->
### 編譯
``` bash
$ hexo g
or
$ hexo generate
```

### 開啟自建伺服器進行預覽
``` bash
$ hexo s
or
$ hexo server
```
照著訊息指示瀏覽網址就可以了


### 在文章中加入圖片
把_config.yml的post_asset_folder設定為true
```
post_asset_folder: true
```

之後建立新文章時，自動在source/_posts資料夾中，以新文章名稱建立對應的資料夾
如果是既有的文章，也可以手動以文章名稱建立對應的資料夾
該文章的圖片或附件就可放在此資料夾
使用方法官網有簡單的[介紹](https://hexo.io/zh-tw/docs/asset-folders.html)
例如圖片叫做hexo-logo.png 將該圖片放在對應的資料夾，大概的結構如下
```
source
--> _posts
--> 文章標題   <----資料夾
------> hexo-logo.png
------> image   <----資料夾
--------> hexo-logo.png
--> 文章標題.md
```
那麼插入圖片的語法就是如下
```
{% asset_img hexo-logo.png 這就是圖片啦 %}
```
{% asset_img hexo-logo.png 這就是圖片啦 %}

包含子資料夾的話也沒問題
```
{% asset_img "image/hexo-logo.png" 這是放在子資料夾中的圖片 %}
```
{% asset_img "image/hexo-logo.png" 這是放在子資料夾中的圖片 %}

當然也可以透過markdown語法直接加入外部圖片的連結
```
![外部圖片](https://raw.githubusercontent.com/hexojs/logo/master/hexo-logo.png)
```
![外部圖片](https://raw.githubusercontent.com/hexojs/logo/master/hexo-logo.png)