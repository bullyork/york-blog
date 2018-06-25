---
title: ES7 decorator
date: 2018-06-21 03:41:59
tags: js ECMAScript decorator
---

### 前情概要

#### 复习对象属性

一、 数据属性

- [[Configurable]]: 表示能否通过 delete 删除属性从而  重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。直接定义的属性的这个特性默认为 true
- [[Enumerable]]: 表示能否通过 for-in 循环返回属性。 直接定义的属性的这个特性默认为 true
- [[Writable]]: 表示能否修改属性的值。直接定义的属性的这个特性默认为 true
- [[Value]]: 包含这个属性的数据值。读取属性值的时候，从这个属性读；写入属性值的时候把新值保存在这个位置

要修改属性的默认特性，必须使用 es5 的 Object.defineProperty()方法。这个方法接收三个参数：属性所在的对象、属性的名字和一个描述符对象。

```
var person = {}
Object.defineProperty(person, 'name', {
  writable: false,
  value: "nicolas"
})
console.log(person.name)
person.name = 'Greg'
console.log(person.name)
```

二、 访问器属性

- [[Configurable]]: 表示能否通过 delete 删除属性从而  重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。直接定义的属性的这个特性默认为 true
- [[Enumerable]]: 表示能否通过 for-in 循环返回属性。 直接定义的属性的这个特性默认为 true
- [[get]]: 在读取属性的时候调用的函数，默认值为 undefined
- [[set]]: 在写入属性的时候调用的函数，默认值为 undefined

访问器属性设置一个值，会导致其他值的变化。例如：

```
var book = {
  _year: 2004,
  edition: 1
}
Object.defineProperty(book, "year", {
  get: function(){
    return this._year
  },
  set: function(newValue){
    if(newValue > 2004){
      this._year = newValue
      this.edition += newValue - 2004
    }
  }
})
book.year = 2005;
console.log(book.edition)
```

设置多个对象可以用 Object.defineProperties

>  注意 Object.defineProperties 和 Object.defineProperty 设置的属性的 Configurable 特性为 false

三、 读取属性的特性

```
var book = {}

Object.defineProperties(book, {
  _year: {
    value: 2004
  },
  edition: {
    value: 1
  },
  year: {
    get: function() {
      return this._year
    },

    set: function(){
      if(newValue > 2004){
        this._year = newValue;
        this.edition += newValue - 2004;
      }
    }
  }
})

var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
console.log(descriptor.value)
console.log(descriptor.configurable)
console.log(descriptor.get)
var descriptor2 = Object.getOwnPropertyDescriptor(book, "year");
console.log(descriptor.value)
console.log(descriptor.configurable)
console.log(descriptor.get)
```

#### 复习高阶函数

在数学和计算机科学中，高阶函数是至少满足下列一个条件的函数：

- 接受一个或多个函数作为输入
- 输出一个函数

### let's begin ES7 decorator

一、再铺垫一层，class 究竟是什么

es6 class

```js
//定义类
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return "(" + this.x + ", " + this.y + ")";
  }
}
let p = new Point(1, 2);
```

对应 es5

```js
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function() {
  return "(" + this.x + ", " + this.y + ")";
};

var p = new Point(1, 2);
```

**结论：es6class 是 es5 构造函数的语法糖**

二、方法或属性上面的装饰，**属性 decorator**

栗子

```js
// 注意这里的 `target` 是 `Dog.prototype`
function readonly(target, key, descriptor) {
  descriptor.writable = false;
  return descriptor;
}
class Dog {
  @readonly
  bark() {
    return "wang!wang!";
  }
}

let dog = new Dog();
dog.bark = "bark!bark!"; // 不起作用
```

**结论：方法或属性上面的装饰其实就是接收 target，key，旧的 descriptor，然后返回新的 descriptor 给 Object.defineProperty 使用(到前面看 Object.defineProperty)**

三、类本身的装饰，**类装饰**

栗子

```js
function doge(isDoge) {
  return function(target) {
    target.isDoge = isDoge;
  };
}

@doge(true)
class Dog {}

console.log(Dog.isDoge);
```

**结论：类本身的装饰可以等价于一个高阶函数，接受类(构造函数)，然后返回一个装饰过的新类**

### 总结

装饰器真的非常简单，技术没有魔法，基础都是非常非常简单的概念。

**属性装饰的再思考：** 观察属性装饰接受的参数，target，key，descriptor。其实也还是一个高阶函数，只不过接收了 target，key，指明了具体修饰的内容，相当于类装饰的一个子概念。

**再抽象：** 本篇所述的所有东西无非是 **高阶函数概念的扩展**。高阶函数本身也很简单：1.接受一个或多个函数作为输入 2.输出一个函数。

**再再抽象：** 语言本身的概念其实不多，很多东西都是一个概念的衍生而已，原生概念类似于内力，各种衍生类似于招数。内力是基础，招数是运用。

### 参考

- [ECMAScript 6 入门之 class](http://es6.ruanyifeng.com/#docs/class)
- [Exploring EcmaScript Decorators](https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841)
- [高阶函数-维基百科](https://zh.wikipedia.org/wiki/%E9%AB%98%E9%98%B6%E5%87%BD%E6%95%B0)
