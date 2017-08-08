---
title: dva细探
date: 2017-07-25 11:28:30
tags: dva
---
### 引子

所谓致知在格物者，言欲致吾之知，在其物而穷其理也。盖人心之灵莫不有知，而天下之物莫不有理，唯于理有未穷，故其知又不尽也，是以《大学》始教，必使学者即凡于天下之物，莫不因其己知之理而益穷之，以求至乎其极。至于用力之久，而一旦豁然贯通焉，则众物之表里精粗无不到，而吾心之全体大用无不明矣。此谓物格，此谓知之至也。

--《大学》

### dva是什么？

[dva](https://github.com/dvajs/dva-docs/blob/master/v1/zh-cn/getting-started.md) 是一个基于 react 和 redux 的轻量应用框架，概念来自 elm，支持 side effects、热替换、动态加载、react-native、SSR 等，已在生产环境广泛应用。

### 前提概要

要有react，redux，react-router的基础，如果不具备，请自行到官网学习，过不去的话请自行翻墙，强烈建议去官网学习，否则道听途说，断章取义。[react官网](https://facebook.github.io/react/)，[redux官网](http://redux.js.org/)，[react-router官网](https://reacttraining.com/react-router/)

### react-router-redux是什么

查看dva源码，index中赫然引用了react-router-redux，这到底是干嘛的? 名字上看起来是react-router和redux的结合。那么到github上看下吧。[react-router-redux github地址](https://github.com/reactjs/react-router-redux)。

这个库解决的问题：如果要回溯到之前的状态时，你重复触发之前的actions的时候，页面是不会跳转的，因为react-router只控制url的状态改变。所以和这个库让url状态和redux store同步。

*这个库只在你关心actions的记录，持久化和重演的过程时有用，如果你不关心这些的话，可以不用这个库！就用redux和react-router就好*

### createDva做了什么？

```javascript
return function dva(hooks = {}) {
    // history and initialState does not pass to plugin
    const history = hooks.history || defaultHistory;
    const initialState = hooks.initialState || {};
    delete hooks.history;
    delete hooks.initialState;

    const plugin = new Plugin();
    plugin.use(hooks);

    const app = {
      // properties
      _models: [],
      _router: null,
      _store: null,
      _history: null,
      _plugin: plugin,
      // methods
      use,
      model,
      router,
      start,
    };
    return app;
    ...(各种方法)
```
所以createDva返回了一个函数，函数接受hooks为参数，返回一个携带各种方法的app对象，那么这些方法到底用来做什么的呢？

### 方法

暂时延后方法描述，直接走使用环节

### es6生成器(generator)详解

生成器使用**function\***和**yield**来简化迭代程序的编写。声明为**function\***的函数返回一个生成器实例。生成器是迭代器的子类型，继承了迭代器的 **next** 和 **throw**。这些特性可以使值反流回生成器，**yield**是一个返回值的表达式（或者使用**throws**）。

注：还可以使‘await’等类异步编程的代码生效，可以查看ES7的await建议

```javascript
var fibonacci = {
  [Symbol.iterator]: function*() {
    var pre = 0, cur = 1;
    for (;;) {
      var temp = pre;
      pre = cur;
      cur += temp;
      yield cur;
    }
  }
}

for (var n of fibonacci) {
  // truncate the sequence at 1000
  if (n > 1000)
    break;
  console.log(n);
}
```
生成器只能使用 typescript 类型语法描述
```typescript
interface Generator extends Iterator {
    next(value?: any): IteratorResult;
    throw(exception: any);
}
```

### Symbol 和 iterator(迭代器)详解

Symbol: 第七种基本类型（前六种：string，number，boolean，null，undefined，object）

Symbol的主要作用是用来生成一个不可能重复的key，保证程序向下兼容！（否则新的key不知道是否被使用过）

迭代器是一种为各种不同的数据结构提供统一的访问机制的接口。一个迭代器对象 ，知道如何每次访问集合中的一项， 并记录它的当前在序列中所在的位置。在  JavaScript 中 迭代器是一个对象，提供了一个 next()  方法，返回序列中的下一项。这个方法返回包含 done 和 value 两个属性的对象。

迭代器支持 for of

