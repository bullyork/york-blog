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

另外，也可以提供一个`构建者` object 提供一些命令接受的选项信息
```js
yargs
  .command('get', 'make a get HTTP request', {
    url: {
      alias: 'u',
      default: 'http://yargs.js.org/'
    }
  })
  .help()
  .argv
```
`构建者` 还可以是一个函数。函数携带一个 `yargs` 实例， 可以用来提供更详细的命令描述。
```js
yargs
  .command('get', 'make a get HTTP request', function (yargs) {
    return yargs.option('url', {
      alias: 'u',
      default: 'http://yargs.js.org/'
    })
  })
  .help()
  .argv
```
而且，还可以加上一个处理函数，参数为转化后的`argv`对象：
```js
yargs
  .command(
    'get',
    'make a get HTTP request',
    function (yargs) {
      return yargs.option('u', {
        alias: 'url',
        describe: 'the URL to make an HTTP request to'
      })
    },
    function (argv) {
      console.log(argv.url)
    }
  )
  .help()
  .argv
```
想要进一步了解command api更详尽的特性，请[点击这里](https://github.com/yargs/yargs/blob/master/docs/advanced.md#commands)

#### .completion([cmd], [description], [fn])

此命令用户命令行补全(切记执行 chmod u+x 文件 改为可执行文件再进行补全尝试)

`cmd` 定义参与命令补全的命令。第一次使用会有提示，如果用的zsh，那么将提示中`.bashrc`改成`.zshrc`，然后按照提示操作（切记将你的js文件设置为可执行文件，然后再操作）

`description` 描述命令的使用方法

`fn`: 提供待补全的参数

如果实现的时候没有携带参数，那么`.completion()`会让补全命令输出补全脚本

```js
var argv = require('yargs')
  .completion('completion', function(current, argv) {
    // 'current' is the current command being completed.
    // 'argv' is the parsed arguments so far.
    // simply return an array of completions.
    return [
      'foo',
      'bar'
    ];
  })
  .argv;
```
还可以提供异步补全
```js
var argv = require('yargs')
  .completion('completion', function(current, argv, done) {
    setTimeout(function() {
      done([
        'apple',
        'banana'
      ]);
    }, 500);
  })
  .argv;
```
还可以使用异步的promise
```js
var argv = require('yargs')
  .completion('completion', function(current, argv, done) {
    return new Promise(function (resolve, reject) {
      setTimeout(function () {
        resolve(['apple', 'banana'])
      }, 10)
    })
  })
  .argv;
```
### .config([key], [description], [parseFn]) .config(object)

此方法使得传入key的值被认为是一个JSON配置文件的路径。文件中的JSON属性被设置为对应的key和value(挂在argv上)。文件使用nodejs的require api加载的，文件名最好为.js, .json。

如果此方法没有携带任何参数，`.config()`将会使用`--config`选项传入JSON配置文件（此时只能传入以.json结尾文件）

`description`同其他方法，用于描述命令

可选项 `parseFn` 可以用来定制转换器。转换函数必须是同步的，并且应当返回一个带有key,value键值对的对象或者一个error。

```js
var argv = require('yargs')
  .config('settings', function (configPath) {
    return JSON.parse(fs.readFileSync(configPath, 'utf-8'))
  })
  .argv
```
你还可以传一个明确的object，同样的，它的键值对会转化为`argv`的键值对。

```js
var argv = require('yargs')
  .config({foo: 1, bar: 2})
  .argv
```
`config`和`pkgConf`可以提供`extends`关键词，用来表明配置应该从别处继承。

`extends`参数可以是绝对路径也可以是相对路径
```js
yargs.config({
  extends: './configs/a.json',
  logLevel: 'verbose'
})
.argv
```
或者，还可以是一个模块
```js
# my-library.js
yargs.pkgConf('nyc')
```
```js
# package.json
{
  "nyc": {
    "extends": "nyc-babel-config"
  }
}
```
nyc-babel-config 是一个提供配置的包

#### .conflicts(x, y)

如果x被设置了，那么y一定不能被设置。y可以是字符串也可以是数组

当然，此方法也可以接受Object对象

#### .count(key)

此命令用来对命令计数，并转化为命令的参数。例如：
 ```js
 #!/usr/bin/env node
var argv = require('yargs')
    .count('verbose')
    .alias('v', 'verbose')
    .argv;

VERBOSE_LEVEL = argv.verbose;

function WARN()  { VERBOSE_LEVEL >= 0 && console.log.apply(console, arguments); }
function INFO()  { VERBOSE_LEVEL >= 1 && console.log.apply(console, arguments); }
function DEBUG() { VERBOSE_LEVEL >= 2 && console.log.apply(console, arguments); }

WARN("Showing only important stuff");
INFO("Showing semi-important stuff too");
DEBUG("Extra chatty mode");
 ```
#### .default(key, value, [description]) .defaults(key, value, [description])

*注意：*这个命令已经弃用。将在将来的主版本中移除

如果没有选项传入的话，为key设置默认值

当然，同样此命令可以接受一个object为参数，设置key的默认值

而且，默认值还可以是一个函数，函数名会被写到help里面的用法说明中
```js
var argv = require('yargs')
  .default('random', function randomValue() {
    return Math.random() * 256;
  }).argv;
```
而且，`description`参数可以优先提供用法说明

#### .demandOption(key, [msg | boolean]) .demandOption(key, msg)

此命令用来强制用户输入某些参数

如果key是一个字符串，且key没有出现在命令行参数中，展示使用说明并退出。

如果key是数组，限制每一个数组中的参数

如果提供了msg，而且没有相应参数，那么msg将会展示，替代原有的标准错误信息

```js
// demand an array of keys to be provided
require('yargs')
  .option('run', {
    alias: 'r',
    describe: 'run your program'
  })
  .option('path', {
    alias: 'p',
    describe: 'provide a path to file'
  })
  .option('spec', {
    alias: 's',
    describe: 'program specifications'
  })
  .demandOption(['run', 'path'], 'Please provide both run and path arguments to work with this tool')
  .help()
  .argv
```
当第二个参数为布尔值的时候，布尔值决定了这个选项是不是必须的。当使用options方法的时候，这个选项很有用
```js
// demand individual options within the option constructor
require('yargs')
  .options({
    'run': {
      alias: 'r',
      describe: 'run your program',
      demandOption: true
    },
    'path': {
      alias: 'p',
      describe: 'provide a path to file',
      demandOption: true
    },
    'spec': {
      alias: 's',
      describe: 'program specifications'
    }
  })
  .help()
  .argv
```
#### .demandCommand([min=1], [minMsg]) .demandCommand([min=1], [max], [minMsg], [maxMsg])

此命令用于限定用户在程序中使用的命令次数。如果命令没有传进来，使用msg提供标准的错误提示

```js
require('yargs')
  .command({
    command: 'configure <key> [value]',
    aliases: ['config', 'cfg'],
    desc: 'Set a config variable',
    builder: (yargs) => yargs.default('value', 'true'),
    handler: (argv) => {
      console.log(`setting ${argv.key} to ${argv.value}`)
    }
  })
  // provide a minimum demand and a minimum demand message
  .demandCommand(1, 'You need at least one command before moving on')
  .help()
  .argv
```
#### .describe(key, desc)

描述key的使用方法，可以使用object

#### .detectLocale(boolean)

yargs是否检查系统的所在地，默认是true

#### .env([prefix])

定义环境变量的值，使用"_"来表明嵌套的选项（nested__foo => nested.foo）

程序参数定义优先的次序:
1. 命令行参数
2. env 参数
3. 配置文件或者object
4. 默认选项

```js
var argv = require('yargs')
  .env('MY_PROGRAM')
  .option('f', {
    alias: 'fruit-thing',
    default: 'apple'
  })
  .argv
console.log(argv)
```
```js
$ node fruity.js
{ _: [],
  f: 'apple',
  'fruit-thing': 'apple',
  fruitThing: 'apple',
  '$0': 'fruity.js' }
```
```js
$ MY_PROGRAM_FRUIT_THING=banana node fruity.js
{ _: [],
  fruitThing: 'banana',
  f: 'banana',
  'fruit-thing': 'banana',
  '$0': 'fruity.js' }
```
```js
$ MY_PROGRAM_FRUIT_THING=banana node fruity.js -f cat
{ _: [],
  f: 'cat',
  'fruit-thing': 'cat',
  fruitThing: 'cat',
  '$0': 'fruity.js' }
```
默认情况下不读取env参数（没有传啊！！！），但是可以用`.env(false)`让环境变量失效。

#### .epilog(str) .epilogue(str)

使用说明之后的收场白
```js
var argv = require('yargs')
  .epilogue('for more information, find our manual at http://example.com');
```
#### .example(cmd, desc)

增加示例，第一个参数`cmd`中，`$0`代表命令行中的第一个参数。例子会随着帮助信息打印出来。

#### .exitProcess(enable)

暂时没发现有什么卵用

#### .fail(fn)

当失败发生的时候，此函数调用，而不是打印错误信息。

`fn` 参数有三个，msg: '本来要打印的信息'；err：最开始抛出来的`Error`实例；'yargs'，参数对象

```js
var argv = require('yargs')
  .fail(function (msg, err, yargs) {
    if (err) throw err // preserve stack
    console.error('You broke it!')
    console.error(msg)
    console.error('You should be doing', yargs.help())
    process.exit(1)
  })
  .argv
```
#### .getCompletion(args, done);

原文是可以程序地补全任何行的选项

args: 命令行要补全的参数

done: 回调函数，参数为补全的结果

示例：
```js
require('yargs')
  .option('foobar')
  .option('foobaz')
  .completion()
  .getCompletion(['./test.js', '--foo'], function (completions) {
    console.log(completions)
  })
```
输入的补全选项：`./test.js --foo` (按tab键)：--foobar 和 --foobaz
> 此方法我没有实际测试通过，特此备注

#### .global(globals, [global=true])

默认option都是全局的，但是此命令可以恢复设置为false的选项重新为全局的。

全局的option在定义的命令执行之后不会清除，非全局的会被清除。默认所有选项都是全局的。

```js
var argv = require('yargs')
  .option('a', {
    alias: 'all',
    default: true,
    global: false
  })
  .option('n', {
    alias: 'none',
    default: true,
    global: false
  })
  .command('foo', 'foo command', function (yargs) {
    return yargs.option('b', {
      alias: 'bar'
    })
  })
  .help('help')
  .global('a')
  .argv
```
#### .group(key(s), groupName)

这个就是在显示使用说明的时候，为同一组option增加一个头标题

```js
var yargs = require('yargs')(['--help'])
  .help()
  .group('batman', 'Heroes:')
  .describe('batman', "world's greatest detective")
  .wrap(null)
  .argv
```

#### .help() .help([option | boolean]) .help([option, [description]])

展示和配置帮主信息，yargs默认支持`--help`选项

option 参数代表可以使用其他选项或命令调出帮助信息。

description 可以修改默认帮助信息的文案

```js
var yargs = require("yargs")(['--info'])
  .usage("$0 -operand1 number -operand2 number -operation [add|subtract]")
  .help('info')
  .argv
```
#### .implies(x, y)

此命令意味着y对x有依赖，y可以是数组，可以是参数名，甚至可以是数字（参数位置）

同样地，这个方法可以接受一个对象参数

```js
var yargs = require("yargs")
.implies('a', 'b')
.argv
```
#### .locale()

开启本地模式，默认yargs会自动检测系统位置，然后使用当地的语言

实用一个静态的locale可以覆盖掉默认的行为，如下所示

#### .locale(locale)

覆盖默认检测到的locale

```js
var argv = require('yargs')
  .usage('./$0 - follow ye instructions true')
  .option('option', {
    alias: 'o',
    describe: "'tis a mighty fine option",
    demandOption: true
  })
  .command('run', "Arrr, ya best be knowin' what yer doin'")
  .example('$0 run foo', "shiver me timbers, here's an example for ye")
  .help('help')
  .wrap(70)
  .locale('pirate')
  .argv

```
***
```js
./test.js - follow ye instructions true

Choose yer command:
  run  Arrr, ya best be knowin' what yer doin'

Options for me hearties!
  --option, -o  'tis a mighty fine option               [requi-yar-ed]
  --help        Parlay this here code of conduct             [boolean]

Ex. marks the spot:
  test.js run foo  shiver me timbers, here's an example for ye

Ye be havin' to set the followin' argument land lubber: option
```
一般用不到这个东东

#### .nargs(key, count)

此方法用于限制 key 后面跟的参数长度

```js
var argv = require('yargs')
  .nargs('token', 1)
  .argv
```
当然，也可以传object参数

#### .normalize(key)

据说是为了传入path的时候，便于调用path.normalize()

#### number(key)

告诉parser，一直将这个`key`当作数字转换

参数可以是数组

选项如果并没有值，默认为`undefined`

不能转化为数字，会被转换为`NaN`

小数，16进制数，科学计数法都是合法的

```js
var argv = require('yargs')
  .number('n')
  .number(['width', 'height'])
  .argv
```
#### .option(key, [opt]) .options(key, [opt])

为`key`配置各种选项的命令

```js
var argv = require('yargs')
    .option('f', {
        alias: 'file',
        demandOption: true,
        default: '/etc/passwd',
        describe: 'x marks the spot',
        type: 'string'
    })
    .argv
;
```
同
```js
var argv = require('yargs')
    .alias('f', 'file')
    .demandOption('f')
    .default('f', '/etc/passwd')
    .describe('f', 'x marks the spot')
    .string('f')
    .argv
;
```
还可以这样
```js
var argv = require('yargs')
    .options({
      'f': {
        alias: 'file',
        demandOption: true,
        default: '/etc/passwd',
        describe: 'x marks the spot',
        type: 'string'
      }
    })
    .argv
;
```
可用的key有以下这些

- alias
- array
- boolean
- choices
- coerce
- config
- configParser
- conflicts
- count
- default
- defaultDescription
- demandOption
- desc/describe/description
- global
- group
- hidden
- implies
- nargs
- normalize
- number
- requiresArg
- skipValidation
- string
- type: 'array','boolean','count','number','string'

#### .parse([args], [context], [parseCallback])

可以替代从`process.argv`传参，返回argv对象。args可以是数组或者原生参数字符串。

context：可以同时携带的一个对象参数，可以为命令提供状态信息，是很实用的技术。

```js
const parser = yargs
  .command('lunch-train <restaurant>', 'start lunch train', function () {}, function (argv) {
    console.log(argv.restaurant, argv.time)
  })
  .parse("lunch-train rudy's", {time: '12:15'})
```
parseCallback: 此方法的回调函数，会携带三个参数

1. err: 转化中出现的验证错误
2. argv: 转化的argv对象
3. output: 将要在终端输出的文本
```js
// providing the `fn` argument to `parse()` runs yargs in headless mode, this
// makes it easy to use yargs in contexts other than the CLI, e.g., writing
// a chat-bot.
const parser = yargs
  .command('lunch-train <restaurant> <time>', 'start lunch train', function () {}, function (argv) {
    api.scheduleLunch(argv.restaurant, moment(argv.time))
  })
  .help()

parser.parse(bot.userText, function (err, argv, output) {
  if (output) bot.respond(output)
})
```
*提醒:* 给parse函数回调参数会导致 `exitProcess` 设置失效，直至回调调用

#### .pkgConf(key, [cwd])

类似于 `.config()`，这表明yargs从 package.json 中寻找特定的配置对象

`cwd`可以选择性地提供，package.json 将从这里读取

#### .positional(key, opt)

> 这个我尝试了，但是没泡通

对命令的参数进行配置，类似于 `.option()`，但是不能作用于顶层yargs实例

```js
const argv = require('yargs')('run --help')
  .command('run <port> <guid>', 'run the server', (yargs) => {
    yargs.positional('guid', {
      describe: 'a unique identifier for the server',
      type: 'string'
    })
  }).argv
console.log(argv)
```
可选的参数有：
- alias
- choices
- coerce
- confilicts
- default
- desc/describe/description
- implies
- normalize
- type: 'boolean','number','string'

#### .recommendCommands()

决定yargs是否在没有匹配命令的时候，提示相关的类似命令

#### .require(key, [msg | boolean]) .required(key, [msg | boolean])

同 `.demand()`

#### .requiresArg(key)

可以接收数组或者字符串

规定所有的key后面必须要跟value，否则，现实用法信心并退出

默认如果key后面不跟value，key赋值为true

#### .reset() [已废弃]

重置所有已经构建的参数。对于嵌套命令行接口特别有用。使用global避免不想要重置的keys被重置

```js
var yargs = require('yargs')
  .usage('$0 command')
  .command('hello', 'hello command')
  .command('world', 'world command')
  .demandCommand(1, 'must provide a valid command'),
  argv = yargs.argv,
  command = argv._[0];

if (command === 'hello') {
  yargs.reset()
    .usage('$0 hello')
    .help('h')
    .example('$0 hello', 'print the hello message!')
    .argv

  console.log('hello!');
} else if (command === 'world'){
  yargs.reset()
    .usage('$0 world')
    .help('h')
    .example('$0 world', 'print the world message!')
    .argv

  console.log('world!');
} else {
  yargs.showHelp();
}
```
#### .showCompletionScript()

生成一个补全脚本。应用的使用者可以在他们的`.bashrc`中安装，yargs会提供命令和参数补全的快捷方式

#### .showHelp(consoleLevel='error')

实用console的`consoleLevel`打印使用数据

Example:

```js
var yargs = require("yargs")
  .usage("$0 -operand1 number -operand2 number -operation [add|subtract]");
yargs.showHelp(); //prints to stderr using console.error()
```
或者，使用标准输出打印使用信息，可以选择`console.log`:
```js
yargs.showHelp("log"); //prints to stdout using console.log()
```
#### .showHelpOnFail(enable, [message])

默认，yargs检测到错误会输出使用信息。我们可以使用这个方法定制行为。enable为false的时候，使用信息不输出。如果message参数存在，这个信息会在错误信息之后输出。
```js
#!/usr/bin/env node
var argv = require('yargs')
    .usage('Count the lines in a file.\nUsage: $0 -f <file>')
    .demandOption('f')
    .alias('f', 'file')
    .describe('f', 'Load a file')
    .string('f')
    .showHelpOnFail(false, 'Specify --help for available options')
    .help('help')
    .argv;

// etc.
```
```js
$ node line_count.js
Missing argument value: f

Specify --help for available options
```
#### .skipValidation(key)

指定一些选项，跳过验证

#### .strict([enabled=true])

如果所有的参数都没有被`demand`, 或者都没有一个正确的描述，这将会被当成错误报告出来

不被认可的命令依然会被当作错误报告出来

#### .string(key)

告诉parser不要将key当成数字或布尔值来转换

#### .updateLocale(obj) .updateStrings(obj)

*没有测试通过*

覆盖默认的字符串

```js
var argv = require('yargs')
  .command('run', 'the run command')
  .help('help')
  .updateStrings({
    'Commands:': 'My Commands -->\n'
  })
  .wrap(null)
  .argv
```
---
```js
My Commands -->

  run  the run command

Options:
  --help  Show help  [boolean]
```
如果特定了一个`locale()`, 应该在`updateStrings()`之前调用

#### .usage(<message|command>, [desc], [builder], [handler])

设置显示哪一条命令的使用信息。在message中，`$0`会被转译成脚本名字，node命令等。

如果 `desc/builder/handler` 等参数提供了，此方法相当于 `.command`。

```js
const argv = require('yargs')
  .usage('$0 <port>', 'start the application server', (yargs) => {
    yargs.positional('port', {
      describe: 'the port that your application should bind to',
      type: 'number'
    })
  }).argv
```

#### .version() .version([version|boolean]) .version([option], [description], [version])

设置显示的版本号并退出程序。默认支持 --version。

如果没有参数传入，yargs会从package.json中寻找version值

如果传入布尔值 `false`, 将会禁掉 --version

#### .wrap(columns)

格式化使用信息的展示 -- 多少列

默认的wrap是`Math.min(80, windowWidth)`, 使用`.wrap(null)`去掉列限制。使用`.wrap(yargs.terminalWidth())`控制显示信息为最大宽度。








