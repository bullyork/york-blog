---
title: webpack源码阅读
date: 2017-07-10 11:35:20
tags: webpack
---
### 引子

天命之谓性；率性之谓道；修道之谓教；

道也者，不可须臾离也；可离，非道也。是故君子戒慎乎其所不睹，恐惧乎其所不闻。

莫见乎隐，莫显乎微。故君子慎其独也。

喜、怒、哀、乐之未发，谓之中。发而中节，谓之和。中也者，天下之大本也。和也者，天下之达道也。

致中和，天地位焉，万物育焉。

-- 《中庸》

### 大纲

1. 执行bin目录下的webpack.js脚本，解析命令行参数以及开始执行编译。
2. 调用lib目录下的webpack.js文件的核心函数webpack，实例化一个Compiler，继承Tapable插件框架，实现注册和调用一系列插件。
3. 调用lib目录下的/webpackOptionApply.js模块的process方法，使用各种各样的插件来逐一编译webpack编译对象的各项。
4. 在3中调用的各种插件编译并输出新文件。

### 起步

入口文件：`./bin/webpack.js`
```javascript
var path = require("path");//引入node path 模块
try {
	var localWebpack = require.resolve(path.join(process.cwd(), "node_modules", "webpack", "bin", "webpack.js")); //解析webpack路径
	if(__filename !== localWebpack) {
		return require(localWebpack);
	}
} catch(e) {} //这里是将webpack的版本定位到本地

var yargs = require("yargs")
	.usage("webpack " + require("../package.json").version + "\n" +
		"Usage: https://webpack.js.org/api/cli/\n" +
		"Usage without config file: webpack <entry> [<entry>] <output>\n" +
		"Usage with config file: webpack");
require("./config-yargs")(yargs);
var DISPLAY_GROUP = "Stats options:";
var BASIC_GROUP = "Basic options:";
yargs.options({
	"json": {
		type: "boolean",
		alias: "j",
		describe: "Prints the result as JSON."
	},
	"progress": {
		type: "boolean",
		describe: "Print compilation progress in percentage",
		group: BASIC_GROUP
	},
	"color": {
		type: "boolean",
		alias: "colors",
		default: function supportsColor() {
			return require("supports-color");
		},
		group: DISPLAY_GROUP,
		describe: "Enables/Disables colors on the console"
	},
	"sort-modules-by": {
		type: "string",
		group: DISPLAY_GROUP,
		describe: "Sorts the modules list by property in module"
	},
	"sort-chunks-by": {
		type: "string",
		group: DISPLAY_GROUP,
		describe: "Sorts the chunks list by property in chunk"
	},
	"sort-assets-by": {
		type: "string",
		group: DISPLAY_GROUP,
		describe: "Sorts the assets list by property in asset"
	},
	"hide-modules": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Hides info about modules"
	},
	"display-exclude": {
		type: "string",
		group: DISPLAY_GROUP,
		describe: "Exclude modules in the output"
	},
	"display-modules": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Display even excluded modules in the output"
	},
	"display-max-modules": {
		type: "number",
		group: DISPLAY_GROUP,
		describe: "Sets the maximum number of visible modules in output"
	},
	"display-chunks": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Display chunks in the output"
	},
	"display-entrypoints": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Display entry points in the output"
	},
	"display-origins": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Display origins of chunks in the output"
	},
	"display-cached": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Display also cached modules in the output"
	},
	"display-cached-assets": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Display also cached assets in the output"
	},
	"display-reasons": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Display reasons about module inclusion in the output"
	},
	"display-depth": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Display distance from entry point for each module"
	},
	"display-used-exports": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Display information about used exports in modules (Tree Shaking)"
	},
	"display-provided-exports": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Display information about exports provided from modules"
	},
	"display-optimization-bailout": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Display information about why optimization bailed out for modules"
	},
	"display-error-details": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Display details about errors"
	},
	"display": {
		type: "string",
		group: DISPLAY_GROUP,
		describe: "Select display preset (verbose, detailed, normal, minimal, errors-only, none)"
	},
	"verbose": {
		type: "boolean",
		group: DISPLAY_GROUP,
		describe: "Show more details"
	}
});

```