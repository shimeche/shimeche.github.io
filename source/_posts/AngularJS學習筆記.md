---
title: AngularJS學習筆記
date: 2016-05-16 17:49:33
tags:
---

### 取得其他controller的scope

每個controller都有自己的$scope，如何取得其他ctrl的$scope呢？
可以透過angular.element("jquery-selector").scope();取得
其中jqeuryselector就是我們常用的jquery語法
這邊當然是要以尋找該對應ng-controller的tag為主
找到後，就可以直接使用
<!-- more -->
### 手動更新特定$scope的畫面

常用於非同步的callback, 無法自動觸發angular apply

方法一：
``` js
$scope.value = 'newValue';
$scope.$apply();
```

方法二：
``` js
$scope.$apply(function($_scope){
  $_scope.value = 'newValue';
});
```