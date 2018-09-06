---
title: 微服务流程以及带来的前端工程化思考
date:  2017-06-24 09:00:36
tags: 微服务
---

### 引子

此谓诚於中，形於外，故君子必慎其独也。

-- 礼记·大学

### 前情概要

传统的 web 开发并没有前后分离的概念，无论是 c#，还是 java，抑或是世界上最好的语言 PHP。大多是采用 cshtml，或者 jsp 作为模版，当作 view 层。这时候的前端扮演的角色很有限，通常工作流是前端根据设计还原出页面（html）。然后使用 js 制作动态效果或者使用 ajax 做少量的异步刷新。 那么带来的问题是：

-  后端同学不仅要关注数据问题，同时还要关注渲染问题
- 前端同学和后端同学很难完全同步开发
-  出现问题不容易迅速归责，而且前端问题，即使前端修改了，后端必须同时修改，浪费人力
-  每次跳转都要向后端发送整个页面的请求，即使两个页面差别不大

鉴于出现上述问题，我们  出于工程化目的，需要前后分离。随着前端的逐渐发展，打包工具不断更新，react，vue 等  优秀框架逐渐完善。前端已经成熟到可以工程化了。 这时候前后分离已经不仅仅是一种设想了，同时是可以在实际中运用的工程化操作。

**思路：前端  后端通过一个*协议规范*定义方法，参数类型，返回类型。协商制定完成协议后，前端根据协议自主使用 mock 数据开发前端视图和交互逻辑。后端根据协议自主实现协议规定的接口定义。最后前端+后端一起  联调 ，联调后交付测试**

###  沟通的规范选择

最开始我们选择的是[thrift](http://thrift.apache.org/)，[thrift 文件  示例](https://git-wip-us.apache.org/repos/asf?p=thrift.git;a=blob_plain;f=tutorial/tutorial.thrift)。

thrift 非常强大，但是原生不支持 go 语言(我们后端使用 go 语言)。

所以我们选用了[proto-buffers](https://developers.google.com/protocol-buffers/)

两者都属于 IDL 语言(接口定义语言)。两者各有优缺点，thrift 比较成熟，但是文档  不完善。proto-buffers 文档齐全，底层采用二进制，文件更小。

[两者具体的比较](https://my.oschina.net/itblog/blog/289965)

ok, 那么一个服务的定义长什么样呢？举个栗子：

```proto
syntax = "proto3";
package test;

service Sample {
	rpc Ping(SamplePing) returns (SamplePingResp);
}

// ---------------------------------------------------------------------------

message SamplePing {
	string Msg = 1;
}

message SamplePingResp {
	string Msg = 1;
}
```

ok，这个例子很重要，我们之后会回来再看这个例子。

## 后端服务生成套路

我们采用的是 rpc 协议，至于 rpc 和 restful 之争，可以参考 [这里](https://www.zhihu.com/question/28570307/answer/47876255)

我们暂时假设上述示例要完成的是一个  名为 test 的服务，那么我们可以通过** 域名／test.Sample/Ping**来语义化的定义我们  的服务 uri。ok，到了这里，我们前端同学会忽略掉你所有的代码逻辑，只关注于你在** 域名／test.Sample/Ping**上面实现的服务是否和定义一致。

ok，接下来是实现定义好的方法，那怎么做才能更好的把控服务性能并且减少开发时间呢？

### 减少开发时间，提升开发效率

#### goflow

什么是 goflow，goflow 做了什么？

goflow 准确的说是一整套开发工具，提供的是命令行街口，可以通过命令行方便的创建服务，启动服务，提交 mr 等；同时 goflow 还将常用的包进行  统一的版本管理。总之，goflow 提供的是更舒服的开发体验。

#### ezorm

 我们通常有可能采用不同的数据哭，比如 mysql，mongo。那么每次都写访问数据  库的代码是重复并且没有必要的。所以 ezorm 可以自动生成数据库调用。[ezorm 文档传送门](https://github.com/ezbuy/ezorm)

所以有了 goflow 和 ezorm，程序员可以专注于业务实现！

### 控制服务性能，进行服务监控

#### consul

 什么是 consul？consul 有什么用？

[consul](https://www.consul.io/)是一个服务发现机制 ，主要用来做服务发现。

为什么需要服务发现？

当我们的服务变得多了起来的时候，去监控每个服务是否正常服务，并在异常时进行报警是非常重要的！

### 性能实时监控

#### grafana

[grafana](https://grafana.com/)是一个开源的性能分析和监控工具，界面很酷。

#### kibana

[kibana](https://www.elastic.co/guide/en/kibana/current/introduction.html)开源的，基于 Elasticsearch 的统计工具

### 错误日志

#### sentry

[sentry](https://github.com/getsentry/sentry)是一个  实时日志平台，方便及时接受错误并报警。

##  前端套路

### api 调用

由于 protobuf 并没有支持直接生成前端调用（有 nodejs 的），所以目前前端是自己解析 protobuf 并且生成调用

### 渲染技术

 通过对比 react 和 vue 我们最终选择的 react，因为考虑到生态  链还有维护人， 我们有理由相信 react 会更稳定一点。

### 接下来要做的事

通过参考后端的套路，我不禁在思索前端是否也可以进行  一套工具的开发？

### jsflow

要达成目的：对公司内部  依赖版本进行统一管理，新建项目时，自动生成 webpack 配置，基本项目结构。通过命令行自动生成  组件基本结构。

### jsorm

根据定义生成基本的 api 调用，异步 action，异步 action 对应 reducer。

### pageGen

如果仅仅是后台应用（增删改查），可以通过 yaml 配置，直接生成页面！

以上，jsflow，jsorm 适用于所有 react 项目，可以帮助人工更少的书写代码，新同学不需要  了解太多概念，就可以提供有效代码，只需要关注于组件的 ui 展示和  交互逻辑。

### 扩展

可以扩展一个统一提供报错机制，将前端错误报送日志服务器。可以在框架层面加上错误处理和性能优化。

 我正在做的是 jsorm， 目前还不成熟，还在努力的 coding。但我觉得有一整套开发工具链是很有必要的， 可以提高开发速度，并且提高开发质量！并且可以在架构层面做优化。

以上，暂时的  思考，各位  大佬，请多多指教！
