---
title: 常用bash指令
date: 2016-05-13 14:04:43
tags:
---

### WGet

``` bash
$ wget -O- [url]
```
將wget取得的內容，直接轉往STUDSTO(pipe方式)


``` bash
$ .... | args wget -O a.zip
```
透過pipe取得wget的目標url, args可以將參數傳遞給wget

### grep

``` bash
$ grep -o 
```
只顯示match的部份

### cut

``` bash
$ cut -d \" -f 9
```
-d 利用"符號分割字串為陣列，其中\是跳脫字元
-f 取得第九個元素，元素起始值為1

### 發表文章

``` bash
$ hexo n [type] [title]
```