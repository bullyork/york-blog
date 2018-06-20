---
title: js高程拾遗
date: 2018-05-28 21:55:43
tags: javascript高级程序设计
---

**typeof 操作符**

* undefined
* boolean
* string
* number
* object
* function

**五种基本类型**

* Undefined
* Null
* Boolean
* Number
* String

**引用类型**

* Object
* Array
* Date
* RegExp
* Function
* 基本包装类型（Boolean Number String）

> 函数实际上是对象，每个函数都是 Function 类型的实例，而且都与其他引用类型一样具有属性和方法。函数名实际上也是一个指向函数对象的指针。

**面向对象**

每个对象都是基于一个引用类型创建的

**属性类型**

一、 数据属性

* [[Configurable]]: 表示能否通过 delete 删除属性从而  重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。直接定义的属性的这个特性默认为 true
* [[Enumerable]]: 表示能否通过 for-in 循环返回属性。 直接定义的属性的这个特性默认为 true
* [[Writable]]: 表示能否修改属性的值。直接定义的属性的这个特性默认为 true
* [[Value]]: 包含这个属性的数据值。读取属性值的时候，从这个属性读；写入属性值的时候把新值保存在这个位置

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

* [[Configurable]]: 表示能否通过 delete 删除属性从而  重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。直接定义的属性的这个特性默认为 true
* [[Enumerable]]: 表示能否通过 for-in 循环返回属性。 直接定义的属性的这个特性默认为 true
* [[get]]: 在读取属性的时候调用的函数，默认值为 undefined
* [[set]]: 在写入属性的时候调用的函数，默认值为 undefined

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

---

**创建对象**

* 工厂模式
* 构造函数模式
* 原型模式
* 动态原型模式（通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型）
* 寄生构造函数模式
* 稳妥构造函数模式
