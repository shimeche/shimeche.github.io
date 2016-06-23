---
title: Facebook Graph API操作
date: 2016-06-23 10:41:25
tags:
---

### 取得無期限token方法

https://developers.facebook.com/tools/explorer

選擇想要授權的應用程式

選擇Get User Access Token

至少勾選pages_show_list權限，其他在視需求勾選，Ex: email...

授權畫面確定後，存取權仗就會出現新的access token

但此時該token的期限只有一小時左右

接著透過瀏覽器或curl要求下面網址

Request:
https://graph.facebook.com/oauth/access_token?client_id=[預授權的應用程式client_id]&client_secret=[預授權的應用程式client_secret]&grant_type=fb_exchange_token&fb_exchange_token=[剛剛獲得的短效accessToken]
Response:
access_token=EAAHFVJ2x6mwBAHwjsxWvhPghIsXqyr3l1cTSbPaLZCeYF8LBZAw27P8kRv6QHxA7LBI6FTgVIQrpiKvwvd67TpS3kASYvIvoHHZBL6RKXKdwXdlk7jZCG6UPZC4fFnCWp6h7Y7XNXHZB5uZBh30ZCEgfe7Em4jGxZALUZD&expires=5184000

此時的accessToken會是兩個月的有效期，其實蠻長了。但是我們要的是永不過期的!

接著再回到Graph tool
https://developers.facebook.com/tools/explorer
將兩個月期效的accessToken貼在存取權仗的欄位
Graph API欄位則填入me/accounts, 按送出後，就會列出相關資訊
這時候神奇的來了，原本的兩個月期效的accessToken就會變成永不過期的accessToken了

### 查詢access Token資訊、有效期限

https://developers.facebook.com/tools/debug/accesstoken

