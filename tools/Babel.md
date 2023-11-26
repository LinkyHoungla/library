# 简介

Babel 是一个 JavaScript 编译器，它的主要作用是将使用了新一代 JavaScript（如 ES6/ES7）或者 JSX 语法编写的代码转换为浏览器可以兼容的旧版 JavaScript 代码（如 ES5/ES3）。

随着 ECMAScript 标准的快速发展，新的语法不断涌现，但是不同浏览器对这些新语法的支持程度不同，导致开发者无法直接在项目中使用这些新特性，因此 Babel 应运而生。它让开发者可以放心使用新语法，通过转换使得代码在浏览器中能够正常执行。

**特性和作用**

1. **语法转换**：将高级语法转换为低级语法，如将 ES6/ES7 语法转换为 ES5/ES3。
2. **支持 JSX 转换**：能够将 JSX 语法转换为 JavaScript，使其能够在浏览器中运行。
3. **Source Map 支持**：提供 Source Map 功能，便于调试转换后的代码，使得调试变得简单高效。

在现代前端开发中，项目通常使用 Webpack 进行模块化打包，而 Babel 则作为 Webpack 的一个 Loader 角色，被整合到项目构建流程中。大多数项目脚手架（如 Vue CLI、React Create App）默认集成了 Webpack 和 Babel，使得开发者可以直接进行开发，而不需要深入了解 Webpack 的具体细节。

**文档**

+ [官方文档](https://babel.docschina.org/docs)

# 使用

Babel 提供了一系列模块化工具包，每个工具包都作为独立的 npm 包发布在 `@babel` 下，自版本 7 开始，这种模块化设计允许针对不同的用例使用特定的工具。

## 核心库

- **核心功能**：@babel/core 模块包含了 Babel 的核心功能。你可以通过以下命令进行安装：

  ```bash
  npm install --save-dev @babel/core
  ```

- **使用方式**：在 JavaScript 中可以直接通过 require 引入并使用它：

  ```javascript
  codeconst babel = require("@babel/core");
  babel.transformSync("code", optionsObject);
  ```

- **集成方式**：作为终端用户，你可能会使用其他工具来整合 @babel/core，例如构建工具或其他前端工具链。这样可以更方便地与你的开发流程集成，尽管大部分选项可以通过其他工具设置，但有些可能需要查阅 Babel 文档以了解更多详情。

## 命令行工具

- **CLI 工具介绍**：@babel/cli 是一个允许你在终端中使用 Babel 的工具。你可以通过以下命令进行安装：

  ```bash
  npm install --save-dev @babel/core @babel/cli
  ```

- **基本用法示例**：通过以下命令使用 CLI 工具进行转换：

  ```bash
  ./node_modules/.bin/babel src --out-dir lib
  ```

  这个命令解析 `src` 目录下的 JavaScript 文件，并使用设置的转换规则将每个文件转换后输出到 `lib` 目录。如果没有设置具体的转换规则，输出代码将保持与输入一致（不保留确切的代码样式）。

- **选项设置**：你可以通过传递选项给 CLI 工具来指定所需的转换规则。一些常用选项包括 `--plugins` 和 `--presets`。使用 `--help` 命令可以查看 CLI 工具接受的其余选项。

## 转换规则

Babel 的转换规则以插件的形式存在，插件是小型 JavaScript 程序，指导 Babel 如何进行代码转换。你甚至可以编写自己的插件来应用自定义的转换规则。若要将 ES2015+ 语法转换为 ES5，可以依赖官方提供的类似 `@babel/plugin-transform-arrow-functions` 的插件。

**使用官方插件转换 ES2015+ 语法**

通过安装 `@babel/plugin-transform-arrow-functions` 插件并在 CLI 中使用指定插件的方式，可以将箭头函数转换为 ES5 兼容的函数表达式：

```bash
npm install --save-dev @babel/plugin-transform-arrow-functions

./node_modules/.bin/babel src --out-dir lib --plugins=@babel/plugin-transform-arrow-functions
```

例如：

```java
codeconst fn = () => 1;
// 转换为
var fn = function fn() {
  return 1;
};
```

这是一个开始！如果需要转换其他 ES2015+ 功能，可以使用一个称为 "preset" 的组合，其中包含了一系列预先设定的插件，而不是逐个添加所需的插件。

**使用 @babel/preset-env**

通过安装 `@babel/preset-env`，可以应用包含现代 JavaScript（如 ES2015、ES2016 等）的所有插件的 preset，即 env preset：

```bash
npm install --save-dev @babel/preset-env

./node_modules/.bin/babel src --out-dir lib --presets=@babel/env
```

该 preset 不需要额外的配置，涵盖了支持现代 JavaScript 的所有插件。但是，也可以向 presets 传递配置选项。相比在终端使用 cli 和 preset 选项，还可以通过配置文件的方式传递选项。配置文件的使用可以更清晰地管理转换规则和插件组合，使得项目的配置更加灵活和易于维护。

### Polyfill

Polyfill 是一种 JavaScript 代码，用于在旧版浏览器中模拟新的内置 API。它填补了浏览器对新标准的不完全支持，使得开发者可以在旧版浏览器中使用新特性。

在 Babel 中，`@babel/polyfill` 提供了对新 API 的全局性支持，它模拟了新的内置 API 并在全局范围内注入所需的功能。通过导入这个 polyfill，开发者可以使用新的 API 而不必担心浏览器是否支持。

**安装 @babel/polyfill**

通过以下命令可以安装 `@babel/polyfill`：

```bash
npm install --save @babel/polyfill
```

**使用 @babel/polyfill**

在代码的入口文件中导入 `@babel/polyfill`：

```javascript
import '@babel/polyfill';
// 或者
require('@babel/polyfill');
```

这样做的好处是，它会在全局范围内引入新的内置 API 的模拟，使得你可以在整个应用程序中使用新的特性，而无需担心浏览器是否支持这些特性。

**注意事项**

尽管 Polyfill 提供了在旧版浏览器中使用新特性的便利性，但也有一些需要注意的事项：

- **包大小**：Polyfill 可能会增加应用的大小，因为它们引入了大量模拟代码以支持新特性。这可能对性能和加载速度产生影响。
- **选择性导入**：使用工具（如 `core-js`）提供的特性选择性导入功能，可以减少 Polyfill 的大小，只导入需要的特性，而非所有的新特性。
- **现代浏览器**：对于现代浏览器来说，并不总是需要 Polyfill。在一些现代浏览器中，已经原生支持了很多新的特性，因此无需引入 Polyfill。
- **随着标准的更新**：随着浏览器的更新和标准的改变，某些特性可能会从 Polyfill 中删除，或者需要更新版本的 Polyfill 才能支持新的特性。

## 配置

Babel 的配置通常存放在项目根目录下的 `.babelrc` 文件中，这个配置文件允许开发者根据需求自定义转换规则，例如指定需要转换的语法、插件、预设等。

从 Babel `v7.8.0` 开始，也可以使用名为 `babel.config.json` 的配置文件。下面是一个示例 `babel.config.json` 文件的内容：

```json
{
  "presets": [
    [
      "@babel/preset-env",
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

以上配置中，使用了 `@babel/preset-env` 预设，并通过 `targets` 指定了目标浏览器的版本。此配置告诉 Babel 只为目标浏览器中没有支持的新语法加载转换插件，而对于已被目标浏览器支持的语法，将不会进行转换。



# 工具集

在使用Babel之前，我们先要知道Babel包含哪些东西，是的，它不是一个单一的工具，一开始它确实是一个单一的Javascript转换器，但现在它变成了一个工具集……别慌，它并没有因此变得很复杂，之所以这么设计是有原因的：

```
复制代码在Babel 6以前，Babel是一个专注且单一的Javascript转换器，然而Babel团队的志向不仅于此，更想把Babel打造成一个平台，由各个不同的功能模块和可插拔插件组成，用于创建下一代JavaScript工具集。
```

> 详见[Babel 6.0之babel-cli和babel-core的用法和区别](https://link.juejin.cn?target=http%3A%2F%2Flinyk.me%2F2017%2F03%2F31%2Fbabel-usage%2F)

自Babel 7起，Babel团队改用作用域软件包，因此您现在必须使用`@babel/core`而不是`babel-core`.但实质上，`@babel/core`只是`babel-core`的较新版本。这样做是为了更好地区分哪些软件包是官方软件包，哪些是第三方软件包。

Babel7包含了核心包`@babel/core`、终端命令行界面工具`@babel/cli`以及各种语法转换规则包（官方称之为`preset`，即“预设”）如`@babel/preset-env`、`@babel/preset-react`、`@babel/preset-typescript`、`@babel/preset-flow`，这些都是按需安装使用的，后面介绍用法时你将了解它们的用途。

```bash
bash
复制代码在Babel 7之前，存在按年度区分的语法预设包如preset-es2015、preset-es2016、preset-es2017等，从Babel 7开始，Babel团队删除（并停止发布）了任何年度的preset（preset-es2015 等）， @babel/preset-env取代了对这些内容的需求，因为它包含了所有年度所添加内容以及针对特定浏览器集兼容的能力。
```

Babel工具集还包含一个叫做`@babel/polyfill`的库，作用是针对ES6+的一些新的内置API使用基础JS语法进行模拟实现，以适用较低版本的浏览器。而`@babel/core`只做语法（Syntax）转换（如class、箭头函数等），不实现Api polyfill。

```typescript
typescript复制代码polyfill在英文中有垫片的意思，意为兜底的东西。

@babel/polyfill模拟一个完全的 ES2015+ 的环境，实现了新的特性比如 Promise 或者 WeakMap， 静态方法比如Array.from 或 Object.assign, 实例方法 比如 Array.prototype.includes 和 generator 函数。

从babel V7.4.0版本开始，已经不建议使用该包，建议使用core-js/stable、regenerator-runtime/runtime替代。
```

> 找个时间另外写一下这块的细节，可以先查阅官网文档[@babel/polyfill](https://link.juejin.cn?target=https%3A%2F%2Fbabeljs.io%2Fdocs%2Fen%2Fbabel-polyfill)

除此之外，这里简单提一下Babel插件（Plugin）的概念，不做深入。在Babel中，代码转换功能以插件的形式出现，插件是小型的 JavaScript 程序，用于指导 Babel 如何对代码进行转换。你甚至可以编写自己的插件将你所需要的任何代码转换功能应用到你的代码上。Babel插件分为语法插件和转换插件两种：

```sql
sql复制代码语法插件：大多数语法都可以被 Babel 转换。在极少数情况下（如果转换还没有实现，或者没有默认的方式来实现），你可以使用语法插件，例如@babel/plugin-syntax-bigint只允许 Babel解析特定类型的语法。

转换插件：转换您的代码，转换插件将启用相应的语法插件，因此您不必同时指定两者。
```

实际上Babel预设（Preset）就是一组Babel插件的集合，比如Babel 6.0中的 `babel-preset-es2015` 包含所有跟ES6转换有关的插件。如果没有预设，babel转化是需要指定用什么插件的，虽然颗粒度小，效率高，但是插件需要逐个安装，还有严格的配置声明顺序。本文后面不再深入叙述插件相关的概念，等有时间将写一篇文章进一步深入学习Babel的原理。



