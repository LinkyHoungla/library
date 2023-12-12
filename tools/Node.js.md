# 简介

> Node.js 是一个开源、跨平台的 JavaScript 运行时环境。

编程语言的运行时环境是用户可以执行以该语言编写的代码的任何环境。该环境提供了运行代码所需的所有工具和资源。Node.js 是一个 JavaScript 运行时环境。

除了 Node.js 之外，JavaScript 运行时环境的另一个示例是 Web 浏览器。浏览器通常拥有执行客户端JavaScript 代码所需的所有资源。

在浏览器中，我们可以使用 JavaScript 与标记元素交互并调整样式。浏览器会立即运行代码，因为它是一个运行时环境。

从上面定义的三个术语可以看出，Node.js 不是 Angular 那样的 JavaScript 框架。Node.js 不是一种编程语言，它不是 JavaScript 库，也不是一组技术的总称。它也不是 JavaScript 的别名。

Node.js 是一个可以执行 JavaScript 代码的软件程序。更准确地说，Node.js 是一个 JavaScript 运行环境。它是一个开发环境，可以将 JavaScript 代码用于服务器端脚本。

Node.js 主要是用 C/C++ 编写的。作为一个运行 Web 服务器的程序，Node.js 需要不断地与设备的操作系统进行交互。

使用像 C 这样的低级语言构建 Node.js 使软件可以轻松访问操作系统的资源并使用它们来执行指令。

但是 Node.js 的工作方式涉及更多的复杂问题。Node.js 运行快速高效的 Web 服务器，但它究竟是如何做到的呢？本节介绍 Node.js 用于实现其效率的过程。

要了解 Node.js 的工作原理，我们必须了解三个主要组件。这些组件是：

+ V8引擎
+ 利布夫
+ 事件循环

## V8

V8 引擎是在 Chrome 浏览器中解释和运行 JavaScript 代码的 JavaScript 引擎。其他一些浏览器使用不同的引擎，例如，Firefox 使用 SpiderMonkey，而Safari 使用 JavaScriptCore。没有 JavaScript 引擎，计算机就无法理解 JavaScript。

V8 引擎包含内存堆和调用栈。它们是 V8 引擎的基石。它们帮助管理 JavaScript 代码的执行。

内存堆是 V8 引擎的数据存储。每当我们在 JavaScript 中创建一个包含对象或函数的变量时，引擎都会将该值保存在内存堆中。为简单起见，它类似于为徒步旅行者存放用品的背包。

每当引擎执行代码并遇到任何这些变量时，它都会从内存堆中查找实际值——就像每当徒步旅行者感到寒冷并想要生火时，他们可以在背包中寻找打火机一样。

对内存堆的理解要深入得多。JavaScript 中的内存管理是一个需要更多时间解释的主题，因为实际过程非常复杂。

调用堆栈是 V8 引擎中的另一个构建块。它是一种数据结构，用于管理要执行的函数的顺序。每当程序调用一个函数时，该函数都会被放置在调用堆栈中，并且只有在引擎处理完该函数后才能离开堆栈。

JavaScript 是一种单线程语言，这意味着它一次只能执行一条指令。由于调用栈包含指令执行的顺序，这意味着JavaScript引擎只有一种顺序，一种调用栈。在此处阅读有关单线程和调用堆栈的更多信息。

![img](https://pic3.zhimg.com/80/v2-459d7e1d3617725b114d7f333358cfca_720w.webp)

## Libuv

除了 V8 引擎，Node.js 的另一个非常重要的组件是Libuv。Libuv 是一个用于执行**输入/输出 (I/O)**操作的 C 库。

I/O 操作与向计算机发送请求和接收响应有关。这些操作包括读写文件、发起网络请求等。



![img](https://pic3.zhimg.com/80/v2-ffa50d1358f9e1eacad824f289e771fe_720w.webp)



从 Libuv 的官方网站上，他们声明：

> Libuv 是一个专注于异步 I/O 的多平台支持库。

这意味着 Libuv 是跨平台的（可以在任何操作系统上运行）并且专注于异步 I/O。

计算机往往需要时间来处理 I/O 指令，但 Libuv（Node.js 用来与计算机交互的库）专注于异步 I/O。它可以一次处理多个 I/O 操作。

这就是让 Node.js 在单线程的情况下仍能高效处理 I/O 指令的原因。这都是因为 Libuv。Libuv 知道如何异步处理请求，从而最大限度地减少延迟。但是 JavaScript 引擎究竟是如何使用 Libuv 的呢？

每当我们将脚本传递给 Node.js 时，引擎都会解析代码并开始处理它。调用堆栈保存调用的函数并跟踪程序。如果 V8 引擎遇到 I/O 操作，它将把该操作传递给 Libuv。然后 Libuv 执行 I/O 操作。

请注意，Libuv 是一个 C 库。我们如何使用JavaScript代码来运行C指令呢？有一些**绑定**将 JavaScript 函数连接到它们在 Libuv 中的实际实现。这些绑定使得将 JavaScript 代码用于 I/O 指令成为可能。

Node.js 使用 Libuv 进行实际实现，但公开了应用程序编程接口 (API)。因此，我们现在可以使用 Node.js API（看起来像 JavaScript 函数）来启动 I/O 操作。

需要注意的一件有趣的事情是，JavaScript 确实是一种单线程语言，但是 Libuv（Node.js 使用的低级库）可以在操作系统中执行指令时使用线程池（多线程） .

现在，您在使用 Node.js 时不必担心这些线程。Libuv 知道如何有效地管理它们。您只需使用提供的 Node.js API 来编写指令。

![img](https://pic4.zhimg.com/80/v2-19435c10177b00d2d1150077009c2d8b_720w.webp)

Libuv 最初是为 Node.js 创建的，但现在不同的编程语言都有它的绑定。Julia和Luvit（基于 Lua 的运行时环境）具有内置的绑定，就像 Node.js 一样，但其他语言具有提供这些绑定的库。Python 中的 uvloop就是一个例子。

## 事件循环

Node.js 中的事件循环是该过程中非常重要的部分。从名字就可以看出是一个循环。当 Node.js 开始执行程序时，循环开始运行。在本节中，我们将研究事件循环的作用。

当我们运行包含一些异步代码（如 I/O 指令或基于计时器的操作）的 JavaScript 程序时，Node.js 使用 Node.js API 处理它们。异步函数通常有指令在函数完成处理后执行。这些指令放在**回调队列**中。

回调队列采用**先进先出 (FIFO)**方法。这意味着进入队列的第一条指令（回调）最先被调用。

当事件循环运行时，它会检查调用堆栈是否为空。如果调用堆栈不为空，则允许正在进行的进程继续。但如果调用堆栈为空，它会将回调队列中的第一条指令发送给 JavaScript 引擎。然后引擎将该指令（函数）放在调用堆栈上并执行它。这与事件循环在浏览器中的工作方式非常相似。

因此，事件循环使用 Node.js 中的 JavaScript V8 引擎从异步指令执行回调。而且它是一个循环，这意味着每次它运行时，它都会检查调用堆栈以了解它是否会删除最重要的回调并将其发送到 JavaScript 引擎。

![img](https://pic1.zhimg.com/80/v2-b1b772bcc6318cc4bb669f0d22d72588_720w.webp)

据说 Node.js 具有事件驱动的架构。这意味着 Node.js 是围绕监听事件并在事件发生时迅速做出反应而构建的。这些事件可以是定时器事件、网络事件等。

Node.js 通过使用事件循环来响应这些事件，在某些事件触发事件后将事件回调加载到引擎。正是出于这个原因，Node.js 非常适合应用程序中的实时数据传输。

# 模块

Node.js 的很多功能都包含在软件附带的模块中。这些模块旨在将程序的构建块拆分为可管理的块，如乐高积木。有了这个，我们只需要导入程序所需的模块。

例如，下面的代码片段导入了一个名为`fs`.

```js
 const fs = require('node:fs')
```

但是我们可以通过其他方式在 Node.js 中使用模块。除了内置模块，我们还可以使用其他开发人员构建的模块（或包）。

**Node Package Manager (NPM)**是与 Node.js 一起提供的软件应用程序。它管理 Node.js 中可用的所有第三方模块。每当您需要第三方包时，您都可以使用`npm install`命令从 NPM 安装它。

导入你从 NPM 安装的模块看起来像这样：

```js
 const newModule = require('newModule')
```

## package.json

package.json 位于模块的目录下，用于定义包的属性。

**属性说明**

+ **name** - 包名。
+ **version** - 包的版本号。
+ **description** - 包的描述。
+ **homepage** - 包的官网 url 。
+ **author** - 包的作者姓名。
+ **contributors** - 包的其他贡献者姓名。
+ **dependencies** - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下。
+ **repository** - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上。
+ **main** - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。
+ **keywords** - 关键字

使用 NPM 下载和发布代码时都会接触到版本号。NPM 使用语义版本号来管理代码，这里简单介绍一下。

语义版本号分为X.Y.Z三位，分别代表主版本号、次版本号和补丁版本号。当代码变更时，版本号按以下原则更新。

+ 如果只是修复bug，需要更新Z位。
+ 如果是新增了功能，但是向下兼容，需要更新Y位。
+ 如果有大变动，向下不兼容，需要更新X位。

版本号有了这个保证后，在申明第三方包依赖时，除了可依赖于一个固定版本号外，还可依赖于某个范围的版本号。例如"argv": "0.0.x"表示依赖于0.0.x系列的最新版argv。

如果你遇到了使用 npm 安 装node_modules 总是提示报错：报错: **npm resource busy or locked.....**。

可以先删除以前安装的 node_modules :

```shell
npm cache clean
npm install
```

### 描述配置

**name**

项目的名称，如果是第三方包的话，其他人可以通过该名称使用 npm install 进行安装。

```json
"name": "react"
```

**version**

项目的版本号，开源项目的版本号通常遵循 semver 语义化规范，具体规则如下图所示：

![图片](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bae52651b78b466cbcf66ac9ad4ff45f~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

简单介绍一下：

+ `1` 代表主版本号 Major，通常在涉及重大功能更新，产生了破坏性变更时会更新此版本号
+ `2` 代表次版本号 Minor，在引入了新功能，但未产生破坏性变更，依然向下兼容时会更新此版本号
+ `3` 代表修订号 Patch，在修复了一些问题，也未产生破坏性变更时会更新此版本号

除了 X.Y.Z 这样的标准版本号，还有 Pre-release 和 Metadata 来描述项目的测试版本，关于 semver 规范更多的内容，可以参考[juejin.cn/post/712224…](https://juejin.cn/post/7122240572491825160) 。

回到 package.json 的 version 字段，name + version 能共同构成一个完全唯一的项目标识符，所以它两是最重要的两个字段。

```json
"version": "18.2.0"
```

**repository**

项目的仓库地址以及版本控制信息。

```json
"repository": {
  "type": "git",
  "url": "https://github.com/facebook/react.git",
  "directory": "packages/react"
}
```

**description**

项目的描述，会展示在 npm 官网，让别人能快速了解该项目。

```json
"description": "React is a JavaScript library for building user interfaces."
```

**keywords**

一组项目的技术关键词，比如 Ant Design 组件库的 keywords 如下：

```json
"keywords": [
  "ant",
  "component",
  "components",
  "design",
  "framework",
  "frontend",
  "react",
  "react-component",
  "ui"
 ],
```

好的关键词可以帮助别人在 npm 官网上更好地检索到此项目，增加曝光率。

**homepage**

项目主页的链接，通常是项目 github 链接，项目官网或文档首页。

```json
"homepage": "https://reactjs.org/"
```

**bugs**

项目 bug 反馈地址，通常是 github issue 页面的链接。

```json
"bugs": "https://github.com/vuejs/core/issues"
```

**license**

项目的开源许可证。项目的版权拥有人可以使用开源许可证来限制源码的使用、复制、修改和再发布等行为。常见的开源许可证有 BSD、MIT、Apache 等，它们的区别可以参考：如何选择开源许可证？[www.ruanyifeng.com/blog/2011/0…](https://link.juejin.cn?target=https%3A%2F%2Fwww.ruanyifeng.com%2Fblog%2F2011%2F05%2Fhow_to_choose_free_software_licenses.html)

```json
"license": "MIT"
```

**author**

项目作者。

```json
"author": "Li jiaxun",
```

### 文件配置

**files**

项目在进行 npm 发布时，可以通过 files 指定需要跟随一起发布的内容来控制 npm 包的大小，避免安装时间太长。

发布时默认会包括 package.json，license，README 和`main` 字段里指定的文件。忽略 node_modules，lockfile 等文件。

在此基础上，我们可以指定更多需要一起发布的内容。可以是单独的文件，整个文件夹，或者使用通配符匹配到的文件。

```json
"files": [
  "filename.js",
  "directory/",
  "glob/*.{js,json}"
 ]
```

一般情况下，files 里会指定构建出来的产物以及类型文件，而 src，test 等目录下的文件不需要跟随发布。

**type**

在 node 支持 ES 模块后，要求 ES 模块采用 `.mjs` 后缀文件名。只要遇到 `.mjs` 文件，就认为它是 ES 模块。如果不想修改文件后缀，就可以在 package.json文件中，指定 type 字段为 module。

```json
"type": "module"
```

这样所有 `.js` 后缀的文件，node 都会用 ES 模块解释。

```php
# 使用 ES 模块规范
$ node index.js
```

如果还要使用 CommonJS 模块规范，那么需要将 CommonJS 脚本的后缀名都改成`.cjs`，不过两种模块规范最好不要混用，会产生异常报错。

**main**

项目发布时，默认会包括 package.json，license，README 和`main` 字段里指定的文件，因为 main 字段里指定的是项目的入口文件，在 browser 和 Node 环境中都可以使用。

如果不设置 `main` 字段，那么入口文件就是根目录下的 `index.js`。

比如 packageA 的 `main` 字段指定为 index.js。

```json
"main": "./index.js"
```

我们引入 packageA 时，实际上引入的就是 `node_modules/packageA/index.js`。

这是早期只有 CommonJS 模块规范时，指定项目入口的唯一属性。

**browser**

main 字段里指定的入口文件在 browser 和 Node 环境中都可以使用。如果只想在 web 端使用，不允许在 server 端使用，可以通过 browser 字段指定入口。

```json
"browser": "./browser/index.js"
```

**module**

同样，项目也可以指定 ES 模块的入口文件，这就是 module 字段的作用。

```json
"module": "./index.mjs"
```

当一个项目同时定义了 main，browser 和 module，像 webpack，rollup 等构建工具会感知这些字段，并会根据环境以及不同的模块规范来进行不同的入口文件查找。

```json
"main": "./index.js", 
"browser": "./browser/index.js",
"module": "./index.mjs"
```

比如 webpack 构建项目时默认的 target 为 `'web'`，也就是 Web 构建。它的 resolve.mainFeilds 字段默认为 ['browser', 'module', 'main']。

```javascript
module.exports = {
  //...
  resolve: {
    mainFields: ['browser', 'module', 'main'],
  },
};
```

此时会按照 browser -> module -> main 的顺序来查找入口文件。

**exports**

node 在 14.13 支持在 package.json 里定义 exports 字段，拥有了条件导出的功能。

exports 字段可以配置不同环境对应的模块入口文件，并且当它存在时，它的优先级最高。

比如使用 `require` 和 `import` 字段根据模块规范分别定义入口：

```json
"exports": {
  "require": "./index.js",
  "import": "./index.mjs"
 }
}
```

这样的配置在使用 `import 'xxx'` 和 `require('xxx')` 时会从不同的入口引入文件，exports 也支持使用 `browser` 和 `node` 字段定义 browser 和 Node 环境中的入口。

上方的写法其实等同于：

```json
"exports": {
  ".": {
    "require": "./index.js",
    "import": "./index.mjs"
  }
 }
}
```

为什么要加一个层级，把 require 和 import 放在 `"."` 下面呢？

因为 exports 除了支持配置包的默认导出，还支持配置包的子路径。

比如一些第三方 UI 包需要引入对应的样式文件才能正常使用。

```go
import `packageA/dist/css/index.css`;
```

我们可以使用 exports 来封装文件路径：

```json
"exports": {
  "./style": "./dist/css/index.css'
},
```

用户引入时只需：

```go
import `packageA/style`;
```

除了对导出的文件路径进行封装，exports 还限制了使用者不可以访问未在 "exports" 中定义的任何其他路径。

比如发布的 dist 文件里有一些内部模块 `dist/internal/module` ，被用户单独引入使用的话可能会导致主模块不可用。为了限制外部的使用，我们可以不在 exports 定义这些模块的路径，这样外部引入 `packageA/dist/internal/module` 模块的话就会报错。

结合上面入口文件配置的知识，再来看看下方 vite 官网推荐的第三方库入口文件的定义，就很容易理解了。

![图片](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6244d780d3e14168b5deae0ef73b3551~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**workspaces**

项目的工作区配置，用于在本地的根目录下管理多个子项目。可以自动地在 npm install 时将 workspaces 下面的包，软链到根目录的 node_modules 中，不用手动执行 `npm link` 操作。

workspaces 字段接收一个数组，数组里可以是文件夹名称或者通配符。比如：

```json
"workspaces": [
  "workspace-a"
]
```

表示在 workspace-a 目录下还有一个项目，它也有自己的 package.json。

```go
package.json
workspace-a
  └── package.json
```

通常子项目都会平铺管理在 packages 目录下，所以根目录下 workspaces 通常配置为：

```json
"workspaces": [
  "packages/*"
]
```

### 脚本配置

**scripts**

指定项目的一些内置脚本命令，这些命令可以通过 npm run 来执行。通常包含项目开发，构建 等 CI 命令，比如：

```json
"scripts": {
  "build": "webpack"
}
```

我们可以使用命令 `npm run build` / `yarn build` 来执行项目构建。

除了指定基础命令，还可以配合 pre 和 post 完成命令的前置和后续操作，比如：

```json
"scripts": {
  "build": "webpack",
  "prebuild": "xxx", // build 执行之前的钩子
  "postbuild": "xxx" // build 执行之后的钩子
}
```

当执行 `npm run build` 命令时，会按照 prebuild -> build -> postbuild 的顺序依次执行上方的命令。

但是这样的隐式逻辑很可能会造成执行工作流的混乱，所以 pnpm 和 yarn2 都已经废弃掉了这种 pre/post 自动执行的逻辑，参考 pnpm issue 讨论 [github.com/pnpm/pnpm/i…](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fpnpm%2Fpnpm%2Fissues%2F2891)

如果需要手动开启，pnpm 项目可以设置 `.npmrc` enable-pre-post-scripts=true。

```ini
enable-pre-post-scripts=true
```

npm 脚本的原理非常简单。每当执行`npm run`，就会自动新建一个 Shell，在这个 Shell 里面执行指定的脚本命令。

因此，只要是 Shell（Bash）可以运行的命令，就可以写在 npm 脚本里面。

比较特别的是，`npm run`新建的这个 Shell，会将当前目录的`node_modules/.bin`子目录加入`PATH`变量，执行结束后，再将`PATH`变量恢复原样。

这意味着，当前目录的`node_modules/.bin`子目录里面的所有脚本，都可以直接用脚本名调用，而不必加上路径。比如，当前项目的依赖里面有 Mocha，只要直接写`mocha test`就可以了。

由于 npm 脚本的唯一要求就是可以在 Shell 执行，因此它不一定是 Node 脚本，任何可执行文件都可以写在里面。npm 脚本的退出规则，也遵守 Shell 脚本规则。如果退出码不是`0`，npm 就认为这个脚本执行失败。

由于 npm 脚本就是 Shell 脚本，因为可以使用 Shell 通配符。`*`表示任意文件名，`**`表示任意一层子目录。如果要将通配符传入原始命令，防止被 Shell 转义，要将星号转义。

向 npm 脚本传入参数，要使用`--`标明。

如果 npm 脚本里面需要执行多个任务，那么需要明确它们的执行顺序。

如果是并行执行（即同时的平行执行），可以使用`&`符号。如果是继发执行（即只有前一个任务成功，才执行下一个任务），可以使用`&&`符号。

npm 脚本有一个非常强大的功能，就是可以使用 npm 的内部变量。通过`npm_package_`前缀，npm 脚本可以拿到`package.json`里面的字段。

npm 脚本还可以通过`npm_config_`前缀，拿到 npm 的配置变量，即`npm config get xxx`命令返回的值。

**config**

config 用于设置 scripts 里的脚本在运行时的参数。比如设置 port 为 3001：

```json
"config": {
  "port": "3001"
}
```

在执行脚本时，我们可以通过 `npm_package_config_port` 这个变量访问到 3001。

```javascript
console.log(process.env.npm_package_config_port); // 3001
```

### 依赖配置

**dependencies**

运行依赖，也就是项目生产环境下需要用到的依赖。比如 react，vue，状态管理库以及组件库等。

使用 `npm install xxx` 或则 `npm install xxx --save` 时，会被自动插入到该字段中。

```json
"dependencies": {
  "react": "^18.2.0",
  "react-dom": "^18.2.0"
}
```

**devDependencies**

开发依赖，项目开发环境需要用到而运行时不需要的依赖，用于辅助开发，通常包括项目工程化工具比如 webpack，vite，eslint 等。

使用 `npm install xxx -D` 或者 `npm install xxx --save-dev` 时，会被自动插入到该字段中。

```json
"devDependencies": {
  "webpack": "^5.69.0"
}
```

**peerDependencies**

同伴依赖，一种特殊的依赖，不会被自动安装，通常用于表示与另一个包的依赖与兼容性关系来警示使用者。

比如我们安装 A，A 的正常使用依赖 [B@2.x](https://link.juejin.cn?target=mailto%3AB@2.x) 版本，那么 [B@2.x](https://link.juejin.cn?target=mailto%3AB@2.x) 就应该被列在 A 的 peerDependencies 下，表示“如果你使用我，那么你也需要安装 B，并且至少是 2.x 版本”。

比如 React 组件库 Ant Design，它的 package.json 里 peerDependencies 为

```json
"peerDependencies": {
  "react": ">=16.9.0",
  "react-dom": ">=16.9.0"
}
```

表示如果你使用 Ant Design，那么你的项目也应该安装 react 和 react-dom，并且版本需要大于等于 16.9.0。

**optionalDependencies**

可选依赖，顾名思义，表示依赖是可选的，它不会阻塞主功能的使用，安装或者引入失败也无妨。这类依赖如果安装失败，那么 npm 的整个安装过程也是成功的。

比如我们使用 colors 这个包来对 console.log 打印的信息进行着色来增强和区分提示，但它并不是必需的，所以可以将其加入到 optionalDependencies，并且在运行时处理引入失败的逻辑。

使用 `npm install xxx -O` 或者 `npm install xxx --save-optional` 时，依赖会被自动插入到该字段中。

```json
"optionalDependencies": {
  "colors": "^1.4.0"
}
```

**peerDependenciesMeta**

同伴依赖也可以使用 peerDependenciesMeta 将其指定为可选的。

```json
"peerDependencies": {
  "colors": "^1.4.0"
},
"peerDependenciesMeta": {
  "colors": {
    "optional": true
   }
 }
```

**bundleDependencies**

打包依赖。它的值是一个数组，在发布包时，bundleDependencies 里面的依赖都会被一起打包。

比如指定 react 和 react-dom 为打包依赖：

```json
"bundleDependencies": [
  "react",
  "react-dom"
]
```

在执行 `npm pack` 打包生成 tgz 压缩包中，将出现 node_modules 并包含 react 和 react-dom。

> 需要注意的是，这个字段数组中的值必须是在 dependencies，devDependencies 两个里面声明过的依赖才行。

普通依赖通常从 npm registry 安装，但当你想用一个不在 npm registry 里的包，或者一个被修改过的第三方包时，打包依赖会比普通依赖更好用。

**overrides**

overrides 可以重写项目**依赖的依赖，及其依赖树下某个依赖**的版本号，进行包的替换。

比如某个依赖 A，由于一些原因它依赖的包 foo@1.0.0 需要替换，我们可以使用 overrides 修改 foo 的版本号：

```json
"overrides": {
  "foo": "1.1.0-patch"
}
```

当然这样会更改整个依赖树里的 foo，我们可以只对 A 下的 foo 进行版本号重写：

```css
"overrides": {
  "A": {
    "foo": "1.1.0-patch",
  }
}
```

overrides 支持任意深度的嵌套。

> 如果在 yarn 里也想复写依赖版本号，需要使用 resolution 字段，而在 pnpm 里复写版本号需要使用 `pnpm.overrides` 字段。

### 发布配置

**private**

如果是私有项目，不希望发布到公共 npm 仓库上，可以将 private 设为 true。

```json
"private": true
```

**publishConfig**

顾名思义，publishConfig 就是 npm 包发布时使用的配置。

比如在安装依赖时指定了 registry 为 taobao 镜像源，但发布时希望在公网发布，就可以指定 publishConfig.registry。

```json
"publishConfig": {
  "registry": "https://registry.npmjs.org/"
}
```

### 系统配置

**engines**

一些项目由于兼容性问题会对 node 或者包管理器有特定的版本号要求，比如：

```json
"engines": {
  "node": ">=14 <16",
  "pnpm": ">7"
}
```

要求 node 版本大于等于 14 且小于 16，同时 pnpm 版本号需要大于 7。

**os**

在 linux 上能正常运行的项目可能在 windows 上会出现异常，使用 os 字段可以指定项目对操作系统的兼容性要求。

```json
"os": ["darwin", "linux"]
```

**cpu**

指定项目只能在特定的 CPU 体系上运行。

```json
"cpu": ["x64", "ia32"]
```

### 第三方配置

**types 或者 typings**

指定 TypeScript 的类型定义的入口文件

```json
"types": "./index.d.ts",
```

**unpkg**

可以让 `npm` 上所有的文件都开启 CDN 服务。

比如 vue package.json 的 unpkg 定义为 `dist/vue.global.js`

```json
"unpkg": "dist/vue.global.js",
```

当我们想通过 CDN 的方式使用链接引入 vue 时。

访问 `https://unpkg.com/vue` 会重定向到 `https://unpkg.com/vue@3.2.37/dist/vue.global.js`，其中 3.2.27 是 Vue 的最新版本。

**jsdelivr**

与 unpkg 类似，vue 通过如下的配置

```json
"jsdelivr": "dist/vue.global.js",
```

访问 `https://cdn.jsdelivr.net/npm/vue` 实际上获取到的是 jsdelivr 字段里配置的文件地址。

![图片](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e1bd8184d60547caaf432bdba77b05c4~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**browserslist**

设置项目的浏览器兼容情况。babel 和 autoprefixer 等工具会使用该配置对代码进行转换。当然你也可以使用 `.browserslistrc` 单文件配置。

```json
"browserslist": [
  "> 1%",
  "last 2 versions"
]
```

**sideEffects**

显示设置某些模块具有副作用，用于 webpack 的 tree-shaking 优化。

比如在项目中整体引入 Ant Design 组件库的 css 文件。

```arduino
import 'antd/dist/antd.css'; // or 'antd/dist/antd.less'
```

如果 Ant Design 的 package.json 里不设置 sideEffects，那么 webapck 构建打包时会认为这段代码只是引入了但并没有使用，可以 tree-shaking 剔除掉，最终导致产物缺少样式。

所以 Ant Design 在 package.json 里设置了如下的 sideEffects，来告知 webpack，这些文件具有副作用，引入后不能被删除。

```json
json复制代码"sideEffects": [
  "dist/*",
  "es/**/style/*",
  "lib/**/style/*",
  "*.less"
]
```

**lint-staged**

lint-staged 是用于对 git 的暂存区的文件进行操作的工具，比如可以在代码提交前执行 lint 校验，类型检查，图片优化等操作。

```css
"lint-staged": {
  "src/**/*.{js,jsx,ts,tsx}": [
    "eslint --fix",
    "git add -A"
  ]
}
```

lint-staged 通常配合 husky 这样的 git-hooks 工具一起使用。git-hooks 用来定义一个钩子，这些钩子方法会在 git 工作流程中比如 pre-commit，commit-msg 时触发，可以把 lint-staged 放到这些钩子方法中。

## 系统

不建议同时使用 exports 和 module.exports。

如果先使用 exports 对外暴露属性或方法，再使用 module.exports 暴露对象，会使得 exports 上暴露的属性或者方法失效。

原因在于，exports 仅仅是 module.exports 的一个引用。在 nodejs 中，是这么设计 require 函数的：

```javascript
function require(...){
  var module = {exports: {}};

  ((module, exports) => {
    function myfn () {}
    // 这个myfn就是我们自己的代码
    exports.myfn = myfn; // 这里是在原本的对象上添加了一个myfn方法。
    module.exports = myfn;// 这个直接把当初的对象进行覆盖。
  })(module,module.exports)
  return module.exports;
}
```

# 事件

根据前面的 event 绑定，直接将事件名定义成路径还是很不错的，不过要区分 request.method 是 get 还是 post ，可以分两个文件处理

```javascript
var http = require("http");
var url = require('url');
const { exit } = require("process");
var events = require('events');

// 创建 eventEmitter 对象 
var eventEmitter = new events.EventEmitter();

// route 根路径 
eventEmitter.on('/', function(method, response){
    response.writeHead(200, {'Content-Type': 'text/plain'});
    response.end('Hello World\n');
});

// route 404 
eventEmitter.on('404', function(method, url, response){
    response.writeHead(404, {'Content-Type': 'text/plain'});
    response.end('404 Not Found\n');
});

// 启动服务 
http.createServer(function (request, response) {
    console.log(request.url);

    // 分发 
    if (eventEmitter.listenerCount(request.url) > 0){
        eventEmitter.emit(request.url, request.method, response);
    }
    else {
        eventEmitter.emit('404', request.method, request.url, response);
    }
    
}).listen(8888);

console.log('Server running at http://127.0.0.1:8888/');
```

# 全局变量

## process

process是node的全局模块，作用比较直观。可以通过它来获得node进程相关的信息，比如运行node程序时的命令行参数。或者设置进程相关信息，比如设置环境变量。

process 是Node中的线程容器，每一个Node应用会有一个process，开发者可以通过process对象控制和获取当前Node的进程。

**Note** 每一个`npm run start`起的项目都是一个Node应用，都会拥有一个process对象。

### 环境变量

process.env 使用频率很高，node服务运行时，时常会判断当前服务运行的环境，如下所示

```JS
JS复制代码if(process.env.NODE_ENV === 'production'){
    console.log('生产环境');
}else{
    console.log('非生产环境');
}
```

运行命令 `NODE_ENV=production node env.js`，输出如下

```
非生产环境
```

new webpack.DefinePlugin({
'process.env.NODE_ENV': '"development"',
'process.env.BASE_API':'"/api/"'
})

1. DefinePlugin是可以将变量暴露在整个项目中,方便在前端项目中调用,webpack在构建(编译)过程中直接替换掉 ,相当于c中预定义定义的常量
2. dotenv的作用是为了方便将大量的将.env文件所有的key-value赋值增加到process.env上面去



3. cross-env 是为了兼容 package中script的命令行是需要考虑在不同系统中的不同写法 NODE_ENV='production' 在windows必须写成set NODE_ENV="production"