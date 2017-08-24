---
title: npm用法
date: 2017-04-24 17:30:38
tags:
---
有關npm各種用法

<!-- more -->

### 安裝特定套件
如hexo
```
npm install hexo
```
預設會將套件安裝在當下專案目錄的node_module底下

### 安裝全域套件
如果想要安裝在Global，讓其他專案也可以一起使用，或隨處都可以執行
安裝時加上-g的參數即可
```
npm install hexo -g
```

### 安裝並更新package.json
安裝套件時同時更新package.json
```
npm install hexo -save
```

### 依據Package.json安裝所需套件
初始專案擁有packag.json時，可以直接透過install安裝所需目錄
```
npm install
```

### 更新npm本身
將npm更新到最新版本，而且加上-g就是安裝在全域的環境
```
npm install npm@latest -g
```