---
title: create-react-app使用
date: 2018-07-16 20:23:17
tags: create-react-app
---
## 引子

### 武陵春·春晚

宋：李清照

风住尘香花已尽，日晚倦梳头。物是人非事事休，欲语泪先流。   
闻说双溪春尚好，也拟泛轻舟。只恐双溪舴艋舟，载不动许多愁。

## 正文

### 更新新版本

Create React App主要分为两部分：
- create-react-app: 全局命令行用于创建新的项目
- react-scripts: 生成项目中的依赖

`create-react-app`几乎永远不用更新，因为所有的配置其实都在`react-scripts`

`create-react-app`每次生成新项目的时候，总会用最新版本的`react-scripts`，所以创建的app必然能够获得所有最新的特性

如果要更新一个现有工程的`react-scripts`，打开[更新日志](https://github.com/facebook/create-react-app/blob/master/CHANGELOG.md)，然后找到现在的版本(如果不确定，看下package.json)，然后根据迁移指南升级。

大部分情况，更新`package.json`中的`react-scripts`版本，然后运行`npm install`即可，但是为了有可能存在的大版本更新，还是要看下[更新日志](https://github.com/facebook/create-react-app/blob/master/CHANGELOG.md)，然后找到现在的版本(如果不确定，看下package.json)，然后根据迁移指南升级。

维护者：我们会尽可能保持更新尽可能小，这样可以保证用户升级`react-scripts`是无痛的。

### 反馈

维护者：我们对于反馈，一直是非常开放的心态，来[反馈](https://github.com/facebook/create-react-app/issues)吧

### 目录结构

创建完之后，工程目录结构如下：
```
my-app/
  README.md
  node_modules/
  package.json
  public/
    index.html
    favicon.ico
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
```
在创建的这个工程中，**以下文件必须是固定的文件名**

- `public/index.html` 是页面模版
- `src/index.js` 是js入口

其他的文件可以酌情删除或修改   

可以在`src`内部创建子目录。为了快速的rebuild，只有在`src`目录才会被webpack处理。所以需要 **把所有css和js文件放到`src`目录下**，否则webpack会忽略掉。   

只有在`public`文件夹下面的文件可以被`public/index.html`使用   
在js和html中使用资源请阅读下面的说明   

其实可以创建更多顶层目录。   
这些文件不会在被正式编译覆盖，所以可以用来放一些像文档之类的东东

### 可用的一些脚本

`npm start`

在development模式下运行，打开 http://localhost:3000 在浏览器中查看

当你编辑并保存的时候，这个页面会自动重载。你还可以在console中看到一些lint的错误

`npm test`

在观测模式下发起测试，更多信息请参考 [运行测试](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#running-tests)

`npm run build`

构建正式环境的app，并放置于`build`文件夹   
这个操作会在正式环境打包react，并优化构建至最优表现   

构建会压缩文件并且文件名包含内容哈希   

参考[发布资料](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#deployment)查看更多信息

`npm run eject`

> 声明：这是一个单项操作。一旦`eject`之后，不能回去

如果你对默认的配置选项不满意，可以在任何时间`eject`。这个命令会移除项目中的单向依赖。   

另外，`eject`会复制所有的配置文件并且迁移所有依赖(webpack，babel，eslint等)到项目中，这样你可以自主修改，覆盖这些东东的默认配置。所有的命令，除了`eject`，依然可以使用，但是他们会指到拷贝出来的脚本，所以你可以更改它们。在这一点上，你完全随意。

你甚至可以完全不用这个命令。一些辅助的特性能解决大部分中小型项目配置需求，所以你不是必须要用这个特性。另外，如果你不曾准备好要定制化，这个命令将很鸡肋。

### 支持的浏览器

默认情况，生成的工程使用最新版本React

支持的浏览器，要看[react相关文档](https://reactjs.org/docs/react-dom.html#browser-support)

### 支持的语言特性和polyfills

此工程支持最新js标准的一个超集。   
除了ES6预发特性外，还支持：
- [Exponentiation Operator](https://github.com/rwaldron/exponentiation-operator) (ES2016).
- [Async/await](https://github.com/tc39/ecmascript-asyncawait) (ES2017).
- [Object Rest/Spread Properties](https://github.com/tc39/proposal-object-rest-spread) (stage 3 proposal).
- [Dynamic import()](https://github.com/tc39/proposal-dynamic-import) (stage 3 proposal)
- [Class Fields and Static Properties](https://github.com/tc39/proposal-class-public-fields) (part of stage 3 proposal).
- [jsx](https://reactjs.org/docs/introducing-jsx.html) 和 [flow](https://flow.org/)

学习更多请查看[各种阶段性提议](https://babeljs.io/docs/en/plugins/#presets-stage-x-experimental-presets-)

鉴于我们推荐谨慎地使用实验性的建议，Facebook在生产环境重度使用这些代码，所以我们打算提供[编码范式](https://medium.com/@cpojer/effective-javascript-codemods-5a6686bb46fb)，如果这些提议在将来更改的话。

请清楚一点，**这个项目仅仅包含很少一部分ES6** [polyfills](https://en.wikipedia.org/wiki/Polyfill):

- [Object.assign()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 基于 [object-assign](https://github.com/sindresorhus/object-assign)
- [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) 基于 [promise](https://github.com/then/promise)
- [fetch()](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 基于 [whatwg-fetch](https://github.com/github/fetch)

如果你是用其他ES6+的特性需要 **runtime支持** (such as Array.from() or Symbol), 确定你已经手动包含了相应的polyfills，或者你要运行的浏览器已然支持这些特性

### 编辑器语法高亮

要在你喜爱的编辑器中配置语法高亮，请参照[babel的相关文档](https://babeljs.io/docs/en/editors/)并且遵循相应的指引，文档指引包含了好几个流行的编辑器。

### 在编辑器中展示lint

> 注意：这个特性依赖`react-scripts#0.2.0`以上和npm 3以上

一些编辑器，Sublime Text，Atom，和Visual Studio Code，提供ESLint插件。

它们本省并不需要linting. 你可以在你的终端和浏览器console中看到linter。但是，如果你更喜欢在编辑器内部看到lint结果，你需要一些额外的步骤。

你需要为你的编辑器安装一个ESLint插件。然后在根目录添加一个`.eslintrc`文件：
```js
{
  "extends": "react-app"
}
```
现在你的编辑器可以看到linting 警告了

要说明一点，即使你将来编辑了你的`eslintrc`，终端和编辑器console中的lint不会变。因为Create React App有意提供一个mini规则集，用于发现一些常见错误。

如果你希望project强制使用一个编码锋哥，考虑使用[Prettier](https://github.com/prettier/prettier)取代ESLint规则。

### 在编辑器中调试

**这个特性目前仅仅被[Visual Studio Code](https://code.visualstudio.com/)和[WebStorm](https://www.jetbrains.com/webstorm/)支持**

#### Visual Studio Code

你需要最新版本的[VS code](https://code.visualstudio.com/)和VS code [chrome调试插件](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)

然后添加下面的代码到你的`launch.json`文件并且将它放到`.vscode`文件夹，在你的app根目录中。

```js
{
  "version": "0.2.0",
  "configurations": [{
    "name": "Chrome",
    "type": "chrome",
    "request": "launch",
    "url": "http://localhost:3000",
    "webRoot": "${workspaceRoot}/src",
    "sourceMapPathOverrides": {
      "webpack:///src/*": "${webRoot}/*"
    }
  }]
}
```
> 注意：如果你更改了[HOST或者PORT环境变量](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#folder-structure)，URL的值会和上面的不同

运行`npm start`启用你的app，按`F5`或者点击绿色的调试图标，开始调试。你现在可以写代码，设断点，调试你新写的代码 —— 全部由编辑器自动处理。

#### WebStorm

略

### 自动格式化代码

Prettier是一个主动的代码格式化工具，支持js，css和JSON。使用Prettier可以自动格式化代码，保证工程内部代码格式一致。查看更多，请到[Prettier github 官网](https://github.com/prettier/prettier)，要看具体介绍，请到[Prettier 官网](https://prettier.io/)

如果想要当我们git commit的时候就format代码，我们需要装载下面的依赖：
```bash
npm install --save husky lint-staged prettier
```
当然，也可以用yarn
```bash
yarn add husky lint-staged prettier
```
- `husky` 使通过npm脚本使用githooks更加简单
- `lint-staged` 允许我们对git 暂存区资料运行脚本。参考[学习使用lint-staged的blog](yarn add husky lint-staged prettier)
- `prettier`是一个js格式化工具，我们会在commits之前运行

现在我们可以确保所有文件会被正确的格式化，当我们在工程根目录的`package.json`中添加几行之后。

将下面的代码添加到`scripts`区域:
```diff
  "scripts": {
+   "precommit": "lint-staged",
    "start": "react-scripts start",
    "build": "react-scripts build",
```
下面我们添加一个`lint-staged`区域到`package.json`中，例如：
```diff
  "dependencies": {
    // ...
  },
+ "lint-staged": {
+   "src/**/*.{js,jsx,json,css}": [
+     "prettier --single-quote --write",
+     "git add"
+   ]
+ },
  "scripts": {
```
现在，当你提交了一个commit，Prettier将会自动格式化变化的文件。第一次你还可以运行`./node_modules/.bin/prettier --single-quote --write "src/**/*.{js,jsx,json,css}"`来格式化整个项目

下一步你可能想要将Prettier整合到你喜欢的编辑器。请阅读[prettier编辑器接入](https://prettier.io/docs/en/editors.html)

### 更新页面的`<title>`

你可以在生成工程的`public`文件夹找到HTML文件，然后编辑`<title>`标签，更新成你想要的。

注意一般情况下不需要频繁编辑public中的文件。例如，[添加一个样式表](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-a-stylesheet)不需要编辑public中的HTML就可以做到

如果你需要基于内容动态更新页面标题，可以使用浏览器`document.title`API。或者更复杂的情景，当你想要根据React组件更新标题的时候，你可以使用[React Helmet](https://github.com/nfl/react-helmet)，一个第三方库

如果你再正式环境使用定制化服务并且想要页面在发送到浏览器之前更改标题，你可以遵循[这个地方](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#generating-dynamic-meta-tags-on-the-server)的建议。另外，可以预编译每个页面至静态html文件，参考资料在[这里](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#pre-rendering-into-static-html-files)

### 装载依赖

生成的项目默认包含React和ReactDOM依赖。当然也包含一系列Create React App开发幻境用到的一系列依赖。你使用可以装在其他依赖（例如：React Router）：
```bash
npm install --save react-router
```
当然还可以用yarn：
```bash
yarn add react-router
```
这种方式适用于任何库，不仅仅是`react-router`

### 引入组件

幸亏有Babel，这个项目支持ES6模块。

同时，你依然可以使用`require()`和`module.exports`，但我们建议你用[import 和 export](http://exploringjs.com/es6/ch_modules.html)取代它。

例如：
#### `Button.js`
```js
import React, { Component } from 'react';

class Button extends Component {
  render() {
    // ...
  }
}

export default Button; // Don’t forget to use export default!
```
#### `DangerButton.js`
```js
import React, { Component } from 'react';
import Button from './Button'; // Import a component from another file

class DangerButton extends Component {
  render() {
    return <Button color="red" />;
  }
}

export default DangerButton;
```
请认清 [default和命名的exports之间的区别](https://stackoverflow.com/questions/36795819/when-should-i-use-curly-braces-for-es6-import/36796281#36796281)。这是常见的导致错误的根源。

我们建议，当exports仅仅一个东东的时候，使用default imports和exports（例如，一个组件）。

当一个实例需要export几个函数的时候，命名的exports就非常有用了。一个模块可以有至多一个default export和任意数量的命名export

学习更多ES6模块化：
- [什么时候使用大括号](https://stackoverflow.com/questions/36795819/when-should-i-use-curly-braces-for-es6-import/36796281#36796281)
- [探索ES6: 模块](http://exploringjs.com/es6/ch_modules.html)
- [理解ES6: 模块](https://leanpub.com/understandinges6/read#leanpub-auto-encapsulating-code-with-modules)

### 代码拆分

区别于每次加载全部的app代码之后，用户才能使用，代码拆分允许你将代码拆成小的代码块，按需加载

示例如下：

**moduleA.js**
```js
const moduleA = `Hello`;

export { moduleA }
```
**App.js**
```js
import React, { Component } from 'react';

class App extends Component {
  handleClick = () => {
    import('./moduleA')
      .then(({ moduleA }) => {
        // Use moduleA
      })
      .catch(err => {
        // Handle failure
      });
  };

  render() {
    return (
      <div>
        <button onClick={this.handleClick}>Load</button>
      </div>
    );
  }
}

export default App;
```
这种写法，只有在用户点击‘load’button的时候，才会加载`moduleA.js`

你还可以使用async/await 语法，如果你喜欢用的话

#### 结合React Router

如果你使用React Router进行代码拆分，请参照[这个教程](http://serverless-stack.com/chapters/code-splitting-in-create-react-app.html)。还有相关的[github repo](https://github.com/AnomalyInnovations/serverless-stack-demo-client/tree/code-splitting-in-create-react-app)

还有，看一下React文档中关于[代码区分的部分](https://reactjs.org/docs/code-splitting.html)

### 添加样式表

这个项目使用[Webpack](https://webpack.js.org/)处理所有的样式。Webpack提供了一个定制化的方法，拓展了`import`概念，可以引入css。为了表明一个js依赖一个css，你需要 **在js中import相应的css**

**Button.css**
```css
.Button {
  padding: 20px;
}
```
**Button.js**
```js
import React, { Component } from 'react';
import './Button.css'; // Tell Webpack that Button.js uses these styles

class Button extends Component {
  render() {
    // You can use them as regular CSS styles
    return <div className="Button" />;
  }
}
```
**这不是一个React必须的模式** 但是许多人发现这种特性非常方便。但是你需要了解到，这样做会让你的代码在使用其他打包工具的时候，不是那么轻便。

在测试环境，使用这种方法表示依赖还会让你的样式更改，能飞快的通过reload来更新。在正式环境，所有的css文件会被结合成一个压缩过的`.css`文件

如果你担心使用webpack特殊语义不太好，可以把你所有的css放到`src/index.css`文件。这样只会被`src/index.js`引入，当你迁移其他打包工具的话，很简单。

### css后处理

这个项目压缩css并且通过[Autoprefixer](https://github.com/postcss/autoprefixer)自动添加前缀，所以你不必担心这些东东
例如：
```css
.App {
  display: flex;
  flex-direction: row;
  align-items: center;
}
```
```css
.App {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
      -ms-flex-direction: row;
          flex-direction: row;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
}
```
如果你想禁掉自动添加前缀，请参考[此处](https://github.com/postcss/autoprefixer#disabling)

### 添加css预处理（Sass，Less等）

通常来说，我们不推荐你在不同的组件中复用相同的class。例如，与其在`<AcceptButton>`和`<RejectButton>`中使用一个`.Button`css类，我们更推荐创建一个`<Button/>`组件，拥有`.Button`css类，这样可以在`<AcceptButton>`和`<RejectButton>`中render(但[不要继承](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#post-processing-css))

遵循以上规则，有时会让css预处理变得没多大用，主要是mixin和nesting被组件组合取代了。当然，如果你觉得预处理有用的话，可以完整的引入一个预处理。我们下面会使用sass演示，当然你可以使用less或其他类似的东东。

首先，命令行装载一个sass的依赖：
```bash
npm install --save node-sass-chokidar
```
当然也可以用yarn啦
```bash
yarn add node-sass-chokidar
```
然后在`package.json`中，把下面的几行加到`scripts`：
```diff

   "scripts": {
+    "build-css": "node-sass-chokidar src/ -o src/",
+    "watch-css": "npm run build-css && node-sass-chokidar src/ -o src/ --watch --recursive",
     "start": "react-scripts start",
     "build": "react-scripts build",
     "test": "react-scripts test --env=jsdom",
```
> 注意：用其他预处理的时候，将`watch-css`和`build-css`命令替换相应预处理文档中描述的命令

现在你可以重命名`src/App.css`至`src/App.scss`，并运行`npm run watch-css`。监控器会监测src文件夹下面所有的sass文件，并紧挨着生成相应的css文件，在我们的例子里面就会覆盖原来的`src/App.css`。由于`src/App.js`引入的依然是`src/App.css`，所以样式依然会成为app的一部分。你现在可以编辑`src/App.scss`，然后`src/App.css`会自动更新。

想要在不同的sass文件间公用变量，你可以使用Sass imports。例如：`src/App.scss`和其他组件样式可以引入`@import "./shared.scss"`；然后可以公用其中的变量定义。

为了使用绝对路径引入文件，你可以在`package.json`的命令中加入`--include-path`选项。   
```js
"build-css": "node-sass-chokidar --include-path ./src --include-path ./node_modules src/ -o src/",
"watch-css": "npm run build-css && node-sass-chokidar --include-path ./src --include-path ./node_modules src/ -o src/ --watch --recursive",
```
这样可以让你像下面那样引入样式
```scss
@import 'styles/_colors.scss'; // assuming a styles directory under src/
@import 'nprogress/nprogress'; // importing a css file from the nprogress node module
```
这时候你可能想要让git忽略所有css文件，所以你可以在`.gitignore`中加入`src/**/*.css`。

最后，你会发现当`npm start`的时候，自动`watch-css`非常方便，还有就是`npm run build`的时候`build-css`。你可以用一个`&&`操作符来分别执行两个脚本。但是没有一个跨平台的方法可以并行运行两个脚本命令，所以我们要装载一个npm包：
```bash
npm install --save npm-run-all
```
或者使用yarn
```bash
yarn add npm-run-all
```
然后我们可以更改`start`和`build`命令，使其包含css预处理的命令：
```diff
   "scripts": {
     "build-css": "node-sass-chokidar src/ -o src/",
     "watch-css": "npm run build-css && node-sass-chokidar src/ -o src/ --watch --recursive",
-    "start": "react-scripts start",
-    "build": "react-scripts build",
+    "start-js": "react-scripts start",
+    "start": "npm-run-all -p watch-css start-js",
+    "build-js": "react-scripts build",
+    "build": "npm-run-all build-css build-js",
     "test": "react-scripts test --env=jsdom",
     "eject": "react-scripts eject"
   }
```
现在运行`npm start`和`npm run build`同时可以打包Sass文件。

**为什么使用node-sass-chokibar**

`node-sass`有下面一些issues：
- `node-sass --watch`在虚拟机或者docker环境下有性能issues
- 不断的编译[#1939](https://github.com/facebook/create-react-app/issues/1939)
- 检测到文件夹下面的新文件时，会有issues，[#1891](https://github.com/sass/node-sass/issues/1891)

### 添加图片，文字，和文件

用webpack打包，类似于图片和文字这样的静态资源的方式和css非常类似

你可以在 **一个js模块中`import`一个文件**。这相当于告诉webpack打包的时候将该文件包含在内。和引入css不同，引入文件拿到的是一个字符串值。这个值是最终关联到你的代码中的最终路径。例如：image的src属性或PDF的href属性。

为了减少服务端的请求，少于10000字节的图片会返回一个[data URI](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs)而不是一个路径。这个默认设置会对下面的文件生效：bmp，gif，jpg，jpeg，和png。SVG文件不包含在内，因为[#1153](https://github.com/facebookincubator/create-react-app/issues/1153)

这里是一个例子：
```js
import React from 'react';
import logo from './logo.png'; // Tell Webpack this JS file uses this image

console.log(logo); // /logo.84287d09.png

function Header() {
  // Import result is the URL of your image
  return <img src={logo} alt="Logo" />;
}

export default Header;
```
这样保证当工程被打包的时候，Webpack将会把图片放到build文件夹下，并且给我们提供正确的路径。

在css中也是一样的：
```css
.Logo {
  background-image: url(./logo.png);
}
```
webpack会找到所有css里面的相对路径并且编译的时候将他们替换成最终的路径。如果你拼写错误或者不小心删掉了一个文件，你将会看到一个编译错误，就像你引入了一个不存在的Javascript模块一样。最终的文件名会包含webpack生成的内容hash值。如果文件内容更新的话，webpack会更新文件名，所以不用担心资源的持久化缓存。

请认清一点，这只不过是webpack的一个定制化特性

**这种方式并不是React必需的**，但是许多人喜欢用它(React Native用了类似的图片处理技术)。另外一种处理静态资源的方法将会在下一节描述

### 使用`public`文件夹
> 注意：这个特性在react@0.5.0版本或以上可用

#### 更新public文件夹的html

html就在public文件夹下面，所以你可以动它，例如，设置标题。包含编译代码的`<script />`标签在编译过程中会被自动加进html。

#### 在模块外部添加静态资源

你可以添加其他静态资源到`public`文件夹

注意我们一般建议在js文件中`import`静态资源。例如：[添加样式表](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-a-stylesheet)，[添加图片，文字](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-images-fonts-and-files)。这种技术可以提供如下优势：
- 脚本和样式被压缩和打包到了一起，减少额外的网络请求
- 文件少了之后，直接在编译阶段报错，而不是曝露给用户404
- 文件名包含内容哈希，所以不必担心浏览器缓存它的旧版本

然而依然有另外一个 **出口**，你可以在模块系统外部添加静态资源。

如果你将一个文件放到`public`文件夹，它不会被webpack处理。另外就是它会原封不动地被复制到build文件夹中。为了关联`public`文件夹下面的静态资源，你需要使用一个特殊的变量-`PUBLIC_URL`

在`index.html`中，你可以像下面一样使用它：
```html
<link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
```
只有包含在`public`文件夹下面才会被`%PUBLIC_URL%`前缀解析到。如果你需要使用`src`或`node_modules`下面的文件，你必须复制它到`public`文件夹，并详细说明你的意图。

当你运行`npm run build`，Create React App会将`%PUBLIC_URL%`替换成一个正确的绝对路径，因此，即使使用客户端路由，工程也能正常运行。

在js代码中，你可以使用`process.env.PUBLIC_URL`，达到相同的目的：

```js
render() {
  // Note: this is an escape hatch and should be used sparingly!
  // Normally we recommend using `import` for getting asset URLs
  // as described in “Adding Images and Fonts” above this section.
  return <img src={process.env.PUBLIC_URL + '/img/logo.png'} />;
}
```
请注意以下几点：
- 在`public`文件夹下面不会被预处理或者压缩
- 如果文件不存在，编译的时候并不会告知用户，并且会抛出一个404给用户
- 这里的文件名不会包含hash值，所以每次文件改动之后需要加一个查询参数，或者更换文件名

#### 什么时候使用`public`文件夹

一般情况下我们推荐从js中引入样式表，图片和字体。`public`文件夹用于处理一些不常见的case。