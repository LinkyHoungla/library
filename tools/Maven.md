# 介绍

Maven，源于"专家"和"内行"的翻译，是基于 Apache 的纯 Java 开发开源项目。它运用项目对象模型（POM）概念，利用一个中央信息片断管理项目的构建、报告、文档等步骤。

Maven 是 Apache 软件基金会的一个跨平台的项目管理工具，最初为基于 Java 平台的项目创建和依赖管理而设计。但它的作用并不仅限于 Java 项目，还可应用于多种语言的项目，如 C#、Ruby 和 Scala。

**作用**

+ **依赖管理**：Maven 通过统一管理第三方 Jar 包简化了项目的依赖引入和版本控制，避免了 Jar 包冲突问题；自动解决包依赖，使用统一的规范方式下载 jar 包，减少手动导入时间成本。
+ **一键构建项目**：提供了标准的 Java 项目结构，使项目创建变得简单高效。

**特点**

+ **构建任务**：包括清理、编译、测试、报告、打包、安装、部署等环节。
+ **多功能工具**：能进行文档生成、依赖管理、SCMs、发布、分发和邮件列表管理等。
+ **统一规则**：项目设置符合一致的规范。
+ **依赖管理**：自动更新并共享依赖库。
+ **插件扩展**：可轻松编写Java或脚本语言插件。
+ **基于模型构建**：将项目构建到预定义输出类型中。
+ **发布管理**：与源代码管理系统集成，管理项目的发布。
+ **向后兼容性**：易于从旧版本迁移。
+ **并行构建**：提高编译速度。
+ **优化错误报告**：提供更清晰详细的错误信息。

# 安装

1. **下载 Maven：**
   + 访问 [Apache Maven 官网](https://maven.apache.org/) 下载最新版本的 Maven 压缩包。
2. **解压缩：**
   + 将下载的压缩包解压到本地文件夹。
3. **配置环境变量：**
   + 在系统环境变量中新建 `MAVEN_HOME` 变量，将 Maven 的安装路径设置为其值。
   + 将 `%MAVEN_HOME%\bin` 添加到 `PATH` 环境变量中。
4. **验证安装：**
   + 打开命令行窗口，输入 `mvn -version` 验证 Maven 是否成功安装。

## 文件目录

```
apache-maven
|-- bin               				# 可执行文件
|   |-- mvn           				# Maven的主要命令行工具
|   |-- mvnDebug     				 	# 用于调试的Maven命令行工具
|   |-- ...
|-- boot              				# Maven启动器和引导类库
|-- conf              				# Maven配置文件
|   |-- settings.xml  				# Maven的主要配置文件，配置全局选项、仓库等
|   |-- ...
|-- lib               				# Maven运行时所需的库文件
|   |-- maven-core-x.x.x.jar  # Maven核心库
|   |-- ...
|-- LICENSE           				# 许可证文件
|-- NOTICE            				# 通知文件
|-- README.txt        				# Maven的README文件
```

+ **`bin` 目录：** 包含 Maven 的可执行文件。
  + `mvn`：Maven 的主要命令行工具。
  + `mvnDebug`：用于调试的 Maven 命令行工具。
  + 其他可执行文件用于不同操作系统。
+ **`boot` 目录：** 包含 Maven 启动器和引导类库。
  + `plexus-classworlds-x.x.x.jar`：Maven 的类加载器。
+ **`conf` 目录：** Maven 的配置文件。
  + `settings.xml`：Maven 的主要配置文件，用于配置全局选项、仓库等。
+ **`lib` 目录：** 包含 Maven 运行时所需的库文件。
  + `maven-core-x.x.x.jar`：Maven 核心库。
  + 其他 Maven 运行所需的 JAR 文件。
+ **`LICENSE` 文件：** Maven 的许可证文件，规定了软件使用的许可条件。
+ **`NOTICE` 文件：** Maven 的通知文件，包含了软件的版权信息和其他信息。
+ **`README.txt` 文件：** Maven 的说明文件，提供了 Maven 的简要介绍和安装指南。

## POM

POM（Project Object Model）是 Maven 工程的基本工作单元，是一个 XML 文件，用于描述项目的基本信息、构建方式、依赖关系等。

POM 文件是 Maven 构建过程的核心，包含配置信息，例如：

+ **项目依赖：** 声明项目所需的外部依赖库。
+ **插件和目标：** 配置执行构建过程中需要的插件和目标。
+ **构建 profile：** 定义不同环境下的构建配置，如开发环境、测试环境、生产环境等。
+ **项目开发者列表和相关邮件列表信息：** 记录项目的开发者信息和相关沟通渠道。

所有 POM 文件都需要 project 元素和三个必需字段：groupId，artifactId，version。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <!-- 模型版本 -->
    <modelVersion>4.0.0</modelVersion>
    
    <!-- 公司或组织的唯一标识 -->
    <groupId>com.companyname.project-group</groupId>
    
    <!-- 项目的唯一ID -->
    <artifactId>project</artifactId>
    
    <!-- 版本号 -->
    <version>1.0</version>
</project>
```

+ **`project` 标签：** 工程的根标签，所有的配置信息都在此标签内部。
+ **`modelVersion`：** POM 模型版本号，固定为 4.0.0，表示遵循 Maven POM 模型的规范版本。
+ **`groupId`：** 项目所属的组织或公司的唯一标识符，通常以域名的方式命名，确保唯一性。
+ **`artifactId`：** 项目的唯一标识符，代表项目名称，与 `groupId` 组合构成了唯一的项目标识。
+ **`version`：** 项目的版本号，用于区分不同的项目版本，也用于在 Maven 仓库中识别不同的构建。

### 父 POM

父 POM 是指导多个子模块的 Maven 项目的顶层 POM 文件。它通过指定通用配置和依赖，实现了子模块之间共享配置和依赖的功能，从而提高了项目的管理和维护效率。

+ **继承关系：** 父 POM 提供了通用配置和依赖信息，子模块通过在其 POM 文件中声明父 POM，继承、覆盖和补充父 POM 中的配置信息，从而减少重复配置。
+ **集中管理：** 父 POM 可以集中管理项目的版本号、依赖库版本等信息，确保各个子模块的一致性。

父 POM 本身的结构和内容与普通 POM 类似，但主要用于定义全局配置和依赖。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <!-- 模型版本 -->
    <modelVersion>4.0.0</modelVersion>
    
    <!-- 公司或组织的唯一标识 -->
    <groupId>com.companyname.parent</groupId>
    
    <!-- 项目的唯一ID -->
    <artifactId>parent</artifactId>
    
    <!-- 版本号 -->
    <version>1.0</version>
    
    <!-- 通用依赖 -->
    <dependencies>
        <!-- 依赖1 -->
        <dependency>
            <!-- 依赖1的坐标 -->
        </dependency>
        
        <!-- 依赖2 -->
        <dependency>
            <!-- 依赖2的坐标 -->
        </dependency>
        <!-- 更多依赖... -->
    </dependencies>
    
    <!-- 其他配置信息 -->
    <!-- 构建配置、插件配置、profiles 等 -->
</project>
```

### Super POM

Super POM 是 Maven 默认的 POM，它为所有的 Maven 项目提供了通用的默认配置和依赖关系。即使在项目中没有显式定义父 POM，所有的 POM 都隐式继承自这个父 POM。这个父 POM 包含了一些默认的设置，为所有的 Maven 项目提供了基本的配置信息。

+ **默认仓库：** 父 POM 配置了默认的仓库地址为 http://repo1.maven.org/maven2。当 Maven 需要下载项目中声明的依赖时，会首先到这个默认仓库中寻找相应的依赖。
+ **默认配置：** 父 POM 包含了一些默认的配置，例如默认的构建插件版本、编译器版本等，为项目提供了基本的设置。





```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0http://maven.apache.org/maven-v4_0_0.xsd">
    <!--父项目的坐标。如果项目中没有规定某个元素的值，那么父项目中的对应值即为项目的默认值。 坐标包括group ID，artifact ID和 
        version。 -->
    <parent>
        <!--被继承的父项目的构件标识符 -->
        <artifactId />
        <!--被继承的父项目的全球唯一标识符 -->
        <groupId />
        <!--被继承的父项目的版本 -->
        <version />
        <!-- 父项目的pom.xml文件的相对路径。相对路径允许你选择一个不同的路径。默认值是../pom.xml。Maven首先在构建当前项目的地方寻找父项 
            目的pom，其次在文件系统的这个位置（relativePath位置），然后在本地仓库，最后在远程仓库寻找父项目的pom。 -->
        <relativePath />
    </parent>
    <!--声明项目描述符遵循哪一个POM模型版本。模型本身的版本很少改变，虽然如此，但它仍然是必不可少的，这是为了当Maven引入了新的特性或者其他模型变更的时候，确保稳定性。 -->
    <modelVersion>4.0.0</modelVersion>
    <!--项目的全球唯一标识符，通常使用全限定的包名区分该项目和其他项目。并且构建时生成的路径也是由此生成， 如com.mycompany.app生成的相对路径为：/com/mycompany/app -->
    <groupId>asia.banseon</groupId>
    <!-- 构件的标识符，它和group ID一起唯一标识一个构件。换句话说，你不能有两个不同的项目拥有同样的artifact ID和groupID；在某个 
        特定的group ID下，artifact ID也必须是唯一的。构件是项目产生的或使用的一个东西，Maven为项目产生的构件包括：JARs，源 码，二进制发布和WARs等。 -->
    <artifactId>banseon-maven2</artifactId>
    <!--项目产生的构件类型，例如jar、war、ear、pom。插件可以创建他们自己的构件类型，所以前面列的不是全部构件类型 -->
    <packaging>jar</packaging>
    <!--项目当前版本，格式为:主版本.次版本.增量版本-限定版本号 -->
    <version>1.0-SNAPSHOT</version>
    <!--项目的名称, Maven产生的文档用 -->
    <name>banseon-maven</name>
    <!--项目主页的URL, Maven产生的文档用 -->
    <url>http://www.baidu.com/banseon</url>
    <!-- 项目的详细描述, Maven 产生的文档用。 当这个元素能够用HTML格式描述时（例如，CDATA中的文本会被解析器忽略，就可以包含HTML标 
        签）， 不鼓励使用纯文本描述。如果你需要修改产生的web站点的索引页面，你应该修改你自己的索引页文件，而不是调整这里的文档。 -->
    <description>A maven project to study maven.</description>
    <!--描述了这个项目构建环境中的前提条件。 -->
    <prerequisites>
        <!--构建该项目或使用该插件所需要的Maven的最低版本 -->
        <maven />
    </prerequisites>
    <!--项目的问题管理系统(Bugzilla, Jira, Scarab,或任何你喜欢的问题管理系统)的名称和URL，本例为 jira -->
    <issueManagement>
        <!--问题管理系统（例如jira）的名字， -->
        <system>jira</system>
        <!--该项目使用的问题管理系统的URL -->
        <url>http://jira.baidu.com/banseon</url>
    </issueManagement>
    <!--项目持续集成信息 -->
    <ciManagement>
        <!--持续集成系统的名字，例如continuum -->
        <system />
        <!--该项目使用的持续集成系统的URL（如果持续集成系统有web接口的话）。 -->
        <url />
        <!--构建完成时，需要通知的开发者/用户的配置项。包括被通知者信息和通知条件（错误，失败，成功，警告） -->
        <notifiers>
            <!--配置一种方式，当构建中断时，以该方式通知用户/开发者 -->
            <notifier>
                <!--传送通知的途径 -->
                <type />
                <!--发生错误时是否通知 -->
                <sendOnError />
                <!--构建失败时是否通知 -->
                <sendOnFailure />
                <!--构建成功时是否通知 -->
                <sendOnSuccess />
                <!--发生警告时是否通知 -->
                <sendOnWarning />
                <!--不赞成使用。通知发送到哪里 -->
                <address />
                <!--扩展配置项 -->
                <configuration />
            </notifier>
        </notifiers>
    </ciManagement>
    <!--项目创建年份，4位数字。当产生版权信息时需要使用这个值。 -->
    <inceptionYear />
    <!--项目相关邮件列表信息 -->
    <mailingLists>
        <!--该元素描述了项目相关的所有邮件列表。自动产生的网站引用这些信息。 -->
        <mailingList>
            <!--邮件的名称 -->
            <name>Demo</name>
            <!--发送邮件的地址或链接，如果是邮件地址，创建文档时，mailto: 链接会被自动创建 -->
            <post>banseon@126.com</post>
            <!--订阅邮件的地址或链接，如果是邮件地址，创建文档时，mailto: 链接会被自动创建 -->
            <subscribe>banseon@126.com</subscribe>
            <!--取消订阅邮件的地址或链接，如果是邮件地址，创建文档时，mailto: 链接会被自动创建 -->
            <unsubscribe>banseon@126.com</unsubscribe>
            <!--你可以浏览邮件信息的URL -->
            <archive>http:/hi.baidu.com/banseon/demo/dev/</archive>
        </mailingList>
    </mailingLists>
    <!--项目开发者列表 -->
    <developers>
        <!--某个项目开发者的信息 -->
        <developer>
            <!--SCM里项目开发者的唯一标识符 -->
            <id>HELLO WORLD</id>
            <!--项目开发者的全名 -->
            <name>banseon</name>
            <!--项目开发者的email -->
            <email>banseon@126.com</email>
            <!--项目开发者的主页的URL -->
            <url />
            <!--项目开发者在项目中扮演的角色，角色元素描述了各种角色 -->
            <roles>
                <role>Project Manager</role>
                <role>Architect</role>
            </roles>
            <!--项目开发者所属组织 -->
            <organization>demo</organization>
            <!--项目开发者所属组织的URL -->
            <organizationUrl>http://hi.baidu.com/banseon</organizationUrl>
            <!--项目开发者属性，如即时消息如何处理等 -->
            <properties>
                <dept>No</dept>
            </properties>
            <!--项目开发者所在时区， -11到12范围内的整数。 -->
            <timezone>-5</timezone>
        </developer>
    </developers>
    <!--项目的其他贡献者列表 -->
    <contributors>
        <!--项目的其他贡献者。参见developers/developer元素 -->
        <contributor>
            <name />
            <email />
            <url />
            <organization />
            <organizationUrl />
            <roles />
            <timezone />
            <properties />
        </contributor>
    </contributors>
    <!--该元素描述了项目所有License列表。 应该只列出该项目的license列表，不要列出依赖项目的 license列表。如果列出多个license，用户可以选择它们中的一个而不是接受所有license。 -->
    <licenses>
        <!--描述了项目的license，用于生成项目的web站点的license页面，其他一些报表和validation也会用到该元素。 -->
        <license>
            <!--license用于法律上的名称 -->
            <name>Apache 2</name>
            <!--官方的license正文页面的URL -->
            <url>http://www.baidu.com/banseon/LICENSE-2.0.txt</url>
            <!--项目分发的主要方式： repo，可以从Maven库下载 manual， 用户必须手动下载和安装依赖 -->
            <distribution>repo</distribution>
            <!--关于license的补充信息 -->
            <comments>A business-friendly OSS license</comments>
        </license>
    </licenses>
    <!--SCM(Source Control Management)标签允许你配置你的代码库，供Maven web站点和其它插件使用。 -->
    <scm>
        <!--SCM的URL,该URL描述了版本库和如何连接到版本库。欲知详情，请看SCMs提供的URL格式和列表。该连接只读。 -->
        <connection>
            scm:svn:http://svn.baidu.com/banseon/maven/banseon/banseon-maven2-trunk(dao-trunk)
        </connection>
        <!--给开发者使用的，类似connection元素。即该连接不仅仅只读 -->
        <developerConnection>
            scm:svn:http://svn.baidu.com/banseon/maven/banseon/dao-trunk
        </developerConnection>
        <!--当前代码的标签，在开发阶段默认为HEAD -->
        <tag />
        <!--指向项目的可浏览SCM库（例如ViewVC或者Fisheye）的URL。 -->
        <url>http://svn.baidu.com/banseon</url>
    </scm>
    <!--描述项目所属组织的各种属性。Maven产生的文档用 -->
    <organization>
        <!--组织的全名 -->
        <name>demo</name>
        <!--组织主页的URL -->
        <url>http://www.baidu.com/banseon</url>
    </organization>
    <!--构建项目需要的信息 -->
    <build>
        <!--该元素设置了项目源码目录，当构建项目的时候，构建系统会编译目录里的源码。该路径是相对于pom.xml的相对路径。 -->
        <sourceDirectory />
        <!--该元素设置了项目脚本源码目录，该目录和源码目录不同：绝大多数情况下，该目录下的内容 会被拷贝到输出目录(因为脚本是被解释的，而不是被编译的)。 -->
        <scriptSourceDirectory />
        <!--该元素设置了项目单元测试使用的源码目录，当测试项目的时候，构建系统会编译目录里的源码。该路径是相对于pom.xml的相对路径。 -->
        <testSourceDirectory />
        <!--被编译过的应用程序class文件存放的目录。 -->
        <outputDirectory />
        <!--被编译过的测试class文件存放的目录。 -->
        <testOutputDirectory />
        <!--使用来自该项目的一系列构建扩展 -->
        <extensions>
            <!--描述使用到的构建扩展。 -->
            <extension>
                <!--构建扩展的groupId -->
                <groupId />
                <!--构建扩展的artifactId -->
                <artifactId />
                <!--构建扩展的版本 -->
                <version />
            </extension>
        </extensions>
        <!--当项目没有规定目标（Maven2 叫做阶段）时的默认值 -->
        <defaultGoal />
        <!--这个元素描述了项目相关的所有资源路径列表，例如和项目相关的属性文件，这些资源被包含在最终的打包文件里。 -->
        <resources>
            <!--这个元素描述了项目相关或测试相关的所有资源路径 -->
            <resource>
                <!-- 描述了资源的目标路径。该路径相对target/classes目录（例如${project.build.outputDirectory}）。举个例 
                    子，如果你想资源在特定的包里(org.apache.maven.messages)，你就必须该元素设置为org/apache/maven /messages。然而，如果你只是想把资源放到源码目录结构里，就不需要该配置。 -->
                <targetPath />
                <!--是否使用参数值代替参数名。参数值取自properties元素或者文件里配置的属性，文件在filters元素里列出。 -->
                <filtering />
                <!--描述存放资源的目录，该路径相对POM路径 -->
                <directory />
                <!--包含的模式列表，例如**/*.xml. -->
                <includes />
                <!--排除的模式列表，例如**/*.xml -->
                <excludes />
            </resource>
        </resources>
        <!--这个元素描述了单元测试相关的所有资源路径，例如和单元测试相关的属性文件。 -->
        <testResources>
            <!--这个元素描述了测试相关的所有资源路径，参见build/resources/resource元素的说明 -->
            <testResource>
                <targetPath />
                <filtering />
                <directory />
                <includes />
                <excludes />
            </testResource>
        </testResources>
        <!--构建产生的所有文件存放的目录 -->
        <directory />
        <!--产生的构件的文件名，默认值是${artifactId}-${version}。 -->
        <finalName />
        <!--当filtering开关打开时，使用到的过滤器属性文件列表 -->
        <filters />
        <!--子项目可以引用的默认插件信息。该插件配置项直到被引用时才会被解析或绑定到生命周期。给定插件的任何本地配置都会覆盖这里的配置 -->
        <pluginManagement>
            <!--使用的插件列表 。 -->
            <plugins>
                <!--plugin元素包含描述插件所需要的信息。 -->
                <plugin>
                    <!--插件在仓库里的group ID -->
                    <groupId />
                    <!--插件在仓库里的artifact ID -->
                    <artifactId />
                    <!--被使用的插件的版本（或版本范围） -->
                    <version />
                    <!--是否从该插件下载Maven扩展（例如打包和类型处理器），由于性能原因，只有在真需要下载时，该元素才被设置成enabled。 -->
                    <extensions />
                    <!--在构建生命周期中执行一组目标的配置。每个目标可能有不同的配置。 -->
                    <executions>
                        <!--execution元素包含了插件执行需要的信息 -->
                        <execution>
                            <!--执行目标的标识符，用于标识构建过程中的目标，或者匹配继承过程中需要合并的执行目标 -->
                            <id />
                            <!--绑定了目标的构建生命周期阶段，如果省略，目标会被绑定到源数据里配置的默认阶段 -->
                            <phase />
                            <!--配置的执行目标 -->
                            <goals />
                            <!--配置是否被传播到子POM -->
                            <inherited />
                            <!--作为DOM对象的配置 -->
                            <configuration />
                        </execution>
                    </executions>
                    <!--项目引入插件所需要的额外依赖 -->
                    <dependencies>
                        <!--参见dependencies/dependency元素 -->
                        <dependency>
                            ......
                        </dependency>
                    </dependencies>
                    <!--任何配置是否被传播到子项目 -->
                    <inherited />
                    <!--作为DOM对象的配置 -->
                    <configuration />
                </plugin>
            </plugins>
        </pluginManagement>
        <!--使用的插件列表 -->
        <plugins>
            <!--参见build/pluginManagement/plugins/plugin元素 -->
            <plugin>
                <groupId />
                <artifactId />
                <version />
                <extensions />
                <executions>
                    <execution>
                        <id />
                        <phase />
                        <goals />
                        <inherited />
                        <configuration />
                    </execution>
                </executions>
                <dependencies>
                    <!--参见dependencies/dependency元素 -->
                    <dependency>
                        ......
                    </dependency>
                </dependencies>
                <goals />
                <inherited />
                <configuration />
            </plugin>
        </plugins>
    </build>
    <!--在列的项目构建profile，如果被激活，会修改构建处理 -->
    <profiles>
        <!--根据环境参数或命令行参数激活某个构建处理 -->
        <profile>
            <!--构建配置的唯一标识符。即用于命令行激活，也用于在继承时合并具有相同标识符的profile。 -->
            <id />
            <!--自动触发profile的条件逻辑。Activation是profile的开启钥匙。profile的力量来自于它 能够在某些特定的环境中自动使用某些特定的值；这些环境通过activation元素指定。activation元素并不是激活profile的唯一方式。 -->
            <activation>
                <!--profile默认是否激活的标志 -->
                <activeByDefault />
                <!--当匹配的jdk被检测到，profile被激活。例如，1.4激活JDK1.4，1.4.0_2，而!1.4激活所有版本不是以1.4开头的JDK。 -->
                <jdk />
                <!--当匹配的操作系统属性被检测到，profile被激活。os元素可以定义一些操作系统相关的属性。 -->
                <os>
                    <!--激活profile的操作系统的名字 -->
                    <name>Windows XP</name>
                    <!--激活profile的操作系统所属家族(如 'windows') -->
                    <family>Windows</family>
                    <!--激活profile的操作系统体系结构 -->
                    <arch>x86</arch>
                    <!--激活profile的操作系统版本 -->
                    <version>5.1.2600</version>
                </os>
                <!--如果Maven检测到某一个属性（其值可以在POM中通过${名称}引用），其拥有对应的名称和值，Profile就会被激活。如果值 字段是空的，那么存在属性名称字段就会激活profile，否则按区分大小写方式匹配属性值字段 -->
                <property>
                    <!--激活profile的属性的名称 -->
                    <name>mavenVersion</name>
                    <!--激活profile的属性的值 -->
                    <value>2.0.3</value>
                </property>
                <!--提供一个文件名，通过检测该文件的存在或不存在来激活profile。missing检查文件是否存在，如果不存在则激活 profile。另一方面，exists则会检查文件是否存在，如果存在则激活profile。 -->
                <file>
                    <!--如果指定的文件存在，则激活profile。 -->
                    <exists>/usr/local/hudson/hudson-home/jobs/maven-guide-zh-to-production/workspace/
                    </exists>
                    <!--如果指定的文件不存在，则激活profile。 -->
                    <missing>/usr/local/hudson/hudson-home/jobs/maven-guide-zh-to-production/workspace/
                    </missing>
                </file>
            </activation>
            <!--构建项目所需要的信息。参见build元素 -->
            <build>
                <defaultGoal />
                <resources>
                    <resource>
                        <targetPath />
                        <filtering />
                        <directory />
                        <includes />
                        <excludes />
                    </resource>
                </resources>
                <testResources>
                    <testResource>
                        <targetPath />
                        <filtering />
                        <directory />
                        <includes />
                        <excludes />
                    </testResource>
                </testResources>
                <directory />
                <finalName />
                <filters />
                <pluginManagement>
                    <plugins>
                        <!--参见build/pluginManagement/plugins/plugin元素 -->
                        <plugin>
                            <groupId />
                            <artifactId />
                            <version />
                            <extensions />
                            <executions>
                                <execution>
                                    <id />
                                    <phase />
                                    <goals />
                                    <inherited />
                                    <configuration />
                                </execution>
                            </executions>
                            <dependencies>
                                <!--参见dependencies/dependency元素 -->
                                <dependency>
                                    ......
                                </dependency>
                            </dependencies>
                            <goals />
                            <inherited />
                            <configuration />
                        </plugin>
                    </plugins>
                </pluginManagement>
                <plugins>
                    <!--参见build/pluginManagement/plugins/plugin元素 -->
                    <plugin>
                        <groupId />
                        <artifactId />
                        <version />
                        <extensions />
                        <executions>
                            <execution>
                                <id />
                                <phase />
                                <goals />
                                <inherited />
                                <configuration />
                            </execution>
                        </executions>
                        <dependencies>
                            <!--参见dependencies/dependency元素 -->
                            <dependency>
                                ......
                            </dependency>
                        </dependencies>
                        <goals />
                        <inherited />
                        <configuration />
                    </plugin>
                </plugins>
            </build>
            <!--模块（有时称作子项目） 被构建成项目的一部分。列出的每个模块元素是指向该模块的目录的相对路径 -->
            <modules />
            <!--发现依赖和扩展的远程仓库列表。 -->
            <repositories>
                <!--参见repositories/repository元素 -->
                <repository>
                    <releases>
                        <enabled />
                        <updatePolicy />
                        <checksumPolicy />
                    </releases>
                    <snapshots>
                        <enabled />
                        <updatePolicy />
                        <checksumPolicy />
                    </snapshots>
                    <id />
                    <name />
                    <url />
                    <layout />
                </repository>
            </repositories>
            <!--发现插件的远程仓库列表，这些插件用于构建和报表 -->
            <pluginRepositories>
                <!--包含需要连接到远程插件仓库的信息.参见repositories/repository元素 -->
                <pluginRepository>
                    <releases>
                        <enabled />
                        <updatePolicy />
                        <checksumPolicy />
                    </releases>
                    <snapshots>
                        <enabled />
                        <updatePolicy />
                        <checksumPolicy />
                    </snapshots>
                    <id />
                    <name />
                    <url />
                    <layout />
                </pluginRepository>
            </pluginRepositories>
            <!--该元素描述了项目相关的所有依赖。 这些依赖组成了项目构建过程中的一个个环节。它们自动从项目定义的仓库中下载。要获取更多信息，请看项目依赖机制。 -->
            <dependencies>
                <!--参见dependencies/dependency元素 -->
                <dependency>
                    ......
                </dependency>
            </dependencies>
            <!--不赞成使用. 现在Maven忽略该元素. -->
            <reports />
            <!--该元素包括使用报表插件产生报表的规范。当用户执行"mvn site"，这些报表就会运行。 在页面导航栏能看到所有报表的链接。参见reporting元素 -->
            <reporting>
                ......
            </reporting>
            <!--参见dependencyManagement元素 -->
            <dependencyManagement>
                <dependencies>
                    <!--参见dependencies/dependency元素 -->
                    <dependency>
                        ......
                    </dependency>
                </dependencies>
            </dependencyManagement>
            <!--参见distributionManagement元素 -->
            <distributionManagement>
                ......
            </distributionManagement>
            <!--参见properties元素 -->
            <properties />
        </profile>
    </profiles>
    <!--模块（有时称作子项目） 被构建成项目的一部分。列出的每个模块元素是指向该模块的目录的相对路径 -->
    <modules />
    <!--发现依赖和扩展的远程仓库列表。 -->
    <repositories>
        <!--包含需要连接到远程仓库的信息 -->
        <repository>
            <!--如何处理远程仓库里发布版本的下载 -->
            <releases>
                <!--true或者false表示该仓库是否为下载某种类型构件（发布版，快照版）开启。 -->
                <enabled />
                <!--该元素指定更新发生的频率。Maven会比较本地POM和远程POM的时间戳。这里的选项是：always（一直），daily（默认，每日），interval：X（这里X是以分钟为单位的时间间隔），或者never（从不）。 -->
                <updatePolicy />
                <!--当Maven验证构件校验文件失败时该怎么做：ignore（忽略），fail（失败），或者warn（警告）。 -->
                <checksumPolicy />
            </releases>
            <!-- 如何处理远程仓库里快照版本的下载。有了releases和snapshots这两组配置，POM就可以在每个单独的仓库中，为每种类型的构件采取不同的 
                策略。例如，可能有人会决定只为开发目的开启对快照版本下载的支持。参见repositories/repository/releases元素 -->
            <snapshots>
                <enabled />
                <updatePolicy />
                <checksumPolicy />
            </snapshots>
            <!--远程仓库唯一标识符。可以用来匹配在settings.xml文件里配置的远程仓库 -->
            <id>banseon-repository-proxy</id>
            <!--远程仓库名称 -->
            <name>banseon-repository-proxy</name>
            <!--远程仓库URL，按protocol://hostname/path形式 -->
            <url>http://192.168.1.169:9999/repository/</url>
            <!-- 用于定位和排序构件的仓库布局类型-可以是default（默认）或者legacy（遗留）。Maven 2为其仓库提供了一个默认的布局；然 
                而，Maven 1.x有一种不同的布局。我们可以使用该元素指定布局是default（默认）还是legacy（遗留）。 -->
            <layout>default</layout>
        </repository>
    </repositories>
    <!--发现插件的远程仓库列表，这些插件用于构建和报表 -->
    <pluginRepositories>
        <!--包含需要连接到远程插件仓库的信息.参见repositories/repository元素 -->
        <pluginRepository>
            ......
        </pluginRepository>
    </pluginRepositories>
 
 
    <!--该元素描述了项目相关的所有依赖。 这些依赖组成了项目构建过程中的一个个环节。它们自动从项目定义的仓库中下载。要获取更多信息，请看项目依赖机制。 -->
    <dependencies>
        <dependency>
            <!--依赖的group ID -->
            <groupId>org.apache.maven</groupId>
            <!--依赖的artifact ID -->
            <artifactId>maven-artifact</artifactId>
            <!--依赖的版本号。 在Maven 2里, 也可以配置成版本号的范围。 -->
            <version>3.8.1</version>
            <!-- 依赖类型，默认类型是jar。它通常表示依赖的文件的扩展名，但也有例外。一个类型可以被映射成另外一个扩展名或分类器。类型经常和使用的打包方式对应， 
                尽管这也有例外。一些类型的例子：jar，war，ejb-client和test-jar。如果设置extensions为 true，就可以在 plugin里定义新的类型。所以前面的类型的例子不完整。 -->
            <type>jar</type>
            <!-- 依赖的分类器。分类器可以区分属于同一个POM，但不同构建方式的构件。分类器名被附加到文件名的版本号后面。例如，如果你想要构建两个单独的构件成 
                JAR，一个使用Java 1.4编译器，另一个使用Java 6编译器，你就可以使用分类器来生成两个单独的JAR构件。 -->
            <classifier></classifier>
            <!--依赖范围。在项目发布过程中，帮助决定哪些构件被包括进来。欲知详情请参考依赖机制。 - compile ：默认范围，用于编译 - provided：类似于编译，但支持你期待jdk或者容器提供，类似于classpath 
                - runtime: 在执行时需要使用 - test: 用于test任务时使用 - system: 需要外在提供相应的元素。通过systemPath来取得 
                - systemPath: 仅用于范围为system。提供相应的路径 - optional: 当项目自身被依赖时，标注依赖是否传递。用于连续依赖时使用 -->
            <scope>test</scope>
            <!--仅供system范围使用。注意，不鼓励使用这个元素，并且在新的版本中该元素可能被覆盖掉。该元素为依赖规定了文件系统上的路径。需要绝对路径而不是相对路径。推荐使用属性匹配绝对路径，例如${java.home}。 -->
            <systemPath></systemPath>
            <!--当计算传递依赖时， 从依赖构件列表里，列出被排除的依赖构件集。即告诉maven你只依赖指定的项目，不依赖项目的依赖。此元素主要用于解决版本冲突问题 -->
            <exclusions>
                <exclusion>
                    <artifactId>spring-core</artifactId>
                    <groupId>org.springframework</groupId>
                </exclusion>
            </exclusions>
            <!--可选依赖，如果你在项目B中把C依赖声明为可选，你就需要在依赖于B的项目（例如项目A）中显式的引用对C的依赖。可选依赖阻断依赖的传递性。 -->
            <optional>true</optional>
        </dependency>
    </dependencies>
    <!--不赞成使用. 现在Maven忽略该元素. -->
    <reports></reports>
    <!--该元素描述使用报表插件产生报表的规范。当用户执行"mvn site"，这些报表就会运行。 在页面导航栏能看到所有报表的链接。 -->
    <reporting>
        <!--true，则，网站不包括默认的报表。这包括"项目信息"菜单中的报表。 -->
        <excludeDefaults />
        <!--所有产生的报表存放到哪里。默认值是${project.build.directory}/site。 -->
        <outputDirectory />
        <!--使用的报表插件和他们的配置。 -->
        <plugins>
            <!--plugin元素包含描述报表插件需要的信息 -->
            <plugin>
                <!--报表插件在仓库里的group ID -->
                <groupId />
                <!--报表插件在仓库里的artifact ID -->
                <artifactId />
                <!--被使用的报表插件的版本（或版本范围） -->
                <version />
                <!--任何配置是否被传播到子项目 -->
                <inherited />
                <!--报表插件的配置 -->
                <configuration />
                <!--一组报表的多重规范，每个规范可能有不同的配置。一个规范（报表集）对应一个执行目标 。例如，有1，2，3，4，5，6，7，8，9个报表。1，2，5构成A报表集，对应一个执行目标。2，5，8构成B报表集，对应另一个执行目标 -->
                <reportSets>
                    <!--表示报表的一个集合，以及产生该集合的配置 -->
                    <reportSet>
                        <!--报表集合的唯一标识符，POM继承时用到 -->
                        <id />
                        <!--产生报表集合时，被使用的报表的配置 -->
                        <configuration />
                        <!--配置是否被继承到子POMs -->
                        <inherited />
                        <!--这个集合里使用到哪些报表 -->
                        <reports />
                    </reportSet>
                </reportSets>
            </plugin>
        </plugins>
    </reporting>
    <!-- 继承自该项目的所有子项目的默认依赖信息。这部分的依赖信息不会被立即解析,而是当子项目声明一个依赖（必须描述group ID和 artifact 
        ID信息），如果group ID和artifact ID以外的一些信息没有描述，则通过group ID和artifact ID 匹配到这里的依赖，并使用这里的依赖信息。 -->
    <dependencyManagement>
        <dependencies>
            <!--参见dependencies/dependency元素 -->
            <dependency>
                ......
            </dependency>
        </dependencies>
    </dependencyManagement>
    <!--项目分发信息，在执行mvn deploy后表示要发布的位置。有了这些信息就可以把网站部署到远程服务器或者把构件部署到远程仓库。 -->
    <distributionManagement>
        <!--部署项目产生的构件到远程仓库需要的信息 -->
        <repository>
            <!--是分配给快照一个唯一的版本号（由时间戳和构建流水号）？还是每次都使用相同的版本号？参见repositories/repository元素 -->
            <uniqueVersion />
            <id>banseon-maven2</id>
            <name>banseon maven2</name>
            <url>file://${basedir}/target/deploy</url>
            <layout />
        </repository>
        <!--构件的快照部署到哪里？如果没有配置该元素，默认部署到repository元素配置的仓库，参见distributionManagement/repository元素 -->
        <snapshotRepository>
            <uniqueVersion />
            <id>banseon-maven2</id>
            <name>Banseon-maven2 Snapshot Repository</name>
            <url>scp://svn.baidu.com/banseon:/usr/local/maven-snapshot</url>
            <layout />
        </snapshotRepository>
        <!--部署项目的网站需要的信息 -->
        <site>
            <!--部署位置的唯一标识符，用来匹配站点和settings.xml文件里的配置 -->
            <id>banseon-site</id>
            <!--部署位置的名称 -->
            <name>business api website</name>
            <!--部署位置的URL，按protocol://hostname/path形式 -->
            <url>
                scp://svn.baidu.com/banseon:/var/www/localhost/banseon-web
            </url>
        </site>
        <!--项目下载页面的URL。如果没有该元素，用户应该参考主页。使用该元素的原因是：帮助定位那些不在仓库里的构件（由于license限制）。 -->
        <downloadUrl />
        <!--如果构件有了新的group ID和artifact ID（构件移到了新的位置），这里列出构件的重定位信息。 -->
        <relocation>
            <!--构件新的group ID -->
            <groupId />
            <!--构件新的artifact ID -->
            <artifactId />
            <!--构件新的版本号 -->
            <version />
            <!--显示给用户的，关于移动的额外信息，例如原因。 -->
            <message />
        </relocation>
        <!-- 给出该构件在远程仓库的状态。不得在本地项目中设置该元素，因为这是工具自动更新的。有效的值有：none（默认），converted（仓库管理员从 
            Maven 1 POM转换过来），partner（直接从伙伴Maven 2仓库同步过来），deployed（从Maven 2实例部 署），verified（被核实时正确的和最终的）。 -->
        <status />
    </distributionManagement>
    <!--以值替代名称，Properties可以在整个POM中使用，也可以作为触发条件（见settings.xml配置文件里activation元素的说明）。格式是<name>value</name>。 -->
    <properties />
</project>
```

# 生命周期

## Clean

Maven 的 `clean` 生命周期包含一个阶段，它的主要任务是清理项目，删除之前构建过程生成的输出文件，确保项目处于干净的状态。这个阶段通常在构建之前执行，以防止旧的编译结果和临时文件对构建过程造成影响。

`clean` 生命周期只有一个阶段：

+ **clean**：删除目标目录中的编译输出文件。默认情况下，这个目录是 `target`，包含编译、测试、打包生成的文件以及其他构建生成的临时文件。

执行 `mvn clean` 命令会触发 `clean` 生命周期的`clean`阶段，Maven会清理项目目录下的 `target` 文件夹，删除之前构建过程生成的所有文件。

## Default

Maven 的 `default` 生命周期（也称为 `build` 生命周期）是构建过程中最常用的生命周期，负责将项目从源代码编译成可分发的产品。这个生命周期定义了从验证到部署的一系列阶段，确保了项目的顺利构建和发布。

![img](https://www.runoob.com/wp-content/uploads/2018/09/maven-package-build-phase.png)

`default`生命周期包括以下阶段：

|   阶段   |   处理   |                             描述                             |
| :------: | :------: | :----------------------------------------------------------: |
| validate | 验证项目 | 验证项目的正确性，检查项目是否正确配置，所有必要信息是否可用 |
| compile  | 执行编译 |      编译项目的源代码，将源代码编译成可执行的字节码文件      |
|   test   |   测试   |        使用适当的单元测试框架（例如 JUnit）运行测试。        |
| package  |   打包   |      将编译后的代码打包成可分发的格式，例如 JAR 或 WAR       |
|  verify  |   检查   |  对集成测试的结果进行检查，以保证质量达标，通常用于集成测试  |
| install  |   安装   |  将项目的构建结果安装到本地 Maven 仓库中，以供其他项目使用   |
|  deploy  |   部署   |   拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程   |

默认情况下，执行 `mvn install` 命令会触发 `default` 生命周期的所有阶段（包括其他未在上面罗列的生命周期阶段），按照上述顺序依次执行。但可以针对特定阶段执行单个阶段，比如 `mvn compile` 只会执行 `compile` 阶段。

## Site

Maven 的 `site` 生命周期主要关注项目文档和站点的生成，是用于创建项目站点文档的生命周期。

这个生命周期包含了以下两个主要阶段：

+ **site**：该阶段用于生成项目文档和站点信息。它通常包括生成文档、报告和站点相关的内容，比如生成文档、API 文档、静态站点内容等。这个阶段的执行结果会存放在 `target/site` 目录下。
+ **deploy-site**：`site` 阶段生成的站点信息，使用 `deploy-site` 阶段可以将生成的站点信息发布到远程服务器，以便共享项目文档。这个阶段通常会将站点发布到远程服务器的特定位置，让团队或用户可以访问项目文档和信息。

通过执行 `mvn site` 命令，Maven 会按照 `site` 生命周期的定义生成项目的站点文档，包括各种文档和报告。这有助于项目团队和用户了解项目的状态、文档、API 等信息。

# 命令

+ **Lifecycle（生命周期）：** Maven 中定义了一系列阶段，按顺序执行以完成构建过程。例如，`clean`、`compile`、`test`、`package`、`install` 等。
+ **Phase（阶段）：** 生命周期中的某个具体阶段，代表了构建过程中的一个步骤。每个生命周期有多个阶段。
+ **Goal（目标）：** 描述了 Maven 插件的功能，用于执行特定任务。每个阶段都包含了一个或多个目标。

## 常用命令

+ **`mvn clean`：** 清理项目，删除目标文件（如编译后的类文件、打包文件等）。
+ **`mvn compile`：** 编译项目源代码。
+ **`mvn test`：** 运行项目中的测试。
+ **`mvn package`：** 将编译后的代码打包，生成可分发的文件，比如 JAR、WAR。
+ **`mvn install`：** 将打包好的文件安装到本地 Maven 仓库中，供其他项目使用。
+ **`mvn deploy`：** 将构建好的文件部署到远程仓库，用于共享和分发。

##  命令使用

+ **命令结构：** `mvn [goal]`，其中 `[goal]` 是指定的目标或阶段。
+ **组合命令：** 可以将多个命令组合在一起使用，例如 `mvn clean package`，以清理项目并打包。
+ **Profiles（配置文件）：** 使用 `-P` 参数指定某个配置文件。比如 `mvn clean install -P production`，表示使用生产环境的配置进行构建。
+ **帮助命令：** `mvn --help` 或 `mvn -h`，用于查看 Maven 命令的帮助信息。

# 目录结构

```
my-project
|-- pom.xml
|-- src
|   |-- main
|   |   |-- java         # Java源代码
|   |   |-- resources    # 资源文件
|   |-- test
|       |-- java         # 测试源代码
|       |-- resources    # 测试资源文件
|-- target               # 构建生成的目标文件
|-- .mvn                 # Maven wrapper配置（可选）
|-- .gitignore           # Git忽略文件配置
|-- .editorconfig        # 编辑器配置（可选）
```

+ **`pom.xml`：** Maven 项目的核心配置文件，包含项目的基本信息、依赖关系、插件配置等。
+ **`src` 目录：** 存放项目的源代码和资源文件。
  + **`main` 目录：**
    + **`java`：** 存放主要的 Java 源代码。
    + **`resources`：** 存放主要的资源文件，如配置文件等。
  + **`test` 目录：**
    + **`java`：** 存放测试用的 Java 源代码。
    + **`resources`：** 存放测试用的资源文件。
+ **`target` 目录：** 存放编译生成的目标文件，包括生成的 JAR 或 WAR 文件等。
+ **`.mvn` 目录（可选）：** Maven wrapper的配置目录，用于支持项目的独立构建，无需全局安装 Maven。
+ **`.gitignore` 文件：** Git 版本控制忽略配置文件，定义哪些文件不会被 Git 跟踪。
+ **`.editorconfig` 文件（可选）：** 编辑器配置文件，定义不同编辑器的一致性配置，以确保团队协作中的一致性。

# 配置

构建配置文件是一系列的配置项的值，可以用来设置或者覆盖 Maven 构建默认值。

使用构建配置文件，你可以为不同的环境，比如说生产环境（Production）和开发（Development）环境，定制构建方式。

配置文件在 pom.xml 文件中使用 activeProfiles 或者 profiles 元素指定，并且可以通过各种方式触发。配置文件在构建时修改 POM，并且用来给参数设定不同的目标环境（比如说，开发（Development）、测试（Testing）和生产环境（Production）中数据库服务器的地址）。

## 类型

1. **项目级（Per Project）**：
   + **位置：** 定义在项目的 POM 文件 `pom.xml` 中。
   + 这种配置是针对单个项目的，放置于项目POM文件中。它允许针对特定项目定义构建和依赖管理的设置，例如编译器版本、依赖库版本等。
2. **用户级 （Per User）**：
   + **位置：** 定义在Maven的用户级设置xml文件中 (`%USER_HOME%/.m2/settings.xml`)。
   + 这种配置是针对特定用户的。它允许在用户级别设置Maven的行为，比如镜像设置、代理设置、服务器凭证等。这些配置对于所有项目都是通用的。
3. **全局（Global）**：
   + **位置：** 定义在Maven全局的设置xml文件中 (`%M2_HOME%/conf/settings.xml`)。
   + 这种配置是全局性的，适用于所有用户和项目。通常由系统管理员或者Maven安装者设置，用于配置全局行为，例如默认的仓库地址、插件组、全局性的代理设置等。

用户级和全局配置的 `setting.xml` 文件中，根标签为 `<settings>` ，项目级中的 `pom.xml` 根标签为 `<project>` 。

## 全局配置

Maven 使用 `settings.xml` 作为全局配置文件，它包含了一些 Maven 运行所需的全局设置和配置选项。这个文件通常位于 `Maven_Home/conf` 目录下。

+ **配置仓库（Repository）信息**：指定 Maven 从哪里下载依赖，以及将依赖上传到哪个仓库。
+ **设置全局属性（Global Properties）**：配置全局使用的属性，比如编码、Maven 版本等。
+ **配置代理服务器（Proxy Server）**：允许在需要时使用代理服务器来访问远程仓库。
+ **定义Maven Profiles**：可以针对不同的环境设置不同的配置。

### 本地仓库路径

这个设置指定了Maven在本地存储依赖库的位置。这个本地仓库路径配置项允许你指定Maven在本地文件系统中存储依赖项的位置。

```xml
<localRepository>/path/to/your/local/repo</localRepository>
```

这个配置指定了本地仓库的路径，默认是 `~/.m2/repository`。

### 代理配置

通过配置代理来让 Maven 能够通过代理服务器访问远程仓库。这在你的网络环境中需要代理才能访问外部资源时非常有用。

```xml
<proxies>
  <proxy>
    <id>example-proxy</id>
    <active>true</active>
    <protocol>http</protocol>
    <host>proxy.example.com</host>
    <port>8080</port>
    <!-- 可选的用户名和密码，如果需要认证访问 -->
    <!-- <username>your_username</username>
      <password>your_password</password> -->
    <!-- 可选的非代理主机列表 -->
    <!-- <nonProxyHosts>local.net|some.host.com</nonProxyHosts> -->
  </proxy>
</proxies>
```

+ `<id>`：代理的唯一标识符。
+ `<active>`：是否激活代理配置。
+ `<protocol>`：代理服务器的协议，例如 `http` 或 `https`。
+ `<host>`：代理服务器的主机名或 IP 地址。
+ `<port>`：代理服务器的端口号。
+ `<username>` 和 `<password>`：可选项，如果代理需要认证，可以提供用户名和密码。
+ `<nonProxyHosts>`：可选项，用于指定不需要使用代理访问的主机列表。

### 服务器凭证

配置服务器凭证来访问需要身份验证的远程仓库或服务。这些凭证可以用于访问私有的 Maven 仓库或者其他需要身份验证的远程资源。

```xml
<servers>
  <server>
    <id>example-repo</id>
    <username>your_username</username>
    <password>your_password</password>
    <!-- 可选的私钥/公钥认证 -->
    <!-- <privateKey>/path/to/privatekey</privateKey>
      <passphrase>your_passphrase</passphrase> -->
  </server>
</servers>
```

+ `<id>`：服务器的唯一标识符，用于关联服务器和凭证。
+ `<username>` 和 `<password>`：用于验证的用户名和密码。
+ `<privateKey>` 和 `<passphrase>`：可选项，如果服务器使用 SSH 密钥认证，可以指定私钥的路径和解密密钥。

### 镜像设置

可以配置镜像来加速构建过程并优化下载依赖项的速度。镜像定义了 Maven 应该从哪个远程仓库获取构建所需的资源。

```xml
<mirrors>
  <mirror>
    <id>mirror_id</id>
    <mirrorOf>central</mirrorOf>
    <url>https://maven.example.com/repository/maven-central/</url>
    <blocked>false</blocked>
    <mirrorOfLayouts>default,legacy</mirrorOfLayouts>
  </mirror>
</mirrors>
```

+ `<mirror>`：镜像配置的起始标签。
+ `<id>`：镜像的唯一标识符。
+ `<mirrorOf>`：指定需要镜像的远程仓库。`central` 通常指代 Maven 中央仓库。可以是一个逗号分隔的列表，指定多个仓库。
+ `<url>`：镜像仓库的地址。
+ `<blocked>`：标志该镜像是否被阻止（`true` 或 `false`）。如果设置为 `true`，Maven 不会使用该镜像。
+ `<mirrorOfLayouts>`：指定镜像匹配的仓库布局。默认情况下，使用 `default,legacy`。

#### 常用镜像

**阿里云**

```xml
<mirror>
    <id>aliyunmaven</id>
    <mirrorOf>*</mirrorOf>
    <name>阿里云公共仓库</name>
    <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```

**华为云**

```xml
<mirror>
　　<id>huaweicloud</id>
   <name>华为云 maven</name>
   <mirrorOf>*</mirrorOf>
   <url>https://mirrors.huaweicloud.com/repository/maven/</url>
</mirror>
```

**腾讯云**

```xml
<mirror>
　　<id>nexus-tencentyun</id>
　　<mirrorOf>*</mirrorOf>
　　<name>Nexus tencentyun</name>
　　<url>http://mirrors.cloud.tencent.com/nexus/repository/maven-public/</url>
</mirror>
```

### Profile 配置

在 Maven 的 `settings.xml` 或者项目的 `pom.xml` 文件中，可以使用 `profiles` 元素定义不同的构建配置文件，以便根据不同的环境需求激活特定的配置。

```xml
<profiles>
  <profile>
    <id>development</id>
    <activation>
      <activeByDefault>true</activeByDefault>
    </activation>
    <properties>
      <!-- 定义特定于开发环境的属性 -->
      <env>development</env>
      <database.url>jdbc:mysql://localhost:3306/dev_db</database.url>
    </properties>
  </profile>

  <profile>
    <id>production</id>
    <properties>
      <!-- 定义特定于生产环境的属性 -->
      <env>production</env>
      <database.url>jdbc:mysql://production-server/db</database.url>
    </properties>
  </profile>
</profiles>
```

+ `<profiles>`：用于定义不同的构建配置文件。
+ `<profile>`：每个配置文件的起始标签。
+ `<id>`：配置文件的唯一标识符。
+ `<activation>`：定义何时激活配置文件的规则。
  + `<activeByDefault>`：如果设置为 `true`，表示默认激活该配置文件。
+ `<properties>`：配置文件中的属性设置。
  + 这里的属性可以根据环境需求设置不同的值，比如数据库连接的 URL、环境标识等。

Profile 可以在不同环境下使用不同的配置，可以设置默认激活的 profile，或根据不同条件激活 profile。

### 插件组

`pluginGroups` 元素是 Maven 设置文件 `settings.xml` 中的一个部分，用于定义自定义插件组。

当使用 Maven 执行构建时，Maven 会在本地仓库中查找并下载所需的插件。通常情况下，Maven 会从 Maven 中央仓库（Central Repository）下载插件。

当可能需要使用自己或组织内部的自定义插件。在这种情况下，你可以使用 `pluginGroups` 元素配置自定义的插件组。

```xml
<pluginGroups>
  <pluginGroup>com.example.myplugins</pluginGroup>
  <pluginGroup>org.company.internalplugins</pluginGroup>
</pluginGroups>
```

+ `<pluginGroup>`：插件组的名称，即插件坐标的前缀。可以指定多个插件组。

通过这种配置，当你在项目的 POM 文件中使用插件时，Maven 将首先查找这些自定义插件组，然后再从 Maven 中央仓库中查找。这样，你就可以方便地使用自己或组织内部的自定义插件，而不必每次都指定完整的插件坐标。

## 项目配置

### 基本配置

+ **project** - `project` 是 pom.xml 中描述符的根。
+ **modelVersion** - `modelVersion` 指定 pom.xml 符合哪个版本的描述符。maven 2 和 3 只能为 4.0.0。

一般 jar 包被识别为： `groupId:artifactId:version` 的形式。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.codehaus.mojo</groupId>
  <artifactId>my-project</artifactId>
  <version>1.0</version>
  <packaging>war</packaging>
</project>
```

###  依赖配置

### 模块配置

### 属性配置

属性列表。定义的属性可以在 pom.xml 文件中任意处使用。使用方式为 `${propertie}` 。

```xml
<properties>
  <maven.compiler.source>1.7<maven.compiler.source>
  <maven.compiler.target>1.7<maven.compiler.target>
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
</properties>
```

### 构建配置



### 插件配置

在 Maven 中，插件是执行构建过程中最核心的组件之一。插件配置允许开发者指定在构建过程中运行的插件以及这些插件的行为。

```xml
<build>
  <plugins>
    <!-- 插件的配置 -->
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.8.1</version>
      <configuration>
        <!-- 插件的配置项 -->
        <source>1.8</source>
        <target>1.8</target>
        <encoding>UTF-8</encoding>
      </configuration>
    </plugin>
  </plugins>
</build>
```

+ `<build>`：用于定义项目构建相关配置。
+ `<plugins>`：插件配置的容器。
+ `<plugin>`：插件的配置。
  + `<groupId>`、`<artifactId>`、`<version>`：插件的坐标信息，用于确定具体的插件。
  + `<configuration>`：用于配置插件的具体选项。
    + 在这个例子中，`maven-compiler-plugin` 插件被配置为使用 Java 1.8 版本的编译源代码。

**插件配置的常见选项**

+ `<groupId>`、`<artifactId>`、`<version>`：插件的坐标信息，指定要使用的插件。
+ `<executions>`：指定插件的执行时机和顺序。
+ `<configuration>`：插件的具体配置参数，取决于插件本身支持的配置项。
+ `<dependencies>`：插件运行所需要的依赖配置。

### 项目信息

### 环境配置



## 构建配置

Maven的构建配置文件可以通过多种方式激活。

+ 使用命令控制台输入显式激活。
+ 通过 maven 设置。
+ 基于环境变量（用户或者系统变量）。
+ 操作系统设置（比如说，Windows系列）。
+ 文件的存在或者缺失。

其中在src/main/resources文件夹下有三个用于测试文件：

| 文件名              | 描述                                 |
| :------------------ | :----------------------------------- |
| env.properties      | 如果未指定配置文件时默认使用的配置。 |
| env.test.properties | 当测试配置文件使用时的测试配置。     |
| env.prod.properties | 当生产配置文件使用时的生产配置。     |

**注意：**这三个配置文件并不是代表构建配置文件的功能，而是用于本次测试的目的；比如，我指定了构建配置文件为 prod 时，项目就使用 env.prod.properties文件。

**注意：**下面的例子仍然是使用 AntRun 插件，因为此插件能绑定 Maven 生命周期阶段，并通过 Ant 的标签不用编写一点代码即可输出信息、复制文件等，经此而已。其余的与本次构建配置文件无关。

### 1、配置文件激活

profile 可以让我们定义一系列的配置信息，然后指定其激活条件。这样我们就可以定义多个 profile，然后每个 profile 对应不同的激活条件和配置信息，从而达到不同环境使用不同配置信息的效果。

以下实例，我们将 maven-antrun-plugin:run 目标添加到测试阶段中。这样我们可以在不同的 profile 中输出文本信息。我们将使用 pom.xml 来定义不同的 profile，并在命令控制台中使用 maven 命令激活 profile。

**注意：****构建配置文件**采用的是 **<profiles>** 节点。

**说明：**上面新建了三个 **<profiles>**，其中 **<id>** 区分了不同的 **<profiles>** 执行不同的 AntRun 任务；而 AntRun 的任务可以这么理解，AntRun 监听 test 的 Maven 生命周期阶段，当 Maven 执行 test 时，就触发了 AntRun 的任务，任务里面为输出文本并复制文件到指定的位置；而至于要执行哪个 AntRun 任务，此时**构建配置文件**起到了传输指定的作用，比如，通过命令行参数输入指定的 **<id>**。

执行命令：

```
mvn test -Ptest
```

提示：第一个 test 为 Maven 生命周期阶段，第 2 个 test 为**构建配置文件**指定的 <id> 参数，这个参数通过 **-P** 来传输，当然，它可以是 prod 或者 normal 这些由你定义的**<id>**。