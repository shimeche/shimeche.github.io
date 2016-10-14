---
title: 查詢google AccessToken狀態
date: 2016-05-18 09:44:03
tags:
---

使用google API取得的Access Token，可以透過以下網址直接查詢對應的Info

[https://www.googleapis.com/oauth2/v1/tokeninfo?access_token=](https://www.googleapis.com/oauth2/v1/tokeninfo?access_token=)


### 正確的話會顯示

```
{
  issued_to: "99999999999.apps.googleusercontent.com",
  audience: ""99999999999.apps.googleusercontent.com",
  user_id: "11106999999999999999999999",
  scope: "https://www.googleapis.com/auth/plus.me",
  expires_in: 3504,
  access_type: "offline"
}
```
其中，expires_in是剩餘幾秒就會過期
<!-- more -->
### 失敗的話則會顯示

```
{
  error: "invalid_token",
  error_description: "Invalid Value"
}
```

也可以查詢id token的info

[https://www.googleapis.com/oauth2/v3/tokeninfo?id_token=](https://www.googleapis.com/oauth2/v3/tokeninfo?id_token=)

預期回傳如下
```
{
  iss: "accounts.google.com",
  at_hash: "qqqqqqqqqqqqqqqqqqqqqqqqqqq",
  aud: "99999999999-99999999999.apps.googleusercontent.com",
  sub: "99999999999",
  email_verified: "true",
  azp: "99999999999-99999999999.apps.googleusercontent.com",
  email: "test@gmail.com",
  iat: "1463550722",
  exp: "1463554322",
  alg: "RS256",
  kid: "99999999999"
}
```
其中，sub其實就是google's userId

如何透過線上工具直接取得access token呢？
可以使用[oauthplayground](https://developers.google.com/oauthplayground/)

