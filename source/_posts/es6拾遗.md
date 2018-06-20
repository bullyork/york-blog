---
title: es6拾遗
date: 2018-05-29 13:58:52
tags: es6
---

**super 关键字**

super 这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。

super 虽然代表了父类 A 的构造函数，但是返回的是子类 B 的实例，即 super 内部的 this 指的是 B，因此 super()在这里相当于 A.prototype.constructor.call(this)。

```js
class A {
  constructor() {
    console.log(new.target.name);
  }
}

class B extends A {
  constructor() {
    super();
  }
}
new A(); //A
new B(); //B
```

第二种情况，super 作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。

> 加上 static 关键字的方法表示不会被实例继承，而是直接通过类来调用
