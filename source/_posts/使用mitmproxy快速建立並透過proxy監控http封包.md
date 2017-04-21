---
title: 使用mitmproxy快速建立並透過proxy監控http封包
date: 2017-04-19 11:01:42
tags:
---
想要查看iphone app的各種網路封包、連線的網址
只要是http Request就可以透過mitmproxy獲得相關資訊
https也可以，只需額外在iphone安裝mitmproxy憑證
<!-- more -->

### 安裝mitmproxy
ubuntu 14.04 透過apt只能安裝到0.9.2版
```
sudo apt-get install mitmproxy
```

目前github最新是2.0.x, 想要升級到該版本就需要比較[複雜的安裝步驟](http://docs.mitmproxy.org/en/stable/install.html#install-advanced)
不過0.9.x版其實就不錯用了

### 啟用mitmproxy
``` bash`
mitmproxy
```
預設使用的prot為8080
{% asset_img mitmproxy_start.png 成功執行mitmproxy %}


設定iphone wifi連線的proxy

查看http網路連線資訊吧

