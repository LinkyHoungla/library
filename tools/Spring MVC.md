# Servlet 引入

```xml
<context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>
    classpath*:/framework/applicationContext.xml
  </param-value>
</context-param>

<context-param>
  <param-name>globalInitializerClasses</param-name>
  <param-value>com.wckj.framework.spring.CustomApplicationContextInitializer</param-value>
</context-param>

<!-- spring bean service-->
<listener>
  <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

这段配置在 **web.xml** 中是用来配置和初始化 Spring Web 应用程序的一部分。

1. **<context-param>** 配置：

- - **<param-name>contextConfigLocation</param-name>**：这个参数用来指定 Spring 的上下文配置文件的位置。
  - **<param-value>classpath\*:/framework/applicationContext.xml</param-value>**：指定了 Spring 的配置文件 **applicationContext.xml** 的路径。**classpath\*:**表示在类路径下查找，**/framework/applicationContext.xml** 是文件的相对路径。

1. **<context-param>** 中的另一个配置：

- - **<param-name>globalInitializerClasses</param-name>**：这个参数定义了全局的初始化类，用于在 Spring 上下文启动时进行自定义的初始化操作。
  - **<param-value>com.wckj.framework.spring.CustomApplicationContextInitializer</param-value>**：指定了一个自定义的 **CustomApplicationContextInitializer** 类，该类是一个实现了 Spring 的 **ApplicationContextInitializer** 接口的自定义初始化类。

1. **<listener>** 配置：

- - **<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>**：定义了一个监听器，**ContextLoaderListener** 是 Spring 提供的用于在 Web 应用程序启动时加载 Spring 上下文的监听器。它会在 Web 应用程序启动时创建 Spring 的根上下文，从指定的配置文件加载 bean 定义，并将其放入 ServletContext 中。

总体来说，这段配置的作用是：

- 指定了 Spring 配置文件 **applicationContext.xml** 的位置和路径。
- 定义了一个全局的初始化类 **CustomApplicationContextInitializer**，用于在 Spring 上下文启动时进行一些自定义的初始化操作。
- 注册了一个 **ContextLoaderListener** 监听器，负责在 Web 应用程序启动时加载 Spring 上下文，并初始化其中的 bean 定义。