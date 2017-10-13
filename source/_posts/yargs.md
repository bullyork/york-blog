---
title: yargs
date: 2017-09-27 20:05:55
tags: yargs
---

### 引子

仲尼曰：君子中庸；小人反中庸

君子中庸也，君子而士中。小人之反中庸也，小人而无忌惮也。

--《中庸》

### yargs api探索
```javascript
var argv = require('yargs').argv //等价于以下方法
var argv = require('yargs').parse() //此二者用于拿到命令行参数
```
#### .alias(key, alias)

此接口用于使用短符号等价比较长的key
例如：
```javascript
  .alias('i', 'ingredient')
```

#### .argv

取得输入参数，用object表示

#### .array(key)

指定key为数组，例：.array('foo')设置之后，--foo foo bar将会变成['foo','bar']

#### .boolean(key)

指定key为bool值，例：.boolean('foo')设置之后， --foo foo将会变成 true

#### .check(fn, [global=true])

函数fn接受两个参数，argv，options
```javascript
{ _: [], help: false, version: false, foo: 'test', '$0': 'app.js' } // argv
{ help: [], version: [] } // options
```
这个函数用于检测参数传的是不是合法，return 不为true的时候会显示抛出的错误，使用说明，然后退出。
global = true的时候意思是这个应该放到顶层（检查嘛，应该在最开始检查）

#### .choices(key, choices)

给key预定义一些列可取的值，如果这个方法调用了多次，那么列举的值会被merge到一起。选项是一般的字符串或者数字，并且对大小写敏感。例：
```javascript
var argv = require('yargs')
    .choices('i',['peanut-butter', 'jelly', 'banana', 'pickles'])
    .choices('i', ['apple'])
    .parse()
```

另外，.choices()可以使用一个对象携带多个key-choices。例：
```javascript
var argv = require('yargs')
    .choices({
      'j' : ['butter', 'ly', 'ana', 'kles'],
      'i' : ['peanut-butter', 'jelly', 'banana', 'pickles']
    })
    .parse()
```
而且，choices可以设置为option()方法的一个key
```javascript
var argv = require('yargs')
  .option('size', {
    alias: 's',
    describe: 'choose a size',
    choices: ['xs', 's', 'm', 'l', 'xl']
  })
  .argv
```
#### .coerce(key, fn)

这个接口是用来强制转换key的value值的。这个强转函数接受一个参数，也就是命令行中的key值。此函数必须返回一个新的值，或者抛出错误。返回的新值将会作为key的value（或者key的alias的value）。

抛出的错误，可以在`fail()`方法中捕获并打印。

强制转换可以在所有其他修改方法之后应用，比如在`.normalize()`方法之后应用。

例如：
```js
var argv = require('yargs')
  .alias('f', 'file')
  .coerce('file', function (arg) {
      throw new Error('test')
  })
  .argv
```
当然，此方法也可以接受对象参数：
```js
var argv = require('yargs')
  .coerce({
    date: Date.parse,
    json: JSON.parse
  })
  .argv
```
还有，你可以使用一个函数对应好几个key：
```js
var path = require('path')
var argv = require('yargs')
  .coerce(['src', 'dest'], path.resolve)
  .argv
```
如果使用点操作或者数组，强制转换会应用到最终的object上：
```js
// --user.name Batman --user.password 123
// gives us: {name: 'batman', password: '[SECRET]'}
var argv = require('yargs')
  .option('user')
  .coerce('user', opt => {
    opt.name = opt.name.toLowerCase()
    opt.password = '[SECRET]'
    return opt
  })
  .argv
```

#### .command(cmd, desc, [builder], [handler]);.command(cmd, desc, [module]);.command(module)

此用法用于定义应用暴露出来的的命令（默认我们程序命名为app.js）

`cmd`用于定义命令(注意这里要和参数key区分开)，可以为字符串或者字符串数组，当然也可以使用alias，下面有专门叙述。

`desc`用于描述应用提供的所有命令（逐条对应）。定义为false可以生成一个隐藏命令，隐藏命令不会在帮助信息中展示，同时不能在`.completion`中使用