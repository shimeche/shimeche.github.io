---
title: 利用ffmpeg合併m3u8串流影片，並且轉成MP4格式
date: 2017-04-13 15:34:49
tags:
---

在開啟影片前，先打開Chrome的 F12，才能觀看網路相關的網址

會發現串流的網路連線狀態，依序下載串流影片
media_00000_0.ts, media_00000_1.ts, media_00000_2.ts...其原理就是透過播放器組起來播放
所以可以試著用wget指令，把每個網址存成檔案，接著在將它們合併起來，果真能播放！
可以一個一個抓
``` bash
wget http://.......media_00000_0.ts
wget http://.......media_00000_1.ts
.
.
.
wget http://.......media_00000_3.ts
```

也可以透過bash批次抓
``` bash
for i in `seq 1 210`;
do
	wget "http://.......media_00000_"$i".ts"
    sleep 2
done
```

接著合併
可以透過cat一個一個合併，但是這樣會打到死
``` bash
cat media_00000_0.ts media_00000_1.ts (省略) media_00000_210.ts > media.ts
```

也可以使用ffmpeg直接合併
既然是串流播放，那麼就會有m3u8 list提供給播放器，當作串流依據
所以只要找到這個檔案，接著在透過ffmpeg就可以合併了
``` bash
ffmpeg -i media.m3u8 -c copy media.ts
File Type                       : M2T
MIME Type                       : video/mpeg
Audio Stream Type               : MPEG-2 AAC Audio
```
合併完後會是M2T的格式，是可以正常透過一般撥放器播放，也可以轉成mp4格式
``` bash
ffmpeg -i media.ts -vcodec copy -acodec copy media.mp4
File Type                       : MP4
MIME Type                       : video/mp4
Major Brand                     : MP4  Base Media v1 [IS0 14496-12:2003]
```
其實也可以直接將m3u8的url直接餵給ffmpeg，就自動下載與合併了 
``` bash
ffmpeg -i http://......m3u8 -c copy media.ts
```