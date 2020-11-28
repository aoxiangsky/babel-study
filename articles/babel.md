# @babel/preset-env

## useBuiltIns 说明

[文档地址](https://babeljs.io/docs/en/babel-preset-env#usebuiltins)

> "usage" | "entry" | false, defaults to false.

### useBuiltIns: entry


根据配置的浏览器兼容，引入浏览器不兼容的 polyfill。需要在入口文件手动添加 import '@babel/polyfill'，会自动根据 browserslist 替换成浏览器不兼容的所有 polyfill。这里需要指定 core-js 的版本, 如果 "corejs": 3, 则 import '@babel/polyfill' 需要改成

```js

import "core-js/stable"; 
import "regenerator-runtime/runtime"

```

### useBuiltIns: usage

usage 会根据配置的浏览器兼容，以及你代码中用到的 API 来进行 polyfill，实现了按需添加。

### useBuiltIns: false

此时不对 polyfill 做操作。如果引入 @babel/polyfill，则无视配置的浏览器兼容，引入所有的 polyfill

## coresjs 说明

该选项只有在`useBuiltIns: usage`和`useBuiltIns: false`时才会生效

### 当使用useBuiltIns: entry时

您可以直接导入提案polyfill：导入` core-js / proposals / string-replace-all`

### 当使用useBuiltIns: usage时，有两种不同的选择项

```js
{
    corejs:{
        shippedProposals: true // 这将启用已在浏览器中发布一段时间的提案的polyfill和transforms
    }
}

```

```js
{
    corejs:{
        version: 3,  // 默认为2
        proposals: true // 这将启用对core-js支持的每个建议的填充。
    }
}
```
