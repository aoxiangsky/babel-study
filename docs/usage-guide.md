# Usage Guide

Babel工具链中有相当多的工具，让你能轻松使用Babel，无论你是Babel终端用户，还是构建Babel本身。这里将会快速的为你介绍这些工具，你可以在文档的`Usage`章节了解很多。

> 如果你正在使用框架，那么Babel工作的配置文件可能不一样，或许已经给你配置好了，而不是看看我们的交互指南。

## 综述

本指南将向你展示如果使用`ES2015+`语法的javascript应用程序代码编译为当前浏览器中工作的代码。这将涉及到转换新语法已经填充不存在的特性
准备工作的整个过程包括

1. 执行这些命令下载依赖包

```shell

npm install --save-dev @babel/core @babel/cli @babel/preset-env
npm install --save @babel/polyfill

```

2. 创建一个配置文件，取名为`babel.config.json`,初始内容为下，置于项目的根目录下：

```shell

{
  "presets": [
    [
      "@babel/env",
      {
        "targets": {
          "edge": "17",
          "firefox": "60",
          "chrome": "67",
          "safari": "11.1",
        },
        "useBuiltIns": "usage",
        "corejs": "3.6.4",
      }
    ]
  ]
}

```

> 上面的浏览器列表只是任意的例子，你未来必须按照自己所需要配置对应的浏览器支持版本

3. 执行命令编译你的代码，从`src`目录到`lib`目录：

```shell

./node_modules/.bin/babel src --out-dir lib

```

> 你可以使用@5.2.0 `npm`版本包管理附带的运行器来使用`npx babel`替换`./node_modules/.bin/babel`缩短命令

请继续阅读本文，将会一步步为你解释Babel如何工具，以及介绍使用的每个工具

## 脚手架基础使用

从版本7开始，你需要得所有Babel包都被做为独立得包发布，并在`@babel`域之下。这种模块化设计允许我们针对各种案例设计相应得工具。接下来我们看看`@babel/core`和`@babel/cli`

## 核心库

Babel的核心代码功能都存在于`@babel/core`之下，安装之后

```shell

npm install --save-dev @babel/core

```

你可以在js项目中直接引入他，并且像这样使用

```js

const babel = require("@babel/core");
babel.transform("code", optionsObject);

```

作为一名终端用户，你可能想要下载其他工具作为`@babel/core`的接口服务，并于你的开发环境很好的兼容.及时如此，你也需要查阅文档学习如何书写配置选项，其中大多数配置项也可以在其他工具中配置。

## 脚手架工具

`@babel/cli`是一个允许你在终端使用的工具。下面是安装命令以及基础的使用示例。

```shell

npm install --save-dev @babel/core @babel/cli

./node_modules/.bin/babel src --out-dir lib

```

他将会以我们配置好的应用方案解析`src`文件夹下所有的js文件，并将编译好的文件放置`lib`文件夹下。若是没有告诉编译器转换应用配置，输入的代码将会与输出的代码相同（精确的代码样式不会保留）。我们可以配置项来得到想要的代码转换

我们能在上面使用`--out-dir`配置选项，可以使用`--help`命令查看CLI工具接受的其他配置命令，目前对于我们来说最重要的指令是`--plugins`and`--presets`。

## 插件&预设

转换以插件的形式存在，实质上是一个js小程序用以指导Babel如何对代码进行转换。你甚至可以自己编写插件来应用任何你想要的代码转换。要将`ES2015+`语法转换为`ES5`,我们可以依赖于像`@babel/plugin-transform-arrow-functions`这样的官方插件

```shell

npm install --save-dev @babel/plugin-transform-arrow-functions

./node_modules/.bin/babel src --out-dir lib --plugins=@babel/plugin-transform-arrow-functions

```

现在，代码中的任何箭头函数都被转换成了兼容`ES5`的函数表达式

```javascript

const fn = () => 1;

// converted to

var fn = function fn() {
  return 1;
};

```

这是一个非常好的开始，但是代码中还有其他需要转换的`ES2015+`特性。我们可以使用预设，一个一组预先确定的插件集合，而不是我们把所有插件一个一个的添加上去.
像插件一样，你也可以创建自己的预设，共用任何你所需要的的插件集合，对于我们自己的案例而言，这有个特别好的案例名为`env`

```shell

npm install --save-dev @babel/preset-env

./node_modules/.bin/babel src --out-dir lib --presets=@babel/env

```

无需任何配置，这个预设包含转换现代js（ES5,ES6等等）代码的所有插件，当然这个预设也可以配置选项，让我们在配置文件中传递配置选项，而不是在终端的脚手架或者预设选项红传达

## 配置

> 基于你的需要，有几种不同方法使用配置文件。请务必阅读关于Babel配置的深入指南以获取更多的配置信息。

现在，让我们一起创建叫`babel.config.json`的文件，并附上下列内容

```json

{
"presets": [
  [
  "@babel/env",
    {
      "targets": {
        "edge": "17",
        "firefox": "60",
        "chrome": "67",
        "safari": "11.1"
        }
      }
    ]
  ]
}

```

现在，`env`预设会为目标浏览器中没有的特性加载转化插件。语法转换方面都说完了，接下来我们说说`polyfills`

## Polyfill

> 从Babel版本7.4.0之后，这个包就被废弃了，取而代之的是直接引入`core-js/stable`(填充ECMAScript新特性)和`regenerator-runtime/runtime`(需要使用经过置换的生成器函数)

```js

import "core-js/stable";
import "regenerator-runtime/runtime";

```

`regenerator runtime`模块包含`core-js`和 定制的`regenerator runtime`模拟一个完整的`ES2015+`环境，这意味着你可以使用的新的嵌入函数如`Promise`or`WeakMap`,静态方法如`Array.from`或者`Object.assign`，实例方法如`Array.prototype.includes`，以及生成器函数（当与再生器插件一起使用时）。为了做到这样，`polyfill`添加了全局作用域和原生原型（如String）

对于库/工具作者来说`polyfill`可能包含的太多了，你若不需要使用像`Array.prototype.includes`这样的实例方法，可通过使用`transform runtime`替换`@babel/polyfill`来避免污染全局作用域。

进一步来说，你若知道需要填充的特性是什么，也可以直接引入`core-js`。

因为我们只需要安装一个应用程序，因此我们只需要安装`@babel/polyfill`

```shell

npm install --save @babel/polyfill

```

> 注意是`--save`选项而不是`--save-dev`，因为这个需要在源代码运行前进行填充

目前对于我们来说是幸运的，我们使用`env`预设时，将`useBuiltIns`选项设置值为`usage`,会应用像上面最后提到的优化，只包含自己需要的`polyfills`，在添加了新的选项后配置文件如下所示：

```json

{
  "presets": [
    [
      "@babel/env",
      {
        "targets": {
          "edge": "17",
          "firefox": "60",
          "chrome": "67",
          "safari": "11.1",
        },
        "useBuiltIns": "usage",
      }
    ]
  ]
}

```

Babel将会检查你的代码在目标环境中，那些特性是缺失需要补齐的，然后只会`polyfills`缺失的特性，实例代码如下

```js

Promise.resolve().finally();

```

将会转换成下面代码（因为Edge浏览器不包含`Promise.prototype.finally`）

```javascript

"use strict";

require("core-js/modules/es.promise");

require("core-js/modules/es.promise.finally");

Promise.resolve().finally();

```

如果我们没有将`env`预设选项`useBuiltins`值设置为`usage`,那么在任何代码之前，我们需要在代码入口一次引入完整的`polyfill`

## 总结

我们在终端使用`@babel/cli`运行Babel，`@babel/polyfill`将会填充JS的新特性，`env`预设将会根据我们所使用的以及目标浏览器缺失的特性进行相应转换和填充

更多有关如何使用构建系统、IDE等设置Babel的详细信息，查阅我们的`交互设置指南`
