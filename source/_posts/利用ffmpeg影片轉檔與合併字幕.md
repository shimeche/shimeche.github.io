---
title: 利用ffmpeg指令影片轉檔與合併字幕
date: 2016-10-14 13:59:17
tags:
---

[mkv格式, 屬於matroska系列](https://zh.wikipedia.org/wiki/Matroska)支援單一檔案包含視訊與字幕功能
透過ffmpeg指令將字幕封裝到影片檔(mkv格式)
> 測試版本: [ffmpeg 2.7.1](https://www.johnvansickle.com/ffmpeg/) ubuntu 12.04
``` bash
$ ffmpeg -i sourceVideo.mkv -i sourceSubtitle.srt -c copy outputVideo.mkv
```
> -i: 輸入來源，可以多個輸入。同時支援輸入多個字幕檔。
> -c: copy採用複製模式, 輸出的影像格式與輸入相同。

因為不作視訊或音訊的轉換，也就不需花太久時間, 測試影片約為90min，實際合併字幕約花了35秒
<!-- more -->
執行後大致的樣子
``` bash
Input #0, matroska,webm, from 'sourceVideo.mkv':
  Metadata:
    creation_time   : 2014-11-05 23:58:38
    encoder         : libebml v1.3.0 + libmatroska v1.4.1
    TITLE           : EVO
  Duration: 01:29:20.65, start: 0.000000, bitrate: 3688 kb/s
    Chapter #0:0: start 0.000000, end 328.078000
    Metadata:
      title           : Chapter 1
    Chapter #0:1: start 328.078000, end 520.062000
    Metadata:
      title           : Chapter 2
..省略..
    Stream #0:0(eng): Audio: ac3, 44100 Hz, stereo, fltp, 192 kb/s (default)
    Stream #0:1(eng): Video: h264 (High), yuv420p(tv, bt709), 1280x534 [SAR 1:1 DAR 640:267], 23.98 fps, 23.98 tbr, 1k tbn, 180k tbc (default)
Input #1, srt, from 'sourceSubtitle.srt':
  Duration: N/A, bitrate: N/A
    Stream #1:0: Subtitle: subrip
Output #0, matroska, to 'outputVideo.mkv':
  Metadata:
    TITLE           : EVO
    encoder         : Lavf56.36.100
    Chapter #0:0: start 0.000000, end 328.078000
    Metadata:
      title           : Chapter 1
..省略..
    Stream #0:0(eng): Video: h264 (H264 / 0x34363248), yuv420p, 1280x534 [SAR 1:1 DAR 640:267], q=2-31, 23.98 fps, 23.98 tbr, 1k tbn, 1k tbc (default)
    Stream #0:1(eng): Audio: ac3 ([0] [0][0] / 0x2000), 44100 Hz, stereo, 192 kb/s (default)
    Stream #0:2: Subtitle: subrip
Stream mapping:
  Stream #0:1 -> #0:0 (copy)
  Stream #0:0 -> #0:1 (copy)
  Stream #1:0 -> #0:2 (copy)
Press [q] to stop, [?] for help
frame=128527 fps=2812 q=-1.0 Lsize= 2414245kB time=01:29:20.52 bitrate=3689.5kbits/s    
video:2293132kB audio:119065kB subtitle:21kB other streams:0kB global headers:0kB muxing overhead: 0.084050%
```
