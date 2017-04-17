---
title: Ubuntu 14.04安裝ffmpeg
date: 2017-04-13 15:35:39
tags:
---
Ubuntu 14.04 版本已經不支援ffmpeg，由Libav取代
想要安裝的話，需要手動執行一些步驟
<!-- more -->
如果使用一般安裝套件指令安裝ffmpeg時，會出現以下的錯誤

``` bash
$ sudo apt-get install ffmpeg
Reading package lists... Done
Building dependency tree       
Reading state information... Done
E: Unable to locate package mpeg
```

另外新增ppa套件來源
``` bash
$ sudo add-apt-repository ppa:mc3man/trusty-media
```

更新套件資訊
``` bash
$ sudo apt-get update
```

再跑一次就可以了
``` bash
$ sudo apt-get install ffmpeg
```