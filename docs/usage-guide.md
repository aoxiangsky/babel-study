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
