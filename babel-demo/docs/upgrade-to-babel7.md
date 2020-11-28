## 升级到Babel7

当用户准备升级到Babel7时请参照此文档
并不是每一个破坏性的变更都会影响每个项目，我们升级时根据改动破坏测试，按照可能性对每节进行了排序

### 所有的Babel

> #5025、#5041、#7755、#5186已经放弃了对Node.js 0.10、0.12、4和5的支持

我们强烈建议你使用`Node.js`的新版本V8，因为这之前的版本不再维护，查看`nodejs/LTS`获取更多信息

这意味着Babel本身不能在旧版本的Node上运行了，但是Babel人可以在老版本的Node上运行并输出代码

### 配置查找变化

查看更多的信息，阅读我们的6.X和7.X的比较关系

先前Babel在处理`node_modules`,`symlinks`,`monorepos`的时候有过问题，为此我们对描述做了一些改变。Babel将会停止查阅`package.json`而不是停止查阅关系链。对于`monorepos `我们需要创建一个新`babel.config.js`文件，以集中所有来自`package`的配置（省去了为每一个包设置一个配置选项）。在版本7.1,我们引进了一个rootMode选项，以便在必要时进行进一步查找。

### 每年预设的用法

`env`预设已经推出一年多了，并且完全取代了我们之前提出与建议的预设

- `babel-preset-es2015`
- `babel-preset-es2016`
- `babel-preset-es2017`
- `babel-preset-latest`
- 以及上述预设的组合

这些预设都被`env`预设所取代

### 预设提案用法

我们已经移除了提案预设而不是明确的建议使用，查阅`stage-0 README`获取更多的迁移步骤
自动化完成，你可以运行`npx babel-upgrade`

### 移除@babel/polyfill中的polyfills提议

基于相似的建议，我们从`@babel/polyfill`中移除了polyfill提议
目前`@babel/polyfill`主要是`core-js`版本二的别名
以前只有两个引入

```javascript

import "core-js/shim"; // included < Stage 4 proposals
import "regenerator-runtime/runtime";

```

如果你想要使用提议，你将需要进行独立的引入，你需要从`core-js`包直接引入他们，以及从`npm`引入其他的包

```javascript

// for core-js v2:
import "core-js/fn/array/flat-map";

// for core-js v3:
import "core-js/features/array/flat-map";

```

下面是core-js v2中阶段< 3的建议填充的列表。
