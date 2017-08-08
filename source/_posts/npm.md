---
title: npm
date: 2017-07-17 04:23:06
tags: npm
---
### 引子

子曰：“听讼，吾犹人也。必也使无讼乎！”无情者不得尽其辞。大畏民志，此谓知本。

此谓知本，此谓知之至也。

--《大学》

### 什么是npm

npm：node package manager。npm是js的一个包管理工具，可以让大家方便的共享和重用代码，可以方便的更新自己共享的代码。包含三种含义，[网站](https://www.npmjs.com/)，登记点(包含人们共享包的信息的巨大数据库)，客户端(下载到本地的客户端)。

### 下载nodejs更新npm

- [下载nodejs](https://nodejs.org/en/download/)
- 更新npm：npm install npm@latest -g
- 手工更新npm：可以手动这样下载npm https://registry.npmjs.org/npm/-/npm-{VERSION}.tgz.

### 治理权限问题

全局安装包裹的时候有可能会遇到权限问题，这表明你对npm用来储存全局包和命令的文件夹没有写权限

你可以使用三种方法解决这个问题：
1. 改变npm默认文件夹权限
2. 更换npm默认文件夹
3. 使用包管理器，帮你解决

#### 改变npm默认文件夹权限
```
 sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
```
#### 更换npm默认文件夹
```
1. mkdir ~/.npm-global
2. npm config set prefix '~/.npm-global'
3. export PATH=~/.npm-global/bin:$PATH
4. source ~/.profile
```
#### 使用包管理器，帮你解决
如果你是在mac上第一次下载node，你可以使用homebrew避免这个问题。homebrew会帮你设置为正确的权限
```
brew install node
```
### 本地安装包

- 安装：npm install <package_name>
- 版本：如果没有package.json，安装最新版本。如果有，按package.json中版本号安装
- 使用：安装完成后可以使用require直接使用

### 使用package.json

如你所知，用package.json来管理npm包是最好的方法

- 表明你工程中所使用包的文档
- 可以使用语义化规则控制版本号
- 可以开发可复制的项目，这意味着我们可以很方便的和其他开发者共享

package.json必要字段

最简情况，package.json需要以下字段
- "name"
  - 全部小写
  - 一个单词，没有空格
  - 可以使用破折号和下划线
- "version"
  - 使用x.x.x模式
  - 使用[语义化规则](https://docs.npmjs.com/getting-started/semantic-versioning)
例子：
```
{
  "name": "my-awesome-package",
  "version": "1.0.0"
}
```
创建package.json

>npm init

--yes或-y标志用于快速建立一个默认的package.json，默认如下
- name: 目前目录名
- version: 1.0.0
- description: readme的信息或者空字符串
- main: index.js
- scripts: 空的测试脚本
- keywords: 空
- author: 空
- license: ISC
- bugs: 当前目录，如果有的话
- homepage: 当前目录，如果有的话

> npm set init.author.email "wombat@npmjs.com"
> npm set init.author.name "ag_dubs"
> npm set init.license "MIT"

**注意:如果没有description字段，npm使用readme.md中的第一行。这个描述有助于人们更好的找到你的npm库**

定制init流程

npm首先会到home文件夹下面去寻找配置文件。~/.npm-init.js。

一个简单的.npm-init.js文件如下：
```javascript
module.exports = {
  customField: 'Custom Field',
  otherCustomField: 'This field is really cool'
}
```
在home文件下如果放置了此文件，运行npm init之后，会生成
```javascript
{
  customField: 'Custom Field',
  otherCustomField: 'This field is really cool'
}
```
同时可以使用prompt函数定制问题
```javascript
  module.exports = prompt("what's your favorite flavor of ice cream buddy?", "I LIKE THEM ALL");
```
更多的定制工作，请参考[文档](https://github.com/npm/init-package-json)

具体指明引入的包

- "dependencies": 正式发布时使用的包
- "devDependencies": 开发和测试时使用的包

手工编辑你的package.json

--save dependencies
--save-dev devDependencies

管理依赖版本

使用npm install会下载满足语义化规则的最新版本的依赖

### 更新本地包

在package.json同一目录运行npm update 更新

使用npm outdated来测试更新是否成功

### 卸载本地包

npm uninstall lodash

npm uninstall --save lodash

npm uninstall --save-dev lodash

### 全局安装包

一般来说，命令工具通常可以选择放到全局，模块包通常放到本地
npm install -g jshint

### 更新全局包

npm update -g jshint

找到那些包需要更新： npm outdated -g --depth=0

npm 2.6.1版本之前，这个脚本会更新掉所有过期的包

### 卸载全局包

npm uninstall -g jshint

### 制作Node.js模块

Node.js模块是一种可以发布到npm上面去的包，可以从创建package.json文件开始，创建模块。

可以使用npm init生成package.json。它会引导你填写一些信息。两个必填字段是version和name。当然需要一个入口main，可以使用默认的index.js

如果你想要在author域添加信息，你可以使用下面的格式（email和网站都是可选的）

名字<邮件地址>(网站地址)

一旦你的package.json文件创建了，你会想要创建当模块被require的时候，被加载的文件。如果你想用设置，那么就是index.js

在入口文件，添加一个函数作为 exports对象的属性，这会让这个函数可以被其他代码调用

```
exports.printMsg = function() {
  console.log('This is a message from the demo package');
}
```

测试：
1. 发布你的包
2. 在你的工程外面创建一个新的目录并进去
3. 运行命令 npm install 你的包名
4. 创建一个test.js文件，加载你的模块，并调用模块中的函数
5. 运行node test.js。信息应该会被输出

 ### 发布npm包

 你可以在任意含有package.json文件的目录中发布。

 ### 语义化版本控制和npm

发布的语义化

如果一个工程要和其他人共享，开始的版本号应该是1.0.0，虽然有些npm工程不符合这个规则。

在此之后，改动应该遵守如下规则：

- bug修复和其他小的改动：补丁发布，增加版本号最后一位，例如：1.0.1
- 新特性但是不影响旧的特性：较小发布，增加版本号中间一位，例如：1.1.0
- 改动不向后兼容：主要发布，增加版本号中的第一位，例如：2.0.0

### 使用域包

域类似于npm模块的命名空间。如果包的名字是以@开始的，那么它就是一个域包。域名就是在@和斜线之间的词。

@scope/project-name ： 所有的npm用户拥有自己的域
@username/project-name： 深入了解域请看文档 [CLI documentation](https://docs.npmjs.com/misc/scope#publishing-public-scoped-packages-to-the-public-npm-registry)

更新和登录npm

你需要版本号大于2.7.0的npm，而且第一次使用域npm模块需要登录

```
sudo npm install -g npm
npm login
```

初始化一个域包

创建一个域包，只需要简单的使用你的域名开始的字段命名name即可

```
{
  "name": "@username/project-name"
}
```

如果使用npm init， 可以增加条件选项
```
npm init --scope=username
```
如果你一直使用相同的域配置，最好在.npmrc文件做一个配置

发布一个域包

域包一般是私有的，你想要发布的话，首先要成为一个[私有模块](https://www.npmjs.com/private-modules)用户

然而，公有的域模块是免费的。发布公有的域模块，要设置access选项，之后所有的发布默认携带此选项

```
npm publish --access=public
```
使用域包

和使用一般的包一样

### 使用标签

标签是[语义化版本控制](http://semver.org/)的一个补充，目的是标记不同版本的包。为了让人类更好读懂，标签可以使发布者有效的发布自己的包

### 增加标签

自己的特定版本包增加tag，使用 
```
npm dist-tag add <pkg>@<version> [<tag>]
```
更多信息，请查看文档 [cli docs](https://docs.npmjs.com/cli/dist-tag)

发布时携带标签

默认地，**npm publish** 会将你的包打上 **latest** 标签。如果你使用 **--tag** 标志，你可以选择使用其它标签。例如：
```
npm publish --tag beta
```
加载携带标签的包

同发布一样，默认使用 **latest** 标签。要覆盖这个默认行为，使用
```
npm install <pkg>@<tag>
```
例如：
```
npm install somepkg@beta
```
警告

因为dist-tag和semver使用相同的命名空间，要避免使用哪些容易导致冲突的标签名。最佳实践是避免使用以数字或v开头的标签
