---
title: long-term-cache
date: 2018-05-09 16:41:16
tags: cache
---

### 获取资源

浏览器向服务器请求资源 foo.js

服务器返回 foo.js，并设置缓存时间： cache-control: max-age=312312, public

### 再次请求

浏览器查询本地磁盘是否有 foo.js，并检查它是否已经过期。如果没有过期就直接返回。

### Cache Busting Technique

如果缓存本地没有过期，服务端已经过期，那么后果很严重，代码更新了，然而用户用的还是本地缓存的旧代码。

1.  修改文件的名字：foo.js -> foo.v2.js
2.  修改文件的路径：/static/foo.js -> /static/v2/foo.js
3.  加 query string : foo.js -> foo.js?v=qwer

一般使用第一种方法我们的理想当然是：哪个文件更新了，就`自动地`生成一个新的文件名。

### code splitting

### 参考资料

[用 webpack 实现持久化缓存](https://sebastianblade.com/using-webpack-to-achieve-long-term-cache/)

[webpack-training-project](https://github.com/GoogleChromeLabs/webpack-training-project)

[happypack](http://taobaofed.org/blog/2016/12/08/happypack-source-code-analysis/)
