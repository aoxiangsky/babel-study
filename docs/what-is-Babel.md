# Bebel是什么

## Bebel是javascript编译器

Babel 是一个工具链，主要用于转换版本`ECMAScript 2015+`的代码，并转换成向后兼容的js代码，以使其能在目前使用或老版本的浏览器或其他环境运行。下面是babel主要能为你干的几件事情：

> 注：Polyfill可以理解为腻子，就是在装修的时候，可以把缺损的地方填充抹平
- 转换语法
- 为你所工作的代码环境填充缺失的特性
> 谁能告诉我这个codemods是个什么意思
- 源代码转换 (codemods)
- 更多等（观看这些视频，以了解更多关于babel的起源灵感）

```javascript
// Babel Input: ES2015 arrow function
[1, 2, 3].map((n) => n + 1);

// Babel Output: ES5 equivalent
[1, 2, 3].map(function(n) {
  return n + 1;
});
```

> 这是一个关于编译器非常好的教程，看看`the-super-tiny-compiler`,这也会为你解释babel在底层是如何编译的

## ES2015及其后续版本

Babel通过语法编译器，支持最新的`javascript`版本
这些插件允许你现在使用新语法，而不用等待浏览器支持，老铁们，还在犹豫什么，点开我们的用户手册，开打开打。

## JSX 和 React

Babel也可以转换JSX语法，学习我们的`React 预设`开始吧，将其与包`babel-sublime`一起使用，将会让你的语法高亮迈向新的层次。
你可以输入下面代码，下载预设

```javascript

npm install --save-dev @babel/preset-react

```

同时添加`@babel/preset-react`到你的Babel配置

```js

export default React.createClass({
  getInitialState() {
    return { num: this.getRandomNumber() };
  },

  getRandomNumber() {
    return Math.ceil(Math.random() * 6);
  },

  render() {
    return <div>
      Your dice roll:
      {this.state.num}
    </div>;
  }
});

```

> 走过路过，学`jsx`不错过

## 类型注释（Flow和TS）

Babel可以去除类型注释，查看`Flow预设`或者`TypeScript预设`开始吧，记住Babel是不会进行类型检查的，你仍然需要下载并使用`Flow`或者`Typescript`
检查类型

你可以下载flow预设如下

```shell

npm install --save-dev @babel/preset-flow

```

```js

// @flow
function square(n: number): number {
  return n * n;
}

```

或者下载typescript预设如下：

```shell

npm install --save-dev @babel/preset-typescript

```

```js

function Greeter(greeting: string) {
    this.greeting = greeting;
}

```

> 学习更多关于`Flow`和`Typescript`的知识

## 插件式

Babel由插件构成，使用现有的插件或者自己编写的插件构成你自己的转化器管道。若是使用预设将轻易构建自己的插件集。
使用`astexplorer.net`动态的创建插件，或者使用`generator-babel-plugin`生成插件模板

```js

// A plugin is just a function
export default function ({types: t}) {
  return {
    visitor: {
      Identifier(path) {
        let name = path.node.name; // reverse the name: JavaScript -> tpircSavaJ
        path.node.name = name.split('').reverse().join('');
      }
    }
  };
}

```

## bug调试

支持`source map`,因此你可以轻易的debug以及编写你的代码

## 规范兼容

Babel尽可能多的保持`ECMAScript `规范，它有很多的选项用于更符合规范，而对性能进行一定的折中减少

## 袖珍

Babel尽可能使用少的代码，不在依赖上浪费很多时间
然而在某些案例上很难做到如此，对于特别的转换，有一些零散的配置选项，可能牺牲代码规范换取可读取性，文件大小和速度。

