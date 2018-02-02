---
title: Logstash使用筆記
date: 2017-09-20 17:27:59
tags:
---

下載mssql jdbc driver
https://docs.microsoft.com/en-us/sql/connect/jdbc/using-the-jdbc-driver

logstash for jdbc plugin
https://github.com/logstash-plugins/logstash-input-jdbc

如果執行後，只出現一筆資料，則有可能是被覆蓋了，檢查index中_id是否都長的一樣

可以透過mutate plugin
過濾html tag, 更改欄位名稱，合併或增加欄位

使用分頁的時候，查詢語法不能包含order by的條件

clean_run => true
每次都會清空last_run的計數器

last_run_value 會是查詢結果的最後一筆資料，所以以時間排序的話，就需要使用遞增排序，才能取到最近的時間

多個設定檔時，input會獨立存在，filter、output則會合併共用

參考教學
http://www.codingmecha.com/tag/logstash/

logstash設定檔也可以寫判斷式？
logstash if/else表达式
http://vinc.top/2016/07/15/logstash-ifelse%E8%A1%A8%E8%BE%BE%E5%BC%8F/

簡體中文教學
https://kibana.logstash.es/content/logstash/get-start/hello-world.html

logstash for jdbc doc
https://www.elastic.co/guide/en/logstash/current/plugins-inputs-jdbc.html#plugins-inputs-jdbc-record_last_run

mutate使用方法
https://www.elastic.co/guide/en/logstash/current/plugins-filters-mutate.html#plugins-filters-mutate-rename

logstash config example
https://www.elastic.co/guide/en/logstash/current/config-examples.html

列出logstash plugins
https://www.elastic.co/guide/en/logstash/current/working-with-plugins.html

設定檔
output加入以下，則會在consolo出現相關訊息
output{
  stdout {
    codec => rubydebug
  }
}

logstash in unix 資料夾定義
https://www.elastic.co/guide/en/logstash/current/dir-layout.html

output加入document_id，數值可以利用%{}方式指定為查詢結果的欄位，例如%{id}

