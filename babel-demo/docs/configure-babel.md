## Babel 配置

Babel是可配置的，许多其他的工具都有相似的配置：Eslint(.eslintrc) , Prettier(.prettierrc)

所有Babel的API选项都是允许的，然而，如果该选项需要`javascript`，那么你也会想要使用`javascript`配置文件

### 你的用例是什么？

- 你在使用一个单例吗？
- 你想编译`node_modules`吗？

> 为你准备了`babel.config.json`

- 你的配置只适用于项目中的某个配置吗？

> 为你准备了`.babelrc.json`

- 盖伊·菲利是你的英雄?

我们建议使用`babel.config.json`格式，Babel本身也在使用它

### `babel.config.json`

在你项目的根目录下（`package.json`所在）创建一个名为`babel.config.json`的文件，并写上下列内容

```json

{
  "presets": [...],
  "plugins": [...]
}

```

```javascript

module.exports = function (api) {
  api.cache(true);

  const presets = [ ... ];
  const plugins = [ ... ];

  return {
    presets,
    plugins
  };
}

```

查阅`babel.config.json`文档查看更多配置选项

### `.babelrc.json`

在你的项目下创建文件`.babelrc.json`，并写上下列内容

```json

{
  "presets": [...],
  "plugins": [...]
}

```

查阅`.babelrc`文档查看更多的配置选项

`package.json`

可选择的，你也可以选择指定将`.babelrc.json`配置写在`package.json`文件内，使用`babel`关键字如下所示：

```json

{
  "name": "my-package",
  "version": "1.0.0",
  "babel": {
    "presets": [ ... ],
    "plugins": [ ... ],
  }
}

```

### JavaScript配置文件

你也可以使用js文件配置`babel.config.json`和`.babelrc.json`

```javascript

const presets = [ ... ];
const plugins = [ ... ];

module.exports = { presets, plugins };

```

你可以访问任何`Node.js`API,例如你可以基于进程环境进行动态配置

```javascript

const presets = [ ... ];
const plugins = [ ... ];

if (process.env["ENV"] === "prod") {
  plugins.push(...);
}

module.exports = { presets, plugins };

```

你可以阅读更多的`javascript`配置文件在专用的文档中

### 使用`@babel/cli`

```shell

babel --plugins @babel/plugin-transform-arrow-functions script.js

```

查阅`babel-cli`文档查看更多的配置选项

### 使用API（`@babel/core`）

```javascript

require("@babel/core").transform("code", {
  plugins: ["@babel/plugin-transform-arrow-functions"]
});

```

查阅`babel-core`文档知道更多的配置选项

### 打印有效配置

#### 此处省略一大段，搞不懂怎么操作~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

你可以告诉Babel在给定的路径上打印有效的配置



```shell



```

### Babel如何合并配置选项

对于上面提到的每个配置项，将会对`plugins`和`presets`使用`Object.assign`合并，用`Array#concat`方法进行合并，例如

```javascript

const config = {
  plugins: [["plugin-1a", { loose: true }], "plugin-1b"],
  presets: ["preset-1a"],
  sourceType: "script"
}

const newConfigItem = {
  plugins: [["plugin-1a", { loose: false }], "plugin-2b"],
  presets: ["preset-1a", "preset-2a"],
  sourceType: "module"
}

BabelConfigMerge(config, newConfigItem);
// returns
({
  plugins: [
    ["plugin-1a", { loose: false }],
    "plugin-1b",
    ["plugin-1a", { loose: false }],
    "plugin-2b"
  ], // new plugins are pushed
  presets: [
    "preset-1a",
    "preset-1a",
    "preset-2b"
  ], // new presets are pushed
  sourceType: "module" // sourceType: "script" is overwritten
})

```

