---
title: 微信开发大纲
date: 2017-06-29 10:25:46
tags: 微信开发
---

### 开发前必读提炼

OpenID: 单个公众号
UnionID: 多个公众号

**注意事项：**

- 每个接口都有每日接口调用频次限制，可以在公众平台官网-开发者中心处查看具体频次。
- 公众平台以access_token为接口调用凭据，来调用接口，所有接口的调用需要先获取access_token，access_token在2小时内有效，过期需要重新获取，但1天内获取次数有限，开发者需自行存储，详见获取接口调用凭据（access_token）文档。

**会话：**

- 群发消息：公众号可以以一定频次（订阅号为每天1次，服务号为每月4次），向用户群发消息，包括文字消息、图文消息、图片、视频、语音等

- 被动回复消息：在用户给公众号发消息后，微信服务器会将消息发到开发者预先在开发者中心设置的服务器地址（开发者需要进行消息真实性验证），公众号可以在5秒内做出回复，可以回复一个消息，也可以回复命令告诉微信服务器这条消息暂不回复。被动回复消息可以设置加密（在公众平台官网的开发者中心处设置，设置后，按照消息加解密文档来进行处理。其他3种消息的调用因为是API调用而不是对请求的返回，所以不需要加解密）。

- 客服消息：在用户给公众号发消息后的48小时内，公众号可以给用户发送不限数量的消息，主要用于客服场景。用户的行为会触发事件推送，某些事件推送是支持公众号据此发送客服消息的，详见微信推送消息与事件说明文档。

- 模板消息：在需要对用户发送服务通知（如刷卡提醒、服务预约成功通知等）时，公众号可以用特定内容模板，主动向用户发送消息。

**公众号内的网页**

1. 网页授权获取用户基本信息：通过该接口，可以获取用户的基本信息（获取用户的OpenID是无需用户同意的，获取用户的基本信息则需用户同意）
2. 微信JS-SDK：是开发者在网页上通过JavaScript代码使用微信原生功能的工具包，开发者可以使用它在网页上录制和播放微信语音、监听微信分享、上传手机本地图片、拍照等许多能力。


### 开发者规范

- 涉及用户数据时：
您的服务需要收集用户任何数据的，必须事先获得用户的明确同意，且仅应当收集为运营及功能实现目的而必要的用户数据， 同时应当告知用户相关数据收集的目的、范围及使用方式等，保障用户知情权；
您收集用户的数据后，必须采取必要的保护措施，防止用户数据被盗、泄漏等；
您收集用户的数据后，必须采取必要的保护措施，防止用户数据被盗、泄漏等；
您在特定微信公众号中收集的用户数据仅可以在该特定微信公众号中使用，不得将其使用在该特定微信公众号之外或为其他任何目的进行使用，也不得以任何方式将其提供给他人；
如果腾讯认为您收集、使用用户数据的方式，可能损害用户体验，腾讯有权要求您删除相关数据并不得再以该方式收集、使用用户数据；
一旦您停止使用本服务，或腾讯基于任何原因终止您使用本服务，您必须立即删除全部因使用本服务而获得的数据（包括各种备份）， 且不得再以任何方式进行使用；
- 其他规范
请勿为任何用户自动登录到微信公众平台提供代理身份验证凭据；
请勿提供跟踪功能，包括但不限于识别其他用户在个人主页上查看、点击等操作行为；
请勿自动将浏览器窗口定向到其他网页；
请勿设置或发布任何违反相关法规、公序良俗、社会公德等的玩法、内容等；
请勿公开表达或暗示，您与腾讯之间存在合作关系，包括但不限于相互持股、商业往来或合作关系等，或声称腾讯对您的认可；

- [公众号接口权限说明](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1433401084)

### 接口调用频次限制说明

公众号调用接口并不是无限制的。为了防止公众号的程序错误而引发微信服务器负载异常，默认情况下，每个公众号调用接口都不能超过一定限制，当超过一定限制时，调用对应接口会收到如下错误返回码：

```
{"errcode":45009,"errmsg":"api freq out of limit"}
```
开发者可以登录微信公众平台，在帐号后台开发者中心接口权限模板查看帐号各接口当前的日调用上限和实时调用量，对于认证帐号可以对实时调用量清零，说明如下：
1. 由于指标计算方法或统计时间差异，实时调用量数据可能会出现误差，一般在1%以内。
2. 每个帐号每月共10次清零操作机会，清零生效一次即用掉一次机会（10次包括了平台上的清零和调用接口API的清零）。
3. 第三方帮助公众号调用时，实际上是在消耗公众号自身的quota。
4. 每个有接口调用限额的接口都可以进行清零操作。

[各接口频次](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1433744592)

### [全局返回码说明](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1433747234)
