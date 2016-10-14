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
<!-- more -->
這時查看mongoDB的log就能清楚為什麼了

``` bash
$ more /var/log/mongodb/mongod.log
```
> I CONTROL  ***** SERVER RESTARTED *****
> W -        [initandlisten] Detected unclean shutdown - <font color="red">/var/lib/mongodb/mongod.lock is not empty.</font>
> I STORAGE  [initandlisten] exception in initAndListen: 98 Unable to create/open lock file: /var/lib/mongodb/mongod.lock errno:13 Permission denied Is a mongod instance already running?, terminating
> I CONTROL  [initandlisten] dbexit:  rc: 100

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
> I CONTROL  ***** SERVER RESTARTED *****
> E NETWORK  [initandlisten] <font color="red">Failed to unlink socket file /tmp/mongodb-27017.sock</font> errno:1 Operation not permitted
> I -        [initandlisten] Fatal Assertion 28578
> I -        [initandlisten] 

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
```
預設應該要有27017的port, 應該就會有了!
> (Not all processes could be identified, non-owned process info will not be shown, you would have to be root to see it all.)
> Active Internet connections (only servers)
> Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
> tcp        0      0 0.0.0.0:443             0.0.0.0:\*               LISTEN      -
> tcp        0      0 <font color="red">127.0.0.1:27017</font>         0.0.0.0:\*               LISTEN      -
> tcp        0      0 0.0.0.0:389             0.0.0.0:\*               LISTEN      -

查看log
```
$ more /var/log/mongodb/mongod.log
```
> I CONTROL  ***** SERVER RESTARTED *****
> I STORAGE  [initandlisten] wiredtiger_open config: ...
> I CONTROL  [initandlisten] MongoDB starting : pid=1533 port=27017 dbpath=/var/lib/mongodb 64-bit host=ubuntu-mongodb
> I CONTROL  [initandlisten] db version v3.0.2
> I CONTROL  [initandlisten] git version: xx
> I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
> I CONTROL  [initandlisten] build info: Linux 
> I CONTROL  [initandlisten] allocator: tcmalloc
> I CONTROL  [initandlisten] options: { config: "/etc/mongod.conf", net: { bindIp: "0.0.0.0", port: 27017 }, processManagement: { fork: true }, storage: { dbPath: "/var/lib/mongodb", engine: "wiredTiger", journal: { enabled: true }, wiredTiger: { collectionConfig: { blockCompressor: "snappy" } } }, systemLog: { destination: "file", logAppend: true, path: "/var/log/mongodb/mongod.log" } }
> I NETWORK  [initandlisten] <font color="red">waiting for connections on port 27017</font>

最後一行，在等待連線了，看來沒問題了！