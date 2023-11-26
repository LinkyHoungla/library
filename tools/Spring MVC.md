# 层级

该框架通常分为四个层次，分别是持久层（Dao层，也称Mapper层），业务层（Service层），表现层（Controller层，也称Handler层）以及前端视图层（View）。

## 持久层

Dao层（Mapper层）是负责处理数据持久化的层，它负责与数据库进行交互，包括数据的读取、写入、更新和删除等操作。

**关键特点**

- Dao层通常首先定义接口，接着在Spring配置文件中定义接口的具体实现类。
- 在业务模块中，通过调用Dao接口来处理数据操作，而不需要关心具体的实现类。
- 数据源的配置和与数据库连接相关的参数通常也在Spring的配置文件中进行定义。

## 业务层

Service层主要负责业务逻辑的设计和处理，它是连接持久层和控制层的桥梁。

**关键特点**

- 首先设计Service接口，然后再实现接口的具体类。
- 在Spring配置文件中配置Service接口和具体实现的关联。
- Service层的实现通常需要调用Dao层提供的接口来进行数据的处理。
- 每个业务模块都有对应的Service接口，其中包含了该模块所需的业务逻辑方法。

## 表现层

Controller层（Handler层）负责控制业务流程，接收客户端请求，调用Service层提供的接口，然后返回视图给前端。

**关键特点**：

- 控制器的配置也在Spring的配置文件中进行。
- 控制器通过调用Service层提供的接口来控制业务流程。
- 不同的业务流程可以由不同的控制器进行控制。
- 在具体开发中，可以将业务流程抽象成可重用的子单元流程模块。

## 前端视图层

View层主要负责展示数据和与用户交互，通常使用JSP等技术来呈现页面。

**关键特点**：

- 前端视图层与控制层紧密结合，负责展示页面并接受用户输入。
- 在SSM框架中，前端视图层通常是JSP页面。

# 工作流程

## 详细流程

![img](assets/383f9a089b3149daaf4c07f78d3ab959.png)

用户发出请求时，Spring MVC 应用程序的请求处理流程通常包括以下步骤：

1. 用户发送请求至前端控制器 `DispatcherServlet`。
2. `DispatcherServlet` 接收请求并调用 `HandlerMapping` 处理器映射器。
3. `HandlerMapping` 根据请求信息找到具体的处理器（可以通过配置文件或注解进行查找），并生成处理器对象及处理器拦截器（如果有的话），然后返回给 `DispatcherServlet`。
4. `DispatcherServlet` 调用 `HandlerAdapter` 处理器适配器。
5. `HandlerAdapter` 经过适配调用具体的处理器，通常是控制器（也叫后端控制器）。
6. 控制器执行完成后返回 `ModelAndView` 对象。
7. `HandlerAdapter` 将控制器执行结果 `ModelAndView` 返回给 `DispatcherServlet`。
8. `DispatcherServlet` 将 `ModelAndView` 传递给 `ViewResolver` 视图解析器。
9. `ViewResolver` 解析 `ModelAndView` 并返回具体的 `View` 对象。
10. `DispatcherServlet` 根据 `View` 对象进行视图渲染，即将模型数据填充至视图中。
11. `DispatcherServlet` 响应用户，将渲染后的视图返回给用户。

这个流程是 Spring MVC 请求处理的核心流程，它使开发者能够通过配置路由、控制器、视图和拦截器来管理和处理请求。

## 简单流程

![在这里插入图片描述](assets/2aeceda1ac36468eb3ada8a0867a04b3.png)

1. 客户端发送请求到 `DispatcherServlet`，即分发器。
2. `DispatcherServlet` 控制器查询 `HandlerMapping`，找到能够处理请求的 `Controller`。
3. `Controller` 调用业务逻辑进行处理，然后返回 `ModelAndView` 对象。
4. `DispatcherServlet` 查询视图解析器，找到 `ModelAndView` 指定的视图。
5. 视图负责将 `ModelAndView` 中的结果显示到客户端。

这个流程简洁地描述了 Spring MVC 的请求处理过程，其中 `DispatcherServlet` 起到核心的分发和控制的作用，`HandlerMapping` 帮助找到合适的 `Controller`，`Controller` 处理具体的业务逻辑，`ModelAndView` 包含处理结果和视图信息，而视图负责呈现最终的结果给客户端。

# 核心组件

Spring MVC框架中的核心组件和九大组件是整个请求处理过程中不可或缺的部分，以下是对它们的详细介绍和概括：

**DispatcherServlet（请求的入口）：**

- Spring MVC的核心组件，负责接收和分发请求，是整个请求处理流程的入口。
- 协调各个组件的工作，负责处理请求的映射、拦截器的执行、视图解析和渲染等。

**MultipartResolver（处理文件上传请求）：**

- 用于解析内容类型（Content-Type）为`multipart/*`的请求，例如处理文件上传的请求。
- 负责获取请求中的参数信息以及上传的文件，方便后续处理。

**HandlerMapping（处理器匹配器）：**

- 请求的处理器匹配器，负责将请求映射到合适的HandlerExecutionChain（处理器执行链）。
- 包含了处理器（handler）和拦截器（interceptors）。

**HandlerAdapter（处理器适配器）：**

- 负责执行处理器（handler）。
- 因为处理器的类型多变，需要一个调用者来实现处理器的执行，包括Controller接口、HttpRequestHandler接口、@RequestMapping注解等。
- 这里的适配器负责执行处理器并返回适当的结果。

**HandlerExceptionResolver（处理器异常解析器）：**

- 处理器异常解析器用于处理执行处理器时发生的异常，将异常解析（转换）为对应的ModelAndView结果。

**RequestToViewNameTranslator（视图名称转换器）：**

- 用于解析出请求的默认视图名。
- 通过请求信息可以确定默认的视图名，然后根据国际化信息获取最终的视图名。

**LocaleResolver（本地化解析器）：**

- 本地化解析器提供国际化支持。
- 负责解析客户端请求的区域信息，以便应用程序可以提供多语言支持。

**ThemeResolver（主题解析器）：**

- 主题解析器提供了可设置应用程序整体样式风格的支持。
- 用于解析客户端请求的主题信息，允许应用程序切换不同的主题样式。

**ViewResolver（视图解析器）：**

- 视图解析器负责根据视图名和国际化信息获取最终的视图View对象。
- 负责解析视图名称，查找视图模板文件，并渲染视图。

**FlashMapManager（FlashMap管理器）：**

- FlashMapManager负责在重定向时，将参数保存在临时存储（默认为Session）中。
- 用于在重定向过程中传递参数信息。

Spring MVC的九大组件各自承担特定的职责，通过协同工作来完成整个请求处理过程。

DispatcherServlet是请求的入口，负责协调各个组件工作，HandlerMapping和HandlerAdapter用于处理请求映射和执行处理器，HandlerExceptionResolver处理异常，ViewResolver负责视图解析，而其他组件则提供更多的功能，如文件上传处理、国际化、主题支持等。

## 注解功能

**@Controller 注解：**

- 用于标记一个类为Spring Web MVC的控制器。
- Spring MVC会扫描带有@Controller注解的类，并生成处理器对象，用于处理请求。
- 除了添加 `@Controller` 注解这种方式以外，你还可以实现 Spring MVC 提供的 `Controller` 或者 `HttpRequestHandler` 接口，对应的实现类也会被作为一个**处理器**对象

**@RequestMapping 注解：**

- 用于配置处理器方法的HTTP请求方法、URI等信息，建立请求和方法的映射。
- 可以用于类和方法上，在类上面一般是配置这个**控制器**的 URI 前缀。

**@RestController 注解：**

- 继承自@Controller注解，增加@ResponseBody注解。
- 适用于前后端分离架构，用于提供Restful API，返回JSON格式数据。
- 数据格式根据客户端的ACCEPT请求头来决定。

**@GetMapping 注解：**

- 是@RequestMapping注解的特例，用于配置GET请求方法的处理器方法。
- 仅可注册在方法上，提高代码的清晰度。

**@RequestParam 和 @PathVariable 注解：**

- 用于处理方法参数的注解。
- @RequestParam用于从请求的参数中获取参数值。
- @PathVariable用于从URI中获取参数值。

**返回JSON格式使用的注解：**

- 通常使用@ResponseBody注解，它告诉Spring MVC框架将方法返回值序列化为JSON格式。
- 在@RestController注解的类中，每个方法都会默认使用@ResponseBody，以返回JSON数据。

> 需要配合相应的支持JSON格式的HttpMessageConverter实现类，如MappingJackson2HttpMessageConverter，来实现JSON格式的数据转换。

# 拦截器

Spring的处理程序映射机制包括处理程序拦截器，当你希望将特定功能应用于某些请求时，例如，检查用户主题时，这些拦截器非常有用。拦截器必须实现org.springframework.web.servlet包的HandlerInterceptor。此接口定义了三种方法：

- preHandle：在执行实际处理程序之前调用。
- postHandle：在执行完实际程序之后调用。
- afterCompletion：在完成请求后调用。

![image-20230318101838876](assets/image-20230318101838876.png)

![image-20230318105757538](assets/image-20230318105757538.png)

![image-20230318110418226](assets/image-20230318110418226.png)

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

