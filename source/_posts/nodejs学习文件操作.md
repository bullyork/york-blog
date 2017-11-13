---
title: nodejs学习文件操作
date: 2017-11-10 17:23:16
tags: nodejs
---
### 引子

至诚之道可以前知。国家将兴，必有祯祥；国家将亡，必有妖孽。见乎蓍龟，动乎四体。祸福将至，善必先知之；不善，必先知之。故至诚如神。

--《中庸》

### 文件操作

#### 两个例子
*小文件拷贝*

```
var fs = require('fs');

function copy(src, dst) {
    fs.writeFileSync(dst, fs.readFileSync(src));
}

function main(argv) {
    copy(argv[0], argv[1]);
}

main(process.argv.slice(2));
```

*大文件拷贝*

```
var fs = require('fs');

function copy(src, dst) {
    fs.createReadStream(src).pipe(fs.createWriteStream(dst));
}

function main(argv) {
    copy(argv[0], argv[1]);
}

main(process.argv.slice(2));
```
> process是一个全局变量，可以通过process.argv获得命令行参数。argv[0]固定等于NodeJS执行程序的绝对路径，argv[1]固定等于主模块的绝对路径。

#### API

*Buffer*

js语言只有字符串数据类型，没有二进制数据类型，因此Nodejs提供了一个与`String`对等的全局构造函数`Buffer`来提供对二进制数据的操作。除了可以读取文件得到`Buffer`的实例外，还能够直接构造，例如：
```
var bin = new Buffer([ 0x68, 0x65, 0x6c, 0x6c, 0x6f ]);
```
`Buffer`实例与字符串类似，除了可以用`.length`属性得到字节长度外，还可以用`[index]`方式读取指定位置的字节，例如：
```
bin[0]; // => 0x68;
```
`Buffer`与字符串能够互相转化，例如可以使用指定编码将二进制数据转化为字符串：
```
var str = bin.toString('utf-8);
```
或者反过来，将字符串转换为指定编码下的二进制数据：
```
var bin = new Buffer('hello', 'utf-8'); // => <Buffer 68 65 6c 6c 6f>
```
`Buffer`与字符串有一个重要区别。字符串是只读的，并且对字符串的任何修改得到的都是一个新字符串，原字符串保持不变。至于`Buffer`，更像是可以做指针操作的C语言数组。例如，可以用`[index]`方式直接修改某个位置的字节。
```
bin[0] = 0x48
```
而`.slice`方法也不是返回一个新的`Buffer`，而更像是返回了指向原`Buffer`中间的某个位置的指针，如下所示。
```
[ 0x68, 0x65, 0x6c, 0x6c, 0x6f ]
    ^           ^
    |           |
   bin     bin.slice(2)
```
因此对`.slice`方法返回的`Buffer`的修改会作用于原`Buffer`，例如：
```js
var bin = new Buffer([0x68, 0x65, 0x6c, 0x6f]);
var sub = bin.slice(2);

sub[0] = 0x65;
console.log(bin); // => <Buffer 68 65 65 6c 6f>
```
也因此，如果想要拷贝一份`Buffer`，得首先创建一个新的`Buffer`，并通过`.copy`方法把原`Buffer`中的数据复制过去。以下是一个例子。
```js
var bin = new Buffer([0x68, 0x65, 0x6c, 0x6c, 0x6c]);
var dup = new Buffer(bing.length)
bin.copy(dup)
dup[0] = 0x48;
console.log(bin); // => <Buffer 68 65 6c 6c 6f>
console.log(dup); // => <Buffer 48 65 65 6c 6f>
```
总之，`Buffer`将js的数据处理能力从字符串扩展到了任意二进制数据。

`Stream`

当内存中无法一次装下需要处理的数据时，或者一边读取一边处理更加高效时，我们就需要用到数据流。NodeJS中通过各种`Stream`来提供对数据流的操作。

以上面的大文件拷贝程序为例，我们可以为数据创建一个只读数据流，示例如下：
```js
var rs = fs.createReadStream(pathname);

rs.on('data', function(chunk){
  doSomeThing(chunk);
})

rs.on('end', function(){
  cleanUp();
})；
```
> `Stream`基于事件机制工作，所有`Stream`的实例都继承于NodeJS提供的`EventEmitter`

上面代码中的`data`事件会源源不断地被触发，不管`doSomething`函数是否处理的过来。代码可以继续如下改造，以解决这个问题。
```js
var rs = fs.createReadStream(src);

rs.on('data', function(chunk){
  res.pause();
  doSomething(chunk, function(){
    res.resume();
  })
});

rs.on('end', function(){
  cleanUp();
})
```
以上代码给`doSomething`函数加上了回调，因此我们可以在处理数据前暂停数据读取，并在处理数据后继续读取数据。

此外，我们也可以为数据目标创建一个只写数据流，示例如下：
```js
var rs = fs.createReadStream(src);
var ws = fs.createWriteStream(dst);

rs.on('data', function(chunk){
  ws.write(chunk);
});

rs.on('end', function(){
  ws.end();
})
```
我们把`doSomething`换成了往只写数据流里写入数据后，以上代码看起来就像是一个文件拷贝程序了。但是以上代码存在上面提到的问题，如果写入速度跟不上读取速度的话，只写数据流内部的缓存会爆仓。我们可以根据`.write`方法的返回值来判断传入的数据是写入目标了，还是临时放在了缓存，并根据`drain`事件来判断什么时候只写数据流已经将缓存中的数据写入目标，可以传入下一个待写数据了。因此代码可以改造如下：
```js
var rs = fs.createReadStream(src);
var ws = fs.createWriteStream(dst);
rs.on('data', function(chunk){
  if(ws.write(chunk) === false){
    rs.pause()
  }
});

rs.on('end', function(){
  ws.end();
})

ws.on('drain', function(){
  res.resume();
});
```
以上代码实现了数据从只读数据流到只写数据流的搬运，并包括了防爆仓控制。因为这种使用场景很多，例如上边的大文件拷贝程序，NodeJS直接提供了`.pipe`方法来做这件事，其内部实现的方式与上面的代码类似

*File System*

NodeJS通过`fs`内置模块提供对文件的操作。`fs`模块提供的API基本上可以分为以下三类：
- 文件属性读写：`fs.stat`、`fs.chmod`、`fs.chown`等
- 文件内容读写： `fs.readFile`、`fs.readdir`、`fs.writeFile`、`fs.mkdir`等等
- 底层文件操作：`fs.open`、`fs.read`、`fs.write`、`fs.close`等等

NodeJS最精华的异步IO模型在`fs`模块里有着充分的体现，例如上边提到的这些API都通过回调函数传递结果。以`fs.readFile`为例：
```js
fs.readFile(pathname, function(err, data){
  if(err){
    // Deal with error.
  } else {
    // Deal with data.
  }
});
```
如上面代码所示，基本上所有的`fs`模块API的回调参数都有两个。第一个参数在有错误发生时等于异常对象，第二个参数始终用于返回API方法执行结果。
此外，`fs`模块的所有一步API都有对应的同步版本，用于无法使用异步操作时，或者同步操作更方便时的情况。同步API除了方法名的末尾多了一个`Sync`之外，异常对象与执行结果的传递方式也有相应变化。同样以`fs.readFileSync`为例：
```js
try {
  var data = fs.readFileSync(pathname);
  // deal with data
} catch (err) {
  // deal with error
}
```
`fs`模块提供的API很多，需要时自行查阅文档。

*Path*

操作文件时难免不与文件路径打交道。NodeJS提供了path内置模块来简化路径相关操作，并提升代码可读性。以下分别介绍几个常用的API。

- path.normalize
  将传入路径转换为标准路径，具体讲的话，出了解析路径中的`.`与`..`外，还能去掉多余的斜杠。如果有程序需要使用路径作某些数据的索引，但又允许用户随意输入路径时，就需要使用该方法保证路径的唯一性。以下是一个例子：
  ```js
  var path = require('path')
  var cache = {}

  function store(key, value){
    cache[path.normalize(key)] = value
  }

  store('foo/bar', 1)
  store('foo//baz//../bar', 2)
  console.log(cache)
  ```
  > 标准化之后的路径里的斜杠在Windows系统下是`\`，而在linux系统下是`/`。如果想保证任何系统夏使用`/`作为路径分隔符的话，需要用`.replace(/\\/g, '/')` 再替换一下标准路径。

- path.join
  将传入的多个路径拼接为标准路径。该方法可避免手工拼接字符串的繁琐，并且能在不同系统下正确使用相应的路径分隔符。以下是一个例子：
  ```js
  path.join('foo/', 'baz/', '../bar'); // => 'foo/bar'
  ```
- path.extname
  当我们需要根据不同文件扩展明做不同的工作时，该方法就显得很好用。以下是一个例子：
  ```js
  path.extname('foo/bar.js'); // => '.js'
  ```
  其余方法请参考官方文档

#### 遍历目录
遍历目录是操作文件时的一个常见需求。比如写了一个程序，需要找到并处理指定目录下的所有js文件时，就需要遍历挣个目录。

*递归算法*

遍历目录时一般使用递归算法，否则就难以写出简洁的编码。递归算法与数学归纳法类似。通过不断缩小问题的规模来解决问题。例：
```js
function factorial(n){
  if(n === 1){
    return 1
  } else {
    return n * factorial(n-1)
  }
}
```
上面的函数用于计算N的阶乘。可以看到，当N大于1时，问题简化为计算N乘以N-1的阶乘。当N等于1时，问题达到最小规模，不需要再简化，因此直接返回1。
> 陷阱：使用递归算法编写的代码虽然简洁，但由于每递归一次就产生一次函数调用，在需要优先考虑性能时，需要把递归算法转换为循环算法，以减少函数调用次数。
*遍历算法*

目录是一个树状结构，在遍历时一般使用深度优先+先序遍历算法。