---
title: '在ubuntu利用mono執行C#程式'
date: 2017-08-24 11:18:58
tags:
---

不需要微軟的環境就可以編譯與執行C#，真的蠻方便的
mac(osx)也可以，但還沒實際試過
mono在ubuntu安裝步驟挺簡單的，可以參考底下的Ref.
<!-- more -->
## 常用的指令
### msbuild
原本叫做xbuild，現在已經被廢除改用msbuild
可以直接進入C#專案目錄，執行msbuild即可開始編譯
```
$ msbuild
```
也可以指定目錄作編譯
```
$ msbuild ./src/
```

### mcs
就是個Complier，自己寫個簡單的.cs就可以在ubuntu環境下編譯為exe檔
透過-r可以在編譯時加入需要的參照
```
$ mcs path/file.cs -out:file.exe -r:Newtonsoft.Json.dll -r:bin/any.dll
```

### mono
執行exe檔的指令，很簡單的執行方式：
```
$ mono file.exe
```
預設執行時，會去尋找編譯時所參照的dll
優先會去找跟exe檔同目錄的dll，沒有的話就會去找/usr/lib/底下，都沒有的話基本上就會報錯
```
Unhandled Exception:
System.IO.FileNotFoundException: Could not load file or assembly 'any, Version=0.7.0.0, Culture=neutral, PublicKeyToken=null' or one of its dependencies.
File name: 'any, Version=0.7.0.0, Culture=neutral, PublicKeyToken=null'
[ERROR] FATAL UNHANDLED EXCEPTION: System.IO.FileNotFoundException: Could not load file or assembly 'any, Version=0.7.0.0, Culture=neutral, PublicKeyToken=null' or one of its dependencies.
File name: 'any, Version=0.7.0.0, Culture=neutral, PublicKeyToken=null'
```
所以如果參照的dll跟exe並不在一起，就要透過環境變數path的方式來執行，例如參照DLL放在bin目錄下：
```
$ MONO_PATH=bin mono file.exe
```

另外，透過以下方式就知道實際執行時，mono的執行過程
```
$ MONO_LOG_LEVEL=debug mono file.exe
```

## Ref:
- Mono Office Site 
http://www.mono-project.com/
- 安裝mono可參考此篇文章，Compile and run C# in the Command Line (Linux, Mac & Windows) 
https://www.codetuts.tech/compile-c-sharp-command-line/
- 透過環境變數加入參照執行mono
DllNotFoundException or TypeLoadException
https://brendanzagaeski.appspot.com/0013.html
