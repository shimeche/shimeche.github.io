---
title: Mongodb啟用問題
date: 2016-05-26 11:27:24
tags:
---

有時候不正常關機或不明原因導致mongoDB停止
接著重新啟用mongoDB時，就叫不醒了
但執行啟用mongoDB指令時，也沒有出現特別的錯誤，還跟你說啟用了!!?
```
$ sudo start mongod
```
> mongod start/running, process 1660
> 看起來是啟用了阿~~~

但察看網路服務狀況，仍然沒有出現mongoDB開啟的port
```
$ netstat -tpnl
預設應該要有27017的port
```

這時查看mongoDB的log就能清楚為什麼了

```
$ more /var/log/mongodb/mongod.log
```
> 2016-05-26T10:10:27.511+0800 I CONTROL  ***** SERVER RESTARTED *****
> 2016-05-26T10:10:33.100+0800 W -        [initandlisten] Detected unclean shutdown - /var/lib/mongodb/mongod.lock is not empty.
> 2016-05-26T10:10:33.728+0800 I STORAGE  [initandlisten] exception in initAndListen: 98 Unable to create/open lock file: /var/lib/mongodb/mongod.lock errno:13 Permission denied Is a mongod instance already running?, terminating
> 2016-05-26T10:10:33.728+0800 I CONTROL  [initandlisten] dbexit:  rc: 100
可以看到`/var/lib/mongodb/mongod.lock`並沒有被清除，有可能是不正常停用的關係
先把他清除試試看
```
$ sudo rm /var/lib/mongodb/mongod.lock
```

在重跑一次啟用指令
```
$ sudo start mongod
```

察看網路服務狀況
```
$ netstat -tpnl
預設應該要有27017的port
```

如果仍然沒有，就在看一次mongoDB log
```
$ more /var/log/mongodb/mongod.log
```
> 2016-05-26T11:13:12.383+0800 I CONTROL  ***** SERVER RESTARTED *****
> 2016-05-26T11:13:12.408+0800 E NETWORK  [initandlisten] Failed to unlink socket file /tmp/mongodb-27017.sock errno:1 Operation not permitted
> 2016-05-26T11:13:12.408+0800 I -        [initandlisten] Fatal Assertion 28578
> 2016-05-26T11:13:12.408+0800 I -        [initandlisten] 

這時候又有新的錯誤訊息，沒有權限去刪除`/tmp/mongodb-27017.sock`
那一樣把他刪掉吧
```
$ sudo rm /tmp/mongodb-27017.sock
```

再試一次!
```
$ sudo start mongod
```

查看網路服務狀況
```
$ netstat -tpnl
預設應該要有27017的port
```
應該就會有了!

查看log
```
$ more /var/log/mongodb/mongod.log
```
> 2016-05-26T11:14:26.988+0800 I CONTROL  ***** SERVER RESTARTED *****
> 2016-05-26T11:14:27.035+0800 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=7G,session_max=20000,eviction=(threads_max=4),statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
> 2016-05-26T11:14:28.763+0800 I CONTROL  [initandlisten] MongoDB starting : pid=1533 port=27017 dbpath=/var/lib/mongodb 64-bit host=ubuntu-mongodb
> 2016-05-26T11:14:28.764+0800 I CONTROL  [initandlisten] 
> 2016-05-26T11:14:28.764+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
> 2016-05-26T11:14:28.764+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
> 2016-05-26T11:14:28.764+0800 I CONTROL  [initandlisten] 
> 2016-05-26T11:14:28.764+0800 I CONTROL  [initandlisten] db version v3.0.2
> 2016-05-26T11:14:28.764+0800 I CONTROL  [initandlisten] git version: xx
> 2016-05-26T11:14:28.764+0800 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
> 2016-05-26T11:14:28.764+0800 I CONTROL  [initandlisten] build info: Linux 
> 2016-05-26T11:14:28.764+0800 I CONTROL  [initandlisten] allocator: tcmalloc
> 2016-05-26T11:14:28.764+0800 I CONTROL  [initandlisten] options: { config: "/etc/mongod.conf", net: { bindIp: "0.0.0.0", port: 27017 }, processManagement: { fork: true }, storage: { dbPath: "/var/lib/mongodb", engine: "wiredTiger", journal: { enabled: true }, wiredTiger: { collectionConfig: { blockCompressor: "snappy" } } }, systemLog: { destination: "file", logAppend: true, path: "/var/log/mongodb/mongod.log" } }
> 2016-05-26T11:14:28.824+0800 I NETWORK  [initandlisten] waiting for connections on port 27017

最後一行，在等待連線了，看來沒問題了！