# 文件结构

## .iml 文件

dea 对[module](https://so.csdn.net/so/search?q=module&spm=1001.2101.3001.7020) 配置信息之意， infomation of module。每个模块都有一个iml文件。

IDEA中的`.iml`文件是项目标识文件，缺少了这个文件，IDEA就无法识别项目。跟[Eclipse](https://so.csdn.net/so/search?q=Eclipse&spm=1001.2101.3001.7020)的`.project`文件性质是一样的。并且这些文件不同的设备上的内容也会有差异，所以我们在管理项目的时候，`.project和.iml文件都需要忽略掉`。

iml文件是IntelliJ IDEA自己创建的模块文件，用于java应用开发，存储一些模块相关的信息，比如一个Java组建，插件组建，[Maven](https://so.csdn.net/so/search?q=Maven&spm=1001.2101.3001.7020)组建等。存储一些模块路径信息，依赖信息以及别的一些设置。

# 项目结构

IDEA 中工程（project）配好我们的 JDK 版本，一个工程可以包含多个模块（moduel），我们开发的一个项目包含一个或多个模块，比如 Web 模块、Spring 模块，还要配好这些模块的配置信息。我们通过 Library 把我们项目中所需依赖 JAR 包导好。

Facet 声明了我们每个模块使用技术的配置（注意和 moduel 结合区分理解），最重要的是在这里配好 web.xml 和 web 资源目录的位置。

在我们把项目发布到 Tomcat 或导出成 JAR 包之前，我们需要把项目打成 artifact 来方面部署，通常我们选择 war_exploded 热部署。

## Project

idea 中没有工作空间的概念，每一个项目都是一个工作空间，一个项目将一个项目的所有源代码、库和指令封装到一个单独的组织单元中。使用 IntelliJ 平台 SDK 完成的所有工作。

项目定义了称为模块和库的集合。project 下的 module 的概念类似于 eclipse 中的 project 。一个聚合项目可以配置多个 module（可以当作 workspace 来用）。

![img](D:\Repository\Typora\v2-da26a0438a5296fe6c74b9c7018bc1fe_720w.webp)

- **Project name**：定义项目的名称；
- **Project SDK：**设置该项目使用的 JDK，也可以在此处新添加其他版本的 JDK；
- **Project language level：**这个和 JDK 的类似，区别在于，假如你设置了 JDK1.8，却只用到 1.6 的特性，那么这里可以设置语言等级为 1.6，这个是限定项目编译检查时最低要求的 JDK 特性；如果是多个Module（可以理解为一组项目）的话，对所有 Module 生效。
- **Project compiler output：**项目中的默认编译输出总目录，如图黄色部分，实际上每个模块可以自己设置特殊的输出目录（Modules - (project) - Paths - Use module compile output path），所以这个设置有点鸡肋。
  - 但对于多个 Module 项目时，会出现大家共用一个 output 目录。此时如果输出的日志文件路径用 “./log” 这种形式时，日志可能并不会打印到其中的子项目中，而是输出在此处指定的路径下了。

## Module

模块是一个独立的功能单元，可以独立地运行、测试和调试。模块包括诸如源代码、构建脚本、单元测试、部署描述符、依赖管理等。

在一个项目中，每个模块都可以使用特定的 SDK 或继承在项目级别定义的 SDK 。一个模块可以依赖于项目的其他模块。

一个项目可以有一个或多个模块，比如 Spring，Web 。这些模块都是现有的，可用的。

![img](D:\Repository\Typora\v2-9e6833535bb4c9a6606a8d9242bb5a4d_720w.webp)

项目目录结构。对Module的开发目录进行文件夹分类，不同类型的文件进行指定的文件类型。上面分了 Sources、Test、Resources、Test Resources、Excluded。

### 增删子项目

![img](D:\Repository\Typora\v2-cd59db4af828a28b7825be9b22de37c4_720w.webp)

一个项目中可以有多个子项目，每个子项目相当于一个模块。一般我们项目只是单独的一个，IntelliJ IDEA 默认也是单子项目的形式，所以只需要配置一个模块。
（此处的两个项目引入仅作示例参考）

### 子项目配置

![img](D:\Repository\Typora\v2-bd8a0064658fab93ffbc92d651f8bf73_720w.webp)

![img](D:\Repository\Typora\v2-458a5d74b07de25c6e4a61dcec70142e_720w.webp)

![img](D:\Repository\Typora\v2-44113ad2a1521cd7f4fe3fbb8f9d75c8_720w.webp)

每个子项目都对应了 Sources、Paths、Dependencies 三大配置选项：

- **Sources：**显示项目的目录资源，那些是项目部署的时候需要的目录，不同颜色代表不同的类型；
- **Paths：**可以指定项目的编译输出目录，即项目类和测试类的编译输出地址（替换掉了Project的默认输出地址）
- **Dependencies：**项目的依赖

顾名思义，Sources 放的是 Java 源码，Test 放的是测试的源码，Resources 放的是资源文件，Test Resources 放的是测试使用的资源文件，Excluded 是排除项（比如编译后的 trarget 目录）。

### 增删框架

每个子项目之下都可以定义它所使用的框架，这里重点说明一下 Web 部分的设置。

![img](D:\Repository\Typora\v2-4eac657a7c74df0a6bc73e66ea657e1b_720w.webp)

## Library

库是模型依赖的代码集合文件（比如 JAR 文件），可以从自己选择 jar 包或者用 maven 。

这里可以显示所添加的 jar 包，同时也可以添加 jar 包，并且可以把多个 jar 放在一个组里面，类似于 jar 包整理。这里默认将每个 jar 包做为了一个单独的组（未测试，待定）。

**IDEA中的库（Libraries）就是用来存放外部 jar 包，我们的项目或模块需要某些 jar 包时，可以从这里把包导入到模块依赖（Dependencies）中。**



## Facet

facet 声明了每个模块使用技术的一些配置。一个模块可能有多个 facet。Spring 模块的配置就声明在 Spring facet 中。

Facets 表述了在 Module 中使用的各种各样的框架、技术和语言。这些 Facets 让 Intellij IDEA 知道怎么对待 module 内容，并保证与相应的框架和语言保持一致。Facets 表示某个 module 有的特征，比如 web、strtus2、spring、hibernate 等；

Spring 的主类和配置文件是 Spring 的一些配置，但是不告诉 IDEA 在哪他自己是不知道在哪的，所以在 facet 中要写清楚在什么地方（默认会配好）。

在 web 模块中，需要配置 web.xml 和 web 资源目录的位置，也可以直接用默认的，一般我们建立 `/src/main/webapp` 。

> web 资源目录的文件夹有一个小蓝点，访问路径时 "/" 就是从这里开始找资源的。

官方的解释是：

> When you select a framework (a facet) in the element selector pane, the settings for the framework are shown in the right-hand part of the dialog.

（当你在左边选择面板点击某个技术框架，右边将会显示这个框架的一些设置）

![img](D:\Repository\Typora\v2-c8fdc732e0285cbe85c282ae0a5fd507_720w.webp)

> 比如我们现在要开发的是一个 web 项目，那就需要 web 相关的 Facet，事实上，如果没有这个配置支持，编译器也不知道这个项目是个 web 项目，也就不会去读取 web.xml 的配置，更无法被 tomcat 这种容器支持。

Facet 是和 Module 紧密结合的，你如果是在 Module 里配置了，那么 Facet 里边也会出现，而如果你先在 Facet 里配置，它会要求你选择 Module，所以结果是一致的。

## Artifact

artifact 的中文释义是工程的意思。项目的打包部署设置，这个是项目配置里面比较关键的地方，重点说一下。

先理解下它的含义，来看看官方定义的 artifacts：

> An artifact is an assembly of your project assets that you put together to test, deploy or distribute your software solution or its part. Examples are a collection of compiled Java classes or a Java application packaged in a Java archive, a Web application as a directory structure or a Web application archive, etc.

即编译后的 Java 类，Web 资源等的整合，用以测试、部署等工作。再白话一点，就是说某个 module 要如何打包，例如 war exploded、war、jar、ear等等这种打包形式。某个module有了 Artifacts 就可以部署到应用服务器中了。

![img](D:\Repository\Typora\v2-cb40b3f39715e0e6d056d29c5a3af601_720w.webp)

artifact 是放在一起测试、部署或发布您的软件解决方案或其部分的项目资产的集合（结合 maven）。例如，已编译的 Java 类的集合或打包在 Java 归档中的 Java应用程序、作为目录结构的 Web 应用程序等等。

artifact 就是为了打包成为 jar 或 war 的一个配置声明。比如你想分享你的项目给小明或者想把项目发布到 Tomcat 上，如何分享或发布？java 提供了专门打包的方法，就是 jar 和 war，而打成 JAR 和 WAR 必须需要有一个 artifact 。

在 web 项目中常用于配置 Tomcat ， 在 Maven 中配置依赖。

Artifact 是 Maven中的一个概念，表示某个 module 要如何打包，这又多个模块的概念，例如 war exploded、war、jar、ear 等等这种打包形式；一个 module 有了 Artifacts 就可以部署到应用服务器中。

+ jar：Java ARchive，通常用于聚合大量的 Java 类文件、相关的元数据和资源（文本、图片等）文件到一个文件，以便分发 Java 平台应用软件或库；
+ war：Web application ARchive，一种 JAR 文件，其中包含用来分发的 JSP、Java Servlet、Java 类、XML 文件、标签库、静态网页（HTML 和相关文件），以及构成 Web 应用程序的其他资源；
+ exploded：在这里你可以理解为展开，不压缩的意思。也就是 war、jar 等产出物没压缩前的目录结构。建议在开发的时候使用这种模式，便于修改了文件的效果立刻显现出来。

其实，实际上，当你点击运行 tomcat 时，默认就开始做以下事情：

- 编译，IDEA 在保存/自动保存后不会做编译，不像 Eclipse 的保存即编译，因此在运行 server 前会做一次编译。编译后 class 文件存放在指定的项目编译输出目录下；
- 根据 artifact 中的设定对目录结构进行创建；
- 拷贝 web 资源的根目录下的所有文件到 artifact 的目录下；
- 拷贝编译输出目录下的 classes 目录到 artifact 下的WEB-INF下；
- 拷贝 lib 目录下所需的 jar 包到 artifact 下的 WEB_INF 下；
- 运行 server，运行成功后，如有需要，会自动打开浏览器访问指定 url 。

在这里还要注意的是，配置完成的 artifact，需要在 tomcat 中进行添加：

![img](D:\Repository\Typora\v2-6e47483c5a1eb70b18752f01e4135f98_720w.webp)

