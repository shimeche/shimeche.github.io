---
title: Xdebug搭配Visual Studio Code使用方法與設定
date: 2017-07-26 17:04:55
tags:
---
方便的在vc code透過xdebug進行php的偵錯
<!-- more -->
### 執行環境
ubuntu 14.04
nginx or apache or cli
visual studio code 1.14.1
php 5

開發端與Server端不在同一台電腦。
php執行環境在遠端，vscode在本地，透過xdebug.remote功能，可以達到遠端debug的需求。

### 安裝xdebug
```
# sudo apt-get install php5-xdebug
```

### 設定xdebug.ini
編輯xdebug設定檔，加上remote相關資訊
```
# vim /etc/php5/mods-available/xdebug.ini
```
```
zend_extension = xdebug.so
xdebug.remote_enable = 1
xdebug.remote_autostart = 1
xdebug.remote_handler = dbgp
xdebug.remote_host = 172.17.0.1     ;指定vscode所在的IP
xdebug.remote_connect_back = 1      ;如果為1，則會忽略remote_host
xdebug.remote_port = 9000

xdebug.remote_log = "/var/log/xdebug.log"

```
其中remote_host, remote_port要填vscode所在的IP與vscode開啟的port(後面會說明)

remote_connect_back 如果設定為1，預設會將debug資訊拋往client，也就是任意開發端。

只要是對server請求的client都可以得到xdebug_session進行debug

因此必須要小心設定。對了，這個設定值對CLI無效。

