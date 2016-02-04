title: Google GDG
date: 2015-11-08 10:20:46
tags: [Google]
---


## Google 应用内检索
- deep link App-indexing
- 不一定要网页, 在google检索自己的应用, 直接跳到自己的应用.

## 移动出海, Google Play

- google developer concole可以直接进行灰度测试, 测试各种不同的icon带来的流量. 给每一种icon分配给用户%多少的流量, 方便与看反馈.
- 可以根据应用的段介绍的分批测试, 来看带来的应用的下载.

## AdMob

- 	从 下载量 到 活跃 到 再次安装
-  	App 从 功能性 到内容型/平台型

## Android 规范开发

### 不要给用户制造麻烦 

  - getExternalFilesDir(Environment.DIRECTORY_DCIM); 过滤掉各种缓存
  - getExternalcacheFile();  获取app里的缓存文件
  - 不要滥用上面的API(跟应用的应用信息挂钩.清除缓存的时候会自动调用上面的API)

  
### 尽可能完善控件的职责 
  - 如果没有触摸屏的Android设备, button如果没有focus就无法选中.
  - 如果不想让当前设备在TV设备上使用,就写一下过滤机制,<uses-feature>



















