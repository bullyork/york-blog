---
title: html5-正文
date: 2017-03-13 08:15:36
tags: html5
---
### 引子

#### 村居 —— 高鼎
草长莺飞二月天，拂堤杨柳醉春烟。儿童散学归来早，忙趁东风放纸鸢。

——此引子与正文无关

### html5新特性

- canvas
- video,audio
- 离线存储
- article、footer、header、nav、section
-  calendar、date、time、email、url、search

### 各种特性解析

#### canvas

html5让人激动人心的特性之一。大多数玩过三国杀的人都会遇到过这样的问题，需要装一个叫做**flash**的插件。理论上html5可以只根据canvas元素做出任何目前flash能做出来的游戏。canvas允许js控制canvas里面的每个像素，所以在这里，js有了极大的自由度。当然以前html还是可以做一些简单的游戏的，但是脱离了canvas所有的动态效果均需要操作dom，开销很大。所以canvas接口相当于html5提供给了js一个极为开放的图形绘制接口。

参考网站：[createjs](http://www.createjs.cc/)

注：等我们介绍完了html5，css3，js，我会专门写专题介绍如何一步步用createjs做出来很惊艳的效果！

#### video，audio

此两个接口可以允许视频，音频的播放不再依赖于类似于**flash**的插件。

新的浏览器看优酷视频是不是总需要下载flash插件？

大家可以试一试[腾讯视频](https://v.qq.com/)，它实现了video标签播放，不需要额外加载flash插件。

#### 离线存储

- localStorage
- sessionStorage
- Application Cache

网上都可以查到的用法我是不会讲的，之后可以放上链接。这里说下三个接口的意义。localStorage，除非用户手动清除，否则不会消失，和网站绑定。sessionStorage，此窗口不跳转的（可刷新）会话期间保留，跳转或者关闭，存储失效。Application Cache，可用于离线应用，将不常变化的网页资源存储在本地。

三个接口（还有一个IndexedDB正在推广）极大的提高了html5的可用性。想一想，当你没有网的时候，你依然可以浏览一个网站，做任何的操作，是不是很酷。不过有些操作不会立即生效，需要等网络接通后程序自动处理掉。但是这对用户来讲，体验性就有了极大的提高。

#### article、footer、header、nav、section

无须多言，这些标签是自解释的，正确的使用对于网站seo有好处。

#### calendar、date、time、email、url、search

本来由js+css实现的空间可以简单的用html标签了！但是鉴于部分ui并不是很漂亮，可选用！

#### 参考

[菜鸟教程](http://www.runoob.com/html/html5-intro.html)