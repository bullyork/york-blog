---
title: js模块化
date: 2018-04-29 00:09:03
tags: js
---
### 引子

诗曰：“衣锦尚絅”。恶其文之著也。故君子之道，闇然而日章；小人之道，的然而日亡。君子之道，淡而不厌、简而文、温而理。知远之近，知风之自，知微之显。可与入德矣。

诗云：“潜虽伏矣，亦孔之昭。“ 故君子内省不疚，无恶于志。君子之所不可及者，其唯人之所不见乎。

诗云：“相在尔室，尚不愧于屋漏。“ 故君子不动而敬，不言而信。

诗曰：“奏假无言，时靡有争。“ 故君子不赏而民劝，不怒而民威于鈇钺。

诗曰：“不显惟德，百辟其刑之。“ 是故君子笃恭而天下平。

诗云：“予怀明德，不大声以色。“ 子曰：“声色之于以化民，末也。“

诗曰：“德輶如毛。“ 毛犹有伦。 “上天之载，无声无臭。“ 至矣。

--《中庸》

### CommonJS

#### module对象

Node内部提供一个Module构建函数。所有模块都是Module的实例。

#### module.exports属性

module.exports属性表示当前模块对外输出的接口，其他文件加载该模块，实际上就是读取module.exports变量。

#### exports变量

为了方便，Node为每个模块提供一个exports变量，指向module.exports。这等同在每个模块头部，有一行这样的命令。

```
var exports = module.exports;
```
造成的结果是，在对外输出模块接口时，可以向exports对象添加方法。

注意，不能直接将exports变量指向一个值，因为这样等于切断了exports与module.exports的联系。
```
exports = function(x) {console.log(x)};
```
上面这样的写法是无效的，因为exports不再指向module.exports了。

下面的写法也是无效的。
```
exports.hello = function() {
  return 'hello';
};

module.exports = 'Hello world';
```
优点：
- 服务器端模块便于重用
- NPM 中已经有将近20万个可以使用模块包
- 简单并容易使用

缺点：
- 同步的模块加载方式不适合在浏览器环境中，同步意味着阻塞加载，浏览器资源是异步加载的
- 不能非阻塞的并行加载多个模块

实现：
- 服务器端的 Node.js
- Browserify，浏览器端的 CommonJS 实现，可以使用 NPM 的模块，但是编译打包后的文件体积可能很大
- modules-webmake，类似Browserify，还不如 Browserify 灵活
- wreq，Browserify 的前身

### AMD

Asynchronous Module Definition 规范其实只有一个主要接口 define(id?, dependencies?, factory)，它要在声明模块的时候指定所有的依赖 dependencies，并且还要当做形参传到 factory 中，对于依赖的模块提前执行，依赖前置。

```
define("module", ["dep1", "dep2"], function(d1, d2) {
  return someExportedValue;
});
require(["module", "../file"], function(module, file) { /* ... */ });
```
优点：
- 适合在浏览器环境中异步加载模块
- 可以并行加载多个模块

缺点：
- 提高了开发成本，代码的阅读和书写比较困难，模块定义方式的语义不顺畅
- 不符合通用的模块化思维方式，是一种妥协的实现

实现：
- RequireJS
- curl

### CMD

Common Module Definition 规范和 AMD 很相似，尽量保持简单，并与 CommonJS 和 Node.js 的 Modules 规范保持了很大的兼容性。

```
define(function(require, exports, module) {
  var $ = require('jquery');
  var Spinning = require('./spinning');
  exports.doSomething = ...
  module.exports = ...
})
```
优点：
- 依赖就近，延迟执行
- 可以很容易在 Node.js 中运行

缺点：
- 依赖 SPM 打包，模块的加载逻辑偏重

实现：
- Sea.js
- coolie

### UMD
Universal Module Definition 规范类似于兼容 CommonJS 和 AMD 的语法糖，是模块定义的跨平台解决方案。

### ES6 模块

ECMAScript6 标准增加了 JavaScript 语言层面的模块体系定义。ES6 模块的设计思想，是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。
```
import "jquery";
export function doStuff() {}
module "localModule" {}
```
优点：
- 容易进行静态分析
- 面向未来的 ECMAScript 标准

缺点：
- 原生浏览器端还没有实现该标准
- 全新的命令字，新版的 Node.js才支持

实现：
- Babel

### 期望的模块系统

可以兼容多种模块风格，尽量可以利用已有的代码，不仅仅只是 JavaScript 模块化，还有 CSS、图片、字体等资源也需要模块化。

#### 前端模块加载

前端模块要在客户端中执行，所以他们需要增量加载到浏览器中。

模块的加载和传输，我们首先能想到两种极端的方式，一种是每个模块文件都单独请求，另一种是把所有模块打包成一个文件然后只请求一次。显而易见，每个模块都发起单独的请求造成了请求次数过多，导致应用启动速度慢；一次请求加载所有模块导致流量浪费、初始化过程慢。这两种方式都不是好的解决方案，它们过于简单粗暴。

分块传输，按需进行懒加载，在实际用到某些模块的时候再增量更新，才是较为合理的模块加载方案。

要实现模块的按需加载，就需要一个对整个代码库中的模块进行静态分析、编译打包的过程。

#### 所有资源都是模块

在上面的分析过程中，我们提到的模块仅仅是指JavaScript模块文件。然而，在前端开发过程中还涉及到样式、图片、字体、HTML 模板等等众多的资源。这些资源还会以各种方言的形式存在，比如 coffeescript、 less、 sass、众多的模板库、多语言系统（i18n）等等。

如果他们都可以视作模块，并且都可以通过require的方式来加载，将带来优雅的开发体验，比如：

```
require("./style.css");
require("./style.less");
require("./template.jade");
require("./image.png");
```
那么如何做到让 require 能加载各种资源呢？

#### 静态分析

在编译的时候，要对整个代码进行静态分析，分析出各个模块的类型和它们依赖关系，然后将不同类型的模块提交给适配的加载器来处理。比如一个用 LESS 写的样式模块，可以先用 LESS 加载器将它转成一个CSS 模块，在通过 CSS 模块把他插入到页面的 <style> 标签中执行。Webpack 就是在这样的需求中应运而生。

同时，为了能利用已经存在的各种框架、库和已经写好的文件，我们还需要一个模块加载的兼容策略，来避免重写所有的模块。