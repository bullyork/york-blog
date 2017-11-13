---
title: nodejs学习基础内容
date: 2017-11-02 14:26:00
tags: nodejs
---

### 引子

其次致曲。曲能有诚。诚则形。形则著。著则明。明则动。动则变。变则化。唯天下至诚为能化。

--《中庸》

### 正文

#### Nodejs基础

运行在nodejs环境中的js作用：操作磁盘文件，搭建http服务，Nodejs提供了`fs`,`http`等内置对象

安装，运行，权限问题略

**模块**

*require*

require函数用于在当前模块中加载和使用别的模块，传入一个模块名，返回一个模块导出对象。模块名可以使用相对路径和绝对路径。另外，模块名中的.js扩展名可以省略。另外，还可以加载和使用一个JSON文件。

*exports*

`exports`对象是当前模块的导出对象，用于导出模块公有方法和属性。别的模块通过`require`函数使用当前模块时得到的就是当前模块的`exports`对象(只可以在`exports`对象上添加属性，不能对它直接赋值，赋值不生效)

*module*

module对象可以访问到当前模块的一些相关信息，但主要用途是替换当前模块的导出对象。
```js
module.exports = function () {
    console.log('Hello World!');
};
```
*模块初始化*

一个模块中的js代码仅在模块第一次被使用时执行一次，并在执行过程中初始化模块的导出对象。之后，缓存起来的到处对象被重复利用。

*主模块*

通过命令行参数传给Nodejs以启动程序的模块被称为主模块。

*二进制模块*

Nodejs支持使用二进制模块。编译好的二进制模块文件扩展名为.node，使用方法同js模块一致。

#### 代码的组织和部署

**模块路径解析规则**

require函数支持相对路径和绝对路径。但这两种路径在模块之间建立了强耦合关系，位置变换会变得很麻烦。所以，`require`函数支持第三种形式的路径，写法类似于`foo/bar`，并以此按照以下规则解析路径，直到找到模块位置。
1. 内置模块，如`require('fs')`
2. node_modules目录：nodejs定义了一个特殊的`node_modules`文件夹
3. NODE\_PATH 环境变量：与PATH环境变量类似，NODE\_PATH环境变量中包含一到多个目录路径，路径之间在linux下用`:`分割，在windows下使用`;`分离。
例如：
```
NODE\_PATH = /home/user/lib:/home/lib
```
使用require('foo/bar')的方式加载模块时，则NodeJS尝试以下路径。
```
/home/user/lib/foo/bar
/home/lib/foo/bar
```
**包（package）**

复杂的模块往往由多个子模块组成。为了便于管理和使用，我们可以把多个子模块组成的大模块称为`包`，并把所有子模块放在同一个目录里。

在组成一个包的所有子模块中，需要有一个入口模块，入口模块的导出对象被称作包的导出对象。例如有以下目录结构。
```
- /home/user/lib/
    - cat/
        head.js
        body.js
        main.js
```
其中`cat`目录定义了一个包，其中包含三个子模块。`main.js`作为入口模块，其内容如下：
```js
var head = require('./head');
var body = require('./body');

exports.create = function(name){
    return {
        name: name,
        head: head.create(),
        body: body.create()
    }
}
```
在其他模块里使用包的时候，需要加载包的入口模块。接着上例，使用`require('/home/user/lib/cat/main')`能达到目的，但是入口模块名称出现在路径里看上去不是个好主意。因此我们需要做点额外的工作，让包使用起来更像是单个模块。

*index.js*

当模块的文件名是`index.js`，加载模块时，可以使用模块所在目录的路径代替模块的文件路径，因此接着上例，以下两条语句等价。

```
 var cat = require('/home/user/lib/cat');
 var cat = require('/home/user/lib/cat/index');
```
这样处理后，就只需要把包目录路径传递给`require`函数，感觉上整个目录被当作单个模块使用，更有整体感。

*package.json*

如果想自定义入口模块的文件名和存放位置，就需要在包目录下包含一个`package.json`文件，并在其中指定入口模块的路径。上例中的`cat`模块可以重构出来。
```
- /home/user/lib/
    - cat/
        + doc/
        - lib/
            head.js
            body.js
            main.js
        + tests/
        package.json
```
其中`package.json`内容如下
```
{
    "name": "cat",
    "main": "./lib/main.js"
}
```
如此一来，就同样可以使用`require('/home/user/lib/cat')`的方式加载模块。NodeJS会根据包目录下的`package.json`找到入口模块的位置。

#### 命令行程序

使用Nodejs编写的东西，要么是一个包，要么是一个命令行程序，而前者最终也会用于开发后者。因此我们在部署代码的时候需要一些技巧，让用户觉得自己是在使用一个命令行程序。

例如我们用NodeJS写了个程序，可以把命令行参数原样打印出来。该程序很简单，在主模块内实现了所有功能。并且写好后，我们把程序部署在`/home/user/bin/node-echo.js`这个位置。为了在任何目录下都能运行该程序，我们需要使用以下终端命令。
```
$ node /home/user/bin/node-echo.js Hello World
Hello World
```
这种方式看起来不怎么像我们使用的命令行程序，下面才是我们期望的方式。
```
$ node-echo Hello World
```
*linux*

1. shell脚本中可以通过 `#!` 注释来制定当前脚本使用的解析器。
    ```
     #! /usr/bin/env node
    ```
2. 然后赋予`node-echo.js`文件执行权限。
    ```
     $ chmod +x /home/user/bin/node-echo.js
    ```
3. 然后在PATH环境变量指定的某个目录下，例如在`/usr/local/bin`下边创建一个软链文件，文件名与我们希望使用的终端命令同名，命令如下
    ```
     $ sudo ln -s /home/user/bin/node-echo.js /usr/local/bin/node-echo
    ```
*windows*

假设`node-echo.js`存放在`C:\Users\user\bin`目录，并且该目录已经添加到PATH环境变量里了。接下来需要在该目录下新建一个名为`node-echo.cmd`的文件，文件内容如下：
    ```
    @node "C:\User\user\bin\node-echo.js" %*
    ```
这样处理后，我们就可以在任何目录下使用`node-echo`命令了。

#### 工程目录

以编写命令行程序为例，一般我们会同时提供命令行模式和api模式两种使用方式，并且我们会借助第三方包来编写代码。除了代码外，一个完整的程序也应该有自己的文档和测试用例。因此，一个标准的工程目录都看起来像下面这样。
```
- /home/user/workspace/node-echo/   # 工程目录
    - bin/                          # 存放命令行相关代码
        node-echo
    + doc/                          # 存放文档
    - lib/                          # 存放API相关代码
        echo.js
    - node_modules/                 # 存放三方包
        + argv/
    + tests/                        # 存放测试用例
    package.json                    # 元数据文件
    README.md                       # 说明文件
```

#### NPM

略

#### 小结

- 编写代码前先规划好目录结构，才能做到有条不紊。
- 稍大点的程序可以将代码拆分为多个模块管理，更大些的程序可以使用包来组织模块
- 合理使用`node_modules`和`NODE_PATH`来解耦包的使用方式和物理路径
- 使用NPM加入Nodejs生态圈互通有无
- 先到了心仪的报名时请提前在npm上抢注





