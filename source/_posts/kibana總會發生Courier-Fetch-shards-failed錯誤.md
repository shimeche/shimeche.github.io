---
title: 'kibana發生Courier Fetch: n of m shards failed警告'
date: 2019-06-19 11:54:14
tags:
---

{% asset_img shards-failed-01.png %}

查看Elasticsearch的logs並沒有噴錯誤或警告
單純就在kibana查詢時，上頭會出現黃色警告 Courier Fetch: 5 of 25 shard failed

Google一下，大部分的解法都是調整elasticsearch.yml的設定
這邊是透過docker-compose啟動服務的，所以直接修改yml的內容
``` yml
version: '2.2'
services:
  elasticsearch:
    build:
      context: ./elasticsearch
    restart: always
    container_name: elasticsearch
    environment:
      - cluster.name=elk-cluster
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms1536m -Xmx1536m"
      - http.cors.enabled=true
      - http.cors.allow-origin=*
    #   參考google解法，加上下列這段設定
      - thread_pool.search.queue_size=10000
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - esdata1:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - esnet
```
但是！加了好像沒用，還是持續跳出這個錯誤阿...

後來改用Dev Tools，將Discover的Request內容重新POST一次
發現，原來是某一個index有問題，導致該index的shard出錯了
``` json
  "_shards": {
    "total": 25,
    "successful": 20,
    "skipped": 0,
    // 明顯跟你說有shard失敗
    "failed": 5,
    "failures": [
      {
        "shard": 0,
        // 就是下面這個index出錯了
        "index": "index-%{published_month}",
        "node": "Tj7AFdTbTvO05iVumSWvLw",
        "reason": {
          "type": "illegal_argument_exception",
          "reason": "Fielddata is disabled on text fields by default. Set fielddata=true on [published] in order to load fielddata in memory by uninverting the inverted index. Note that this can however use significant memory. Alternatively use a keyword field instead."
        }
      }
    ]
  }
```

透過下列語法將這個有問題的index砍掉後，就沒問題了
``` curl
DELETE index-%25{published_month}/
```