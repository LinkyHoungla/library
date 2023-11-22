# 简介

Tomcat 隶属于 Apache 基金会，是一个免费的、开源的、轻量级的 Web 应用服务器。适合在并发量不是很高的中小企业项目中使用。

`server.xml` 是 Tomcat 中最重要的配置文件，**`server.xml` 的每一个元素都对应了 Tomcat 中的一个组件**；通过对 xml 文件中元素的配置，可以实现对 Tomcat 中各个组件的控制。

**参考书籍**

+ Apahce Tomcat 高级配置

# 整体架构

## 文件结构

+ **bin**：存放可执行的文件，如 startup 和 shutdown。
+ **conf**：存放配置文件，如核心配置文件 server.xml 和应用默认的部署描述文件 web.xml。
+ **lib**：存放 Tomcat 运行需要的 jar 包。
+ **logs**：存放运行的日志文件。
+ **webapps**：存放默认的 web 应用部署目录。
+ **work**：存放 web 应用代码和编译文件的临时目录。

## 功能组件结构

![img](D:\Repository\Typora\041934454411587.png)

Tomcat 的核心功能有两个，分别是负责接收和反馈外部请求的连接器 Connector，和负责处理请求的容器 Container。其中连接器和容器相辅相成，一起构成了基本的 web 服务 Service。每个 Tomcat 服务器可以管理多个 Service。

+ **Connector**：负责对外接收反馈请求。它是 Tomcat 与外界的交通枢纽，监听端口接收外界请求，并将请求处理后传递给容器做业务处理，最后将容器处理后的结果反馈给外界。
+ **Container**：负责对内处理业务逻辑。其内部由 Engine、Host、Context 和 Wrapper 四个容器组成，用于管理和调用 Servlet 相关逻辑。
+ **Service**：对外提供的 Web 服务。主要包含连接器和容器两个核心组件，以及其他功能组件。Tomcat 可以管理多个 Service，且各 Service 之间相互独立。

![img](D:\Repository\Typora\v2-bfb98ddb4744fe080d2f29ac7c20c194_720w.webp)

# 配置

Tomcat 的 `server.xml` 配置文件是配置 Tomcat 服务器的核心文件之一，它包含了整个服务器的全局配置信息。

> 更改 `server.xml` 后，通常需要重新启动 Tomcat 才能使更改生效。

**整体结构**

```xml
<Server>
    <Service>
        <Connector />
        <Connector />
        <Engine>
            <Host>
                <Context />
            </Host>
        </Engine>
    </Service>
</Server>
```

## 服务器

`<Server>` 元素是整个 Tomcat 服务器的顶级元素，包含全局的配置信息和相关的服务配置。它必须是 `server.xml` 中唯一一个最外层的元素。**一个 Server 元素中可以有一个或多个 Service 元素**。

Server 的主要任务，就是提供一个接口让客户端能够访问到这个Service集合，同时维护它所包含的所有的Service的声明周期，包括如何初始化、如何结束服务、如何找到客户端要访问的Service。

```xml
<Server port="8005" shutdown="SHUTDOWN">
    <!-- 全局设置和组件 -->
</Server>
```

- **port：** 定义 Tomcat Server 进程间通信用的端口号，默认为 `8005`，设为 -1 可以禁掉该端口。
- **shutdown：** 用于关闭 Tomcat 服务器的命令字符串。默认是 `SHUTDOWN`。

## 服务

`<Service>` 元素在 Tomcat 的 `server.xml` 配置文件中定义了 Tomcat 的服务。每个 Tomcat 实例可以包含一个或多个服务，每个服务都有自己的一组连接器（Connectors）来接受客户端请求。

一个 **Service 可以包含多个 Connector ，但是只能包含一个 Engine **；其中 Connector 的作用是从客户端接收请求，Engine 的作用是处理接收进来的请求。

```xml
<Service name="Catalina">
    <!-- Connectors -->
    <!-- Engine -->
</Service>
```

**name：** 定义服务的名称，通常默认为 `"Catalina"`。

### 连接器

**连接器（Connectors）配置：**定义 Tomcat 如何接收和处理传入的连接请求，例如 HTTP 连接器、AJP 连接器等。

Connector 的主要功能，是接收连接请求，创建 Request 和 Response 对象用于和请求端交换数据；然后分配线程让 Engine 来处理这个请求，并把产生的 Request 和 Response 对象传给 Engine 。

**子标签**

- **`<SSLHostConfig>`：** 配置 SSL 主机的详细信息，如证书、密码等。
- **`<Valve>`：** 配置管道阀门，用于拦截并处理请求或响应，如记录访问日志到 `logs` 目录。

**HTTP 连接器的基本配置：**

```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443">

    <!-- Valve 子标签 -->
    <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
           prefix="localhost_access_log." suffix=".txt"
           pattern="%h %l %u %t &quot;%r&quot; %s %b" />

    <!-- SSLHostConfig 子标签 -->
    <SSLHostConfig>
        <Certificate certificateKeystoreFile="conf/localhost-rsa.jks"
                     type="RSA" />
    </SSLHostConfig>

</Connector>
```

- **port：** 监听的端口号，默认为 `8080`。
- **protocol：** 协议类型，例如 `HTTP/1.1`、`AJP/1.3` 等。
- **connectionTimeout：** 连接超时时间，以毫秒为单位，默认为 `20000` 毫秒。
- **redirectPort：** 当使用安全连接时（HTTPS），重定向端口，默认为 `8443`。

**安全连接器（HTTPS）配置：**

```xml
<Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
           maxThreads="150" scheme="https" secure="true"
           clientAuth="false" sslProtocol="TLS" />
```

- **maxThreads：** 处理连接的最大线程数，默认为 `200`。
- **minSpareThreads：** 最小空闲线程数，默认为 `4`。
- **maxSpareThreads：** 最大空闲线程数，默认为 `75`。
- **acceptCount：** 接受连接的队列长度，默认为 `100`。
- **URIEncoding：** 对 URL 编码进行配置。

### 引擎

**引擎（Engine）：**定义 Tomcat 的处理引擎，通常每个服务（`<Service>`）只包含一个引擎。`<Engine>` 可以包含多个虚拟主机（`<Host>`）来处理客户端请求。

Engine 组件从一个或多个 Connector 中接收请求并处理，并将完成的响应返回给 Connector，最终传递给客户端。

Engine、Host 和 Context 都是容器，但它们不是平行的关系，而是父子关系：Engine 包含 Host ，Host 包含 Context 。

**子标签**

+ **`<Host>`：** 定义引擎中的虚拟主机，用于处理客户端的请求。
+ **`<Cluster>`：** 定义集群配置，用于将多个 Tomcat 实例组合成一个集群，配置集群的各种组件，如管理器、频道、接收器、发送器、拦截器等。
+ **`<Realm>`：** 配置身份验证和授权。

```xml
<Engine name="Catalina" defaultHost="localhost">
    <!-- Hosts -->
    
    <!-- Realm -->
    <Realm className="org.apache.catalina.realm.LockOutRealm">
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
               resourceName="UserDatabase"/>
    </Realm>
    
    <!-- Cluster -->
    <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"
             channelSendOptions="8">
        <Manager className="org.apache.catalina.ha.session.DeltaManager"
                 expireSessionsOnShutdown="false"
                 notifyListenersOnReplication="true"/>
        <Channel className="org.apache.catalina.tribes.group.GroupChannel">
            <Membership className="org.apache.catalina.tribes.membership.McastService"
                        address="228.0.0.4"
                        port="45564"
                        frequency="500"
                        dropTime="3000"/>
            <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"
                      address="auto"
                      port="4000"
                      autoBind="100"
                      selectorTimeout="5000"
                      maxThreads="6"/>
            <Sender className="org.apache.catalina.tribes.transport.ReplicationTransmitter">
                <Transport className="org.apache.catalina.tribes.transport.nio.PooledParallelSender"/>
            </Sender>
            <Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector"/>
            <Interceptor className="org.apache.catalina.tribes.group.interceptors.MessageDispatch15Interceptor"/>
        </Channel>
        <Valve className="org.apache.catalina.ha.tcp.ReplicationValve"
               filter=""/>
        <Valve className="org.apache.catalina.ha.session.JvmRouteBinderValve"/>
        <ClusterListener className="org.apache.catalina.ha.session.JvmRouteSessionIDBinderListener"/>
        <ClusterListener className="org.apache.catalina.ha.session.ClusterSessionListener"/>
    </Cluster>
</Engine>
```

- **name：** 引擎的名称，默认为 `"Catalina"`，用于日志和错误信息，在整个 Server 中应该唯一。
- **defaultHost：** 默认虚拟主机的名称，当发往本机的请求指定的 host 名称不存在时，一律使用 defaultHost 指定的 host 进行处理。
  - defaultHost 的值，必须与 Engine 中的一个 Host 组件的 name 属性值匹配。

### 虚拟主机

**虚拟主机配置（Host）：**用于定义 Tomcat 引擎中的虚拟主机（Virtual Host）。每个 `<Host>` 元素通常对应着一个域名，用于处理客户端请求，并提供特定于主机的 Web 应用程序服务。允许配置多个虚拟主机，以支持多个域名和应用程序。

**子标签**

+ **`<Context>`：** 用于配置特定于主机的 Web 应用程序的上下文路径和其他属性。
+ **`<Valve>`：** 配置阀门用于拦截、处理或记录请求或响应。

```xml
<Host name="localhost" appBase="webapps" unpackWARs="true" autoDeploy="true">
    <Context path="/myapp" docBase="myapp" reloadable="true" />
    <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
           prefix="localhost_access_log." suffix=".txt"
           pattern="%h %l %u %t &quot;%r&quot; %s %b" />
</Host>
```

- **name：** 虚拟主机的名称，通常是域名，例如 `"localhost"`。
- **appBase：** 存放该虚拟主机下 Web 应用程序的基础目录。
- **unpackWARs：** 是否在部署 WAR 文件时解压。设置为 `"true"` 时，Tomcat 会自动将 WAR 文件解压到应用程序目录。
- **autoDeploy：** 是否自动部署。设置为 `"true"` 时，Tomcat 会监视该目录下的 WAR 文件和目录变化，并自动部署应用程序。

### 上下文

`<Context>` 元素用于配置特定于 Web 应用程序的上下文路径和其他属性。在 Tomcat 的 `server.xml` 文件中，可以在 `<Host>` 元素内部或通过单独的 XML 文件来配置 `<Context>`。

**Context 元素代表在特定虚拟主机上运行的一个Web 应用**。提到 Context、应用或 Web 应用，它们指代的都是 Web 应用。每个 Web 应用基于 WAR 文件，或 WAR 文件解压后对应的目录（这里称为应用目录）。

```xml
<Context path="/myapp" docBase="myapp" reloadable="true">
    <!-- 其他配置 -->
</Context>
```

- **path：** Web 应用程序的上下文路径，即访问应用程序的 URL 路径，例如 `"/myapp"`。
  - 如果一个 Context 元素的 path 属性为 ””，那么这个 Context 是虚拟主机的默认 Web 应用；当请求的 uri 与所有的 path 都不匹配时，使用该默认 Web 应用来处理。
  - **在自动部署场景下，不能指定 path 属性，path 属性由配置文件的文件名、WAR 文件的文件名或应用目录的名称自动推导出来**。如果名称是 ROOT，则该Web应用是虚拟主机默认的 Web 应用，此时 path 属性推导为””。
- **docBase：** 应用程序的目录或 WAR 文件的路径，指定应用程序的位置。
  - 需要注意的是，**在自动部署场景下，docBase 不在 appBase 目录中，才需要指定；如果 docBase 指定的 WAR 包或应用目录就在 appBase 中，则不需要指定**，因为 Tomcat 会自动扫描 appBase 中的 WAR 包和应用目录，指定了反而会造成问题。
- **reloadable：** 是否允许应用程序在运行时重新加载。设置为 `"true"` 时，应用程序可以在不重启 Tomcat 的情况下重新加载。
- **workDir：** 应用程序的工作目录路径。
- **antiResourceLocking：** 是否防止资源锁定问题。设置为 `"true"` 可避免在扩展 WAR 文件时出现文件锁定问题。
- **unpackWAR：** 是否在部署时解压 WAR 文件。
- **useNaming：** 是否启用命名资源。设置为 `"true"` 时，启用 JNDI 命名和资源管理。

#### 自动部署

Web 应用的动态部署在 Tomcat 中通过配置所在的虚拟主机（Host）来实现。配置方式是利用 Host 元素的两个属性：deployOnStartup 和 autoDeploy。

- `deployOnStartup` 和 `autoDeploy` 属性设置为 `true` 时，Tomcat 启动时会自动部署。检测到新的 Web 应用或更新时，会触发应用的部署或重新部署。

这两者主要区别在于：

- `deployOnStartup` 为 `true` 时，Tomcat 在启动时检查 Web 应用，所有检测到的 Web 应用都被视为新应用。
- `autoDeploy` 为 `true` 时，Tomcat 在运行时定期检查新的 Web 应用或已有应用的更新。

这些属性依赖于 Host 元素的两个重要设置：

- `appBase` 属性指定了 Web 应用的目录，默认为 `webapps`，这是相对于 Tomcat 根目录下的 `webapps` 文件夹。
- `xmlBase` 属性指定了 Web 应用的 XML 配置文件所在目录，默认为 `conf/<engine_name>/<host_name>`，例如：主机 localhost 的 `xmlBase` 默认路径为 `$TOMCAT_HOME$/conf/Catalina/localhost`。

**除了自动部署外，也可以通过 server.xml 中的 `<Context>` 元素静态部署 Web 应用**。静态部署与自动部署可同时存在，但不推荐静态部署，因为 `server.xml` 无法动态重加载，需要重新启动服务器来应用修改。

Tomcat 扫描应用更新遵循以下顺序：

1. 检查虚拟主机指定的 `xmlBase` 下的 XML 配置文件。
2. 检查虚拟主机指定的 `appBase` 下的 WAR 文件。
3. 检查虚拟主机指定的 `appBase` 下的应用目录。

## 全局环境

**全局环境配置（GlobalNamingResources）：**用于配置全局命名资源，它包含了整个 Tomcat 实例共享的 JNDI 资源。这些资源可以被所有在 Tomcat 中运行的 web 应用程序访问。

通过配置全局 JNDI 资源，可以实现对数据库连接池、消息队列工厂、邮件会话等共享资源的集中管理和配置。

**子标签**

- **`<Resource>`：** 配置一个全局 JNDI 资源。
- **`<ResourceLink>`：** 配置一个指向另一个 Context 中定义的资源的链接。
  - **name：** JNDI 名称，用于在 JNDI 中标识资源的名称。
  - **global：** 指向另一个 Context 中定义的资源的全局 JNDI 名称。
  - **type：** 资源类型，如 `javax.sql.DataSource`。

```xml
<GlobalNamingResources>
    <!-- 配置一个全局 JNDI 数据库连接池 -->
    <Resource name="jdbc/myDB" auth="Container" type="javax.sql.DataSource"
               maxTotal="100" maxIdle="30" maxWaitMillis="10000"
               username="root" password="password" driverClassName="com.mysql.cj.jdbc.Driver"
               url="jdbc:mysql://localhost:3306/myDB"/>

    <!-- 配置一个资源链接 -->
    <ResourceLink name="linkToGlobalDB" global="jdbc/myDB" type="javax.sql.DataSource"/>
</GlobalNamingResources>
```

## 监听器

`<Listener>` 标签用于配置监听器，它可以捕获和响应 Tomcat 发出的事件，如应用程序的启动、停止、会话创建和销毁等。通过监听器，可以执行特定的操作来响应这些事件。

主要的监听器有以下几种：

- **ServletContextListener：** 用于监听 Web 应用的启动和关闭事件。
- **ServletContextAttributeListener：** 用于监听 ServletContext 范围（application）内属性的改变。
- **ServletRequestListener：** 用于监听用户请求的创建和销毁。
- **ServletRequestAttributeListener：** 用于监听 ServletRequest 范围（request）内属性的改变。
- **HttpSessionListener：** 用于监听用户会话（session）的创建和销毁。
- **HttpSessionAttributeListener：** 用于监听 HttpSession 范围（session）内属性的改变。

```xml
<Listener className="com.example.MyServletContextListener" />
<Listener className="com.example.MySessionListener" />
```

监听器不允许内嵌其他组件。

> Tomcat 的监听器并不完全等同于 Servlet 的监听器，尽管它们有一些相似之处。
>
> 在 Tomcat 中，监听器是用于捕获服务器端特定事件的组件，而 Servlet 监听器是 Servlet API 的一部分，用于捕获和响应 Servlet 上下文、请求和会话等事件。
>
> Tomcat 的监听器能够捕获更广泛的事件，不仅限于 Servlet 相关的事件。它们可以捕获服务器级别的事件，如服务器启动和关闭、应用程序的部署和销毁等。而 Servlet 监听器主要关注于 Servlet 生命周期和请求、会话等与 Servlet 相关的事件。

## 资源配置

**资源配置（Resources）：**标签通常用于在 `<GlobalNamingResources>` 元素中定义全局 JNDI 资源。它允许配置和管理整个 Tomcat 实例中共享的资源，如数据库连接池、JMS 连接工厂、邮件会话等。

 `<Resource>` 标签也可以在某些情况下用于 `<Context>` 元素内，用于为特定的 web 应用程序配置局部 JNDI 资源。这样的资源只在该上下文范围内可见。

```xml
<Resource name="jdbc/TestDB" auth="Container"
          type="javax.sql.DataSource"
          maxActive="100" maxIdle="30" maxWait="10000"
          username="root" password="password"
          driverClassName="com.mysql.jdbc.Driver"
          url="jdbc:mysql://localhost:3306/test"/>
```

- **name：** JNDI 名称，用于在 JNDI 中标识资源的名称。
- **auth：** 认证方式，可以是 `Container` 或 `Application`。`Container` 表示 Tomcat 将在容器级别进行认证，`Application` 表示应用程序级别的认证。
- **type：** 资源类型，如 `javax.sql.DataSource`。
- **maxTotal：** 连接池中允许的最大连接数。
- **maxIdle：** 连接池中允许的最大空闲连接数。
- **maxWaitMillis：** 获取连接的最大等待时间（毫秒）。
- **username：** 访问资源所需的用户名。
- **password：** 访问资源所需的密码。
- **driverClassName：** 数据库驱动类名。
- **url：** 资源的 URL。

## 阀门

`<Valve>` 标签用于配置 Tomcat 的阀门（Valve）。Valve 是 Tomcat 中的一种组件，用于拦截并处理请求和响应，允许开发者在请求处理的各个阶段插入自定义的处理逻辑。Valve 可以在请求和响应的不同阶段执行额外的处理，比如日志记录、安全控制、性能监控等。

Valve 可以在全局范围内应用于整个 Tomcat 服务器，也可以在特定的 Context 或 Host 中配置。它们以管道（pipeline）的方式工作，可以串联多个 Valve，每个 Valve 负责特定的任务或拦截请求。

在 Tomcat 中代表了请求处理流水线上的一个组件；Valve可以与Tomcat的容器（Engine、Host 或 Context）关联。

以下是一些常见的 Valve：

- **AccessLogValve：** 用于记录访问日志。
- **RemoteAddrValve：** 根据远程客户端的 IP 地址过滤请求。
- **RequestDumperValve：** 用于详细记录请求和响应信息，用于调试和分析。
- **ErrorReportValve：** 处理错误报告和异常信息的显示。
- **SingleSignOn：** 用于单点登录（SSO）。

```xml
<Valve className="org.apache.catalina.valves.AccessLogValve"
       directory="logs" prefix="localhost_access_log" suffix=".txt"
       pattern="%h %l %u %t &quot;%r&quot; %s %b" />
```

- **className：** 指定 Valve 的 Java 类的完全限定类名，用于告诉 Tomcat 使用哪个类作为 Valve。
- **directory：** 用于 AccessLogValve，指定存储访问日志文件的目录。
- **prefix 和 suffix：** 用于 AccessLogValve，指定访问日志文件的前缀和后缀。
- **pattern：** 用于 AccessLogValve，指定记录访问日志的格式。
- **allow：** 用于 RemoteAddrValve，指定允许访问的 IP 地址范围。
- **deny：** 用于 RemoteAddrValve，指定拒绝访问的 IP 地址范围。

## 域

`<Realm>` 元素用于配置身份验证（Authentication）和授权（Authorization）机制。Realm 定义了如何获取用户的安全信息，如用户名、密码以及角色信息，并用于验证用户的身份和授权访问受限资源。

Tomcat 支持多种类型的 Realm，包括：

- **MemoryRealm：** 使用内存中的用户和角色信息进行认证和授权。
- **JDBCRealm：** 通过 JDBC 连接到数据库获取用户信息进行认证和授权。
- **JNDIRealm：** 通过 JNDI 获取用户信息进行认证和授权。
- **LDAPRealm：** 通过 LDAP 获取用户信息进行认证和授权。

```xml
<Realm className="org.apache.catalina.realm.JDBCRealm"
       driverName="com.mysql.jdbc.Driver"
       connectionURL="jdbc:mysql://localhost:3306/userdb"
       connectionName="username"
       connectionPassword="password"
       userTable="users"
       userNameCol="username"
       userCredCol="password"
       userRoleTable="user_roles"
       roleNameCol="role"
       digest="MD5"
       />  
```

- **className：** 指定 Realm 的类名，确定使用哪种类型的 Realm。
- **connectionURL：** 仅适用于 JDBCRealm，指定数据库连接的 URL。
- **userTable 和 userNameCol：** 仅适用于 JDBCRealm，指定存储用户名的表和列。
- **userRoleTable 和 roleNameCol：** 仅适用于 JDBCRealm，指定存储用户角色的表和列。
- **userSearch：** 仅适用于 JNDIRealm，指定搜索用户信息的方式。

# 连接器

Tomcat 连接器框架——Coyote。

**核心功能**

+ **监听网络端口**：接收和响应网络请求。
+ **网络字节流处理**：将收到的网络字节流转换成 Tomcat Request 再转成标准的 ServletRequest 给容器，同时将容器传来的 ServletResponse 转成 Tomcat Response 再转成网络字节流。

## 模块

为满足连接器的两个核心功能，需要一个通讯端点来监听端口；需要一个处理器来处理网络字节流；最后还需要一个适配器将处理后的结果转成容器需要的结构。

+ **Endpoint**：端点，用来处理 Soket 接收发的逻，其内部由 Acceptor 监听调求、Handler 处理数据、AsyncTimeout 检查两求超时，具体的安现有NioEndPoint，AprEndpolnt 等。
+ **Processor**：处理器，负击构建 Tomcat Request 和 Response 对象，具体的现有 Http11Processor、StreamProcessor 等。
+ **Adapter**：透配器，实现 Tomcat Request，Response 与 ServletRequest、ServletResponse 之间的相互转换，这采用的是经典的适配器设计模式。
+ **ProtocolHandler**：协议处理器，将不同的协议和通讯方式组合封装成对应的协议处理器，如 Http11NioProtocol 封装的是 HTTP + NIO。

对应的源码包路径 `org.apache.coyote` 。

![img](D:\Repository\Typora\v2-1575c01f238e1890f737cb069e87f06c_720w.webp)

## 线程池

Servlet 容器处理请求，是需要 Connector 进行调度和控制的，**Connector 是Tomcat 处理请求的主干**，因此 Connector 的配置和使用对 Tomcat 的性能有着重要的影响。

根据协议的不同，Connector 可以分为 HTTP Connector、AJP Connector 等。

Connector 在处理 HTTP 请求时，会使用不同的 protocol。不同的 Tomcat 版本支持的 protocol 不同，其中最典型的 protocol 包括 BIO、NIO 和 APR（Tomcat 7 中支持这 3 种，Tomcat8 增加了对 NIO2 的支持，而到了 Tomcat8.5 和 Tomcat9.0，则去掉了对 BIO 的支持）。

BIO 是 Blocking IO，顾名思义是阻塞的 IO；NIO 是 Non-blocking IO，则是非阻塞的 IO。而 APR 是 Apache Portable Runtime，是 Apache 可移植运行库，利用本地库可以实现高可扩展性、高性能；Apr 是在 Tomcat 上运行高并发应用的首选模式，但是需要安装 apr、apr-utils、tomcat-native 等包。

### 差异

在Tomcat中，无论是基于BIO（Blocking I/O）还是NIO（Non-blocking I/O）的Connector，其处理请求的基本流程是类似的：

1. **接收连接：** 当客户端与服务器建立连接时，连接会被放入 accept 队列。
2. **获取请求数据并生成 Request：** 从连接中读取数据，将其转换为 HTTP 请求（Request）。
3. **调用 Servlet 容器处理请求：** 将请求交给 Servlet 容器处理，经过处理后生成响应（Response）。
4. **返回 Response：** 将响应数据发送回客户端。

在BIO实现的Connector中，主要使用 `JIoEndpoint` 对象来处理请求。这个对象维护了 `Acceptor` 和 `Worker`。`Acceptor` 负责接收传入的连接请求，然后从 `Worker` 线程池中选取空闲线程处理连接，如果线程池没有空闲线程，则 `Acceptor` 会阻塞等待。`Worker` 是Tomcat自带的线程池，负责处理连接。

![img](D:\Repository\Typora\1174710-20171108203711513-1728828893.png)

在NIO实现的Connector中，使用 `NIoEndpoint` 对象来处理请求。除了 `Acceptor` 和 `Worker`，还有 `Poller`。`Acceptor` 接收连接后，不直接将连接交给 `Worker`，而是先发送给 `Poller`，`Poller` 实现了NIO的关键功能。`Poller` 维护了一个 `Selector` 对象，将接收的连接注册到 `Selector` 中。通过轮询 `Selector`，`Poller` 找出可读的连接，并使用 `Worker` 中的线程处理请求。

在NIO模式中，与BIO最大的不同是“读取连接并将其交给 `Worker` 中的线程”的过程是非阻塞的。这意味着，在长连接的情况下，当一个请求结束后，连接并不会立即释放。

在BIO中，处理连接的工作线程会一直被占用，无法释放，因此同时处理的连接数受限于最大线程数。

而在NIO中，处理连接的过程是非阻塞的，工作线程不会被长时间占用，因此同时处理的连接数远远超过了最大线程数，大大提高了并发性能。

### 指定

Connector可以使用不同的协议，而协议的选择通过 `<Connector>` 元素中的 `protocol` 属性进行指定。这个属性允许设置不同的协议来处理连接。

以下是一些常见的 protocol 属性取值及其对应的协议：

- **HTTP/1.1**：默认值，使用的协议与Tomcat版本有关，通常在Tomcat 7中默认使用BIO或APR，而在Tomcat 8中默认使用NIO或APR。
- **org.apache.coyote.http11.Http11Protocol**：代表BIO（Blocking I/O）协议。
- **org.apache.coyote.http11.Http11NioProtocol**：代表NIO（Non-blocking I/O）协议。
- **org.apache.coyote.http11.Http11Nio2Protocol**：代表NIO2协议。
- **org.apache.coyote.http11.Http11AprProtocol**：代表APR（Apache Portable Runtime）协议。

若未指定 `protocol` 属性，则默认使用 `HTTP/1.1`。在不同的Tomcat版本中，这意味着不同的协议选择策略。在Tomcat 7中，会自动选择使用BIO或APR（如果找到APR需要的本地库，则使用APR，否则使用BIO）。而在Tomcat 8中，会自动选择使用NIO或APR（如果找到APR需要的本地库，则使用APR，否则使用NIO）。

### 参数

在Tomcat处理请求的过程中，Connector中的一些参数起着关键作用：

1. **acceptCount**：
   - **功能**：表示accept队列的长度。当客户端向服务器发送请求时，如果客户端与OS完成三次握手建立了连接，则OS将该连接放入accept队列。`acceptCount` 就是指定了这个队列的长度。
   - **默认值**：通常是100。当队列中连接数达到 `acceptCount` 时，新进来的请求会被拒绝。
2. **maxConnections**：
   - **功能**：代表Tomcat在任意时刻接收和处理的最大连接数。当接收的连接数达到 `maxConnections` 时，Acceptor线程不会读取accept队列中的连接。
   - **默认值**：与连接器使用的协议有关。对于NIO，默认值是10000；APR/native的默认值是8192；而对于BIO，默认值是maxThreads（如果配置了Executor，则默认值是Executor的maxThreads）。
   - **其他**：若设置为-1，则连接数不受限制。在Windows下，APR/native的 `maxConnections` 会自动调整为设置值以下最大的1024的整数倍。
3. **maxThreads**：
   - **功能**：表示请求处理线程的最大数量。若未使用Executor，`maxThreads` 规定的是Connector内部线程池的最大线程数。
   - **默认值**：通常是200。若Connector绑定了Executor，这个值会被忽略，因为Connector将使用绑定的Executor来执行任务。
   - **补充**：实际运行线程的数量可能远大于CPU核心数，因为大部分时间线程可能在阻塞状态，如等待数据库返回数据或等待硬盘读写数据。

**参数设置建议**：

- **maxThreads** 应远大于CPU核心数，以充分利用CPU资源。然而，设置过大可能导致线程切换带来的性能下降。
- **maxConnections** 应根据Tomcat运行模式来设置：对于BIO，与maxThreads一致；对于NIO，应远大于maxThreads。
- **acceptCount** 的设置需考虑应用在连接过高情况下的反应。设置过大会导致请求等待时间长，而设置过小会立即返回connection refused。

### Executor

Executor 元素在 Tomcat 中扮演着管理线程池的角色，可以被多个组件共享。为了使用该线程池，其他组件需要通过 executor 属性指定。

通常，Executor 作为 Service 元素的内嵌元素使用。主要用途是为 Connector 组件提供线程池支持。Executor 元素需放置在 Connector 之前，确保 Connector 能够正确使用线程池。

```xml
<Executor name="tomcatThreadPool" namePrefix="catalina-exec-" maxThreads="150" minSpareThreads="4" />
<Connector executor="tomcatThreadPool" port="8080" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" acceptCount="1000" />
```

Executor 元素的主要属性包括：

- **name**：线程池的标识符。
- **maxThreads**：线程池中的最大活跃线程数，默认为 200（适用于 Tomcat 7 和 8）。
- **minSpareThreads**：线程池中保持的最小线程数，最小值是 25。
- **maxIdleTime**：线程空闲的最大时间，超过该值时关闭线程（除非线程数小于 minSpareThreads），单位为毫秒，默认为 60000（1 分钟）。
- **daemon**：线程是否为后台线程，默认为 true。
- **threadPriority**：线程优先级，默认为 5。
- **namePrefix**：线程名字的前缀。线程池中的线程名字通常为 `namePrefix` 加上线程编号。

# 容器

Tomcat 容器框架——Catalina。

## 模块

每个 Service 会包含一个容器。容器由一个引擎可以管理多个虚拟主机。每个虚拟主机可以管理多个 Web 应用。每个 Web 应用会有多个 Servlet 包装器。Engine、Host、Context 和 Wrapper，四个容器之间属于父子关系。

+ **Engine**：引擎，管理多个虚拟主机。
+ **Host**：虑拟主机，负责 Web 应用的部署。
+ **Context**：Web 应用，包含多个 Servlet 封装器。
+ **Wrapper**：封装器，容器的最底层。对 Servlet 进行封装，负责实例的创建、执行和销毁功能。

对应的源码包路径 `org.apache.coyote` 。

![img](D:\Repository\Typora\v2-4e93d47769861f9179df5be4399f1f9a_720w.webp)

## 流程

容器的请求处理过程就是在 Engine、Host、Context 和 Wrapper 这四个容器之间层层调用，最后在 Servlet 中执行对应的业务逻辑。各容器都会有一个通道 Pipeline，每个通道上都会有一个 Basic Valve（如StandardEngineValve）， 类似一个闸门用来处理 Request 和 Response 。

![img](D:\Repository\Typora\v2-4981dad85e8512646419b5992d13dd79_720w.webp)

打开 tomcat/conf 目录下的 server.xml 文件来分析一个http://localhost:8080/docs/api 请求。

![img](D:\Repository\Typora\v2-cf795c8322e5c13cfed76b04ac676868_720w.webp)

第一步：连接器监听的端口是8080。由于请求的端口和监听的端口一致，连接器接受了该请求。

第二步：因为引擎的默认虚拟主机是 localhost，并且虚拟主机的目录是webapps。所以请求找到了 tomcat/webapps 目录。

第三步：解析的 docs 是 web 程序的应用名，也就是 context。此时请求继续从 webapps 目录下找 docs 目录。有的时候我们也会把应用名省略。

第四步：解析的 api 是具体的业务逻辑地址。此时需要从 docs/WEB-INF/web.xml 中找映射关系，最后调用具体的函数。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Server port="8005" shutdown="SHUTDOWN">

  <Service name="Catalina">

	<!-- 连接器监听端口是 8080，默认通讯协议是 HTTP/1.1 -->
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
			   
	<!-- 名字为 Catalina 的引擎，其默认的虚拟主机是 localhost -->
    <Engine name="Catalina" defaultHost="localhost">

	  <!-- 名字为 localhost 的虚拟主机，其目录是 webapps-->
      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">

      </Host>
    </Engine>
  </Service>
</Server>
```

# 域

Realm，可以把它理解成“域”，也可以理解成“组”，因为它类似类 Unix 系统中组的概念。

Realm 域提供了一种用户密码与 web 应用的映射关系。

因为 Tomcat 中可以同时部署多个应用，因此并不是每个管理者都有权限去访问或者使用这些应用，因此出现了用户的概念。但是想想，如果每个应用都去配置具有权限的用户，那是一件很麻烦的事情，因此出现了 role 这样一个概念。具有某一角色，就可以访问该角色对应的应用，从而达到一种域的效果。

![img](D:\Repository\Typora\041922378941318.png)

参考上面的图：

- 每个用户我们可以设置不同的角色（在 tomcat-users.xml 中配置），
- 每个应用中会设定可以访问的角色（在 web.xml 中配置），
- 当 tomcat 启动后，就会通过 Realm 进行验证（在 server.xml 中配置），通过验证才可以访问该应用，
- 从而达到角色安全管理的作用。

可以看到默认情况下，Realm 的配置位置是在 Engine 标签内部，并且使用的是 UserDatabase 的方式。其他的方式会在下面部分说明。

其中 Realm 的不同位置也会影响到它作用的范围。

- 在 `<Engine>` 元素内部 —— Realm 将会被所有的虚拟主机上的web应用共享，除非它被 `<Host>` 或者 `<Context>` 元素内部的 Realm 元素重写。
- 在 `<Host>` 元素内部 —— 这个 Realm 将会被本地的虚拟主机中的所有的 web 应用共享，除非被 `<Context>` 元素内部的 Realm 元素重写。
- 在 `<Context>` 元素内部 —— 这个 Realm 元素仅仅被该 Context 指定的应用使用。

## 获取用户信息

目前 tomcat 支持多种 Realm 管理方式，即支持多种方式来读取用户信息进行验证。参考如下：

1. JDBCRealm 用户授权信息存储于某个关系型数据库中，通过 JDBC 驱动获取信息验证
2. DataSourceRealm 用户授权信息存储于关于型数据中，通过 JNDI 配置 JDBC 数据源的方式获取信息验证
3. JNDIRealm 用户授权信息存储在基于 LDAP 的目录服务的服务器中，通过 JNDI 驱动获取并验证
4. UserDatabaseRealm **默认的配置方式**，信息存储于 XML 文档中 `conf/tomcat-users.xml`
5. MemoryRealm 用户信息存储于内存的集合中，对象集合的数据来源于 xml 文档 `conf/tomcat-users.xml`
6. JAASRealm 通过 JAAS 框架访问授权信息

## 配置过程

1. **在server.xml中配置realm访问方式**

   参考下默认的配置 server.xml 中，可以看到默认情况下使用的就是 UserDatabaseRealm 的方式：

![img](D:\Repository\Typora\041927237389043.png)

　　上图中的代码配置了 UserDatabase 的目录文件，为 conf/tomcat-users.xml

![img](D:\Repository\Typora\041928039266987.png)

　　上图中的代码配置使用的Realm方式。

2. **在tomcat-users.xml中配置用户密码以及分配角色**

![img](D:\Repository\Typora\042011359419173.png)

　　上面是 tomcat-users.xml 中的配置内容。

3. **在应用的 web.xml 中配置其访问角色以及安全限制的内容**

   关于 Realm 域的使用，一般都是用来管理一些安全性要求很高的应用，最常见的就是 manager 应用。

   manager 应用用于在不停止 tomcat 的情况下部署或者停止某些应用，处于安全考虑，默认情况下时不能访问 manager 应用的，因此需要现在 tomcat-users.xml 中添加用户以及相应的角色，才能访问。

```xml
<security-constraint>
    <web-resource-collection>
      <web-resource-name>HTMLManger and Manager command</web-resource-name>
      <url-pattern>/jmxproxy/*</url-pattern>
      <url-pattern>/html/*</url-pattern>
      <url-pattern>/list</url-pattern>
      <url-pattern>/sessions</url-pattern>
      <url-pattern>/start</url-pattern>
      <url-pattern>/stop</url-pattern>
      <url-pattern>/install</url-pattern>
      <url-pattern>/remove</url-pattern>
      <url-pattern>/deploy</url-pattern>
      <url-pattern>/undeploy</url-pattern>
      <url-pattern>/reload</url-pattern>
      <url-pattern>/save</url-pattern>
      <url-pattern>/serverinfo</url-pattern>
      <url-pattern>/status/*</url-pattern>
      <url-pattern>/roles</url-pattern>
      <url-pattern>/resources</url-pattern>
    </web-resource-collection>
    <auth-constraint>
       <!-- NOTE:  This role is not present in the default users file -->
       <role-name>manager</role-name>
    </auth-constraint>
  </security-constraint>

  <!-- Define the Login Configuration for this Application -->
  <login-config>
    <auth-method>BASIC</auth-method>
    <realm-name>Tomcat Manager Application</realm-name>
  </login-config>

  <!-- Security roles referenced by this web application -->
  <security-role>
    <description>
      The role that is required to log in to the Manager Application
    </description>
  <role-name>manager</role-name>
</security-role>
```

其中 role-name 就定义了可以访问的角色。

其他内容中上面定义了限制访问的资源，下面的 Login-config 比较重要。它定义了验证的方式，BASIC 就是基本的弹出对话框输入用户名密码。还是 DIGEST 方式，这种方式会对网络中的传输信息进行加密，更安全。

# 问题

1. 多虚拟主机域名

   对于 Tomcat 来说，在一台服务器上可能会配置多个虚拟主机，这对应着不同的域名或者 IP 地址。当你访问服务器的某个域名或 IP 时，Tomcat 会根据请求中的主机头信息（Host Header）来决定将请求交给哪个虚拟主机处理。这意味着即使在同一个物理服务器上，你可以配置多个虚拟主机来提供不同的服务。

   **虚拟主机允许你在单个物理服务器上托管多个不同的域名，并且可以根据不同的域名来提供不同的网站或应用程序**。

   举个例子，假设你有一台服务器，它有一个公网IP地址。你可能会在这个服务器上配置多个虚拟主机：

   1. **example.com** - 指向Tomcat的一个虚拟主机，用于托管一个网站。
   2. **api.example.com** - 指向同一台Tomcat服务器上的另一个虚拟主机，用于托管API服务。
   3. **blog.example.com** - 指向Tomcat的第三个虚拟主机，用于托管博客。

   当用户访问 **example.com**、**api.example.com** 或 **blog.example.com** 中的任何一个时，Tomcat 都能够根据请求中的主机头信息来区分请求，然后将请求路由到正确的虚拟主机或应用程序上。