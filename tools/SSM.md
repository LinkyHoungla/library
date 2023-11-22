参考文档

https://blog.csdn.net/weixin_45650003/article/details/121623824

https://www.w3schools.cn/

https://springref.com/docs/index/spring-framework

https://blog.csdn.net/u010648555/article/details/76299467

https://www.cnblogs.com/alter888/p/9083963.html

# 简介

SSM 框架是一个集成了 Spring、Spring MVC 和 MyBatis 三大框架的体系，它遵循标准的MVC（Model-View-Controller）模式。

# Spring MVC

## 层级

该框架通常分为四个层次，分别是持久层（Dao层，也称Mapper层），业务层（Service层），表现层（Controller层，也称Handler层）以及前端视图层（View）。

### 持久层

Dao层（Mapper层）是负责处理数据持久化的层，它负责与数据库进行交互，包括数据的读取、写入、更新和删除等操作。

**关键特点**

- Dao层通常首先定义接口，接着在Spring配置文件中定义接口的具体实现类。
- 在业务模块中，通过调用Dao接口来处理数据操作，而不需要关心具体的实现类。
- 数据源的配置和与数据库连接相关的参数通常也在Spring的配置文件中进行定义。

### 业务层

Service层主要负责业务逻辑的设计和处理，它是连接持久层和控制层的桥梁。

**关键特点**

- 首先设计Service接口，然后再实现接口的具体类。
- 在Spring配置文件中配置Service接口和具体实现的关联。
- Service层的实现通常需要调用Dao层提供的接口来进行数据的处理。
- 每个业务模块都有对应的Service接口，其中包含了该模块所需的业务逻辑方法。

### 表现层

Controller层（Handler层）负责控制业务流程，接收客户端请求，调用Service层提供的接口，然后返回视图给前端。

**关键特点**：

- 控制器的配置也在Spring的配置文件中进行。
- 控制器通过调用Service层提供的接口来控制业务流程。
- 不同的业务流程可以由不同的控制器进行控制。
- 在具体开发中，可以将业务流程抽象成可重用的子单元流程模块。

### 前端视图层

View层主要负责展示数据和与用户交互，通常使用JSP等技术来呈现页面。

**关键特点**：

- 前端视图层与控制层紧密结合，负责展示页面并接受用户输入。
- 在SSM框架中，前端视图层通常是JSP页面。

## 工作流程

### 详细流程

![img](D:\Repository\Typora\383f9a089b3149daaf4c07f78d3ab959.png)

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

### 简单流程

![在这里插入图片描述](D:\Repository\Typora\2aeceda1ac36468eb3ada8a0867a04b3.png)

1. 客户端发送请求到 `DispatcherServlet`，即分发器。
2. `DispatcherServlet` 控制器查询 `HandlerMapping`，找到能够处理请求的 `Controller`。
3. `Controller` 调用业务逻辑进行处理，然后返回 `ModelAndView` 对象。
4. `DispatcherServlet` 查询视图解析器，找到 `ModelAndView` 指定的视图。
5. 视图负责将 `ModelAndView` 中的结果显示到客户端。

这个流程简洁地描述了 Spring MVC 的请求处理过程，其中 `DispatcherServlet` 起到核心的分发和控制的作用，`HandlerMapping` 帮助找到合适的 `Controller`，`Controller` 处理具体的业务逻辑，`ModelAndView` 包含处理结果和视图信息，而视图负责呈现最终的结果给客户端。

## 核心组件

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

### 注解功能

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

## 拦截器

Spring的处理程序映射机制包括处理程序拦截器，当你希望将特定功能应用于某些请求时，例如，检查用户主题时，这些拦截器非常有用。拦截器必须实现org.springframework.web.servlet包的HandlerInterceptor。此接口定义了三种方法：

- preHandle：在执行实际处理程序之前调用。
- postHandle：在执行完实际程序之后调用。
- afterCompletion：在完成请求后调用。

![image-20230318101838876](D:\Repository\Typora\image-20230318101838876.png)

![image-20230318105757538](D:\Repository\Typora\image-20230318105757538.png)

![image-20230318110418226](D:\Repository\Typora\image-20230318110418226.png)

# [Spring](spring.io)

Spring Framework 是一个功能强大的Java应用程序框架，旨在提供高效且可扩展的开发环境。

- **轻量级容器和依赖注入**：Spring框架采用了轻量级容器的概念，允许开发者管理Java对象的生命周期和配置，同时提供了依赖注入（DI）功能，使开发更加灵活和可维护。开发者可以使用普通的Java对象（POJO）来进行容器配置，而不必依赖于特定的框架类。
- **面向切面编程（AOP）**：Spring框架支持面向切面编程，允许开发者将横切关注点（如日志、事务管理）从主要业务逻辑中分离出来，以提高代码的模块化和可重用性。
- **模块化架构**：Spring框架被划分为多个模块，应用程序可以根据需要选择所需的模块。核心模块包括核心容器、依赖注入机制等。此外，Spring提供了对消息传递、事务管理、对象/关系映射（ORM）、JavaBeans、JDBC、JMS等技术的支持，从而确保高效的开发。
- **Web框架**：Spring框架包括基于Servlet的Spring MVC Web框架，用于构建Web应用程序，以及响应式的Spring WebFlux框架，支持响应式编程。
- **支持移动应用开发**：Spring框架还支持各种移动应用开发技术，包括Android和iOS。
- **注解支持**：Spring框架支持依赖注入（JSR 330）和通用注解（JSR 250）规范，允许开发人员选择使用这些规范来代替Spring框架提供的专有机制，从而实现更加标准化的开发。
- **Jakarta EE 9支持**：从Spring框架6.0开始，Spring已升级到Jakarta EE 9级别，使用`jakarta`命名空间而不是传统的`javax`包。这意味着Spring准备为Jakarta EE API的未来发展提供支持，与最新的Web服务器和ORM框架兼容。

## IoC

反转控制 （IoC, Inversion of Control）也被称为依赖注入（DI），这是一个重要的概念，它改变了对象之间的依赖关系管理方式。

![img](D:\Repository\Typora\v2-005b53a7d400a5c21314ae58cbd606b7_720w.webp)

传统的方式是对象自己负责创建和管理它所依赖的其他对象，而在IoC容器中，对象不再负责自己的依赖关系，而是将这些依赖关系交给容器进行管理。

> 从以上两种开发方式的对比来看：我们 “丧失了一个权力” (创建、管理对象的权力)，从而也得到了一个好处（不用再考虑对象的创建、管理等一系列的事情）
>

它是一个过程，对象仅通过构造参数、工厂方法的参数或在对象实例被构造或从工厂方法返回后在其上设置的属性来定义其依赖关系（即它们与之合作的其他对象）。然后容器在创建 Bean 时注入这些依赖关系。这个过程从根本上说是Bean本身通过使用直接构建类或诸如服务定位模式的机制来控制其依赖关系的实例化或位置的逆过程（因此被称为控制反转）。

IoC是一种**设计思想** 或者说是某种模式，**将原本在程序中手动创建对象的控制权，交由 Spring 框架来管理。** IoC 在其他语言中也有应用，并非 Spring 特有。**IoC 容器是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个 Map（key，value）,Map 中存放的是各种对象。**

在Spring框架中，IoC容器的基础构建在`org.springframework.beans`和`org.springframework.context`包中。有两个关键接口，分别是 BeanFactory 和 ApplicationContext：

- **BeanFactory**：BeanFactory是Spring IoC容器的基础接口，提供了高级的配置机制，能够管理任何类型的对象。它负责对象的实例化和依赖关系的管理。BeanFactory是IoC容器的基本形式，提供了核心的容器功能。
- **ApplicationContext**：ApplicationContext是BeanFactory的一个子接口，它增加了更多的企业特定功能。这包括更容易与Spring的AOP功能集成、Message资源处理（用于国际化）、事件发布、以及特定于应用程序层的上下文，如WebApplicationContext，用于Web应用程序。ApplicationContext是BeanFactory的一个完整的超集，通常更适合在实际的应用程序中使用。

在Spring中，构成应用程序骨干并由Spring IoC容器管理的对象被称为Bean；Bean是由 IoC 容器实例化、组装和管理的对象，而不是由应用程序自己创建，否则，Bean只是你的应用程序中众多对象中的一个。Bean以及它们之间的依赖关系都反映在容器使用的配置元数据中。

通过IoC，Spring能够提供更松散耦合的组件，使应用程序更容易维护和扩展。

### 容器

`org.springframework.context.ApplicationContext` 接口代表Spring IoC容器，它负责实例化、配置和组装Bean。容器通过读取配置元数据来获得指示要实例化、配置和组装哪些对象的信息。

配置元数据以XML、Java注解或Java代码表示，它可以让你表达构成你的应用程序的对象以及这些对象之间丰富的相互依赖关系。

Spring提供了多个 `ApplicationContext` 接口的实现。在独立的应用程序中，通常使用 [`ClassPathXmlApplicationContext`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/context/support/ClassPathXmlApplicationContext.html) 或 [`FileSystemXmlApplicationContext`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/context/support/FileSystemXmlApplicationContext.html) 的实例。尽管XML一直是定义配置元数据的传统方式，但你也可以使用少量的XML配置来指示容器使用Java注解或代码作为元数据格式，以声明性地启用对这些额外元数据格式的支持。

在大多数应用场景中，不需要显式地编写代码来实例化Spring IoC容器的一个或多个实例。例如，在Web应用程序中，通常只需在`web.xml` 文件中编写8行（或更多）模板式的 Web 描述符就足够了。

![container magic](D:\Repository\Typora\container-magic.png)

上图展示了Spring框架工作方式的高层视图。你的应用程序类与配置元数据结合在一起，一旦 `ApplicationContext` 被创建和初始化，你就拥有了一个完全配置好的可执行系统或应用程序。

#### 差异

|        BeanFactory         |    ApplicationContext    |
| :------------------------: | :----------------------: |
|        它使用懒加载        |      它使用即时加载      |
| 它使用语法显式提供资源对象 | 它自己创建和管理资源对象 |
|        不支持国际化        |        支持国际化        |
|    不支持基于依赖的注解    |    支持基于依赖的注解    |

BeanFactory的优缺点：

- 优点：应用启动的时候占用资源很少，对资源要求较高的应用，比较有优势；
- 缺点：运行速度会相对来说慢一些。而且有可能会出现空指针异常的错误，而且通过Bean工厂创建的Bean生命周期会简单一些。

ApplicationContext的优缺点：

- 优点：所有的Bean在启动的时候都进行了加载，系统运行的速度快；在系统启动的时候，可以发现系统中的配置问题。
- 缺点：把费时的操作放到系统启动中完成，所有的对象都可以预加载，缺点就是内存占用较大。

#### 配置元数据

Spring的IoC容器需要配置元数据来实例化、配置和组装对象。这种配置元数据可以采用不同的格式，而Spring IoC容器与配置元数据的实际编写格式是解耦的。

Spring的配置包括至少一个，通常是一个以上的 Bean 定义，容器必须管理这些定义。

##### 基于XML

传统而简单的配置元数据形式，使用XML格式。使用`<bean>`元素定义每个Bean的配置。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="exampleBean" class="com.example.ExampleBean">
        <!-- 这个bean的合作者和配置在这里 -->
    </bean>

    <bean id="anotherBean" class="com.example.AnotherBean">
        <!-- 这个bean的合作者和配置在这里 -->
    </bean>
</beans>
```

- `id` 属性是一个字符串，用于识别单个Bean定义，可以用来指代协作对象。
- `class` 属性定义了 Bean 的类型，并使用类的全路径名。

在Spring中，你可以将Bean的定义跨越多个XML文件，这通常用于将应用程序的不同逻辑层或模块的配置分隔开。为了实现这一点，你可以使用 `<import/>` 元素或应用程序上下文的构造函数来加载外部Bean定义。

```xml
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```

这些文件的位置路径都是相对于包含它们的定义文件的位置的。注意，前导斜线 `/` 会被忽略，因此最好避免使用斜线。被导入的文件必须包含有效的XML Bean定义，符合 Spring 的 Schema 规范。

> 也可以使用相对的 "../" 路径来引用父目录中的文件，但这不是推荐的做法，因为它可能导致对文件之外的位置的依赖。这种引用方式不建议用于 classpath 路径，因为它依赖于 classpath 配置和可能导致选择不正确的目录。

可以使用完全限定的资源位置，如 `file:C:/config/services.xml` 或 `classpath:/config/services.xml`。但是要注意，这种做法会将配置与特定的绝对位置耦合在一起，通常最好使用占位符（如 `${...}`）并在运行时基于JVM系统属性来解析位置。

此外，Spring的命名空间提供了更多的配置功能，你可以使用它们来扩展XML配置，例如 context 和 util 命名空间。

##### 基于注解

使用注解来定义Bean的配置元数据，使用注解如`@Component`, `@Service`, `@Repository`, `@Controller`等。

```java
@Component
public class ExampleBean {
    // Configuration for exampleBean
}
```

基于注解的配置提供了XML设置的替代方案，它依靠字节码元数据来注入组件而不是XML声明。开发者通过在相关的类、方法或字段声明上使用注解，将配置移入组件类本身，而不是使用XML来描述bean的装配。

正如 [示例： `AutowiredAnnotationBeanPostProcessor`](https://springdoc.cn/spring/core.html#beans-factory-extension-bpp-examples-aabpp) 中提到的，将 `BeanPostProcessor` 与注解结合使用是扩展Spring IoC容器的常见手段。例如，[`@Autowired`](https://springdoc.cn/spring/core.html#beans-autowired-annotation) 注解提供了与 [注入协作者（Autowiring Collaborators）](https://springdoc.cn/spring/core.html#beans-factory-autowire) 中所描述的相同的功能，但控制范围更细，适用性更广。

此外，Spring还提供了对JSR-250注解的支持，如 `@PostConstruct` 和 `@PreDestroy`，以及对JSR-330（Java的依赖注入）注解的支持，该注解包含在 `jakarta.inject` 包中，如 `@Inject` 和 `@Named`。

> 注解注入是在XML注入之前进行的。因此，XML配置覆盖了通过这两种方法注入的属性的注解。

一如既往，你可以将后处理器注册为单独的Bean定义，但也可以通过在基于XML的Spring配置中包含以下标签来隐式注册（注意包含 `context` 命名空间）。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

`<context:annotation-config/>` 元素隐含地注册了下列后处理器（ post-processor）。

- [`ConfigurationClassPostProcessor`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/context/annotation/ConfigurationClassPostProcessor.html)
- [`AutowiredAnnotationBeanPostProcessor`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/beans/factory/annotation/AutowiredAnnotationBeanPostProcessor.html)
- [`CommonAnnotationBeanPostProcessor`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/context/annotation/CommonAnnotationBeanPostProcessor.html)
- [`PersistenceAnnotationBeanPostProcessor`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/orm/jpa/support/PersistenceAnnotationBeanPostProcessor.html)
- [`EventListenerMethodProcessor`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/context/event/EventListenerMethodProcessor.html)

> `<context:annotation-config/>` 只在定义它的同一应用上下文（application context）中寻找对Bean的注解。
>
> 这意味着，如果你把 `<context:annotation-config/>` 放在 `DispatcherServlet` 的 `WebApplicationContext` 中，它只检查 controller 中的 `@Autowired` Bean，而不是 service。

######  `@Autowired`

> JSR 330的 `@Inject` 注解可以代替Spring的 `@Autowired` 注解在本节包含的例子中使用。

你可以将 `@Autowired` 注解应用于构造函数，如下例所示。

```java
public class MovieRecommender {

    private final CustomerPreferenceDao customerPreferenceDao;

    @Autowired
    public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) {
        this.customerPreferenceDao = customerPreferenceDao;
    }

    // ...
}
```

> 从Spring Framework 4.3开始，如果目标Bean一开始就只定义了一个构造函数，那么在这样的构造函数上就不再需要 `@Autowired` 注解。然而，如果有几个构造函数，而且没有主要/默认构造函数，那么至少有一个构造函数必须用 `@Autowired` 注解，以便指示容器使用哪一个。

你也可以将 `@Autowired` 注解应用于传统的setter方法，如下例所示。

```java
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Autowired
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // ...
}
```

你也可以将注解应用于具有任意名称和多个参数的方法，如下例所示。

```java
public class MovieRecommender {

    private MovieCatalog movieCatalog;

    private CustomerPreferenceDao customerPreferenceDao;

    @Autowired
    public void prepare(MovieCatalog movieCatalog,
            CustomerPreferenceDao customerPreferenceDao) {
        this.movieCatalog = movieCatalog;
        this.customerPreferenceDao = customerPreferenceDao;
    }

    // ...
}
```

你也可以将 `@Autowired` 应用于字段，甚至将其与构造函数混合，如下例所示。

```java
public class MovieRecommender {

    private final CustomerPreferenceDao customerPreferenceDao;

    @Autowired
    private MovieCatalog movieCatalog;

    @Autowired
    public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) {
        this.customerPreferenceDao = customerPreferenceDao;
    }

    // ...
}
```

> 确保你的目标组件（例如 `MovieCatalog` 或 `CustomerPreferenceDao`）由你用于 `@Autowired` 注解的注入点的类型统一声明。否则，注入可能会在运行时由于 "no type match found" 的错误而失败。
>
> 对于通过classpath扫描找到的XML定义的Bean或组件类，容器通常预先知道具体类型。然而，对于 `@Bean` 工厂方法，你需要确保声明的返回类型有足够的表现力。
>
> 对于实现了多个接口的组件或可能被其实现类型引用的组件，考虑在你的工厂方法上声明最具体的返回类型（至少要与引用你的 bean 的注入点所要求的具体类型一样）。

默认情况下，当一个给定的注入点没有匹配的候选Bean可用时，自动注入就会失败。在声明的数组、collection或map的情况下，预计至少有一个匹配的元素。

默认行为是将注解的方法和字段视为表示必须的依赖关系。你可以改变这种行为，就像下面的例子所展示的那样，通过将其标记为非必需（即通过将 `@Autowired` 中的 `required` 属性设置为 `false`），使框架能够跳过一个不可满足的注入点。

```java
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Autowired(required = false)
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // ...
}
```

> 如果一个非必须（required）的方法（或者在有多个参数的情况下，它的一个依赖关系）不可用，那么它将根本不会被调用。在这种情况下，一个非必须（required）字段将根本不会被填充，而是将其默认值留在原地。
>
> 换句话说，将 `required` 属性设置为 `false` 表示相应的属性对于自动注入来说是可选的，如果该属性不能被自动注入，它将被忽略。这使得属性可以被分配默认值，这些默认值可以通过依赖注入选择性地被重写。

注入的构造函数和工厂方法参数是一种特殊情况，因为由于Spring的构造函数解析算法有可能处理多个构造函数，所以 `@Autowired` 中的 `required` 属性有一些不同的含义。

构造函数和工厂方法参数实际上是默认需要的，但在单构造函数的情况下有一些特殊的规则，比如多元素注入点（数组、collection、map）如果没有匹配的Bean，则解析为空实例。

这允许一种常见的实现模式，即所有的依赖关系都可以在一个独特的多参数构造函数中声明—例如，声明为一个没有 `@Autowired` 注解的单一公共构造函数。

> 任何给定的Bean类中只有一个构造函数可以声明 `@Autowired`，并将 `required` 属性设置为 `true`，表示该构造函数在用作Spring Bean时要自动注入。
>
> 因此，如果 `required` 属性的默认值为 `true`，则只有一个构造函数可以使用 `@Autowired` 注解。如果有多个构造函数声明该注解，它们都必须声明 `required=false`，才能被视为自动注入的候选者（类似于XML中的 `autowire=constructor`）。
>
> 具有最大数量的依赖关系的构造函数将被选中，这些依赖关系可以通过Spring容器中的匹配Bean来满足。如果没有一个候选者可以被满足，那么将使用一个主要的/默认的构造函数（如果存在的话）。
>
> 同样地，如果一个类声明了多个构造函数，但没有一个是用 `@Autowired` 注解的，那么将使用一个主要/默认构造函数（如果存在的话）。如果一个类一开始只声明了一个构造函数，那么即使没有注解，它也会被使用。请注意，被注解的构造函数不一定是公共的（public）。

##### 基于Java类

使用Java类来定义Bean的配置元数据，通常需要一个Java配置类，使用`@Configuration`注解，使用`@Bean`注解定义Bean。

```java
@Configuration
public class AppConfig {
    @Bean
    public ExampleBean exampleBean() {
        // Configuration for exampleBean
        return new ExampleBean();
    }
}
```

这些配置元数据定义了Spring IoC容器需要管理的Bean，通常会定义服务层对象、持久层对象（如存储库或数据访问对象（DAO））、表现对象（如Web控制器）、基础设施对象（如JPA `EntityManagerFactory`）、JMS队列等等。不同层次的对象可能需要不同的配置。

> 通常，不会在容器中配置细粒度的domain 对象，因为创建和加载 domain 对象通常是 repository 和业务逻辑的责任。

基于XML的配置通常用于传统Spring应用程序，而基于注解和基于Java的配置更适合现代的Spring应用程序，因为它们更具可读性和类型安全性。

#### 实例化

在Spring中，通过`ApplicationContext`的构造函数，可以将一个或多个资源路径作为参数传递给容器，这些路径可以是资源字符串，让容器能够从不同的外部资源加载配置信息。

这些资源路径可以是本地文件系统上的文件，也可以是Java类路径（CLASSPATH）中的文件。

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```

这个例子中，`services.xml` 和 `daos.xml` 是资源路径，Spring容器将从这些路径加载XML文件以构建应用程序上下文。

下面的例子显示了 service 对象（`services.xml`）配置文件。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- services -->

    <bean id="petStore" class="org.springframework.samples.jpetstore.services.PetStoreServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <property name="itemDao" ref="itemDao"/>
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for services go here -->

</beans>
```

下面的例子显示了数据访问对象（data access object） `daos.xml` 文件。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountDao"
        class="org.springframework.samples.jpetstore.dao.jpa.JpaAccountDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <bean id="itemDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaItemDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for data access objects go here -->

</beans>
```

在前面的例子中，服务层由 `PetStoreServiceImpl` 类和两个类型为 `JpaAccountDao` 和 `JpaItemDao` 的数据访问对象组成（基于JPA对象-关系映射标准）。

`property name` 元素指的是 JavaBean 属性的名称，而 `ref` 元素指的是另一个Bean定义的名称。`id` 和 `ref` 元素之间的这种联系表达了协作对象之间的依赖关系。

#### 容器使用

`ApplicationContext` 接口是Spring的高级工厂，它维护了一个Bean的注册表，使你能够访问和管理不同Bean及它们之间的依赖关系。通过使用`getBean(String name, Class<T> requiredType)`方法，你可以检索已配置Bean的实例。

```java
// 创建和配置Bean
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// 检索配置的实例
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// 使用配置的实例
List<String> userList = service.getUsernameList();
```

`ApplicationContext`接口还提供了其他方法来检索Bean，但通常情况下，你的应用代码不应该直接调用这些方法。

事实上，最好的实践是尽量避免在应用程序代码中调用`getBean()`方法，从而不依赖于Spring的API。

相反，Spring提供了依赖注入机制，允许你通过元数据（如 autowiring 注解）声明Bean之间的依赖关系，从而将依赖关系的管理交给Spring容器。例如，在与Web框架的集成中，Spring提供了依赖注入的支持，允许你通过声明依赖关系，而不是通过手动检索Bean，来管理控制器和其他组件之间的依赖。

#### 拓展点

通常情况下，应用程序开发人员不需要对 `ApplicationContext` 实现类进行子类化。相反，Spring IoC容器可以通过插入特殊集成接口的实现来进行扩展。接下来的几节将描述这些集成接口。

##### 自定义 Bean

`BeanPostProcessor` 接口定义了回调方法，你可以实现这些方法来提供你自己的（或覆盖容器的默认）实例化逻辑、依赖性解析逻辑等。如果你想在Spring容器完成实例化、配置和初始化Bean之后实现一些自定义逻辑，你可以插入一个或多个自定义 `BeanPostProcessor` 实现。

你可以配置多个 `BeanPostProcessor` 实例，你可以通过设置 `order` 属性控制这些 `BeanPostProcessor` 实例的运行顺序。只有当 `BeanPostProcessor` 实现了 `Ordered` 接口时，你才能设置这个属性。如果你编写自己的 `BeanPostProcessor`，你也应该考虑实现 `Ordered` 接口。

> `BeanPostProcessor` 实例对Bean（或对象）实例进行操作。也就是说，Spring IoC容器实例化一个Bean实例，然后由 `BeanPostProcessor` 实例来完成其工作。
>
> `BeanPostProcessor` 实例是按容器范围的。这只有在你使用容器层次结构时才有意义。如果你在一个容器中定义了一个 `BeanPostProcessor`，它只对该容器中的Bean进行后处理。
>
> 换句话说，在一个容器中定义的 `BeanPostProcessor` 不会对另一个容器中定义的 `BeanPostProcessor` 进行后处理，即使两个容器都是同一层次结构的一部分。
>
> 要改变实际的Bean定义（即定义Bean的蓝图），你需要使用 `BeanFactoryPostProcessor`，如 [用 `BeanFactoryPostProcessor` 定制配置元数据](https://springdoc.cn/spring/core.html#beans-factory-extension-factory-postprocessors) 中所述。

`org.springframework.beans.factory.config.BeanPostProcessor` 接口正好由两个回调方法组成。当这样的类被注册为容器的后处理器时，对于容器创建的每个 bean 实例，后处理器在容器初始化方法（如 `InitializingBean.afterPropertiesSet()` 或任何已声明的 `init` 方法）被调用之前和任何 bean 初始化回调之后都会从容器获得一个回调。后处理程序可以对Bean实例采取任何行动，包括完全忽略回调。bean类后处理器通常会检查回调接口，或者用代理来包装bean类。一些Spring AOP基础设施类被实现为Bean后处理器，以提供代理封装逻辑。

`ApplicationContext` 会自动检测在配置元数据中定义的实现 `BeanPostProcessor` 接口的任何Bean。`ApplicationContext` 将这些 bean 注册为后处理器，以便以后在 bean 创建时可以调用它们。Bean后处理器可以像其他Bean一样被部署在容器中。

请注意，通过在配置类上使用 `@Bean` 工厂方法来声明 `BeanPostProcessor` 时，工厂方法的返回类型应该是实现类本身，或者至少是 `org.springframework.beans.factory.config.BeanPostProcessor` 接口，明确表示该 Bean 的后处理性质。否则，`ApplicationContext` 无法在完全创建它之前按类型自动检测它。由于 `BeanPostProcessor` 需要尽早被实例化，以便应用于上下文中其他Bean的初始化，所以这种早期的类型检测是至关重要的。

> 虽然推荐的 `BeanPostProcessor` 注册方法是通过 `ApplicationContext` 自动检测（如前所述），但你可以通过使用 `addBeanPostProcessor` 方法，针对 `ConfigurableBeanFactory` 以编程方式注册它们。当你需要在注册前评估条件逻辑或甚至在一个层次结构中跨上下文复制Bean Post处理器时，这可能很有用。
>
> 然而，请注意，以编程方式添加的 `BeanPostProcessor` 实例并不尊重 `Ordered` 接口。这里，是注册的顺序决定了执行的顺序。还要注意的是，以编程方式注册的 `BeanPostProcessor` 实例总是在通过自动检测注册的实例之前被处理，而不考虑任何明确的顺序。

> 实现了 `BeanPostProcessor` 接口的类是特殊的，会被容器区别对待。所有 `BeanPostProcessor` 实例和它们直接引用的Bean在启动时被实例化，作为 `ApplicationContext` 特殊启动阶段的一部分。接下来，所有的 `BeanPostProcessor` 实例被分类注册，并应用于容器中的所有其他Bean。
>
> 因为AOP的自动代理是作为 `BeanPostProcessor` 本身实现的，所以 `BeanPostProcessor` 实例和它们直接引用的Bean都不符合自动代理的条件，因此，没有切面被织入进去。
>
> 对于任何这样的Bean，你应该看到一个信息性的日志消息：`Bean someBean is not eligible for getting processed by all BeanPostProcessor interfaces (for example: not eligible for auto-proxying)`。
>
> 如果你通过使用自动注入或 `@Resource`（可能会退回到自动注入）将 `BeanPostProcessor` 连接起来，Spring在搜索类型匹配的依赖候选者时可能会访问意想不到的Bean，因此，使它们没有资格进行自动代理或其他种类的Bean后处理。例如，如果你有一个用 `@Resource` 注解的依赖，其中字段或setter的名称不直接对应于Bean的声明名称，并且没有使用name属性，Spring会访问其他Bean以通过类型匹配它们。

##### 配置元数据

我们看的下一个扩展点是 `org.springframework.beans.factory.config.BeanFactoryPostProcessor`。这个接口的语义与 `BeanPostProcessor` 的语义相似，但有一个主要区别。`BeanFactoryPostProcessor` 对Bean配置元数据进行操作。也就是说，Spring IoC容器让 `BeanFactoryPostProcessor` 读取配置元数据，并在容器实例化 `BeanFactoryPostProcessor` 实例以外的任何Bean之前对其进行潜在的修改。

你可以配置多个 `BeanFactoryPostProcessor` 实例，你可以通过设置 `order` 属性控制这些 `BeanFactoryPostProcessor` 实例的运行顺序。然而，只有当 `BeanFactoryPostProcessor` 实现了 `Ordered` 接口时，你才能设置这个属性。如果你编写自己的 `BeanFactoryPostProcessor`，你也应该考虑实现 `Ordered` 接口。

> 如果你想改变实际的Bean实例（即从配置元数据中创建的对象），那么你需要使用 `BeanPostProcessor`（如前面 [使用 `BeanPostProcessor` 自定义 Bean](https://springdoc.cn/spring/core.html#beans-factory-extension-bpp) 中的描述）。虽然在技术上可以在 `BeanFactoryPostProcessor` 中处理Bean实例（例如，通过使用 `BeanFactory.getBean()`），但这样做会导致过早的Bean实例化，违反了标准容器的生命周期。这可能会导致负面的副作用，比如绕过Bean的后期处理。
>
> 另外，`BeanFactoryPostProcessor` 实例是按容器范围的。这只有在你使用容器层次结构时才有意义。如果你在一个容器中定义了一个 `BeanFactoryPostProcessor`，它将只应用于该容器中的Bean定义。一个容器中的 Bean 定义不会被另一个容器中的 `BeanFactoryPostProcessor` 实例进行后处理，即使两个容器都是同一层次结构的一部分。

当Bean工厂在 `ApplicationContext` 内声明时，会自动运行Bean工厂后处理器，以便对定义容器的配置元数据进行修改。Spring包括一些预定义的Bean Factory后处理器，如 `PropertyOverrideConfigurer` 和 `PropertySourcesPlaceholderConfigurer`。你也可以使用一个自定义的 `BeanFactoryPostProcessor`--例如，注册自定义的属性编辑器（property editor）。

`ApplicationContext` 会自动检测被部署到其中的实现了 `BeanFactoryPostProcessor` 接口的任何Bean。它在适当的时候将这些Bean用作Bean Factory后处理器。你可以像部署其他Bean一样部署这些后处理器Bean。

> 与 `BeanPostProcessor` 一样，你通常不希望将 `BeanFactoryPostProcessor` 配置为懒加载。如果没有其他bean引用 `Bean(Factory)PostProcessor`，该 `PostProcessor` 将根本不会被实例化。因此，将其标记为懒加载将被忽略，即使你在 `<beans />` 元素的声明中把 `default-lazy-init` 属性设置为 `true`，`Bean(Factory)PostProcessor` 也会被急切地实例化。

#### 路径扫描

本节描述了一个通过扫描classpath来隐式检测候选组件的选项。候选组件是与过滤器标准相匹配的类，并且有一个在容器中注册的相应的bean定义。

这样就不需要使用 XML 来执行 bean 注册了。相反，你可以使用注解（例如 `@Component`）、AspectJ类型表达式或你自己的自定义过滤标准来选择哪些类在容器中注册了Bean定义。

##### Stereotype 注解

`@Repository` 注解是任何满足 repository（也被称为数据访问对象或 DAO）角色或 stereotype 的类的标记。这个标记的用途包括异常的自动翻译，如 [异常翻译](https://springdoc.cn/spring/data-access.html#orm-exception-translation) 中所述。

Spring提供了更多的 stereotype 注解。`@Component`, `@Service`, 和 `@Controller`。 `@Component` 是一个通用的stereotype，适用于任何Spring管理的组件。

`@Repository`、 `@Service` 和 `@Controller` 是 `@Component` 的特殊化，用于更具体的使用情况（分别在持久层、服务层和表现层）。

因此，你可以用 `@Component` 来注解你的组件类，但是，通过用 `@Repository`、`@Service` 或 `@Controller` 来注解它们，你的类更适合于被工具处理或与切面关联。例如，这些stereotype注解是指向性的理想目标。

在Spring框架的未来版本中，`@Repository`、`@Service` 和 `@Controller` 还可以携带额外的语义。因此，如果你要在服务层使用 `@Component` 或 `@Service` 之间进行选择，`@Service` 显然是更好的选择。同样地，如前所述，`@Repository` 已经被支持作为持久层中自动异常翻译的标记。

##### 原注解

Spring提供的许多注解都可以在你自己的代码中作为元注解使用。元注解是一个可以应用于另一个注解的注解。例如，[前面](https://springdoc.cn/spring/core.html#beans-stereotype-annotations) 提到的 `@Service` 注解是用 `@Component` 进行元注解的，如下例所示。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component 
public @interface Service {

    // ...
}
```

你也可以结合元注解来创建 “composed annotations”（组合注解）。例如，Spring MVC的 `@RestController` 注解是由 `@Controller` 和 `@ResponseBody` 组成。

此外，组合注解可以选择性地重新声明来自元注解的属性以允许定制。当你想只暴露元注解的一个子集的属性时，这可能特别有用。

例如，Spring的 `@SessionScope` 注解将 scope 名称硬编码为 `session`，但仍然允许自定义 `proxyMode`。下面的列表显示了 `SessionScope` 注解的定义。

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Scope(WebApplicationContext.SCOPE_SESSION)
public @interface SessionScope {

    /**
     * Alias for {@link Scope#proxyMode}.
     * <p>Defaults to {@link ScopedProxyMode#TARGET_CLASS}.
     */
    @AliasFor(annotation = Scope.class)
    ScopedProxyMode proxyMode() default ScopedProxyMode.TARGET_CLASS;

}
```

##### 自动检测

Spring可以自动检测 stereotype 的类，并在 `ApplicationContext` 中注册相应的 `BeanDefinition` 实例。例如，以下两个类符合这种自动检测的条件。

```java
@Service
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
}
```

```java
@Repository
public class JpaMovieFinder implements MovieFinder {
    // implementation elided for clarity
}
```

为了自动检测这些类并注册相应的Bean，你需要在你的 `@Configuration` 类中添加 `@ComponentScan`，其中 `basePackages` 属性是这两个类的共同父包。(或者，你可以指定一个用逗号或分号或空格分隔的列表，其中包括每个类的父包。)

```java
@Configuration
@ComponentScan(basePackages = "org.example")
public class AppConfig  {
    // ...
}
```

> 为了简洁起见，前面的例子可以使用注解的 `value` 属性（即 `@ComponentScan("org.example")`）。

以下是使用XML的替代方案。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="org.example"/>

</beans>
```

> 使用 `<context:component-scan>` 就隐含地实现了 `<context:annotation-config>` 的功能。在使用 `<context:component-scan>` 时，通常不需要包括 `<context:annotation-config>` 元素。

此外，当你使用组件扫描元素时，`AutowiredAnnotationBeanPostProcessor` 和 `CommonAnnotationBeanPostProcessor` 都被隐含地包括在内。这意味着这两个组件被自动检测并连接在一起—所有这些都不需要在XML中提供任何bean配置元数据。



### Bean

在Spring的IoC容器中，Bean的定义通常表示为`BeanDefinition`对象，它包含了一些重要的元数据，用于配置Bean的创建和管理。

- **全路径类名**：通常是被定义的Bean的实际实现类。
- **Bean的行为配置元素**：这些元素说明了Bean在容器中的行为方式，包括其作用域（scope）、生命周期回调等等。
- **对其他Bean的引用**：这些是Bean需要与之协作的其他Bean，也被称为合作者或依赖。
- **其他配置设置**：这些是要应用到新创建的对象中的其他配置设置，例如池大小限制或在连接池管理Bean中使用的连接数。

**常见属性**

- **Class（类）**：通常是指实际实现Bean的类的全路径名。
- **Name（名称）**：用于标识Bean的名称，以便在容器中唯一识别。
- **Scope（作用范围）**：定义了Bean的作用范围，如单例、原型等。
- **Constructor arguments（构造函数参数）**：描述了构造Bean时需要传递的依赖关系。
- **Properties（属性）**：包含了在Bean创建后设置的依赖关系。
- **Autowiring mode（自动装配方式）**：指定了如何自动装配合作者或依赖。
- **Lazy initialization mode（懒加载模式）**：指示Bean是否应该延迟初始化。
- **Initialization method（初始化方法）**：定义了Bean初始化后需要调用的方法。
- **Destruction method（销毁方法）**：定义了Bean销毁前需要调用的方法。

Spring的`ApplicationContext`实现还支持将在容器外部（由用户）创建的现有对象注册到容器中。

可以通过访问`ApplicationContext`的`BeanFactory`，并使用`registerSingleton()`和`registerBeanDefinition()`方法来实现。但通常情况下，应用程序只会与通过Bean定义元数据定义的Bean一起工作。

> Bean的元数据和手动注册的单例实例需要在容器初始化过程中尽早注册，以确保容器能够正确识别和管理它们；通常最佳做法是在容器初始化前定义所有需要的Bean。
>
> 虽然在某种程度上支持覆盖现有的元数据和现有的单体实例，但 **官方不支持在运行时注册新的Bean**（与对工厂的实时访问同时进行），这可能会导致并发访问异常、Bean容器中的不一致状态，或者两者都有。

这是因为Spring容器需要在启动时对Bean的依赖关系和配置进行全面分析和准备；如果在运行时注册新Bean，容器无法进行有效的管理和预处理。

#### 命名

每个Bean在Spring容器中都有一个或多个标识符（identifier），这些标识符必须在容器中保持唯一。通常，一个Bean只有一个标识符，但如果需要，可以为Bean指定多个标识符，这些额外的标识符都可以视为同一个 Bean 的等效别名（aliases）。

> 特别是当你希望不同的组件引用一个共同的依赖关系时，可以为每个组件分别指定一个特定于该组件的名称。

在使用基于XML的配置元数据时，你可以使用`id`属性、`name`属性或两者结合来为Bean指定标识符。

+ `id`属性允许你精确指定一个标识符。通常情况下，这些标识符是字母数字组合，例如 `'myBean'`、`'someService'`，但它们也可以包含特殊字符。
+ 如果你想为Bean引入其他别名，你可以在`name`属性中指定它们，使用逗号（`,`）、分号（`;`）或空格分隔。
+ `id`属性被定义为`xsd:string`类型，但Bean的唯一性由容器强制执行，而不是由XML解析器执行。

如果你不明确提供`name`或`id`，那么容器会为该Bean生成一个唯一的名称。但是，如果你希望通过`ref`元素或服务定位器风格的查找引用该Bean，那么你必须提供一个名称。不提供名称通常是用于内部Bean和注入协作者的情况。

> 为了提高配置的可读性，Bean的命名遵循标准的Java命名规则，即小写字母开头，然后采用驼峰命名法，例如`accountManager`、`accountService`、`userDao`、`loginController`等。这种命名约定有助于让你的配置更易于理解和维护。
>
> 如果你使用Spring AOP，在将advice应用于一组相关Bean时，统一的命名规则也非常有帮助。

在classpath中进行组件扫描（component scanning）时，Spring会为未命名的组件生成Bean名称，遵循类似的命名规则。

通常，它会采用类的简单名称并将首字母小写。然而，有一个不寻常的特殊情况，即如果类名的前两个字符都是大写，那么保留原有的大小写。这些命名规则与Java的`java.beans.Introspector.decapitalize`规则相同，Spring也使用这些规则。

有时在Bean定义的地方为Bean引入别名并不够；这种情况通常出现在大型系统中，其中配置被分散到多个子系统，每个子系统都有自己的对象定义集。

在使用基于XML的配置元数据时，你可以使用`<alias/>`元素来实现这一点。

```xml
<alias name="fromName" alias="toName"/>
```

在这种情况下，一个名为 `fromName` 的Bean（在同一个容器中）经过这个别名定义后，也可以被称为 `toName`。

例如，子系统A的配置元数据可以引用一个名为 `subsystemA-dataSource` 的数据源。子系统B的配置元数据可以引用一个名为 `subsystemB-dataSource` 的数据源。当组成使用这两个子系统的主应用程序时，主应用程序以 `myApp-dataSource` 的名称来引用数据源。为了确保这三个名称都指代同一个对象，你可以在配置元数据中添加以下别名定义。

```xml
<alias name="myApp-dataSource" alias="subsystemA-dataSource"/>
<alias name="myApp-dataSource" alias="subsystemB-dataSource"/>
```

现在，每个组件和主应用程序都可以通过一个唯一的名称来引用dataSource，而不会与其他定义发生冲突，实际上创建了一个有效的命名空间，但它们引用的是同一个Bean。这种方式允许不同的子系统在各自的配置中保持独立性，但又能够共享某些Bean的引用。

#### 实例化

Bean 定义（definition）本质上是创建一个或多个对象的“配方”。容器在被要求时查看命名的Bean的“配方”，并使用该Bean定义所封装的配置元数据来创建（或获取）一个实际的对象。

如果你使用基于XML的配置元数据，你需要在`<bean/>`元素的`class`属性中指定要实例化的对象的类型（或类）。通常情况下，`class`属性是必需的（有一些例外情况，实例工厂方法进行实例化和 Bean 定义的继承）。

- 通常情况下，当容器通过反射调用构造函数直接创建Bean时，你可以指定要构造的Bean类，类似于Java代码中的`new`操作符。	

```xml
<bean id="myBean" class="com.example.MyClass"/>
```

- 在不太常见的情况下，即容器在一个类上调用 `static` 工厂方法来创建 bean 时，要指定包含被调用的 `static` 工厂方法的实际类。从 `static` 工厂方法的调用中返回的对象类型可能是同一个类或完全是另一个类。

```xml
<bean id="myBean" class="com.example.MyFactoryClass" factory-method="createInstance"/>
```

**嵌套类名**

如果你想为一个嵌套类配置Bean定义，你可以使用嵌套类的二进制名称或源名称。

例如，如果你有一个名为`SomeThing`的类位于`com.example`包中，而`SomeThing`类有一个名为`OtherThing`的静态嵌套类，你可以使用美元符号`$`或点`.`来分隔它们的名称。

因此，在Bean定义的`class`属性中的值可以是`com.example.SomeThing$OtherThing`或`com.example.SomeThing.OtherThing`。这允许你配置嵌套类的Bean定义，无论是二进制名称还是源名称。

##### 构造函数实例化

当你用构造函数的方法创建一个Bean时，所有普通的类都可以被Spring使用并与之兼容。也就是说，被开发的类不需要实现任何特定的接口，也不需要以特定的方式进行编码。只需指定Bean类就足够了。然而，根据你对该特定Bean使用的IoC类型，你可能需要一个默认（空）构造函数。

> 如果一个普通类拥有一个无参数的构造函数，那么它可以被直接创建为一个Spring Bean，而无需为该类添加特定的接口、注解或其他特殊的配置去修饰其构建为 Bean 的过程。

Spring IoC容器几乎可以管理任何你希望它管理的类。它并不局限于管理真正的JavaBean。大多数Spring用户更喜欢真正的JavaBean，它只有一个默认的（无参数）构造函数，以及按照容器中的属性建模的适当的setter和getter。你也可以在你的容器中拥有更多奇特的非bean风格的类。例如，如果你需要使用一个绝对不遵守JavaBean规范的传统连接池，Spring也可以管理它。

Spring提供了强大的依赖注入机制，允许你向构造函数提供参数（如果需要的话），并在对象被构造后设置对象实例的属性。

##### 静态工厂实例化

在定义一个用静态工厂方法创建的Bean时，使用 `class` 属性来指定包含 `static` 工厂方法的类，并使用名为 `factory-method` 的属性来指定工厂方法本身的名称。你应该能够调用这个方法（有可选的参数，如后文所述）并返回一个活的对象，随后该对象被视为通过构造函数创建的。这种Bean定义的一个用途是在遗留代码中调用 `static` 工厂。

下面的Bean定义规定，Bean将通过调用工厂方法来创建。该定义并没有指定返回对象的类型（class），而是指定了包含工厂方法的类。在这个例子中，`createInstance()` 方法必须是一个 `static` 方法。下面的例子显示了如何指定一个工厂方法。

```xml
<bean id="clientService"
    class="examples.ClientService"
    factory-method="createInstance"/>
```

下面的例子显示了一个可以与前面的Bean定义（definition）一起工作的类。

```java
public class ClientService {
    private static ClientService clientService = new ClientService();
    private ClientService() {}

    public static ClientService createInstance() {
        return clientService;
    }
}
```

##### 实例工厂实例化

实例工厂方法进行的实例化从容器中调用现有 bean 的非静态方法来创建一个新的 bean。

要使用这种机制，请将 `class` 属性留空，并在 `factory-bean` 属性中指定当前（或父代或祖代）容器中的一个 Bean 的名称，该容器包含要被调用来创建对象的实例方法。用 `factory-method` 属性设置工厂方法本身的名称。下面的例子显示了如何配置这样一个Bean。

```xml
<!-- the factory bean, which contains a method called createInstance() -->
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<!-- the bean to be created via the factory bean -->
<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>
```

```java
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }
}
```

一个工厂类也可以容纳一个以上的工厂方法。

> 在Spring文档中，“factory bean” 是指在Spring容器中配置的Bean，它通过 [实例](https://springdoc.cn/spring/core.html#beans-factory-class-instance-factory-method) 或 [静态](https://springdoc.cn/spring/core.html#beans-factory-class-static-factory-method)工厂方法创建对象。相比之下，`FactoryBean`（注意大写字母）是指Spring特定的[`FactoryBean`](https://springdoc.cn/spring/core.html#beans-factory-extension-factorybean) 实现类。

#### 运行类型

在Spring中，一个Bean的类型（或者说其实际的运行时类型）可能会比看起来复杂，因为它可以受到多种因素的影响。

1. 在Bean元数据定义中指定的类只是一个初始的类引用，可能与已声明的工厂方法相结合，或者是一个 `FactoryBean` 类，或者在实例级工厂方法的情况下根本没有被设置（而是通过指定的 `factory-bean` 名称来解决）。
   - 这意味着Bean的类型可能由工厂方法来确定，而不是在Bean定义中指定的类型
2. AOP代理的影响：如果在应用中使用了AOP（面向切面编程），那么可能会对Bean实例应用代理，这个代理可能是基于接口的代理，这就导致Bean的类型的暴露受到限制，只能看到接口部分。

要了解某个特定Bean的实际运行时类型，推荐的方法是对指定的Bean名称进行 `BeanFactory.getType` 调用。这将考虑到上述所有情况，并返回 `BeanFactory.getBean` 调用将为同一Bean名称返回的对象类型。

#### 工作流程

Bean的生命周期是由容器来管理的。主要在创建和销毁两个时期。

![img](D:\Repository\Typora\1583675090641_51.png)

##### 创建过程

1. **实例化Bean对象并设置Bean属性**：首先，Spring容器会根据配置文件或注解实例化Bean对象，然后为该Bean设置属性值。
2. **处理依赖关系**：如果Bean实现了Aware接口（如BeanFactoryAware、ApplicationContextAware），容器会通过相应的接口方法注入Bean对容器基础设施层面的依赖。这允许Bean感知容器的状态和获取容器相关信息。
3. **前置初始化方法 - postProcessBeforeInitialization**：在完成Bean实例化后，但在初始化之前，Spring容器调用BeanPostProcess的postProcessBeforeInitialization方法，这允许开发者对Bean的初始化前执行自定义逻辑，类似于AOP。
4. **BeanFactoryPostProcessor - afterPropertiesSet方法**：如果Bean实现了BeanFactoryPostProcessor接口，容器将调用它的afterPropertiesSet方法，可以在这里进行一些属性设置后的自定义操作。
5. **Bean自身的初始化方法**：容器会调用Bean定义中配置的初始化方法（通常是init-method属性指定的方法），在这个方法中可以执行Bean的初始化工作。
6. **后置初始化方法 - postProcessAfterInitialization**：类似于前置初始化，Spring容器在初始化Bean之后会调用BeanPostProcess的postProcessAfterInitialization方法，允许在Bean初始化后执行自定义逻辑，也类似于AOP。
7. **Bean初始化完成**：Bean经过上述步骤完成初始化，可以在应用中使用它了。

##### 销毁过程

1. **实现DisposableBean接口**：如果Bean实现了DisposableBean接口，Spring容器在销毁Bean时会调用其destroy方法，以执行自定义的销毁逻辑。
2. **配置销毁方法**：在Bean的定义中，可以通过配置destroy-method属性指定一个自定义的销毁方法，该方法会在销毁Bean时被调用。

#### 生命周期

当你创建一个 Bean 定义时，实际上创建了一个“配方”（recipe），用于创建 Bean 的实际实例。这个 Bean 定义包含了创建 Bean 所需的信息，包括 Bean 的类、属性、依赖关系等。Bean 定义的概念类似于类的定义，可以使用一个 Bean 定义来创建多个 Bean 实例。

通过 Bean 定义不仅可以控制各种依赖和配置值，将其插入到从特定Bean定义创建的对象中，还可以可以控制 Bean 的作用域（Scope），Bean 的作用域定义了 Bean 实例的生命周期，即它们在容器中的存在方式。

这种方法是强大而灵活的，因为你可以通过配置来选择你所创建的对象的scope，而不是在Java类级别上烘托出一个对象的scope。

Bean可以被定义为部署在若干scope中的一个。Spring框架支持六个scope，其中四个只有在你使用Web感知（aware）的 `ApplicationContext` 时才可用。你也可以创建 [一个自定义 scope](https://springdoc.cn/spring/core.html#beans-factory-scopes-custom)。

- **Singleton**（默认）：单列，每个 Spring IoC 容器只有一个 Bean 实例。
- **Prototype**：原型，每次请求时都会创建一个新的 Bean 实例。
- **Request**：每个 HTTP 请求有自己的 Bean 实例，只在 Web 感知的 Spring ApplicationContext 中有效。
- **Session**：每个 HTTP Session 有自己的 Bean 实例，只在 Web 感知的 Spring ApplicationContext 中有效。
- **Application**：整个 ServletContext 有一个 Bean 实例，只在 Web 感知的 Spring ApplicationContext 中有效。
- **WebSocket**：WebSocket 生命周期内有一个 Bean 实例，仅在具有 Web 感知的 Spring ApplicationContext 的上下文中有效。
- Thread（自定义）：可以注册的自定义线程范围，需要通过注册来启用。可用于特定线程范围的 Bean 实例。

> `request`、`session`、`application` 和 `websocket` scope只有在你使用Web感知的Spring `ApplicationContext` 实现（如 `XmlWebApplicationContext`）时才可用。
>
> 如果你将这些scope与常规的Spring IoC容器（如 `ClassPathXmlApplicationContext`）一起使用，就会抛出一个 `IllegalStateException`，抱怨有未知的Bean scope。

##### `Singleton`

只有一个单例 Bean 的共享实例被管理，所有对具有符合该Bean定义的ID的Bean的请求都会被Spring容器返回该特定的Bean实例。

换句话说，当你定义了一个Bean定义（define），并且它被定义为 singleton，Spring IoC容器就会为该Bean定义的对象创建一个确切的实例。这个单一的实例被存储在这种单体Bean的缓存中，所有后续的请求和对该命名Bean的引用都会返回缓存的对象。

![singleton](D:\Repository\Typora\singleton.png)

Spring 的 singleton Bean概念与Gang of Four（GoF）模式书中定义的singleton模式不同。

GoF singleton模式对对象的范围进行了硬编码，即每个ClassLoader创建一个且仅有一个特定类的实例。

Spring单例的范围最好被描述为每个容器和每个bean。

这意味着，如果你在一个Spring容器中为一个特定的类定义了一个Bean，Spring容器就会为该Bean定义的类创建一个且只有一个实例。Singleton scope 是Spring的默认 scope。

要在XML中把一个Bean定义为singleton，你可以像下面的例子那样定义一个Bean。

```xml
<bean id="accountService" class="com.something.DefaultAccountService"/>

<!-- the following is equivalent, though redundant (singleton scope is the default) -->
<bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
```

##### `Prototype`

Bean 部署的非 singleton prototype scope 导致每次对该特定Bean的请求都会创建一个新的Bean实例。也就是说，该 bean 被注入到另一个 bean 中，或者你通过容器上的 `getBean()` 方法调用来请求它。

作为一项规则，你应该对所有有状态的 bean 使用 prototype scope，对无状态的 bean 使用 singleton scope。

![prototype](D:\Repository\Typora\prototype.png)

（数据访问对象(DAO)通常不被配置为 prototype，因为典型的DAO并不持有任何对话状态。对我们来说，重用 singleton 图的核心是比较容易的）。

下面的例子在XML中定义了一个 prototype bean。

```xml
<bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
```

与其他scope相比，Spring并不管理 prototype Bean的完整生命周期。

容器对prototype对象进行实例化、配置和其他方面的组装，并将其交给客户端，而对该prototype实例没有进一步的记录。

因此，尽管初始化生命周期回调方法在所有对象上被调用，而不考虑scope，但在prototype的情况下，配置的销毁生命周期回调不会被调用。客户端代码必须清理prototype scope 内的对象，并释放原prototype Bean持有的昂贵资源。为了让Spring容器释放由 prototype scopeBean 持有的资源，可以尝试使用自定义 [Bean后处理器](https://springdoc.cn/spring/core.html#beans-factory-extension-bpp)，它持有对需要清理的Bean的引用。

在某些方面，Spring容器在 prototype scope Bean 方面的作用是替代Java的 `new` 操作。所有超过该点的生命周期管理必须由客户端处理。

##### 自定义

Bean的Scope机制是可扩展的。你可以定义你自己的Scope，甚至重新定义现有的Scope，尽管后者被认为是不好的做法，你不能覆盖内置的 `singleton` 和 `prototype` scope。

###### 创建

为了将你的自定义scope集成到 Spring 容器中，你需要实现 `org.springframework.beans.factory.config.Scope` 接口。

`Scope` 接口有四个方法来从scope中获取对象，从scope中移除对象，以及让对象被销毁。

例如，session scope的实现会返回 session scope 的 bean（如果它不存在，该方法会返回一个新的 bean 实例，在把它绑定到 session 上供将来引用）。

下面的方法从底层scope返回对象。

```java
Object get(String name, ObjectFactory<?> objectFactory)
```

例如，session scope的实现是将session scope的Bean从底层session中移除。该对象应该被返回，但是如果没有找到指定名称的对象，你可以返回 `null`。

下面的方法将对象从底层scope中删除。

```java
Object remove(String name)
```

下面的方法注册了一个callback，当scope被销毁或scope中的指定对象被销毁时，该callback应该被调用。

```java
void registerDestructionCallback(String name, Runnable destructionCallback)
```

下面的方法获得底层scope的conversation id。

```java
String getConversationId()
```

这个 id 对每个 scope 都是不同的。对于一个 session scope 的实现，这个 id 可以是 session id。

##### 使用

在你编写并测试了一个或多个自定义 `Scope` 实现之后，你需要让 Spring 容器知道你的新 Scope。下面的方法是向Spring容器注册新 `Scope` 的核心方法。

```java
void registerScope(String scopeName, Scope scope);
```

这个方法是在 `ConfigurableBeanFactory` 接口上声明的，它可以通过Spring的大多数具体 `ApplicationContext` 实现上的 `BeanFactory` 属性获得。

`registerScope(..)` 方法的第一个参数是与一个 scope 相关的唯一的名称。在 Spring 容器本身中这种名称的例子是 `singleton` 和 `prototype`。`registerScope(..)` 方法的第二个参数是你希望注册和使用的自定义 `Scope` 实现的实际实例。

假设你写了你的自定义 `Scope` 的实现，然后按下一个例子所示注册它。

> 下一个例子使用了 `SimpleThreadScope`，它包含在Spring中，但默认没有注册。对于你自己的自定义 `Scope` 实现，其说明是一样的。

```java
Scope threadScope = new SimpleThreadScope();
beanFactory.registerScope("thread", threadScope);
```

然后你可以创建符合你的自定义 `Scope` 的 scope 规则的bean定义，如下所示。

```xml
<bean id="..." class="..." scope="thread">
```

有了自定义的 `Scope` 实现，你就不局限于以编程方式注册该scope了。你也可以通过使用 `CustomScopeConfigurer` 类，以声明的方式进行 `Scope` 注册，如下例所示。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean class="org.springframework.beans.factory.config.CustomScopeConfigurer">
        <property name="scopes">
            <map>
                <entry key="thread">
                    <bean class="org.springframework.context.support.SimpleThreadScope"/>
                </entry>
            </map>
        </property>
    </bean>

    <bean id="thing2" class="x.y.Thing2" scope="thread">
        <property name="name" value="Rick"/>
        <aop:scoped-proxy/>
    </bean>

    <bean id="thing1" class="x.y.Thing1">
        <property name="thing2" ref="thing2"/>
    </bean>

</beans>
```

> 当你把 `<aop:scoped-proxy/>` 放在 `FactoryBean` 实现的 `<bean>` 声明中时，是 factory bean 本身被限定了scope，而不是从 `getObject()` 返回的对象。

#### 生命周期回调

为了与容器对Bean生命周期的管理进行交互，你可以实现Spring `InitializingBean` 和 `DisposableBean` 接口。

容器为前者调用 `afterPropertiesSet()`，为后者调用 `destroy()`，让Bean在初始化和销毁你的Bean时执行某些动作。

> JSR-250的 `@PostConstruct` 和 `@PreDestroy` 注解通常被认为是在现代Spring应用程序中接收生命周期回调的最佳实践。使用这些注解意味着你的Bean不会被耦合到Spring特定的接口。详情请参见 使用 [使用 `@PostConstruct` 和 `@PreDestroy`](https://springdoc.cn/spring/core.html#beans-postconstruct-and-predestroy-annotations)。
>
> 如果你不想使用JSR-250注解，但你仍然想消除耦合，可以考虑用 `init-method` 和 `destroy-method` bean 定义元数据。

在内部，Spring框架使用 `BeanPostProcessor` 实现来处理它能找到的任何回调接口并调用相应的方法。如果你需要自定义功能或其他Spring默认不提供的生命周期行为，你可以自己实现一个 `BeanPostProcessor`。

除了初始化和销毁回调外，Spring管理的对象还可以实现 `Lifecycle` 接口，以便这些对象能够参与启动和关闭过程，这是由容器自己的生命周期驱动的。

##### 初始化回调

`org.springframework.beans.factory.InitializingBean` 接口让Bean在容器对Bean设置了所有必要的属性后执行初始化工作。`InitializingBean` 接口指定了一个方法。

```java
void afterPropertiesSet() throws Exception;
```

我们建议你不要使用 `InitializingBean` 接口，因为它不必要地将代码与Spring耦合。

另外，我们建议使用 `@PostConstruct` 注解或指定一个POJO初始化方法。

在基于XML的配置元数据中，你可以使用 `init-method` 属性来指定具有 void 无参数签名的方法的名称。

对于Java配置，你可以使用 `@Bean` 的 `initMethod` 属性。

```xml
<bean id="exampleInitBean" class="examples.ExampleBean" init-method="init"/>
```

```java
public class ExampleBean {

    public void init() {
        // do some initialization work
    }
}
```

前面的例子与下面的例子（由两个列表组成）的效果几乎完全相同。

```xml
<bean id="exampleInitBean" class="examples.AnotherExampleBean"/>
```

```java
public class AnotherExampleBean implements InitializingBean {

    @Override
    public void afterPropertiesSet() {
        // do some initialization work
    }
}
```

然而，前面两个例子中的第一个并没有将代码与Spring耦合。

##### 销毁回调

实现 `org.springframework.beans.factory.DisposableBean` 接口可以让Bean在包含它的容器被销毁时获得一个回调。`DisposableBean` 接口指定了一个方法。

```java
void destroy() throws Exception;
```

我们建议你不要使用 `DisposableBean` 回调接口，因为它不必要地将代码耦合到Spring。

另外，我们建议使用 [`@PreDestroy`](https://springdoc.cn/spring/core.html#beans-postconstruct-and-predestroy-annotations) 注解或指定一个bean定义所支持的通用方法。

对于基于XML的配置元数据，你可以使用 `<bean/>` 上的 `destroy-method` 属性。

使用Java配置，你可以使用 `@Bean` 的 `destroyMethod` 属性。

```xml
<bean id="exampleInitBean" class="examples.ExampleBean" destroy-method="cleanup"/>
```

```java
public class ExampleBean {

    public void cleanup() {
        // do some destruction work (like releasing pooled connections)
    }
}
```

前面的定义与下面的定义几乎有完全相同的效果。

```xml
<bean id="exampleInitBean" class="examples.AnotherExampleBean"/>
```

```java
public class AnotherExampleBean implements DisposableBean {

    @Override
    public void destroy() {
        // do some destruction work (like releasing pooled connections)
    }
}
```

然而，前面两个定义中的第一个并没有将代码与Spring耦合。

> 你可以给 `<bean>` 元素的 `destroy-method` 属性分配一个特殊的 `(inferred)` 值，它指示Spring自动检测特定bean类上的public `close` 或 `shutdown` 方法。（任何实现了 `java.lang.AutoCloseable` 或 `java.io.Closeable` 的类都可以匹配）。
>
> 你也可以在 `<beans>` 元素的 `default-destroy-method` 属性上设置这个特殊的 `(inferred)` 值，将这个行为应用于整个Bean集合（参见 [默认的初始化和销毁方法](https://springdoc.cn/spring/core.html#beans-factory-lifecycle-default-init-destroy-methods)）。请注意，这是用Java配置的默认行为。

##### 默认方法

当你写初始化和销毁方法回调时，如果不使用Spring特定的 `InitializingBean` 和 `DisposableBean` 回调接口，你通常会写一些名称为 `init()`、`initialize()`、`dispose()` 等的方法。

理想情况下，这种生命周期回调方法的名称在整个项目中是标准化的，这样所有的开发者都会使用相同的方法名称，确保一致性。

你可以将Spring容器配置为在每个Bean上 "寻找" 命名的初始化和销毁回调方法名称。

这意味着你，作为应用开发者，可以编写你的应用类并使用名为 `init()` 的初始化回调，而不必为每个Bean定义配置 `init-method="init"` 属性。

当Bean被创建时，Spring IoC容器会调用该方法（并且符合 [之前描述](https://springdoc.cn/spring/core.html#beans-factory-lifecycle) 的标准生命周期回调约定）。这一特性也为初始化和销毁方法的回调执行了一致的命名规则。

假设你的初始化回调方法被命名为 `init()`，你的销毁回调方法被命名为 `destroy()`。

```java
public class DefaultBlogService implements BlogService {

    private BlogDao blogDao;

    public void setBlogDao(BlogDao blogDao) {
        this.blogDao = blogDao;
    }

    // this is (unsurprisingly) the initialization callback method
    public void init() {
        if (this.blogDao == null) {
            throw new IllegalStateException("The [blogDao] property must be set.");
        }
    }
}
```

然后你可以在一个类似于以下的bean中使用该类。

```xml
<beans default-init-method="init">

    <bean id="blogService" class="com.something.DefaultBlogService">
        <property name="blogDao" ref="blogDao" />
    </bean>

</beans>
```

顶层 `<beans/>` 元素属性中 `default-init-method` 属性的存在会使Spring IoC容器识别出Bean类中名为 `init` 的方法作为初始化方法的回调。当一个Bean被创建和装配时，如果Bean类有这样的方法，它就会在适当的时候被调用。

你可以通过使用顶层 `<beans/>` 元素上的 `default-destroy-method` 属性，类似地配置 `destroy` 方法回调（在XML中，也就是）。

如果现有的Bean类已经有了与惯例不同的回调方法，你可以通过使用 `<bean/`> 本身的 `init-method` 和 `destroy-method` 属性来指定（在XML中）方法的名称，从而覆盖默认值。

Spring容器保证在Bean被提供了所有的依赖关系后立即调用配置的初始化回调。

因此，初始化回调是在原始Bean引用上调用的，这意味着AOP拦截器等还没有应用到Bean上。首先完全创建一个目标Bean，然后应用一个带有拦截器链的AOP代理（比如说）。

如果目标Bean和代理是分开定义的，你的代码甚至可以绕过代理，与原始的目标Bean进行交互。因此，将拦截器应用于 `init` 方法是不一致的，因为这样做会将目标Bean的生命周期与它的代理或拦截器联系起来，当你的代码直接与原始目标Bean交互时，会留下奇怪的语义。

##### 回调控制

`Lifecycle` 接口定义了任何有自己的生命周期要求的对象的基本方法（如启动和停止一些后台进程）。

```java
public interface Lifecycle {

    void start();

    void stop();

    boolean isRunning();
}
```

任何Spring管理的对象都可以实现 `Lifecycle` 接口。

然后，当 `ApplicationContext` 本身收到启动和停止信号时（例如，在运行时的停止/重启场景），它将这些调用级联到定义在该上下文中的所有 `Lifecycle` 实现。

它通过委托给一个 `LifecycleProcessor` 来实现，如下表所示。

```java
public interface LifecycleProcessor extends Lifecycle {

    void onRefresh();

    void onClose();
}
```

请注意，`LifecycleProcessor` 本身就实现了 `Lifecycle` 接口。它还添加了另外两个方法来对 context 的刷新和关闭做出反应。

> 请注意，常规的 `org.springframework.context.Lifecycle` 接口是一个明确的start和stop通知的普通约定，并不意味着在上下文刷新时自动启动。如果要对特定Bean的自动启动进行细粒度控制（包括启动阶段），可以考虑实现 `org.springframework.context.SmartLifecycle` 来代替。
>
> 另外，请注意，stop通知并不保证在销毁之前出现。在定期关机时，所有的 `Lifecycle` Bean都会在一般的销毁回调被传播之前首先收到stop通知。然而，在上下文生命周期中的热刷新或stop刷新尝试时，只有销毁方法被调用。

启动和关闭调用的顺序可能很重要。

如果任何两个对象之间存在 "依赖" 关系，被依赖方在其依赖方之后启动，在其依赖方之前停止。然而，有时候，直接的依赖关系是未知的。你可能只知道某种类型的对象应该在另一种类型的对象之前启动。

在这些情况下， `SmartLifecycle` 接口定义了另一个选项，即其超接口 `Phased` 上定义的 `getPhase()` 方法。

下面的列表显示了 `Phased` 接口的定义。

```java
public interface Phased {

    int getPhase();
}
```

下面列出了 `SmartLifecycle` 接口的定义。

```java
public interface SmartLifecycle extends Lifecycle, Phased {

    boolean isAutoStartup();

    void stop(Runnable callback);
}
```

启动时，phase最低的对象先启动。当停止时，遵循相反的顺序。因此，一个实现了 `SmartLifecycle` 并且其 `getPhase()` 方法返回 `Integer.MIN_VALUE` 的对象将是最先启动和最后停止的对象。

在 spectrum 的另一端，一个 `Integer.MAX_VALUE` 的 phase 值将表明该对象应该最后启动并首先停止（可能是因为它依赖于其他进程的运行）。

在考虑 phase 值时，同样重要的是要知道，任何没有实现 `SmartLifecycle` 的 "正常" `Lifecycle` 对象的默认 phase 是 `0`。 

因此，任何负的 phase 值表示一个对象应该在那些标准组件之前开始（并在它们之后停止）。反之，任何正的 phase 值也是如此。

由 `SmartLifecycle` 定义的 stop 方法接受一个回调。任何实现都必须在该实现的关闭过程完成后调用该回调的 `run()` 方法。这在必要时可以实现异步关机，因为 `LifecycleProcessor` 接口的默认实现 `DefaultLifecycleProcessor` 会等待每个阶段内的对象组调用该回调，直到其超时值。

每个阶段的默认超时是 30 秒。你可以通过在上下文中定义一个名为 `lifecycleProcessor` 的bean来覆盖默认的生命周期处理器实例。

如果你只想修改超时时间，定义以下内容就足够了。

```xml
<bean id="lifecycleProcessor" class="org.springframework.context.support.DefaultLifecycleProcessor">
    <!-- timeout value in milliseconds -->
    <property name="timeoutPerShutdownPhase" value="10000"/>
</bean>
```

如前所述，`LifecycleProcessor` 接口也定义了用于刷新和关闭上下文（context ）的回调方法。后者驱动关闭过程，就像明确调用 `stop()` 一样，但它发生在上下文关闭的时候。

另一方面，"refresh" 回调方法实现了 `SmartLifecycle` Bean的另一个特性。当上下文被刷新时（在所有对象都被实例化和初始化后），该回调被调用。

这时，默认的生命周期处理器会检查每个 `SmartLifecycle` 对象的 `isAutoStartup()` 方法所返回的布尔值。如果为 `true`，该对象将在此时启动，而不是等待上下文或其自身 `start()` 方法的显式调用（与上下文刷新不同，上下文的启动不会自动发生在标准的上下文实现中）。

如前所述，`phase` 值和任何 "依赖" 关系决定了启动的顺序。

#### aware 接口

在 Spring 框架中，"Aware" 接口是一组特殊的接口，用于让你的 Bean 获取有关 Spring 容器和其他资源的信息。

**常见接口**

1. **ApplicationContextAware**：通过实现 ApplicationContextAware 接口，Bean 可以获取对 ApplicationContext（Spring 容器）的引用。这使得 Bean 可以编程式地与容器交互，例如，检索其他 Bean 或执行特定于 Spring 容器的操作。
2. **BeanNameAware**：通过实现 BeanNameAware 接口，Bean 可以获取在 Spring 容器中定义的它自己的 Bean 名称。这允许 Bean 在初始化之前访问它的名称，并执行一些额外的操作，如果需要。
3. **BeanFactoryAware**：通过实现 BeanFactoryAware 接口，Bean 可以获取对 BeanFactory（Spring IoC 容器）的引用。这允许 Bean 访问容器的基本功能，例如，获取其他 Bean 或检索 Bean 的属性值。
4. **MessageSourceAware**：通过实现 MessageSourceAware 接口，Bean 可以获取对 MessageSource（用于国际化和本地化的资源管理）的引用。这允许 Bean 访问消息和文本，以便进行本地化。
5. **ResourceLoaderAware**：通过实现 ResourceLoaderAware 接口，Bean 可以获取对 ResourceLoader（用于资源加载的接口）的引用。这使 Bean 可以加载外部资源，如文件或 URL。
6. **ServletConfigAware**：用于 Web 应用程序的 Aware 接口。通过实现 ServletConfigAware 接口，Bean 可以获取对 ServletConfig 的引用，这允许 Bean 访问 Servlet 配置信息。

------

当 `ApplicationContext` 创建一个实现 `org.springframework.context.ApplicationContextAware` 接口的对象实例时，该实例被提供给该 `ApplicationContext` 的引用。下面的列表显示了 `ApplicationContextAware` 接口的定义。

```java
public interface ApplicationContextAware {

    void setApplicationContext(ApplicationContext applicationContext) throws BeansException;
}
```

因此，Bean可以通过 `ApplicationContext` 接口或通过将引用转换为该接口的已知子类（如 `ConfigurableApplicationContext`，它暴露了额外的功能），以编程方式操作创建它们的 `ApplicationContext`。

一个用途是对其他Bean进行编程式检索。有时这种能力是很有用的。然而，一般来说，你应该避免这样做，因为它将代码与Spring耦合在一起，并且不遵循控制反转（Inversion of Control）的风格，即合作者作为属性提供给Bean。

`ApplicationContext` 的其他方法提供了对文件资源的访问，发布应用程序事件，以及访问 `MessageSource`。

Autowire 是获得对 `ApplicationContext` 引用的另一种选择。传统的 `constructor` 和 `byType` 自动注入模式可以分别为构造器参数或设 setter 方法参数提供 `ApplicationContext` 类型的依赖。

为了获得更多的灵活性，包括自动注入字段和多个参数方法的能力，请使用基于注解的自动注入功能。如果你这样做，`ApplicationContext` 将被自动注入到字段、构造函数参数或方法参数中，如果有关字段、构造函数或方法带有 `@Autowired` 注解，则期望 `ApplicationContext` 类型。更多信息请参见 [使用 `@Autowired`](https://springdoc.cn/spring/core.html#beans-autowired-annotation)。

当 `ApplicationContext` 创建一个实现 `org.springframework.beans.factory.BeanNameAware` 接口的类时，该类被提供给其相关对象定义中定义的名称的引用。下面的列表显示了 `BeanNameAware` 接口的定义。

```java
public interface BeanNameAware {

    void setBeanName(String name) throws BeansException;
}
```

这个回调是在正常的Bean属性之后，但在 `InitializingBean.afterPropertiesSet()` 或自定义 `init-method` 等初始化回调之前调用的。

> 请再次注意，使用这些接口会将你的代码与Spring API捆绑在一起，并且不遵循反转控制的风格。因此，我们建议那些需要对容器进行编程访问的基础设施Bean使用这些接口。

#### 继承

一个Bean定义可以包含很多配置信息，包括构造函数参数、属性值和容器特有的信息，如初始化方法、静态工厂方法名称等等。一个子Bean定义从父定义继承配置数据。子定义可以覆盖一些值或根据需要添加其他值。使用父Bean定义和子Bean定义可以节省大量的打字工作。有效地，这是一种模板化的形式。

如果你以编程方式处理 `ApplicationContext` 接口，子bean定义由 `ChildBeanDefinition` 类表示。大多数用户不会在这个层面上与他们一起工作。相反，他们在 `ClassPathXmlApplicationContext` 这样的类中声明性地配置Bean定义。当你使用基于XML的配置元数据时，你可以通过使用 `parent` 属性来指示子Bean定义，将父Bean指定为这个属性的值。下面的例子显示了如何做到这一点。

```xml
<bean id="inheritedTestBean" abstract="true"
        class="org.springframework.beans.TestBean">
    <property name="name" value="parent"/>
    <property name="age" value="1"/>
</bean>

<bean id="inheritsWithDifferentClass"
        class="org.springframework.beans.DerivedTestBean"
        parent="inheritedTestBean" init-method="initialize">  
    <property name="name" value="override"/>
    <!-- the age property value of 1 will be inherited from parent -->
</bean>
```

如果没有指定，子Bean定义会使用父定义中的Bean类，但也可以覆盖它。在后一种情况下，子Bean类必须与父类兼容（也就是说，它必须接受父类的属性值）。

子Bean定义从父级继承scope、构造函数参数值、属性值和方法重写，并可以选择添加新的值。你指定的任何scope、初始化方法、销毁（destroy）方法或 `static` 工厂方法设置都会覆盖相应的父类设置。

其余的设置总是来自于子定义：依赖、自动注入模式、依赖检查、singleton和懒加载。

前面的例子通过使用 `abstract` 属性明确地将父类Bean定义标记为抽象的。如果父定义没有指定一个类，就需要明确地将父Bean定义标记为抽象的，如下例所示。

```xml
<bean id="inheritedTestBeanWithoutClass" abstract="true">
    <property name="name" value="parent"/>
    <property name="age" value="1"/>
</bean>

<bean id="inheritsWithClass" class="org.springframework.beans.DerivedTestBean"
        parent="inheritedTestBeanWithoutClass" init-method="initialize">
    <property name="name" value="override"/>
    <!-- age will inherit the value of 1 from the parent bean definition-->
</bean>
```

父类Bean不能被单独实例化，因为它是不完整的，而且它也被明确标记为 `abstract` 的。当一个定义是 `abstract` 的，它只能作为一个纯模板Bean定义使用，作为子定义的父定义。试图单独使用这样的 `abstract` 父类 bean，通过将其作为另一个 bean 的 `ref` 属性来引用，或者用父类 bean 的 ID 进行显式 `getBean()` 调用，会返回一个错误。同样地，容器内部的 `preInstantiateSingletons()` 方法也会忽略被定义为抽象的 bean 定义。



### 依赖

一个典型的企业应用程序并不是由单一的对象（或Spring术语中的bean）组成的。即使是最简单的应用也有一些对象，它们一起工作，呈现出最终用户所看到的连贯的应用。

在Java Spring中，"依赖"通常指的是一个组件（或Bean）依赖于另一个组件或类，这意味着一个组件需要另一个组件的功能或数据，因此需要引用或依赖它。

#### 依赖注入

依赖注入（Dependency Injection，DI）是一个过程，对象仅通过构造参数、工厂方法的参数或在对象实例被构造或从工厂方法返回后在其上设置的属性来定义它们的依赖（即与它们一起工作的其它对象）；然后，容器在创建 Bean 时注入这些依赖。

这个过程从根本上说是 Bean 本身通过使用类的直接构造或服务定位模式来控制其依赖的实例化或位置的逆过程（因此被称为控制反转）。

- **依赖注入是一个过程**：它是一种模式或机制，用于将一个对象的依赖关系（即与其他对象的协作）动态地注入或提供给这个对象。
- **定义依赖**：对象通过构造参数、工厂方法的参数或属性来定义它们的依赖。这意味着对象会明确地声明需要哪些其他对象来完成其功能。
- **容器注入依赖**：在应用程序中，容器（通常是Spring容器）负责实例化和管理这些对象。当容器创建一个Bean时，它会根据Bean的定义，自动注入（提供）这些依赖。
- **控制反转（IoC）**：依赖注入是控制反转的一种表现。通常，一个对象负责创建或查找它所依赖的其他对象，而在依赖注入中，这个过程被颠倒了。Bean不再主动查找或创建它所依赖的对象，而是等待容器将这些依赖注入到它内部。这就是所谓的控制反转。

采用DI原则，代码会更干净，当对象被提供其依赖时，解耦会更有效。对象不会查找其依赖，也不知道依赖的位置或类别。因此，你的类变得更容易测试，特别是当依赖是在接口或抽象基类上时，这允许在单元测试中使用stub或mock实现。

DI 有两个主要的变体： **基于构造器的依赖注入** 和 **基于setter的依赖注入**。

##### 构造器注入

基于构造函数的 DI 是通过容器调用**带有许多参数的构造函数来完成的，每个参数代表一个依赖**。调用带有特定参数的 `static` 工厂方法来构造 Bean 几乎是等价的。

```java
public class SimpleMovieLister {

    // SimpleMovieLister 依赖于 MovieFinder
    private final MovieFinder movieFinder;

    // 构造方法，用于Spring容器注入 MovieFinder
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // 省略了使用注入的 MovieFinder 执行业务逻辑的部分...
}
```

###### 参数解析

构造函数参数解析匹配是通过使用参数的类型进行的。如果 Bean 定义中的构造器参数不存在潜在的歧义，那么构造器参数在 Bean 定义中的定义顺序就是这些参数在 Bean 被实例化时被提供给适当的构造器的顺序。

```java
package x.y;

public class ThingOne {

    public ThingOne(ThingTwo thingTwo, ThingThree thingThree) {
        // ...
    }
}
```

假设 `ThingTwo` 和 `ThingThree` 类没有继承关系，就不存在潜在的歧义。因此，下面的配置可以正常工作，你不需要在 `<constructor-arg/>` 元素中明确指定构造函数参数的索引或类型。

```xml
<beans>
    <bean id="beanOne" class="x.y.ThingOne">
        <constructor-arg ref="beanTwo"/>
        <constructor-arg ref="beanThree"/>
    </bean>

    <bean id="beanTwo" class="x.y.ThingTwo"/>
    <bean id="beanThree" class="x.y.ThingThree"/>
</beans>
```

当引用另一个Bean时，类型是已知的，并且可以进行匹配（就像前面的例子那样）。当使用一个简单的类型时，比如 `<value>true</value>`，Spring不能确定值的类型，所以在没有帮助的情况下不能通过类型进行匹配。

```java
package examples;

public class ExampleBean {

    private final int years;
    private final String ultimateAnswer;

    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

在前面的情况下，如果你通过使用 `type` 属性显式地指定构造函数参数的类型，容器就可以使用简单类型的类型匹配，如下例所示。

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```

可以使用 `index` 属性来明确指定构造函数参数的索引，如下例所示。

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```

除了解决多个简单值的歧义外，指定一个索引还可以解决构造函数有两个相同类型的参数的歧义。

也可以使用构造函数的参数名称来进行消歧，如下面的例子所示。

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```

请记住，要使这一方法开箱即用，你的代码在编译时必须启用debug标志，以便Spring能够从构造函数中查找参数名称。

> 因为在默认情况下，Java编译器将构造函数的参数名称擦除。

如果你不能或不想用debug标志编译你的代码，你可以使用 `@ConstructorProperties ` JDK注解来明确命名你的构造函数参数。

```java
package examples;

public class ExampleBean {
    @ConstructorProperties({"years", "ultimateAnswer"})
    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

##### `Setter` 注入

基于 Setter 的 DI 是通过容器在调用无参数的构造函数或无参数的 `static` 工厂方法来实例化你的 bean 之后调用 Setter 方法来实现的。

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- 使用嵌套的 ref 元素进行 setter 注入 -->
    <property name="beanOne">
        <ref bean="anotherExampleBean"/>
    </property>

    <!-- 使用简洁的 ref 属性进行 setter 注入 -->
    <property name="beanTwo" ref="yetAnotherBean"/>
    <property name="integerProperty" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

```java
public class ExampleBean {

    private AnotherBean beanOne;

    private YetAnotherBean beanTwo;

    private int i;

    public void setBeanOne(AnotherBean beanOne) {
        this.beanOne = beanOne;
    }

    public void setBeanTwo(YetAnotherBean beanTwo) {
        this.beanTwo = beanTwo;
    }

    public void setIntegerProperty(int i) {
        this.i = i;
    }
}
```

`ApplicationContext` 支持它所管理的Bean的基于构造器和基于setter的DI。它还支持在一些依赖已经通过构造器方法注入后的基于setter的DI。

以 `BeanDefinition` 的形式配置依赖关系，你将其与 `PropertyEditor` 实例一起使用，将属性从一种格式转换为另一种；`PropertyEditor` 是一个 Java 类，它用于将属性从一种格式（如字符串）转换为另一种格式（如对象）。

然而，大多数Spring用户并不直接使用这些类（即以编程方式），而是使用XML Bean定义、注解组件（即用 `@Component`、 `@Controller` 等注解的类），或基于Java的 `@Configuration` 类中的 `@Bean` 方法。然后这些来源在内部被转换为 `BeanDefinition` 的实例，并用于加载整个Spring IoC容器实例。

##### 差异

由于可以混合使用基于构造函数的DI和基于setter的DI，一个好的经验法则是对强制依赖使用构造函数，对可选依赖使用setter方法或配置方法。

> 在setter方法上使用 `@Autowired` 注解可以使属性成为必须的依赖；然而，带有参数程序化验证的构造器注入是更好的。

Spring团队通常提倡构造函数注入，因为它可以让你将应用组件实现为不可变的对象，并确保所需的依赖不为 `null`，这意味着在创建 Bean 实例时，必须提供所有必需的依赖项。此外，**构造函数注入的组件总是以完全初始化的状态返回给客户端（调用）代码**。顺便提一下，大量的构造函数参数是一种不好的代码气味，意味着该类可能有太多的责任，应该重构以更好地解决适当的分离问题。

Setter注入主要应该只用于在类中可以分配合理默认值的可选依赖。否则，必**须在代码使用依赖的所有地方进行非null值检查**。Setter注入的一个好处是，Setter方法使该类的对象可以在以后重新配置或重新注入。因此，通过 JMX MBean 进行管理是setter注入的一个引人注目的用例。

> 这也就意味着，通过构造器注入的依赖可以直接使用，不用担心 null 异常，因为在 Bean 被构建时就已经注入；但是对于 Setter 注入必须考虑是否为 null，因为这种方式是运行时注入，调用时不一定存在。

对于一个特定的类，使用最合理的DI风格。有时，在处理你没有源代码的第三方类时，你会做出选择。例如，如果一个第三方类没有暴露任何setter方法，那么构造函数注入可能是唯一可用的DI形式。

#### 解析过程

容器按如下方式执行 bean 依赖解析。

- `ApplicationContext` 是用描述所有bean的配置元数据创建和初始化的。配置元数据可以由XML、Java代码或注解来指定。
- 对于每个Bean来说，它的依赖是以属性、构造函数参数或静态工厂方法的参数（如果你用它代替正常的构造函数）的形式表达的。在实际创建Bean时，这些依赖被提供给Bean。
- 每个属性或构造函数参数都是要设置的值的实际定义，或对容器中另一个Bean的引用。
- 每个作为值的属性或构造函数参数都会从其指定格式转换为该属性或构造函数参数的实际类型。默认情况下，Spring 可以将以字符串格式提供的值转换为所有内置类型，如 `int`、`long`、`String`、`boolean` 等等。

当容器被创建时，Spring容器会验证每个Bean的配置。

在实际创建Bean之前，Bean 的属性本身不会被设置。当容器被创建时，那些具有单例作用域并被设置为预实例化的 Bean（默认）被创建。作用域在 Bean Scope 中定义。否则，Bean只有在被请求时才会被创建。

创建 bean 意味着实例化和初始化这些对象，同时解析它们的依赖关系。这就构成了一个 bean 图，其中各个 bean 相互关联，依赖于其他 bean。

有时候在容器创建 bean 的过程中，bean 之间的依赖关系可能出现问题，这些问题可能不会在容器加载时立即暴露，而是在第一次创建某个受影响的 bean 时才会显现出来。

> ##### 循环依赖
>
> 如果你使用主要的构造函数注入，就有可能产生一个无法解决的循环依赖情况。
>
> 比如说。类A通过构造函数注入需要类B的一个实例，而类B通过构造函数注入需要类A的一个实例。如果你将A类和B类的Bean配置为相互注入，Spring IoC容器会在运行时检测到这种循环引用，并抛出一个 `BeanCurrentlyInCreationException`。
>
> 一个可能的解决方案是编辑一些类的源代码，使其通过setter而不是构造器进行配置。或者，避免构造器注入，只使用setter注入。换句话说，虽然不推荐这样做，但你可以用setter注入来配置循环依赖关系。
>
> 与典型的情况（没有循环依赖关系）不同，Bean A和Bean B之间的循环依赖关系迫使其中一个Bean在被完全初始化之前被注入到另一个Bean中（一个典型的鸡生蛋蛋生鸡的场景）。

Spring 在容器加载时会进行配置检查，包括检测配置中是否存在对不存在的 bean 的引用以及是否存在循环依赖。

**Spring 尽可能晚地设置 bean 的属性和解析它们之间的依赖关系**。这意味着当你请求获取一个对象时，如果在创建该对象或其某个依赖关系时出现了问题，已经正确加载的 Spring 容器会立即产生异常。

**默认情况下，`ApplicationContext` 在容器启动时会立即初始化（实例化和初始化）预定义的单例 bean**。这意味着这些单例 bean 会在应用程序运行之前被创建，无论是否在应用程序的初始阶段实际需要它们。

这种早期初始化的方式有一些好处和代价。好处在于，它能够在应用程序启动时就发现配置问题，以确保容器的状态在运行时是稳定和可靠的。也就是说，如果有任何与配置相关的问题，它们将在应用程序启动时立即被检测到，而不是在稍后的操作中导致异常。

然而，这种方式可能会导致一些前期时间和内存的开销，因为它会立即创建并初始化所有的单例 bean，即使在应用程序启动阶段并没有立即使用它们。如果希望减少这种前期开销，你仍然可以覆盖默认行为，将单例 bean 设置为懒加载。这意味着它们将在首次被请求时才会被实例化和初始化，而不是在容器启动时就立即创建。

如果不存在循环依赖关系，当一个或多个协作（Collaborate） Bean被注入到依赖Bean中时，每个协作Bean在被注入到依赖Bean中之前被完全配置。

这意味着，如果Bean A对Bean B有依赖，Spring IoC容器会在调用Bean A的setter方法之前完全配置Bean B。换句话说，Bean被实例化（如果它不是预先实例化的单例），其依赖被设置，相关的生命周期方法（如 配置的 init 方法 或 InitializingBean 回调方法）被调用。

#### 配置细节

##### 字面值

`<property/>` 元素的 `value` 属性将属性或构造函数参数指定为人类可读的字符串表示。Spring 的 [转换服务](https://springdoc.cn/spring/core.html#core-convert-ConversionService-API) 被用来将这些值从 `String` 转换成属性或参数的实际类型。

```java
<bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mydb"/>
    <property name="username" value="root"/>
    <property name="password" value="misterkaoli"/>
</bean>
```

**Spring将属性等的空参数视为空字符串**，`<null/>` 元素处理 `null` 值。

##### 其他引用

`idref` 元素仅仅是将容器中另一个 bean 的 `id`（一个字符串值—不是引用）传递给 `<constructor-arg/>` 或 `<property/>` 元素的一种防错方式。下面的例子展示了如何使用它。

```xml
<bean id="theTargetBean" class="..."/>

<bean id="theClientBean" class="...">
    <property name="targetName">
        <idref bean="theTargetBean"/>
    </property>
</bean>
```

前面的Bean定义片段完全等同于（在运行时）下面的片段。

```xml
<bean id="theTargetBean" class="..." />

<bean id="client" class="...">
    <property name="targetName" value="theTargetBean"/>
</bean>
```

第一种形式比第二种形式好，因为使用 `idref` 标签可以让容器在部署时验证被引用的、命名的 bean 是否真的存在。

在第二种变体中，没有对传递给 `client` Bean 的 `targetName` 属性的值进行验证。只有在 `client` Bean实际被实例化时，才会发现错误（很可能是致命的结果）。

如果 `client` Bean是一个 [prototype](https://springdoc.cn/spring/core.html#beans-factory-scopes) Bean，那么这个错别字和由此产生的异常可能只有在容器被部署后很久才能被发现。

> 4.0 版Bean XSD中不再支持 `idref` 元素上的 `local` 属性，因为它不再提供比普通 `bean` 引用更多的价值。在升级到4.0 schema时，将你现有的 `idref local` 引用改为 `idref bean`。

`ref` 元素是 `<constructor-arg/>` 或 `<property/>` 定义元素中的最后一个元素。在这里，你把一个 bean 的指定属性的值设置为对容器所管理的另一个 bean（协作者）的引用。被引用的 bean 是其属性要被设置的 bean 的依赖关系，它在属性被设置之前根据需要被初始化。（如果协作者是一个单例bean，它可能已经被容器初始化了）。所有的引用最终都是对另一个对象的引用。scope和验证取决于你是否通过 `bean` 或 `parent` 属性来指定其他对象的ID或名称。

通过 `<ref/>` 标签的 `bean` 属性指定目标 bean 是最一般的形式，它允许创建对同一容器或父容器中的任何 bean 的引用，不管它是否在同一个 XML 文件中。`bean` 属性的值可以与目标bean的 `id` 属性相同，或者与目标bean的 `name` 属性中的一个值相同。下面的例子显示了如何使用一个 `ref` 元素。

```xml
<ref bean="someBean"/>
```

通过 `parent` 属性指定目标Bean，可以创建对当前容器的父容器中的Bean的引用。 `parent` 属性的值可以与目标Bean的 `id` 属性或目标Bean的 `name` 属性中的一个值相同。目标Bean必须在当前容器的一个父容器中。当你有一个分层的容器，你想用一个与父级Bean同名的代理来包装父级容器中的现有Bean时，你应该使用这种Bean引用变体。下面的一对列表展示了如何使用 `parent` 属性。

```xml
<!-- in the parent context -->
<bean id="accountService" class="com.something.SimpleAccountService">
    <!-- insert dependencies as required here -->
</bean>
<!-- in the child (descendant) context -->
<bean id="accountService" <!-- bean name is the same as the parent bean -->
    class="org.springframework.aop.framework.ProxyFactoryBean">
    <property name="target">
        <ref parent="accountService"/> <!-- notice how we refer to the parent bean -->
    </property>
    <!-- insert other configuration and dependencies as required here -->
</bean>
```

> 在4.0 beans XSD中不再支持 `ref` 元素上的 `local` 属性，因为它不再提供比普通 `bean` 引用更多的价值。在升级到4.0 schema时，将你现有的 `ref local` 引用改为 `ref bean`。

##### 内部 Bean

在 `<property/>` 或 `<constructor-arg/`> 元素内的 `<bean/>` 元素定义了一个内部Bean，如下例所示。

```xml
<bean id="outer" class="...">
    <!-- 而不是使用对目标Bean的引用，只需在行内定义目标Bean即可 -->
    <property name="target">
        <bean class="com.example.Person"> <!-- 这是内部Bean -->
            <property name="name" value="Fiona Apple"/>
            <property name="age" value="25"/>
        </bean>
    </property>
</bean>
```

内部 bean 定义不需要定义 ID 或名称。如果指定了，容器不会使用这样的值作为标识符。容器也会忽略创建时的 `scope` 标志，因为内层 bean 总是匿名的，并且总是与外层 bean 一起创建。不可能独立地访问内层 bean，也不可能将它们注入到除包裹 bean 之外的协作 bean 中。

作为一个转折点，可以从自定义scope中接收销毁回调—例如，对于包含在单例 bean 中的请求scope的内层 bean。内层 bean 实例的创建与它所包含的 bean 相联系，但是销毁回调让它参与到请求作用域的生命周期中。这并不是一种常见的情况。内层Bean通常只是共享其包含Bean的scope。

##### 集合

`<list/>`、`<set/>`、`<map/>` 和 `<props/>` 元素分别设置Java `Collection` 类型 `List`、`Set`、`Map` 和 `Properties` 的属性和参数。下面的例子展示了如何使用它们。

```xml
<bean id="moreComplexObject" class="example.ComplexObject">
    <!-- results in a setAdminEmails(java.util.Properties) call -->
    <property name="adminEmails">
        <props>
            <prop key="administrator">administrator@example.org</prop>
            <prop key="support">support@example.org</prop>
            <prop key="development">development@example.org</prop>
        </props>
    </property>
    <!-- results in a setSomeList(java.util.List) call -->
    <property name="someList">
        <list>
            <value>a list element followed by a reference</value>
            <ref bean="myDataSource" />
        </list>
    </property>
    <!-- results in a setSomeMap(java.util.Map) call -->
    <property name="someMap">
        <map>
            <entry key="an entry" value="just some string"/>
            <entry key="a ref" value-ref="myDataSource"/>
        </map>
    </property>
    <!-- results in a setSomeSet(java.util.Set) call -->
    <property name="someSet">
        <set>
            <value>just some string</value>
            <ref bean="myDataSource" />
        </set>
    </property>
</bean>
```

map 的 key 值或 value 值，或 set 值，也可以是以下任何元素。

```xml
bean | ref | idref | list | set | map | props | value | null
```

##### 集合合并

Spring容器也支持合并集合。开发者可以定义一个父 <list/>、<map/>、<set/> 或 <props/> 元素，让子 <list/>、<map/>、<set/> 或 <props/> 元素继承和覆盖父集合的值。也就是说，子集合的值是合并父集合和子集合的元素的结果，子集合的元素覆盖父集合中指定的值。

关于合并的这一节讨论了父子bean机制。不熟悉父子Bean定义的读者可能希望在继续阅读 [相关章节](https://springdoc.cn/spring/core.html#beans-child-bean-definitions)。

下面的例子演示了集合的合并。

```xml
<beans>
    <bean id="parent" abstract="true" class="example.ComplexObject">
        <property name="adminEmails">
            <props>
                <prop key="administrator">administrator@example.com</prop>
                <prop key="support">support@example.com</prop>
            </props>
        </property>
    </bean>
    <bean id="child" parent="parent">
        <property name="adminEmails">
            <!-- the merge is specified on the child collection definition -->
            <props merge="true">
                <prop key="sales">sales@example.com</prop>
                <prop key="support">support@example.co.uk</prop>
            </props>
        </property>
    </bean>
<beans>
```

注意在子Bean定义的 `adminEmails` 属性的 `<props/>` 元素上使用了 `merge=true` 属性。当 `child` Bean被容器解析并实例化时，产生的实例有一个 `adminEmails` `Properties` 集合，它包含了将 `child` Bean的 `adminEmails` 集合与父Bean的 `adminEmails` 集合合并的结果。下面的列表显示了这个结果。

```
administrator=administrator@example.com
sales=sales@example.com
support=support@example.co.uk
```

子代 `Properties` 集合的值继承了父代 `<props/>` 中的所有属性元素，子代的 `support` 值会覆盖父代集合中的值。

这种合并行为类似于适用于 `<list/>`、`<map/>` 和 `<set/>` 集合类型。在 `<list/>` 元素的特殊情况下，与 `List` 集合类型相关的语义（也就是值的有序集合的概念）被保持。父列表的值在所有子列表的值之前。在 `Map`、`Set` 和 `Properties` 集合类型的情况下，不存在排序。因此，对于容器在内部使用的相关的 `Map`、`Set` 和 `Properties` 实现类型的基础上的集合类型，没有排序语义。

> 你不能合并不同的集合类型（例如 `Map` 和 `List`）。如果你试图这样做，会抛出一个适当的 `Exception`。`merge` 属性必须被指定在较低的、继承的、子定义上。在父级集合定义上指定 `merge` 属性是多余的，并且不会导致期望的合并。

##### 强类型集合

由于Java对泛型的支持，你可以使用强类型的 `Collection`。也就是说，我们可以声明一个 `Collection` 类型，使其只能包含（例如）`String` 元素。如果你使用Spring将一个强类型的 `Collection` 依赖性注入到Bean中，你可以利用Spring的类型转换支持，这样你的强类型 `Collection` 实例的元素在被添加到集合中之前就被转换为适当的类型。下面的Java类和Bean定义展示了如何做到这一点。

```java
public class SomeClass {

    private Map<String, Float> accounts;

    public void setAccounts(Map<String, Float> accounts) {
        this.accounts = accounts;
    }
}
<beans>
    <bean id="something" class="x.y.SomeClass">
        <property name="accounts">
            <map>
                <entry key="one" value="9.99"/>
                <entry key="two" value="2.75"/>
                <entry key="six" value="3.99"/>
            </map>
        </property>
    </bean>
</beans>
```

当 `something` bean 的 `account` 属性准备注入时，关于强类型的 `Map<String, Float>` 的元素类型的泛型信息可以通过反射获得。因此，Spring的类型转换基础设施将各种值元素识别为 `Float` 类型，而字符串值（`9.99`、`2.75` 和 `3.99`）被转换为实际的 `Float` 类型。

##### 命名空间

p-namespace（命名空间） 让你使用 `bean` 元素的属性（而不是嵌套的 `<property/>` 元素）来描述你的属性值合作Bean，或者两者都是。

Spring支持具有 [命名空间](https://springdoc.cn/spring/core.html#core.appendix.xsd-schemas) 的可扩展配置格式，这些命名空间是基于XML Schema定义的。本章讨论的 `beans` 配置格式是在 XML Schema 文件中定义的。然而，p-namespace 没有在XSD文件中定义，只存在于Spring的核心（core）中。

下面的例子显示了两个XML片段（第一个使用标准的XML格式，第二个使用p-namespace），它们的解析结果相同。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="classic" class="com.example.ExampleBean">
        <property name="email" value="someone@somewhere.com"/>
    </bean>

    <bean name="p-namespace" class="com.example.ExampleBean"
        p:email="someone@somewhere.com"/>
</beans>
```

这个例子显示了在bean定义中，p-namespace中有一个名为 `email` 的属性。这告诉Spring包括一个属性声明。如前所述，p-namespace没有schema定义，所以你可以将attribute的名称设置为property名称。

接下来的例子包括了另外两个Bean定义，它们都有对另一个Bean的引用。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean name="john-classic" class="com.example.Person">
        <property name="name" value="John Doe"/>
        <property name="spouse" ref="jane"/>
    </bean>

    <bean name="john-modern"
        class="com.example.Person"
        p:name="John Doe"
        p:spouse-ref="jane"/>

    <bean name="jane" class="com.example.Person">
        <property name="name" value="Jane Doe"/>
    </bean>
</beans>
```

这个例子不仅包括使用p命名空间的属性值，而且还使用了一种特殊的格式来声明属性引用。第一个Bean定义使用 `<property name="spouse" ref="jane"/>` 来创建一个从Bean `john` 到Bean `jane` 的引用，而第二个Bean定义使用 `p:spouse-ref="jane"` 作为属性来做完全相同的事情。在这种情况下，`spouse` 是属性名称，而 `-ref` 部分表明这不是一个直接的值，而是对另一个bean的引用。

> p命名空间不像标准的XML格式那样灵活。例如，声明属性引用的格式与以 `Ref` 结尾的属性发生冲突，而标准的XML格式则不会。我们建议你仔细选择你的方法，并将其传达给你的团队成员，以避免产生同时使用三种方法的XML文档。

------

与 [使用p命名空间的XML快捷方式](https://springdoc.cn/spring/core.html#beans-p-namespace) 类似，Spring 3.1中引入的c命名空间允许配置构造器参数的内联属性，而不是嵌套的 `constructor-arg` 元素。

下面的例子使用 `c:` 命名空间来做与 [基于构造器的依赖注入](https://springdoc.cn/spring/core.html#beans-constructor-injection) 相同的事情。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:c="http://www.springframework.org/schema/c"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="beanTwo" class="x.y.ThingTwo"/>
    <bean id="beanThree" class="x.y.ThingThree"/>

    <!-- traditional declaration with optional argument names -->
    <bean id="beanOne" class="x.y.ThingOne">
        <constructor-arg name="thingTwo" ref="beanTwo"/>
        <constructor-arg name="thingThree" ref="beanThree"/>
        <constructor-arg name="email" value="something@somewhere.com"/>
    </bean>

    <!-- c-namespace declaration with argument names -->
    <bean id="beanOne" class="x.y.ThingOne" c:thingTwo-ref="beanTwo"
        c:thingThree-ref="beanThree" c:email="something@somewhere.com"/>

</beans>
```

`c:` 命名空间使用了与 `p:` 命名空间相同的约定（Bean引用的尾部 `-ref`），用于按名称设置构造函数参数。同样，它也需要在XML文件中声明，尽管它没有在XSD schema中定义（它存在于Spring 核心（core）中）。

对于构造函数参数名称不可用的罕见情况（通常是字节码编译时没有debug信息），你可以使用回退到参数索引（下标），如下所示。

```xml
<!-- c-namespace index declaration -->
<bean id="beanOne" class="x.y.ThingOne" c:_0-ref="beanTwo" c:_1-ref="beanThree"
    c:_2="something@somewhere.com"/>
```

>   由于XML语法的原因，索引符号需要有前面的 `_`，因为XML属性名不能以数字开头（尽管有些IDE允许这样做）。相应的索引符号也可用于 `<constructor-arg>` 元素，但并不常用，因为通常情况下，普通的声明顺序已经足够了。

在实践中，构造函数解析 [机制](https://springdoc.cn/spring/core.html#beans-factory-ctor-arguments-resolution) 在匹配参数方面相当有效，所以除非你真的需要，否则我们建议在整个配置中使用名称符号。

##### 复合属性名

当你设置Bean属性时，你可以使用复合或嵌套的属性名，只要路径中除最终属性名外的所有组件不为 `null`。考虑一下下面的Bean定义。

```xml
<bean id="something" class="things.ThingOne">
    <property name="fred.bob.sammy" value="123" />
</bean>
```

`something` Bean有一个 `fred` 属性，它有一个 `bob` 属性，它有一个 `sammy` 属性，最后的 `sammy` 属性被设置为 `123` 的值。为了使这个方法奏效，`something` 的 `fred` 属性和 `fred` 的 `bob` 属性在构建 bean 后不能为 `null`。否则就会抛出一个 `NullPointerException`。

#### 关系指定

如果一个Bean是另一个Bean的依赖，这通常意味着一个Bean被设置为另一个Bean的一个属性。通常，你可以通过基于XML的配置元数据中的 [`` 元素](https://springdoc.cn/spring/core.html#beans-ref-element) 来实现这一点。然而，有时Bean之间的依赖关系并不那么直接。一个例子是当一个类中的静态初始化器需要被触发时，比如数据库驱动程序的注册。`depends-on` 属性可以明确地强制一个或多个Bean在使用此元素的Bean被初始化之前被初始化。下面的例子使用 `depends-on` 属性来表达对单个 bean 的依赖性。 s

```xml
<bean id="beanOne" class="ExampleBean" depends-on="manager"/>
<bean id="manager" class="ManagerBean" />
```

要表达对多个Bean的依赖，请提供一个Bean名称的列表作为 `depends-on` 属性的值（逗号、空格和分号是有效的分隔符）。

```xml
<bean id="beanOne" class="ExampleBean" depends-on="manager,accountDao">
    <property name="manager" ref="manager" />
</bean>

<bean id="manager" class="ManagerBean" />
```

> `depends-on` 属性可以指定初始化时间的依赖关系，而在 [单例](https://springdoc.cn/spring/core.html#beans-factory-scopes-singleton) Bean的情况下，也可以指定相应的销毁时间的依赖关系。与给定Bean定义了 `depends-on` 的依赖Bean会在给定Bean本身被销毁之前被首先销毁。因此，`depends-on` 也可以控制关闭的顺序。

#### 懒加载

默认情况下，`ApplicationContext` 的实现会急切地创建和配置所有的 [单例](https://springdoc.cn/spring/core.html#beans-factory-scopes-singleton) Bean，作为初始化过程的一部分。一般来说，这种预实例化是可取的，因为配置或周围环境中的错误会立即被发现，而不是几小时甚至几天之后。当这种行为不可取时，你可以通过将Bean定义标记为懒加载来阻止单例Bean的预实例化。懒加载的 bean 告诉IoC容器在第一次被请求时创建一个bean实例，而不是在启动时。

在XML中，这种行为是由 `<bean/>` 元素上的 `lazy-init` 属性控制的，如下例所示。

```xml
<bean id="lazy" class="com.something.ExpensiveToCreateBean" lazy-init="true"/>
<bean name="not.lazy" class="com.something.AnotherBean"/>
```

当前面的配置被 `ApplicationContext` 消耗时，当 `ApplicationContext` 启动时，`lazy` Bean不会被急切地预实化，而 `not.lazy` Bean则被急切地预实化了。

然而，当懒加载Bean是未被懒加载的单例Bean的依赖关系时，`ApplicationContext` 会在启动时创建懒加载 Bean，因为它必须满足单例的依赖关系。懒加载的 Bean 被注入到其他没有被懒加载的单例 Bean中。

你也可以通过使用 `<beans/>` 元素上的 `default-lazy-init` 属性来控制容器级的懒加载，如下例所示。

```xml
<beans default-lazy-init="true">
    <!-- no beans will be pre-instantiated... -->
</beans>
```

#### 自动注入

Spring容器可以自动连接协作Bean之间的关系。

你可以让Spring通过检查 `ApplicationContext` 的内容为你的Bean自动解决协作者（其他Bean）。

+ 自动注入可以大大减少对指定属性或构造函数参数的需要。
+  自动注入可以随着你的对象的发展而更新配置，自动在开发过程中可能特别有用，而不会否定在代码库变得更加稳定时切换到显式注入的选择。
  + 例如，如果你需要给一个类添加一个依赖，这个依赖可以自动满足，而不需要你修改配置。

当使用基于XML的配置元数据时，你可以用 `<bean/>` 元素的 `autowire` 属性来指定bean定义的自动注入模式。自动注入功能有四种模式。你可以为每个Bean指定自动注入，从而选择哪些要自动注入。

**注入模式**

- no（默认模式）：没有自动注入，所有Bean引用必须使用`<ref>`元素明确定义。这种方式提供了更大的控制力和清晰度。
- byName：通过属性名称自动注入，Spring会寻找与需要自动注入的属性同名的Bean定义。
- byType：根据属性的类型自动注入。如果容器中有且仅有一个匹配类型的Bean，它将被自动注入，否则会抛出异常。
- constructor：类似byType，但用于构造函数参数。如果没有匹配类型的Bean，将产生一个致命错误。

byType或constructor自动注入模式还可以用于数组和泛型集合的注入。如果预期的类型是String，你可以自动注入强类型的Map实例，其中值由所有符合预期类型的Bean实例组成，而键包含相应的Bean名称。

##### 限制和缺点

当自动注入在整个项目中被一致使用时，它的效果最好。如果自动注入没有被普遍使用，那么只用它来注入一个或两个Bean定义可能会让开发者感到困惑。

考虑自动注入的限制和弊端。

- `property` 和 `constructor-arg` 设置中的明确依赖关系总是覆盖自动注入。你不能自动注入简单的属性，如基本数据、`String` 和 `Class`（以及此类简单属性的数组）。这个限制是设计上的。
- 自动注入不如显式注入精确。尽管正如前面的表格中所指出的，Spring很小心地避免在模糊不清的情况下进行猜测，这可能会产生意想不到的结果。你的Spring管理的对象之间的关系不再被明确地记录下来。
- 对于可能从Spring容器中生成文档的工具来说，注入信息可能无法使用。
- 容器中的多个Bean定义可以与setter方法或构造参数指定的类型相匹配，以实现自动注入。对于数组、集合或 `Map` 实例，这不一定是个问题。然而，对于期待单一值的依赖关系，这种模糊性不会被任意地解决。如果没有唯一的Bean定义，就会抛出一个异常。

在后一种情况下，你有几种选择。

- 放弃自动注入，改用明确注入。
- 通过将bean定义的 `autowire-candidate` 属性设置为 `false` 来避免bean定义的自动注入，
- 通过将 `<bean/>` 元素的 `primary` 属性设置为 `true`，将单个Bean定义指定为主要候选者。

##### 排除项

在每个bean的基础上，你可以将一个bean排除在自动注入之外。在Spring的XML格式中，将 `<bean/>` 元素的 `autowire-candidate` 属性设置为 `false`。容器使特定的Bean定义对自动注入基础设施不可用（包括注解式配置，如 `@Autowired`）。

> `autowire-candidate` 属性被设计为只影响基于类型的自动注入。它不影响通过名称的显式引用，即使指定的 bean 没有被标记为 autowire 候选者，它也会被解析。因此，如果名称匹配，通过名称进行的自动注入还是会注入一个Bean。

你也可以根据对Bean名称的模式匹配来限制autowire候选人。顶层的 `<beans/>` 元素在其 `default-autowire-candidates` 属性中接受一个或多个模式。例如，要将自动注入候选状态限制在名称以 `Repository` 结尾的任何 bean，请提供 `*Repository` 的值。要提供多个模式，请用逗号分隔的列表定义它们。

Bean定义的 `autowire-candidate` 属性的明确值为 `true` 或 `false`，总是优先考虑。对于这样的Bean，模式匹配规则并不适用。

这些技术对于那些你永远不想通过自动注入注入到其他 Bean 中的 Bean 是很有用的。这并不意味着排除在外的 Bean 本身不能通过使用 autowiring 进行配置。相反，Bean本身不是自动注入其他 Bean 的候选人。

#### 方法注入

在大多数应用场景中，容器中的大多数Bean是 [单例](https://springdoc.cn/spring/core.html#beans-factory-scopes-singleton)。

当一个单例Bean需要与另一个单例Bean协作或一个非单例Bean需要与另一个非单例Bean协作时，你通常通过将一个Bean定义为另一个Bean的一个属性来处理这种依赖关系。当Bean的生命周期不同时，问题就出现了。

假设单例Bean A需要使用非单例（prototype）Bean B，也许是在A的每个方法调用上。容器只创建一次单例Bean A，因此只有一次机会来设置属性。容器不能在每次需要Bean B的时候为Bean A提供一个新的实例。

解决这个问题的方法是，允许一个单例Bean在需要时从容器中获取另一个Bean的新实例。

这可以通过在单例Bean中实现`ApplicationContextAware`接口来实现。`ApplicationContextAware`接口要求Bean实现`setApplicationContext`方法，这样它可以获取应用程序的上下文。

然后，当单例Bean需要与其他Bean协作时，它可以通过应用程序上下文请求（通常通过`getBean`方法）新的实例。

```java
package fiona.apple;

// Spring-API导入
import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;

/**
 * 一个使用有状态的Command风格类执行一些处理的类。
 */
public class CommandManager implements ApplicationContextAware {

    // 这个字段用来保存应用程序上下文
    private ApplicationContext applicationContext;

    // 这个方法负责处理具体的业务逻辑
    public Object process(Map commandState) {
        // 创建一个新的Command实例
        Command command = createCommand();
        // 设置Command实例的状态
        command.setState(commandState);
        // 执行Command并返回结果
        return command.execute();
    }

    // 这个方法用来获取Command实例
    protected Command createCommand() {
        // 使用应用程序上下文获取名为"command"的Bean，并将其返回
        return this.applicationContext.getBean("command", Command.class);
    }

    // 这个方法由ApplicationContextAware接口定义，用来设置应用程序上下文
    public void setApplicationContext(
            ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }
}

```

在这个示例中，`CommandManager`类实现了`ApplicationContextAware`接口，这允许它获取应用程序上下文的引用。

当`process`方法需要创建一个新的`Command`实例时，它会调用`createCommand`方法。`createCommand`方法使用应用程序上下文获取名为"command"的Bean，并返回一个新的`Command`实例。

##### 查找方法注入

查询方法注入是指容器能够覆盖容器管理的 bean 上的方法并返回容器中另一个命名的 bean 的查询结果。这种查找通常涉及到一个原型（prototype）Bean。

它允许一个 Bean 在运行时通过查找方法来获取对其他 Bean 的引用，而不是在初始化时通过注入属性或构造函数来建立依赖关系。

Spring框架通过使用 CGLIB 库的字节码生成来实现这种方法注入，动态地生成一个覆盖该方法的子类。

> - 为了实现此动态子类化功能，被 Spring bean 容器子类化的类不能为 `final`，要被复写的方法也不能为 `final`。
> - 对一个包含 `abstract` 方法的类进行单元测试，需要你自己对这个类进行子类化，并提供一个 `abstract` 方法的 stub 实现。
> - 体的方法对于组件扫描也是必要的，这需要具体的类来接续。
> - 另一个关键的限制是，查找方法对工厂方法不起作用，特别是对配置类中的 `@Bean` 方法不起作用，因为在这种情况下，容器不负责创建实例，因此不能即时创建运行时生成的子类。

在前面代码片段中的 `CommandManager` 类的情况下，Spring容器动态地覆写了 `createCommand()` 方法的实现。`CommandManager` 类没有任何Spring的依赖，正如重写的例子所示。

```java
package fiona.apple;

// no more Spring imports!

public abstract class CommandManager {

    public Object process(Object commandState) {
        // grab a new instance of the appropriate Command interface
        Command command = createCommand();
        // set the state on the (hopefully brand new) Command instance
        command.setState(commandState);
        return command.execute();
    }

    // okay... but where is the implementation of this method?
    protected abstract Command createCommand();
}
```

在包含要注入的方法的客户端类中（本例中是 `CommandManager`），要注入的方法需要一个如下形式的签名。

```xml
<public|protected> [abstract] <return-type> theMethodName(no-arguments);
```

如果这个方法是 `abstract` 的，动态生成的子类实现这个方法。否则，动态生成的子类将覆盖原始类中定义的具体方法。考虑一下下面的例子。

```xml
<!-- a stateful bean deployed as a prototype (non-singleton) -->
<bean id="myCommand" class="fiona.apple.AsyncCommand" scope="prototype">
    <!-- inject dependencies here as required -->
</bean>

<!-- commandProcessor uses statefulCommandHelper -->
<bean id="commandManager" class="fiona.apple.CommandManager">
    <lookup-method name="createCommand" bean="myCommand"/>
</bean>
```

每当需要一个新的 `myCommand` Bean的实例时，被识别为 `commandManager` 的bean就会调用它自己的 `createCommand()` 方法。

你必须注意将 `myCommand` Bean部署为一个原型（prototype），如果这确实是需要的。如果它是一个 [单例](https://springdoc.cn/spring/core.html#beans-factory-scopes-singleton)，每次都会返回同一个 `myCommand` Bean实例。

另外，在基于注解的组件模型中，你可以通过 `@Lookup` 注解来声明一个查找方法，如下例所示。

```java
public abstract class CommandManager {

    public Object process(Object commandState) {
        Command command = createCommand();
        command.setState(commandState);
        return command.execute();
    }

    @Lookup("myCommand")
    protected abstract Command createCommand();
}
```

或者，更习惯性的是，你可以依靠目标Bean对查找方法的声明返回类型进行解析。

```java
public abstract class CommandManager {

    public Object process(Object commandState) {
        Command command = createCommand();
        command.setState(commandState);
        return command.execute();
    }

    @Lookup
    protected abstract Command createCommand();
}
```

请注意，你通常应该用具体的 stub 实现来声明这种注解的查找方法，以使它们与Spring的组件扫描规则兼容，因为抽象类会被默认忽略。这一限制并不适用于明确注册或明确导入的Bean类。

> 访问不同 scope 的目标Bean的另一种方式是 `ObjectFactory`/`Provider` 注入点。
>
> 你可能还会发现 `ServiceLocatorFactoryBean`（在 `org.springframework.beans.factory.config` 包中）很有用。







































## AOP

面向切面的编程（AOP）是一种补充面向对象的编程（OOP）的方式，通过提供不同的思考程序结构的方式；在OOP中，模块化的关键单位是类，而在AOP中，模块化的单位是切面（Aspect）。

![img](D:\Repository\Typora\v2-a17604edbf3b70b4eae50aa87d1772ee_720w.webp)

切面允许将关注点（如事务管理）模块化，这些关注点横跨多个不同类型和对象。这些横跨多个组件的关注点在AOP文献中通常被称为 "交叉（crosscutting）" 关注点。

AOP 主要用来解决：在不改变原有业务逻辑的情况下，增强横切逻辑代码，根本上解耦合，避免横切逻辑代码重复。

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/11/30/167652d47de5c4c4~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)

**切** ：指的是横切逻辑，原有业务逻辑代码不动，只能操作横切逻辑代码，所以面向横切逻辑。

**面** ：横切逻辑代码往往要影响的是很多个方法，每个方法如同一个点，多个点构成一个面。这里有一个面的概念。

Spring框架的一个关键组件是AOP框架。虽然Spring IoC容器不依赖于AOP（也就是说，你可以选择不使用AOP），但AOP提供了一个强大的中间件解决方案，可以与Spring IoC容器集成，以实现各种横切关注点。

Spring的AOP框架支持两种主要的风格，一种是基于schema的方法，另一种是使用`@AspectJ`注解风格。这两种风格都提供了强大的自定义切面编写方式，使用了AspectJ的pointcut语言，并结合了Spring AOP进行织入。

AOP在Spring框架中的应用包括以下方面：

- 提供声明式的企业服务，其中最重要的是声明式事务管理。
- 允许用户实现自定义切面，以补充他们对OOP的使用。

### 相关概念

- **切面（Aspect）**：切面是模块化的关注点，它跨越多个类的功能。例如，事务管理是一个企业级Java应用程序中的横切关注点的例子。在Spring AOP中，切面可以通过常规类（基于[基于schema的方法](https://springdoc.cn/spring/core.html#aop-schema)）或`@Aspect`注解的常规类（[AspectJ风格](https://springdoc.cn/spring/core.html#aop-ataspectj)）来实现。
- **连接点（Join point）**：程序执行过程中的一个点，例如方法的执行或异常的处理。在Spring AOP中，连接点通常代表方法的执行。
- **通知（Advice）**：通知是切面在特定连接点上采取的行动。通知可以分为不同类型，包括"before"、"after returning"和"after (finally)"等。Spring AOP通常将通知建模为拦截器，它们维护在连接点周围执行的拦截器链。
- **切点（Pointcut）**：切点是一个谓词，用于匹配连接点。通知与切点表达式相关联，并在匹配的连接点上运行，例如执行具有特定名称的方法。Spring默认使用AspectJ的切点表达式语言。
- **引入（Introduction）**：引入允许为一个对象引入新的接口或字段。Spring AOP允许通过引入来扩展bean的功能，例如使一个bean实现`IsModified`接口以实现缓存功能。
- **目标对象（Target object）**：目标对象是一个或多个切面通知的对象，也被称为"advised object"。因为Spring AOP是通过运行时代理来实现的，目标对象通常是被代理的对象。
- **AOP代理（AOP proxy）**：AOP代理是AOP框架创建的对象，用于实现切面的契约，例如通知方法的执行等。在Spring框架中，AOP代理可以是JDK动态代理或CGLIB代理。
- **织入（Weaving）**：织入是将切面与其他应用程序类型或对象连接起来，以创建一个通知对象。织入可以在编译时、加载时或运行时完成。Spring AOP与其他Java AOP框架一样，通常在运行时进行织入。

![image-20230317130835128](D:\Repository\Typora\image-20230317130835128.png)

Spring的AOP框架包括几种类型的通知，包括"before"、"after returning"、"after (finally)"和"around"等。"Around"通知是最强大的一种通知，因为它可以在方法调用前后执行自定义行为。通常，建议使用最具体的通知类型以实现所需的行为，以简化编程模型并减少潜在的错误。例如，如果只需要在方法返回时更新缓存，那么建议使用"after returning"通知，而不是"around"通知。

### 模型举例

要理解切面编程，就需要先理解什么是切面。用刀把一个西瓜分成两瓣，切开的切口就是切面；炒菜，锅与炉子共同来完成炒菜，锅与炉子就是切面。web层级设计中，web层->网关层->服务层->数据层，每一层之间也是一个切面。**编程中，对象与对象之间，方法与方法之间，模块与模块之间都是一个个切面。**

我们一般做活动的时候，一般对每一个接口都会做活动的有效性校验（是否开始、是否结束等等）、以及这个接口是不是需要用户登录。

按照正常的逻辑，我们可以这么做。
![这里写图片描述](D:\Repository\Typora\20180530172331597)

这有个问题就是，有多少接口，就要多少次代码copy。对于一个“懒人”，这是不可容忍的。好，提出一个公共方法，每个接口都来调用这个接口。这里有点切面的味道了。
![这里写图片描述](D:\Repository\Typora\20180530172427993)

同样有个问题，我虽然不用每次都copy代码了，但是，每个接口总得要调用这个方法吧。于是就有了切面的概念，我将方法注入到接口调用的某个地方（切点）。

![这里写图片描述](D:\Repository\Typora\20180530172528617)

这样接口只需要关心具体的业务，而不需要关注其他非该接口关注的逻辑或处理。**红框处，就是面向切面编程。**

下面我以一个简单的例子来比喻一下 AOP 中 `Aspect`， `Joint point`， `Pointcut` 与 `Advice`之间的关系。

假设有一个叫爪哇的小县城，在一个月黑风高的夜晚，这个县城发生了一起神秘的罪案。犯罪现场没有留下任何有用的线索，凶手行踪诡秘。但幸运的是，老王，在偶然间返回时目击了犯罪的过程。然而，由于天色昏暗，以及凶手蒙面，老王无法辨认凶手的身份，只知道凶手是个男性，身高大约七尺五寸。

爪哇县的县令收到了老王的描述，他下令对守卫士兵说：“凡是发现有身高七尺五寸的男性，都要立刻抓住并带到我这里审问。” 士兵不敢违抗县令的命令，于是他们将所有满足这一描述的人都带到了县令那里，准备进行审问。

- **Joint point（连接点）**：在这个比喻中，连接点相当于小县城的居民，也可以类比为程序执行中的各种方法执行点，其中每个人/方法都是一个潜在的连接点。
- **Pointcut（切点）**：切点就好比老王提出的指控，即凶手是个男性，身高约七尺五寸。Pointcut是一种描述，它修饰了连接点，就像老王的指控规定了可以被抓住审问的嫌疑人。Pointcut定义了哪些连接点将被织入Advice。
- **Advice（通知）**：Advice就是采取的行动，这里是“抓住并审问”。Advice是一段具体的行为，它会在满足切点规则的连接点上执行。在这个比喻中，Advice是士兵执行的任务，也就是抓住所有满足描述条件的人。
- **Aspect（切面）**：在这个比喻中，Aspect是整个动作，即"根据老王的线索，凡是发现有身高七尺五寸的男性，都要抓过来审问"。Aspect由Pointcut和Advice组合而成，描述了一种特定的横切关注点。

**最后是一个描述这些概念之间关系的图**：
![这里写图片描述](D:\Repository\Typora\20180530175605692)

### 能力

Spring AOP是用纯Java实现的。不需要特殊的编译过程。Spring AOP不需要控制类加载器的层次结构，因此适合在Servlet容器或应用服务器中使用。

Spring AOP目前只支持方法执行连接点（advice 在Spring Bean上执行方法）。字段拦截没有实现，尽管可以在不破坏Spring AOP核心API的情况下增加对字段拦截的支持。如果你需要advice字段访问和更新连接点，可以考虑使用AspectJ这样的语言。

Spring AOP的AOP方法与其他大多数AOP框架不同。其目的不是提供最完整的AOP实现（尽管Spring AOP的能力很强）。相反，其目的是在AOP实现和Spring IoC之间提供一个紧密的整合，以帮助解决企业应用中的常见问题。

因此，例如，Spring框架的AOP功能通常是与Spring IoC容器一起使用的。切面是通过使用正常的Bean定义语法来配置的（尽管这允许强大的 "自动代理" 功能）。这是与其他AOP实现的一个重要区别。你不能用Spring AOP轻松或有效地做一些事情，比如advice非常细粒度的对象（通常是domain对象）。在这种情况下，AspectJ是最好的选择。然而，我们的经验是，Spring AOP为企业级Java应用中的大多数问题提供了一个很好的解决方案，这些问题是可以用AOP来解决的。

Spring AOP从未努力与AspectJ竞争，以提供一个全面的AOP解决方案。我们认为，Spring AOP等基于代理的框架和AspectJ等完整的框架都很有价值，它们是互补的，而不是竞争的。Spring将Spring AOP和IoC与AspectJ无缝集成，以便在基于Spring的应用架构中实现AOP的所有用途。这种整合并不影响Spring AOP API或AOP联盟的API。Spring AOP仍然是向后兼容的。

> Spring框架的核心原则之一是非侵入性。这是一个想法，即你不应该被迫在你的业务或domain模型中引入框架特定的类和接口。然而，在某些地方，Spring框架确实让你选择将Spring框架特定的依赖关系引入你的代码库。
>
> 给你这样的选项的理由是，在某些情况下，用这样的方式来阅读或编码一些特定的功能可能会更容易。然而，Spring框架（几乎）总是为你提供选择：你可以自由地做出明智的决定，哪种选择最适合你的特定用例或情景。
>
> 与本章相关的一个选择是，选择哪种AOP框架（以及哪种AOP风格）。你可以选择AspectJ、Spring AOP或两者。你还可以选择 `@AspectJ` 注解式方法或Spring XML配置式方法。本章选择首先介绍 `@AspectJ` 风格的方法，这不应该被认为是Spring团队倾向于 `@AspectJ` 注解风格的方法，而不是Spring XML配置风格。

### 实现模式

实现 AOP 的技术，主要分为两大类：

- 静态代理 - 指使用 AOP 框架提供的命令进行编译，从而在编译阶段就可生成 AOP 代理类，因此也称为编译时增强；
  - 编译时编织（特殊编译器实现）
  - 类加载时编织（特殊的类加载器实现）。
- 动态代理 - 在运行时在内存中“临时”生成 AOP 动态代理类，因此也被称为运行时增强。
  - `JDK` 动态代理：通过反射来接收被代理的类，并且要求被代理的类必须实现一个接口 。JDK 动态代理的核心是 InvocationHandler 接口和 Proxy 类 。
  - `CGLIB`动态代理： 如果目标类没有实现接口，那么 `Spring AOP` 会选择使用 `CGLIB` 来动态代理目标类 。`CGLIB` （ Code Generation Library ），是一个代码生成的类库，可以在运行时动态的生成某个类的子类，注意， `CGLIB` 是通过继承的方式做的动态代理，因此如果某个类被标记为 `final` ，那么它是无法使用 `CGLIB` 做动态代理的。

Spring AOP 基于动态代理方式实现；AspectJ 基于静态代理方式实现。 

Spring AOP 仅支持方法级别的 PointCut；提供了完全的 AOP 支持，它还支持属性级别的 PointCut。

### AOP 代理

Spring AOP默认使用标准的JDK动态代理进行AOP代理。这使得任何接口（或一组接口）都可以被代理。

Spring AOP也可以使用CGLIB代理。这对于代理类而不是接口来说是必要的。默认情况下，如果一个业务对象没有实现一个接口，就会使用CGLIB。

由于对接口而不是类进行编程是很好的做法，业务类通常实现一个或多个业务接口。在那些（希望是罕见的）需要向没有在接口上声明的方法提供advice的情况下，或者在需要将代理对象作为具体类型传递给方法的情况下，可以 [强制使用 CGLIB](https://springdoc.cn/spring/core.html#aop-proxying)。

#### `AspectJ`

`@AspectJ` 指的是将aspect作为带有注解的普通Java类来声明的一种风格。`@AspectJ` 风格是由 [AspectJ项目](https://www.eclipse.org/aspectj) 引入的，作为AspectJ 5版本的一部分。Spring解释了与AspectJ 5相同的注解，使用AspectJ提供的库进行 pointcut 解析和匹配。不过，AOP运行时仍然是纯粹的Spring AOP，而且不依赖AspectJ编译器或织入器（weaver）。

##### 启动

要在Spring配置中使用 `@AspectJ` 切面，你需要启用Spring支持，以根据 `@AspectJ` 切面配置Spring AOP，并根据Bean是否被这些切面advice而自动代理。通过自动代理，我们的意思是，如果Spring确定一个Bean被一个或多个切面所advice，它就会自动为该Bean生成一个代理来拦截方法调用，并确保 advice 在需要时被运行。

`@AspectJ` 支持可以通过XML或Java风格的配置来启用。在这两种情况下，您还需要确保AspectJ的 `aspectjweaver.jar` 库在你应用程序的classpath上（1.9或更高版本）。该库可在AspectJ发行版的 `lib` 目录中找到，也可从Maven Central仓库中找到。

###### 用Java配置启用 @AspectJ 的支持

要用Java `@Configuration` 启用 `@AspectJ` 支持，请添加 `@EnableAspectJAutoProxy` 注解，如下例所示。

```java
@Configuration
@EnableAspectJAutoProxy
public class AppConfig {
}
```

###### 用XML配置启用 @AspectJ 的支持

要用基于XML的配置启用 `@AspectJ` 支持，请使用 `aop:aspectj-autoproxy` 元素，如下例所示。

```xml
<aop:aspectj-autoproxy/>
```

这假定你使用了 [基于XML Schema的配置](https://springdoc.cn/spring/core.html#core.appendix.xsd-schemas)中描述的模式支持。关于如何导入 `aop` 命名空间中的标签，请参见 [AOP schema](https://springdoc.cn/spring/core.html#core.appendix.xsd-schemas-aop)。

##### 声明 Aspect

启用 `@AspectJ` 支持后，任何在你的 application context 中定义的bean，其类是 `@AspectJ` 切面（有 `@Aspect` 注解），会被Spring自动检测到，并用于配置Spring AOP。接下来的两个例子展示了一个不怎么有用的切面所需的最小步骤。

两个例子中的第一个显示了 application context 中的一个普通Bean定义，它指向一个用 `@Aspect` 注解的Bean类。

```xml
<bean id="myAspect" class="com.xyz.NotVeryUsefulAspect">
    <!-- configure properties of the aspect here -->
</bean>
```

两个例子中的第二个展示了 `NotVeryUsefulAspect` 类的定义，它被 `@Aspect` 注解了。

```java
package com.xyz;

import org.aspectj.lang.annotation.Aspect;

@Aspect
public class NotVeryUsefulAspect {
}
```

Aspects（用 `@Aspect` 注解的类）可以有方法和字段，和其他类一样。它们也可以包含pointcut、advice和引入（间型的）声明。

> 你可以通过 `@Configuration` 类中的 `@Bean` 方法，将aspect类注册为Spring XML配置中的普通Bean，或者让Spring通过classpath扫描来自动检测它们—与任何其他Spring管理的Bean一样。然而，请注意，`@Aspect` 注解并不足以实现classpath中的自动检测。为此，你需要添加一个单独的 `@Component` 注解（或者，根据Spring组件扫描器的规则，添加一个符合条件的自定义 stereotype 注解）

> 在Spring AOP中，切面本身不能成为其他切面的advice的目标。类上的 `@Aspect` 注解将其标记为一个切面，因此，将其排除在自动代理之外。

##### 声明 Pointcut

Pointcuts确定感兴趣的连接点（join points），从而使我们能够控制 advice 的运行时间。

Spring AOP只支持Spring Bean的方法执行连接点，所以你可以把pointcut看作是对Spring Bean上的方法执行的匹配。

一个切点声明有两个部分：一个由名称和任何参数组成的签名，以及一个切点表达式，它决定了我们到底对哪些方法的执行感兴趣。

在AOP的 `@AspectJ` 注解式中，一个pointcut签名是由一个常规的方法定义提供的，而pointcut表达式是通过使用 `@Pointcut` 注解来表示的（作为pointcut签名的方法必须是一个 `void` 返回类型）。

一个例子可以帮助我们清楚地了解切点签名和切点表达式之间的区别。下面的例子定义了一个名为 `anyOldTransfer` 的切点，它匹配任何名为 `transfer` 的方法的执行。

```java
@Pointcut("execution(* transfer(..))") // the pointcut expression
private void anyOldTransfer() {} // the pointcut signature
```

构成 `@Pointcut` 注解的值的 pointcut 表达式是一个常规的AspectJ pointcut表达式。关于AspectJ的 pointcut 语言的全面讨论，请参见 [《AspectJ编程指南》](https://www.eclipse.org/aspectj/doc/released/progguide/index.html)（以及 [AspectJ 5开发者笔记](https://www.eclipse.org/aspectj/doc/released/adk15notebook/index.html) 的扩展）或关于AspectJ的书籍之一（如Colyer等人的《Eclipse AspectJ》，或Ramnivas Laddad的《AspectJ in Action》）。

**支持的 `Pointcut` 指定器**

Spring AOP支持以下AspectJ的切点指定器（PCD），用于切点表达式中。

- `execution`: 用于匹配方法执行的连接点。这是在使用Spring AOP时要使用的主要切点指定器。
- `within`: 将匹配限制在某些类型内的连接点（使用Spring AOP时，执行在匹配类型内声明的方法）。
- `this`: 将匹配限制在连接点（使用Spring AOP时方法的执行），其中bean引用（Spring AOP代理）是给定类型的实例。
- `target`: 将匹配限制在连接点（使用Spring AOP时方法的执行），其中目标对象（被代理的应用程序对象）是给定类型的实例。
- `args`: 将匹配限制在连接点（使用Spring AOP时方法的执行），其中参数是给定类型的实例。
- `@target`: 限制匹配到连接点（使用Spring AOP时方法的执行），其中执行对象的类有一个给定类型的注解。
- `@args`: 将匹配限制在连接点（使用Spring AOP时方法的执行），其中实际传递的参数的运行时类型有给定类型的注解。
- `@within`: 将匹配限制在具有给定注解的类型中的连接点（使用Spring AOP时，执行在具有给定注解的类型中声明的方法）。
- `@annotation`: 将匹配限制在连接点的主体（Spring AOP中正在运行的方法）具有给定注解的连接点上。

> 完整的AspectJ点式语言支持Spring不支持的其他点式指定符：`call`、`get`、`set`、`preinitialization`、`staticinitialization`、`initialization`、`handler`、`adviceexecution`、`withincode`、`cflow`、`cflowbelow`、`if`、`@this` 和 `@withincode`。在由Spring AOP解释的pointcut表达式中使用这些 pointcut 指定器会导致抛出 `IllegalArgumentException`。
>
> Spring AOP支持的切点指定器集合可能会在未来的版本中扩展，以支持更多的AspectJ切点指定器。

因为Spring AOP将匹配限制在方法的执行连接点上，所以前面对切点（pointcut）指定器的讨论给出的定义比你在AspectJ编程指南中找到的要窄。

此外，AspectJ本身有基于类型的语义，在执行连接点，`this` 和 `target` 都是指同一个对象：执行方法的对象。Spring AOP是一个基于代理的系统，区分了代理对象本身（与 `this` 绑定）和代理背后的目标对象（与 `target` 绑定）。

> 由于Spring的AOP框架是基于代理的，根据定义，目标对象内部的调用是不会被拦截的。对于JDK代理，只有代理上的public接口方法调用可以被拦截。使用CGLIB，代理上的public和protected的方法调用都可以被拦截（如果有必要，甚至可以拦截包中的可见方法）。然而，通过代理的普通交互应该始终通过public签名来设计。
>
> 请注意，切点（pointcut）定义通常与任何拦截的方法相匹配。如果一个切点是严格意义上的只对public开放的，即使是在CGLIB代理场景下，通过代理进行潜在的非public交互，它也需要被相应地定义。
>
> 如果你的拦截需求包括目标类中的方法调用甚至构造函数，可以考虑使用Spring驱动的 [原生 AspectJ 织入](https://springdoc.cn/spring/core.html#aop-aj-ltw)，而不是Spring的基于代理的AOP框架。这构成了不同的AOP使用模式，具有不同的特点，所以在做决定之前，一定要让自己熟悉织入。

Spring AOP还支持一个额外的名为 `bean` 的PCD。这个PCD可以让你把连接点的匹配限制在一个特定的命名的Spring Bean或命名的Spring Bean的集合（当使用通配符时）。`bean` PCD有以下形式。

```
bean(idOrNameOfBean)
```

`idOrNameOfBean` 标记可以是任何Spring Bean的名称。我们提供了使用 `*` 字符的有限通配符支持，因此，如果你为你的Spring Bean建立了一些命名惯例，你可以写一个 `bean` PCD表达式来选择它们。就像其他的pointcut 指定器一样，Bean PCD也可以和 `&&`（和）、`||`（或）、以及 `!` （否定） 操作符一起使用。

> 在Spring AOP中支持 `bean` PCD，而在原生的AspectJ织入中不支持。它是AspectJ定义的标准PCD的Spring特定扩展，因此不适用于 `@Aspect` 模型中声明的切面。
>
> `bean` PCD在实例级（建立在Spring Bean名称概念的基础上）而不是仅在类型级（基于织入的AOP仅限于此）进行操作。基于实例的指向指定器是Spring基于代理的AOP框架的特殊能力，它与Spring Bean Factory紧密结合，通过名称来识别特定的bean是很自然和直接的。

##### 声明 Advice

Advice 与一个切点表达式相关联，在切点匹配的方法执行之前、之后或周围（around）运行。切点表达式可以是一个内联切点，也可以是对一个 [命名切点](https://springdoc.cn/spring/core.html#aop-common-pointcuts) 的引用。

###### Before Advice

你可以通过使用 `@Before` 注解在一个切面中声明 before advice。

下面的例子使用了一个内联的切点表达式。

```java
@Aspect
public class BeforeExample {

    @Before("execution(* com.xyz.dao.*.*(..))")
    public void doAccessCheck() {
        // ...
    }
}
```

如果我们使用一个 [命名的切点](https://springdoc.cn/spring/core.html#aop-common-pointcuts)，我们可以把前面的例子改写成如下。

```java
@Aspect
public class BeforeExample {

    @Before("com.xyz.CommonPointcuts.dataAccessOperation()")
    public void doAccessCheck() {
        // ...
    }
}
```

###### After Returning Advice

当一个匹配的方法执行正常返回时，After returning advice 运行。你可以通过使用 `@AfterReturning` 注解来声明它。

```java
@Aspect
public class AfterReturningExample {

    @AfterReturning("execution(* com.xyz.dao.*.*(..))")
    public void doAccessCheck() {
        // ...
    }
}
```

> 你可以有多个 advice 声明（也可以有其他成员），都在同一个切面。我们在这些例子中只展示了一个 advice 声明，以集中展示每个 advice 的效果。

有时，你需要在 advice body 中访问被返回的实际值。你可以使用绑定返回值的 `@AfterReturning` 的形式来获得这种访问权，正如下面的例子所示。

```java
@Aspect
public class AfterReturningExample {

    @AfterReturning(
        pointcut="execution(* com.xyz.dao.*.*(..))",
        returning="retVal")
    public void doAccessCheck(Object retVal) {
        // ...
    }
}
```

`returning` 属性中使用的名称必须与advice方法中的参数名称相对应。当一个方法执行返回时，返回值会作为相应的参数值传递给advice方法。`returning` 子句也限制了匹配，只匹配那些返回指定类型的值的方法执行（在这种情况下是 `Object`，它匹配任何返回值）。

###### After Throwing Advice

当一个匹配的方法执行通过抛出异常退出时，After throwing advice 运行。你可以通过使用 `@AfterThrowing` 注解来声明它，如下例所示。

```java
@Aspect
public class AfterThrowingExample {

    @AfterThrowing("execution(* com.xyz.dao.*.*(..))")
    public void doRecoveryActions() {
        // ...
    }
}
```

通常情况下，你希望advice只在给定类型的异常被抛出时运行，而且你也经常需要在advice body中访问被抛出的异常。你可以使用 `throwing` 属性来限制匹配（如果需要的话—否则使用 `Throwable` 作为异常类型），并将抛出的异常绑定到 advice 参数上。下面的例子展示了如何做到这一点。

```java
@Aspect
public class AfterThrowingExample {

    @AfterThrowing(
        pointcut="execution(* com.xyz.dao.*.*(..))",
        throwing="ex")
    public void doRecoveryActions(DataAccessException ex) {
        // ...
    }
}
```

在 `throwing` 属性中使用的名称必须与advice方法中的参数名称相对应。当一个方法的执行通过抛出一个异常退出时，该异常将作为相应的参数值传递给advice方法。`throwing` 子句也限制了匹配，只能匹配那些抛出指定类型的异常的方法执行（本例中是 `DataAccessException`）。

> 注意，`@AfterThrowing` 并不表示一般的异常处理回调。具体来说，`@AfterThrowing` advice方法只应该接收来自连接点（用户声明的目标方法）本身的异常，而不是来自附带的 `@After`/`@AfterReturning` 方法。

###### After (Finally) Advice

当一个匹配的方法执行退出时，After (finally) advice 会运行。它是通过使用 `@After` 注解来声明的。After advice必须准备好处理正常和异常的返回条件。它通常被用于释放资源和类似的目的。下面的例子展示了如何使用 After finally advice。

```java
@Aspect
public class AfterFinallyExample {

    @After("execution(* com.xyz.dao.*.*(..))")
    public void doReleaseLock() {
        // ...
    }
}
```

> 请注意，AspectJ中的 `@After` advice 被定义为 "after finally advice"，类似于try-catch语句中的finally块。它将对任何结果、正常返回或从连接点（用户声明的目标方法）抛出的异常进行调用，这与 `@AfterReturning` 不同，后者只适用于成功的正常返回。

###### Around Advice

最后一种 Advice 是Around Advice。 "围绕" 一个匹配的方法的执行而运行。它有机会在方法运行之前和之后进行工作，并决定何时、如何、甚至是否真正运行该方法。如果你需要以线程安全的方式分享方法执行前后的状态，例如启动和停止一个定时器，那么 Around advice 经常被使用。

> 始终使用符合你要求的最不强大的 advice 形式。例如，如果 before advice 足以满足你的需要，就不要使用 around advice。

Around advice 是通过用 `@Around` 注解来声明一个方法的。该方法应该声明 `Object` 为其返回类型，并且该方法的第一个参数必须是 `ProceedingJoinPoint` 类型。在 advice 方法的body中，你必须在 `ProceedingJoinPoint` 上调用 `proceed()`，以使底层方法运行。在没有参数的情况下调用 `proceed()` 将导致调用者的原始参数在底层方法被调用时被提供给它。对于高级用例，有一个重载的 `proceed()` 方法，它接受一个参数数组（`Object[]`）。当底层方法被调用时，数组中的值将被用作该方法的参数。

> 当用 `Object[]` 调用时， `proceed` 的行为与 AspectJ 编译器编译的around advice的行为有些不同。对于使用传统AspectJ语言编写的around advice，传递给 `Proceed` 的参数数必须与传递给 around advice 的参数数相匹配（而不是底层连接点的参数数），并且在给定的参数位置传递给 `Proceed` 的值将取代该值所绑定的实体在连接点的原始值（如果现在这没有意义，请不要担心）。Spring所采取的方法更简单，也更符合其基于代理、只执行的语义。只有当你编译为Spring编写的 `@AspectJ` 切面并使用AspectJ编译器和织入器的参数进行 `proceed` 时，你才需要意识到这种差异。有一种方法可以在Spring AOP和AspectJ之间100%兼容地编写这些切面，这将在 [下面关于 advice 参数的章节](https://springdoc.cn/spring/core.html#aop-ataspectj-advice-proceeding-with-the-call) 中讨论。

around advice 返回的值是方法的调用者看到的返回值。例如，一个简单的缓存切面可以从缓存中返回一个值（如果有的话），或者调用 `proceed()` （并返回该值），如果没有的话。请注意， `proceed` 可以被调用一次，多次，或者根本就不在 around advice 的 body 中调用。所有这些都是合法的。

> 如果你将 around advice 方法的返回类型声明为 `void`，那么将总是返回给调用者 `null`，有效地忽略了任何调用 `proceed()` 的结果。因此，我们建议 around advice 方法声明一个 `Object` 的返回类型。该advice方法通常应该返回调用 `proceed()` 所返回的值，即使底层方法的返回类型为 `void`。然而，advice可以根据使用情况选择性地返回一个缓存的值、一个封装的值或一些其他的值。

下面的例子显示了如何使用 around advice。

```java
@Aspect
public class AroundExample {

    @Around("execution(* com.xyz..service.*.*(..))")
    public Object doBasicProfiling(ProceedingJoinPoint pjp) throws Throwable {
        // start stopwatch
        Object retVal = pjp.proceed();
        // stop stopwatch
        return retVal;
    }
}
```

##### Advice 参数

Spring提供了完全类型化的advice，这意味着你可以在advice签名中声明你需要的参数（就像我们在前面看到的返回和抛出的例子一样），而不是一直用 `Object[]` 数组工作。我们将在本节后面看到如何使参数和其他上下文值对advice主体可用。首先，我们看一下如何编写通用advice，它可以找出advice当前所advice的方法。

###### 访问当前的 `JoinPoint`

任何 advice method 都可以声明一个 `org.aspectj.lang.JoinPoint` 类型的参数作为其第一个参数。请注意，around advice 方法需要声明一个 `ProceedingJoinPoint` 类型的第一个参数，它是 `JoinPoint` 的一个子类。

`JoinPoint` 接口提供了许多有用的方法。

- `getArgs()`: 返回方法的参数。
- `getThis()`: 返回代理对象。
- `getTarget()`: 返回目标对象。
- `getSignature()`: 返回正在被 advice 的方法的描述。
- `toString()`: 打印对所 advice 的方法的有用描述。

##### Advice 顺序

当多个advice都想在同一个连接点上运行时会发生什么？

Spring AOP遵循与AspectJ相同的优先级规则来决定advice的执行顺序。优先级最高的advice在 "进入" 时首先运行（因此，给定两个 before advice，优先级最高的那个先运行）。

从一个连接点 "出去" 时，优先级最高的advice最后运行（所以，给定两个 after advice，优先级最高的advice将第二运行）。

当两个定义在不同切面的advice都需要在同一个连接点运行时，除非你另外指定，否则执行的顺序是不确定的。

你可以通过指定优先级来控制执行的顺序。这可以通过在切面类中实现 `org.springframework.core.Ordered` 接口或用 `@Order` 注解来完成，这是正常的Spring方式。给定两个切面，从 `Ordered.getOrder()` 返回较低值的切面（或注解值）具有较高的优先权。

> 一个特定切面的每个不同的advice类型在概念上都是为了直接适用于连接点。因此， `@AfterThrowing` 建议方法不应该从伴随的 `@After` / `@AfterReturning` 方法中接收一个异常。
>
> 从Spring Framework 5.2.7开始，在同一 `@Aspect` 类中定义的、需要在同一连接点运行的advice方法会根据其advice类型被分配优先权，优先权从高到低的顺序如下。`@Around`, `@Before`, `@After`, `@AfterReturning`, `@AfterThrowing`。
>
> 然而，请注意，`@After` advice 方法将在同一切面的任何 `@AfterReturning` 或 `@AfterThrowing` advice方法之后有效地被调用，遵循AspectJ对 `@After` 的 "after finally advice" 语义。
>
> 当在同一个 `@Aspect` 类中定义的两块相同类型的advice（例如，两个 `@After` advice方法）都需要在同一个连接点运行时，排序是未定义的（因为对于 javac 编译的类，没有办法通过反射检索源代码声明顺序）。
>
> 考虑将这种advice方法折叠成每个 `@Aspect` 类中每个连接点的一个advice方法，或者将advice的片段重构为单独的 `@Aspect` 类，你可以通过 `Ordered` 或 `@Order` 在切面级别上排序。

##### Introduction

Introduction（在AspectJ中被称为类型间声明）使一个切面能够声明advice对象实现一个给定的接口，并代表这些对象提供该接口的实现。

你可以通过使用 `@DeclareParents` 注解来做一个introduction。这个注解被用来声明匹配的类型有一个新的父类（因此而得名）。

例如，给定一个名为 `UsageTracked` 的接口和一个名为 `DefaultUsageTracked` 的接口的实现，下面这个切面声明所有服务接口的实现者也实现 `UsageTracked` 接口（例如，通过JMX进行统计）。

```java
@Aspect
public class UsageTracking {

    @DeclareParents(value="com.xyz.service.*+", defaultImpl=DefaultUsageTracked.class)
    public static UsageTracked mixin;

    @Before("execution(* com.xyz..service.*.*(..)) && this(usageTracked)")
    public void recordUsage(UsageTracked usageTracked) {
        usageTracked.incrementUseCount();
    }

}
```

要实现的接口由被注解字段的类型决定。`@DeclareParents` 注解的 `value` 属性是AspectJ类型模式。任何匹配类型的bean都会实现 `UsageTracked` 接口。

注意，在前面例子的 before advice，service Bean可以直接作为 `UsageTracked` 接口的实现。如果以编程方式访问一个Bean，你会写下以下内容。

```java
UsageTracked usageTracked = context.getBean("myService", UsageTracked.class);
```

#### `Schema`

如果你喜欢基于XML的格式，Spring也提供了对使用 `aop` 命名空间标签定义切面的支持。它支持与使用 `@AspectJ` 风格时完全相同的 pointcut 表达式和advice种类。因此，在这一节中，我们将重点讨论这种语法，并请读者参考上一节的讨论（[@AspectJ 的支持](https://springdoc.cn/spring/core.html#aop-ataspectj)），以了解编写切点（pointcut）表达式和advice参数的绑定。

要使用本节描述的 `aop` 命名空间标签，你需要导入 `spring-aop` schema，如 [基于XML schema的配置](https://springdoc.cn/spring/core.html#core.appendix.xsd-schemas) 中所述。关于如何导入 `aop` 命名空间的标签，请参见 [AOP schema](https://springdoc.cn/spring/core.html#core.appendix.xsd-schemas-aop)。

在你的Spring配置中，所有的aspect和 advisor元素都必须放在一个 `<aop:config>` 元素中（你可以在一个应用上下文配置中拥有多个 `<aop:config>` 元素）。一个 `<aop:config>` 元素可以包含 pointcut、 advisor 和 aspect 元素（注意这些必须按顺序声明）。

> `<aop:config>` 式的配置大量使用了 Spring 的 [自动代理](https://springdoc.cn/spring/core.html#aop-autoproxy) 机制。如果你已经通过使用 `BeanNameAutoProxyCreator` 或类似的东西来使用显式自动代理，这可能会导致问题（比如advice不被织入）。推荐的使用模式是只使用 `<aop:config>` 样式或只使用 `AutoProxyCreator` 样式，不要将它们混合使用。

##### 声明 Aspect

当你使用schema支持时，一个切面是一个普通的Java对象，在你的Spring应用上下文中被定义为一个Bean。状态和行为被捕获在对象的字段和方法中，而pointcut和advice信息被捕获在XML中。

你可以通过使用 `<aop:aspect>` 元素来声明一个切面，并通过使用 `ref` 属性来引用支持 Bean，如下例所示。

```xml
<aop:config>
    <aop:aspect id="myAspect" ref="aBean">
        ...
    </aop:aspect>
</aop:config>

<bean id="aBean" class="...">
    ...
</bean>
```

支持切面的Bean（本例中的 `aBean`）当然可以像其他Spring Bean一样被配置和依赖注入。

##### 声明  Pointcut

你可以在一个 `<aop:config>` 元素中声明一个命名的 pointcut，让 pointcut 的定义在几个切面和advice之间共享。

代表服务层中任何业务服务的执行的 pointcut 可以被定义如下。

```xml
<aop:config>

    <aop:pointcut id="businessService"
        expression="execution(* com.xyz.service.*.*(..))" />

</aop:config>
```

请注意，pointcut 表达式本身使用与 [@AspectJ 的支持](https://springdoc.cn/spring/core.html#aop-ataspectj) 中描述的AspectJ pointcut 表达式语言。如果你使用基于schema的声明风格，你也可以在pointcut表达式中引用 `@Aspect` 类型中定义的命名pointcut。因此，另一种定义上述pointcut的方式是：

```xml
<aop:config>

    <aop:pointcut id="businessService"
        expression="com.xyz.CommonPointcuts.businessService()" /> 

</aop:config>
```

> 引用 [共享命名切点的定义](https://springdoc.cn/spring/core.html#aop-common-pointcuts) 中定义的 `businessService` 命名的 pointcut。

正如下面的例子所示，在一个切面内声明一个 pointcut 与声明一个顶层 pointcut 非常相似。

```xml
<aop:config>

    <aop:aspect id="myAspect" ref="aBean">

        <aop:pointcut id="businessService"
            expression="execution(* com.xyz.service.*.*(..))"/>

        ...
    </aop:aspect>

</aop:config>
```

与 `@AspectJ` 切面的方式相同，通过使用基于schema的定义风格来声明的 pointcuts 可以收集连接点上下文。例如，下面这个 pointcut 收集了 `this` 对 象作为连接点上下文并将其传递给advice。

```xml
<aop:config>

    <aop:aspect id="myAspect" ref="aBean">

        <aop:pointcut id="businessService"
            expression="execution(* com.xyz.service.*.*(..)) &amp;&amp; this(service)"/>

        <aop:before pointcut-ref="businessService" method="monitor"/>

        ...
    </aop:aspect>

</aop:config>
```

必须通过包括匹配名称的参数来声明该 advice 以接收收集的连接点上下文，如下所示。

```java
public void monitor(Object service) {
    // ...
}
```

当组合 pointcut 的子表达式时，`&&` 在XML文档中是很尴尬的，所以你可以使用 `and`、`or` 和 `not` 关键字来代替 `&&`、`||` 和 `!`，分别。例如，前面的pointcut可以更好地写成如下。

```xml
<aop:config>

    <aop:aspect id="myAspect" ref="aBean">

        <aop:pointcut id="businessService"
            expression="execution(* com.xyz.service.*.*(..)) and this(service)"/>

        <aop:before pointcut-ref="businessService" method="monitor"/>

        ...
    </aop:aspect>

</aop:config>
```

请注意，以这种方式定义的 pointcuts 是通过其XML `id` 来引用的，并且不能被用作命名的 pointcuts 来形成复合 pointcuts。因此，基于 schema 的定义风格中的命名的 pointcuts 支持比@AspectJ风格所提供的支持更加有限。

##### 声明 Advice

基于schema的AOP支持使用了与 @AspectJ 风格相同的五种advice，而且它们的语义完全相同。

###### Before Advice

Before advice 在一个匹配的方法执行之前运行。它是通过使用 `<aop:aspect>` 元素在 `<aop:before>` 内声明的，如下面的例子所示。

```xml
<aop:aspect id="beforeExample" ref="aBean">

    <aop:before
        pointcut-ref="dataAccessOperation"
        method="doAccessCheck"/>

    ...

</aop:aspect>
```

在上面的例子中，`dataAccessOperation` 是在顶层（`<aop:config>`）定义的一个命名的 pointcut 的 `id`（见 [声明一个 Pointcut](https://springdoc.cn/spring/core.html#aop-schema-pointcuts)）。

> 正如我们在讨论@AspectJ风格时指出的，使用命名pointcuts可以显著提高代码的可读性。详见 [共享命名切点的定义](https://springdoc.cn/spring/core.html#aop-common-pointcuts)。

要代替定义内联的pointcut，请用 `pointcut` 性属性替换 `pointcut-ref` 属性，如下所示。

```xml
<aop:aspect id="beforeExample" ref="aBean">

    <aop:before
        pointcut="execution(* com.xyz.dao.*.*(..))"
        method="doAccessCheck"/>

    ...

</aop:aspect>
```

`method` 属性标识了一个提供 advice body 的方法（`doAccessCheck`）。这个方法必须为包含advice的切面元素所引用的bean定义。在执行数据访问操作（由切点表达式匹配的方法执行连接点）之前，aspect bean上的 `doAccessCheck` 方法被调用。

###### After Returning Advice

After returning advice，当一个匹配的方法执行正常完成时，会运行advice。它被声明在一个 `<aop:aspect>` 内，方法与 before advice 相同。下面的例子展示了如何声明它。

```xml
<aop:aspect id="afterReturningExample" ref="aBean">

    <aop:after-returning
        pointcut="execution(* com.xyz.dao.*.*(..))"
        method="doAccessCheck"/>

    ...
</aop:aspect>
```

和@AspectJ风格一样，你可以在 advice body 中获得返回值。要做到这一点，请使用 `returning` 属性来指定返回值应该被传递的参数名称，如下例所示。

```xml
<aop:aspect id="afterReturningExample" ref="aBean">

    <aop:after-returning
        pointcut="execution(* com.xyz.dao.*.*(..))"
        returning="retVal"
        method="doAccessCheck"/>

    ...
</aop:aspect>
```

`doAccessCheck` 方法必须声明一个名为 `retVal` 的参数。这个参数的类型以与 `@AfterReturning` 相同的方式约束匹配。例如，你可以这样声明方法的签名。

```java
public void doAccessCheck(Object retVal) {...
```

###### After Throwing Advice

当一个匹配的方法的执行通过抛出一个异常而退出时，After throwing advice 就会运行。它是通过使用 `after-throwing` 元素在 `<aop:aspect>` 内声明的，如下面的例子所示。

```xml
<aop:aspect id="afterThrowingExample" ref="aBean">

    <aop:after-throwing
        pointcut="execution(* com.xyz.dao.*.*(..))"
        method="doRecoveryActions"/>

    ...
</aop:aspect>
```

与@AspectJ风格一样，你可以在 advice body 中获得被抛出的异常。要做到这一点，请使用 `throwing` 属性来指定应该被传递的异常的参数名称，如下例所示。

```xml
<aop:aspect id="afterThrowingExample" ref="aBean">

    <aop:after-throwing
        pointcut="execution(* com.xyz.dao.*.*(..))"
        throwing="dataAccessEx"
        method="doRecoveryActions"/>

    ...
</aop:aspect>
```

`doRecoveryActions` 方法必须声明一个名为 `dataAccessEx` 的参数。这个参数的类型与 `@AfterThrowing` 的描述方式相同，制约着匹配。例如，该方法的签名可以被声明如下。

```java
public void doRecoveryActions(DataAccessException dataAccessEx) {...
```

###### After (Finally) Advice

After (finally) advice 无论匹配的方法执行如何退出都会运行。你可以通过使用 `after` 元素来声明它，如下面的例子所示。

```xml
<aop:aspect id="afterFinallyExample" ref="aBean">

    <aop:after
        pointcut="execution(* com.xyz.dao.*.*(..))"
        method="doReleaseLock"/>

    ...
</aop:aspect>
```

###### Around Advice

最后一种建议是Around advice。Around advice "围绕" 一个匹配的方法的执行而运行。它有机会在方法运行之前和之后进行工作，并决定何时、如何、甚至是否真正运行该方法。如果你需要以线程安全的方式分享方法执行前后的状态，例如启动和停止一个定时器，那么 Around advice 经常被使用。

> 始终使用符合你要求的最不强大的advice形式。例如，如果before advice足以满足你的需要，就不要使用 around advice。

你可以通过使用 `aop:around` 元素来声明around advice。advice 方法应该声明 `Object` 作为它的返回类型，并且该方法的第一个参数必须是 `ProceedingJoinPoint` 类型。在advice方法的主体中，你必须在 `ProceedingJoinPoint` 上调用 `proceed()`，以使底层方法运行。在没有参数的情况下调用 `proceed()` 将导致调用者的原始参数被提供给底层方法。对于高级用例，有一个重载的 `proceed()` 方法，它接受一个参数数组（`Object[]`）。当底层方法被调用时，数组中的值将被用作该方法的参数。关于使用 `Object[]` 调用 `Proceed` 的注意事项，请参见 [Around Advice](https://springdoc.cn/spring/core.html#aop-ataspectj-around-advice)。

下面的例子显示了如何在XML中声明 around advice。

```xml
<aop:aspect id="aroundExample" ref="aBean">

    <aop:around
        pointcut="execution(* com.xyz.service.*.*(..))"
        method="doBasicProfiling"/>

    ...
</aop:aspect>
```

`doBasicProfiling` advice 的实现可以和@AspectJ的例子完全一样（当然，除去注解），正如下面的例子所示。

```java
public Object doBasicProfiling(ProceedingJoinPoint pjp) throws Throwable {
    // start stopwatch
    Object retVal = pjp.proceed();
    // stop stopwatch
    return retVal;
}
```

##### Advice 参数

基于schema的声明风格支持完全类型化的advice，其方式与@AspectJ支持的方式相同—通过将pointcut参数与 advice 方法参数进行名称匹配。详情请参见 [Advice 参数](https://springdoc.cn/spring/core.html#aop-ataspectj-advice-params)。如果你想明确地指定advice方法的参数名称（不依赖于前面描述的检测策略），你可以通过使用建议元素的 `arg-names` 属性来实现，它的处理方式与advice注解中的 `argNames` 属性相同（如 [确定参数名称](https://springdoc.cn/spring/core.html#aop-ataspectj-advice-params-names)）。下面的例子显示了如何在XML中指定一个参数名称。

```xml
<aop:before
    pointcut="com.xyz.Pointcuts.publicMethod() and @annotation(auditable)" 
    method="audit"
    arg-names="auditable" />
```

> 引用 “[组合切点（Pointcut）表达式](https://springdoc.cn/spring/core.html#aop-pointcuts-combining)” 中定义的 `publicMethod` 命名的 pointcut。

`arg-names` 属性接受一个以逗号分隔的参数名称列表。

下面这个基于XSD的方法的例子稍微有点复杂，显示了一些与一些强类型参数一起使用的around advice。

```java
package com.xyz.service;

public interface PersonService {

    Person getPerson(String personName, int age);
}

public class DefaultPersonService implements PersonService {

    public Person getPerson(String name, int age) {
        return new Person(name, age);
    }
}
```

接下来是切面的问题。请注意，`profile(…)` 方法接受一些强类型的参数，其中第一个恰好是用于继续进行方法调用的连接点。这个参数的存在表明 `profile(…)` 将被用作 `around` advice，正如下面的例子所示。

```java
package com.xyz;

public class SimpleProfiler {

    public Object profile(ProceedingJoinPoint call, String name, int age) throws Throwable {
        StopWatch clock = new StopWatch("Profiling for '" + name + "' and '" + age + "'");
        try {
            clock.start(call.toShortString());
            return call.proceed();
        } finally {
            clock.stop();
            System.out.println(clock.prettyPrint());
        }
    }
}
```

最后，下面这个XML配置的例子对一个特定的连接点执行 preceding advice 产生影响。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- this is the object that will be proxied by Spring's AOP infrastructure -->
    <bean id="personService" class="com.xyz.service.DefaultPersonService"/>

    <!-- this is the actual advice itself -->
    <bean id="profiler" class="com.xyz.SimpleProfiler"/>

    <aop:config>
        <aop:aspect ref="profiler">

            <aop:pointcut id="theExecutionOfSomePersonServiceMethod"
                expression="execution(* com.xyz.service.PersonService.getPerson(String,int))
                and args(name, age)"/>

            <aop:around pointcut-ref="theExecutionOfSomePersonServiceMethod"
                method="profile"/>

        </aop:aspect>
    </aop:config>

</beans>
```

考虑一下下面的driver脚本。

```java
public class Boot {

    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("beans.xml");
        PersonService person = ctx.getBean(PersonService.class);
        person.getPerson("Pengo", 12);
    }
}
```

有了这样一个 `Boot` 类，我们会在标准输出上得到类似以下的输出。

```
StopWatch 'Profiling for 'Pengo' and '12': running time (millis) = 0
-----------------------------------------
ms     %     Task name
-----------------------------------------
00000  ?  execution(getFoo)
```

##### Advice 顺序

当多个advice需要在同一个连接点（执行方法）运行时，排序规则如 [Advice 顺序](https://springdoc.cn/spring/core.html#aop-ataspectj-advice-ordering) 中所述。切面之间的优先级通过 `<aop:aspect>` 元素中的 `order` 属性来确定，或者通过向支持该切面的 bean 添加 `@Order` 注解，或者通过让 bean 实现 `Ordered` 接口。

> 与定义在同一个 `@Aspect` 类中的advice方法的优先规则不同，当定义在同一个 `<aop:aspect>` 元素中的两个advice都需要在同一个连接点运行时，优先顺序由advice元素在包围的 `<aop:aspect>` 元素中的声明顺序决定，从高到低的顺序。例如，在同一个 `<aop:aspect>` 元素中定义了一个 `around` advice和一个 `before` advice，适用于同一个连接点，为了确保 `around` advice比 `before` advice有更高的优先权，`<aop:around>` 元素必须在 `<aop:before>` 元素之前声明。作为一般的经验法则，如果你发现你在同一个 `<aop:aspect>` 元素中定义了多个advice，并且适用于同一个连接点，请考虑将这些advice方法折叠成每个 `<aop:aspect>` 元素中每个连接点的一个advice方法，或者将这些advice重构为单独的 `<aop:aspect>` 元素，你可以在切面层次上排序。

##### Introduction

Introduction（在AspectJ中被称为类型间声明）让一个切面声明 advice 对象实现一个给定的接口，并代表这些对象提供该接口的实现。

你可以通过在 `aop:aspect` 里面使用 `aop:declaration-parents` 元素来做一个introduction。你可以使用 `aop:declaration-parents` 元素来声明匹配的类型有一个新的父类（因此得名）。例如，给定一个名为 `UsageTracked` 的接口和一个名为 `DefaultUsageTracked` 的接口的实现，下面这个切面声明服务接口的所有实现者也实现 `UsageTracked` 接口。（例如，为了通过JMX公开统计数据）。

```xml
<aop:aspect id="usageTrackerAspect" ref="usageTracking">

    <aop:declare-parents
        types-matching="com.xyz.service.*+"
        implement-interface="com.xyz.service.tracking.UsageTracked"
        default-impl="com.xyz.service.tracking.DefaultUsageTracked"/>

    <aop:before
        pointcut="execution(* com.xyz..service.*.*(..))
            and this(usageTracked)"
            method="recordUsage"/>

</aop:aspect>
```

支持 `usageTracking` bean的类将包含以下方法。

```java
public void recordUsage(UsageTracked usageTracked) {
    usageTracked.incrementUseCount();
}
```

要实现的接口是由 `implement-interface` 属性决定的。`types-matching` 属性的值是一个AspectJ类型pattern。任何匹配类型的Bean都会实现 `UsageTracked` 接口。请注意，在前面例子的advice前，服务Bean可以直接作为 `UsageTracked` 接口的实现。要以编程方式访问一个Bean，你可以写如下。

```java
UsageTracked usageTracked = context.getBean("myService", UsageTracked.class);
```

## 应用

以下是使用 Spring 框架的几个巨大好处的列表 −

- **基于 POJO** - Spring 使开发人员能够使用 POJO 开发企业级应用程序。 仅使用 POJO 的好处是您不需要 EJB 容器产品，例如应用程序服务器，但您可以选择仅使用强大的 servlet 容器，例如 Tomcat 或某些商业产品。
- **模块化** - Spring 以模块化方式组织。 即使包和类的数量很多，您也只需要担心您需要的，而忽略其余的。
- **与现有框架集成** - Spring 并没有重新发明，而是真正利用了一些现有技术，如几个 ORM 框架、日志框架、JEE、Quartz 和 JDK 计时器以及其他视图技术。
- **可测试性** - 测试用 Spring 编写的应用程序很简单，因为依赖于环境的代码被移到了这个框架中。 此外，通过使用 JavaBeanstyle POJO，使用依赖注入来注入测试数据变得更加容易。
- **Web MVC** - Spring 的 web 框架是一个设计良好的 web MVC 框架，它为 Struts 等 web 框架或其他过度设计或不太流行的 web 框架提供了一个很好的替代方案。
- **中央异常处理** - Spring 提供了一个方便的 API 来将特定于技术的异常（例如，由 JDBC、Hibernate 或 JDO 抛出）转换为一致的、未经检查的异常。
- **轻量级** - 轻量级 IoC 容器往往是轻量级的，例如，与 EJB 容器相比时尤其如此。 这有利于在内存和 CPU 资源有限的计算机上开发和部署应用程序。
- **事务管理** - Spring 提供一致的事务管理接口，可以缩小到本地事务（例如，使用单个数据库）并扩展到全局事务（例如，使用 JTA）。

## 系列架构图

![image-20230316154735436](D:\Repository\Typora\image-20230316154735436.png)

### 核心容器

核心容器（Core Container）是Spring框架的基础构建块，由多个模块组成，这些模块提供了关键的功能来支持应用程序的开发和管理。

下面是核心容器的主要模块及其功能：

1. **spring-core**：spring-core 模块提供了Spring框架的核心组件，包括控制反转（IoC）和依赖注入的基本支持。它定义了Bean的生命周期管理、依赖解析等核心功能，为整个框架提供了基础。
2. **spring-beans**：spring-beans 模块引入了BeanFactory接口，这是Spring框架中IoC容器的核心接口。它提供了工厂模式的微妙实现，使我们能够配置和管理Bean的生命周期。这种方式去除了硬编码的单例对象，并允许我们将配置和依赖从实际编码逻辑中解耦。
3. **spring-context**：spring-context 模块构建在spring-core和spring-beans模块之上，扩展了框架的功能。它以类似于JNDI注册的方式访问对象，提供了国际化、事件传播、资源加载、透明创建上下文等功能。Context模块还支持Java EE功能，如EJB、JMX和远程调用。其中，ApplicationContext接口是Context模块的核心，它为应用程序提供了上下文环境的管理和访问。
4. **spring-context-support**：spring-context-support模块提供了对第三方库的集成支持，这些库可以与Spring上下文一起使用。例如，它支持缓存（如EhCache、Guava、JCache）、邮件（JavaMail）、调度（CommonJ、Quartz）、模板引擎（FreeMarker、JasperReports、Velocity）等。这个模块扩展了Spring框架的功能，使得可以更轻松地与其他库协同工作。
5. **spring-expression**：spring-expression 模块提供了强大的表达式语言，用于在运行时查询和操作对象图。这是一种强大的表达式语言，支持属性值的读取和赋值、方法调用、访问数组、集合和索引、逻辑和算术运算、命名变量、从Spring IoC容器检索对象、列表的处理等功能。这个模块增强了Spring框架的灵活性，使开发人员能够更轻松地操作和管理对象。

![Spring 体系结构](D:\Repository\Typora\16ab3cdc88e9dc35803d09db5bb168e0.png)

### 数据访问/集成（Data Access/Integration）

- **JDBC 模块**：提供了JDBC的抽象层，简化了数据库访问，减少了冗长的JDBC编码，以及数据库供应商特定错误代码的处理。
- **ORM 模块**：支持多种对象关系映射（ORM）API集成，如JPA、JDO、Hibernate、MyBatis等，使其与Spring的其他功能（如事务管理）整合。
- **OXM 模块**：提供对不同对象-XML映射（OXM）实现的支持，如JAXB、Castor、XML Beans、JiBX、XStream等。
- **JMS 模块**：包含生产和消费消息的功能，支持Java消息服务（Java Message Service）。从Spring 4.1开始，与spring-messaging模块集成，提供更多的消息处理功能。
- **事务模块**：支持编程式和声明式事务管理，可用于特殊接口类和所有POJO。编程式事务需要自行处理事务方法，而声明式事务可以通过注解或配置进行自动处理。

### Web

- **Web 模块**：提供基本的Web功能和Web应用上下文，包括多部分文件上传、使用Servlet监听器初始化IoC容器等。还包括HTTP客户端以及与Spring远程调用相关的Web部分。
- **Servlet 模块**：为Web应用程序提供模型-视图-控制（MVC）和REST Web服务的实现。Spring的MVC框架支持领域模型代码和Web表单的分离，与Spring框架的其他功能无缝集成。
- **Web-Socket 模块**：提供WebSocket支持，允许客户端和服务器端之间的双向通信。
- **Web-Portlet 模块**：提供了用于Portlet环境的MVC实现，反映了spring-webmvc模块的功能。

### AOP

- **AOP 模块**：提供了面向方面编程的支持，允许定义方法拦截器和切入点，以将功能代码解耦，减少重复代码，降低模块间的耦合度，有利于系统的可扩展性和可维护性。

### Aspects

- **Aspects 模块**：提供了与AspectJ的集成，这是一个功能强大的面向切面编程（AOP）框架。

### Instrumentation

- **Instrumentation 模块**：在某些应用服务器中提供了类仪器化的支持和类加载器的实现。

### Messaging

- **Messaging 模块**：支持STOMP作为WebSocket子协议的使用，提供WebSocket客户端和服务器端之间的通信。还支持注解编程模型，用于选路和处理来自WebSocket客户端的STOMP消息。

### Test

- **测试模块**：支持对使用JUnit或TestNG框架的Spring组件进行测试。

## 设计模式

- **工厂设计模式** : Spring使用工厂模式通过 `BeanFactory`、`ApplicationContext` 创建 bean 对象。
- **代理设计模式** : Spring AOP 功能的实现。
- **单例设计模式** : Spring 中的 Bean 默认都是单例的。
- **模板方法模式** : Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- **包装器设计模式** : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- **观察者模式:** Spring 事件驱动模型就是观察者模式很经典的一个应用。
- **适配器模式** :Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller`。

## 事务

数据库事务是被视为单个工作单元的一系列操作。 这些操作要么完全完成，要么完全不生效。 事务管理是面向RDBMS的企业应用程序的重要组成部分，用于保证数据的完整性和一致性。 事务的概念可以用以下四个关键属性描述为 **ACID** −

- **原子性** − 事务应被视为单个操作单元，这意味着整个操作序列要么成功，要么不成功。
- **一致性** − 这代表了数据库的引用完整性、表中唯一主键等的一致性。
- **隔离性** − 一个事务的执行不能其它事务干扰。即一个事务内部的操作及使用的数据对其它并发事务是隔离的，并发执行的各个事务之间不能互相干扰。
- **持续性** − 也称永久性，指一个事务一旦提交，它对数据库中的数据的改变就应该是永久性的。接下来的其它操作或故障不应该对其执行结果有任何影响。

一个真正的 RDBMS 数据库系统将保证每个事务的所有四个属性。 使用 SQL 向数据库发出事务的简单视图如下：

- 使用 *begin transaction* 命令开始事务。
- 使用 SQL 查询执行各种删除、更新或插入操作。
- 如果所有操作都成功则执行 *commit*提交，否则 *rollback* 回滚所有操作。

Spring 框架在不同的底层事务管理 API 之上提供了一个抽象层。 Spring 的事务支持旨在通过向 POJO 添加事务功能来提供 EJB 事务的替代方案。 Spring 支持编程式和声明式事务管理。 EJB 需要一个应用服务器，但是 Spring 事务管理可以在不需要应用服务器的情况下实现。

### 传播规则

- PROPAGATION_REQUIRED: 支持当前事务，如果当前没有事务，就新建一个事务。这是最常见的选择。
- PROPAGATION_SUPPORTS: 支持当前事务，如果当前没有事务，就以非事务方式执行。
- PROPAGATION_MANDATORY: 支持当前事务，如果当前没有事务，就抛出异常。
- PROPAGATION_REQUIRES_NEW: 新建事务，如果当前存在事务，把当前事务挂起。
- PROPAGATION_NOT_SUPPORTED: 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
- PROPAGATION_NEVER: 以非事务方式执行，如果当前存在事务，则抛出异常。
- PROPAGATION_NESTED:如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则进行与PROPAGATION_REQUIRED类似的操作。

### 本地与全局事务

本地事务特定于单个事务资源，如 JDBC 连接，而全局事务可以跨越多个事务资源，如分布式系统中的事务。

本地事务管理在应用程序组件和资源位于单个站点的集中式计算环境中很有用，并且事务管理仅涉及在单个机器上运行的本地数据管理器。 本地事务更容易实现。

在所有资源分布在多个系统中的分布式计算环境中，需要全局事务管理。 在这种情况下，事务管理需要在本地和全局级别进行。 分布式或全局事务跨多个系统执行，其执行需要全局事务管理系统与所有相关系统的所有本地数据管理器之间的协调。

### 程序化与声明式事务管理

Spring 支持两种类型的事务管理 −

- [程序化事务管理](https://www.w3schools.cn/spring/programmatic_management.html) − 这意味着您必须借助编程来管理事务。 这为您提供了极大的灵活性，但很难维护。
- [声明式事务管理](https://www.w3schools.cn/spring/declarative_management.html) − 这意味着您将事务管理与业务代码分开。 您只使用注解或基于 XML 的配置来管理事务。

声明式事务管理优于程序式事务管理，尽管它不如程序式事务管理灵活，后者允许您通过代码控制事务。 但是作为一种横切关注点，声明式事务管理可以使用 AOP 方法进行模块化。 Spring 通过 Spring AOP 框架支持声明式事务管理。

### Spring 事务抽象

Spring 事务抽象的关键是由*org.springframework.transaction.PlatformTransactionManager*接口定义的，如下 −

```java
public interface PlatformTransactionManager {
   TransactionStatus getTransaction(TransactionDefinition definition);
   throws TransactionException;
   
   void commit(TransactionStatus status) throws TransactionException;
   void rollback(TransactionStatus status) throws TransactionException;
}
```

![image-20230317153337290](D:\Repository\Typora\image-20230317153337290.png)

![image-20230317160918652](D:\Repository\Typora\image-20230317160918652.png)

io异常默认不参与回滚

![image-20230317161324734](D:\Repository\Typora\image-20230317161324734.png)

![image-20230317161711105](D:\Repository\Typora\image-20230317161711105.png)

![image-20230317161820892](D:\Repository\Typora\image-20230317161820892.png)



## 引入其他配置文件

### 引入xml配置文件

实际开发中，Spring配置的内容非常多，这就导致Spring配置很繁杂且体积很大，所以，可以将部分配置拆解到其他配置文件中，而在Spring主配置文件中通过import标签进行加载。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<import resource="applicationContext-product.xml"/>
<import resource="applicationContext-user.xml"/>
</beans>
```

### 引入properties配置文件

方法1：

增添命名空间，`beans`标签中添加

```xml
xmlns:context="http://www.springframework.org/schema/context"
```

`xsi:schemaLocation`属性中添加

```xml
http://www.springframework.org/schema/context 
http://www.springframework.org/schema/context/spring-beans.xsd
```

导入配置

```xml
    <!--引入数据库配置信息 -->
    <context:property-placeholder location="classpath*:properties/db.properties" />
```

方法2：

情况1配置一个：

```xml
    <bean id="propertyConfigurer" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="location" value="classpath*:db/jdbc.properties" />
    </bean>
```

情况2配置多个：

```xml
    <bean id="propertyConfigure" class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="locations">
            <list>
                <value>classpath:/opt/demo/config/demo-db.properties</value> 
                <value>classpath:/opt/demo/config/demo-db2.properties</value> 
            </list>
        </property>
    </bean>
```

这些properties中就是key-value的键值对，使用的时候可以使用${xxx} 获取。

> 读取配置文件时，系统会有限读取系统的环境变量，注意key的命名。
>
> 可以在导入配置文件的标签中添加`system-properties-mode=“NEVER”`属性来避免使用系统环境变量。
>
> 使用`classpath:*.properties`导入所有配置文件，但只能导入当前工作目录中读取，再classpath填*可以导入jar包和类路径中的配置文件。

# MyBatis

# SpringBoot

在使用Spring框架进行开发的过程中，需要配置很多Spring框架包的依赖，如spring-core、spring-bean、spring-context等，而这些配置通常都是重复添加的，而且需要做很多框架使用及环境参数的重复配置，如开启注解、配置日志等。

Spring Boot致力于弱化这些不必要的操作，提供默认配置，当然这些默认配置是可以按需修改的，快速搭建、开发和运行Spring应用。

以下是使用SpringBoot的一些好处：

- 自动配置，使用基于类路径和应用程序上下文的智能默认值，当然也可以根据需要重写它们以满足开发人员的需求。
- 创建Spring Boot Starter 项目时，可以选择选择需要的功能，Spring Boot将为你管理依赖关系。
- SpringBoot项目可以打包成jar文件。可以使用Java-jar命令从命令行将应用程序作为独立的Java应用程序运行。
- 在开发web应用程序时，springboot会配置一个嵌入式Tomcat服务器，以便它可以作为独立的应用程序运行。（Tomcat是默认的，当然你也可以配置Jetty或Undertow）
- SpringBoot包括许多有用的非功能特性（例如安全和健康检查）。

## 环境配置

Spring Boot支持不同环境的属性配置文件切换，通过创建application-{profile}.properties文件，其中{profile}是具体的环境标识名称，例如：application-dev.properties用于开发环境，application-test.properties用于测试环境，application-uat.properties用于uat环境。如果要想使用application-dev.properties文件，则在application.properties文件中添加spring.profiles.active=dev。

如果要想使用application-test.properties文件，则在application.properties文件中添加spring.profiles.active=test。

## 核心注解

启动类上面的注解是@SpringBootApplication，它也是 Spring Boot 的核心注解，主要组合包含了以下 3 个注解：

@SpringBootConfiguration：组合了 @Configuration 注解，实现配置文件的功能。

@EnableAutoConfiguration：打开自动配置的功能，也可以关闭某个自动配置的选项，如关闭数据源自动配置功能： @SpringBootApplication(exclude = { DataSourceAutoConfiguration.class })。

@ComponentScan：Spring组件扫描。

## 工作原理

Starters可以理解为启动器，它包含了一系列可以集成到应用里面的依赖包，你可以一站式集成 Spring 及其他技术，而不需要到处找示例代码和依赖包。如你想使用 Spring JPA 访问数据库，只要加入 spring-boot-starter-data-jpa 启动器依赖就能使用了。

Starters包含了许多项目中需要用到的依赖，它们能快速持续的运行，都是一系列得到支持的管理传递性依赖。

Spring Boot 在启动的时候会干这几件事情：

- Spring Boot 在启动时会去依赖的 Starter 包中寻找 resources/META-INF/spring.factories 文件，然后根据文件中配置的 Jar 包去扫描项目所依赖的 Jar 包。
- 根据 spring.factories 配置加载 AutoConfigure 类
- 根据 @Conditional 注解的条件，进行自动配置并将 Bean 注入 Spring Context

总结一下，其实就是 Spring Boot 在启动的时候，按照约定去读取 Spring Boot Starter 的配置信息，再根据配置信息对资源进行初始化，并注入到 Spring 容器中。这样 Spring Boot 启动完毕后，就已经准备好了一切资源，使用过程中直接注入对应 Bean 资源即可。



## 工程错误

https://blog.csdn.net/weixin_46005650/article/details/122003335

解决方法：不使用默认的url，在Custom的输入框中输入 https://start.springboot.io/

![img](D:\Repository\Typora\21ca84fefd2340d7a7c7ed2602654a2d.png)



![image-20230318121848834](D:\Repository\Typora\image-20230318121848834.png)

![image-20230606151322149](D:\Repository\Typora\image-20230606151322149.png)



## 基础文件

![image-20230318122048157](D:\Repository\Typora\image-20230318122048157.png)

![image-20230318122124189](D:\Repository\Typora\image-20230318122124189.png)

![image-20230318122629653](D:\Repository\Typora\image-20230318122629653.png)

![image-20230318122636980](D:\Repository\Typora\image-20230318122636980.png)

![image-20230318123620458](D:\Repository\Typora\image-20230318123620458.png)

![image-20230318123653535](D:\Repository\Typora\image-20230318123653535.png)

![image-20230318123750571](D:\Repository\Typora\image-20230318123750571.png)

![image-20230318123806400](D:\Repository\Typora\image-20230318123806400.png)

![image-20230318124013593](D:\Repository\Typora\image-20230318124013593.png)

![image-20230318124505895](D:\Repository\Typora\image-20230318124505895.png)

![image-20230318124530338](D:\Repository\Typora\image-20230318124530338.png)

![image-20230318124535869](D:\Repository\Typora\image-20230318124535869.png)

![image-20230318124546112](D:\Repository\Typora\image-20230318124546112.png)

![image-20230318124815652](D:\Repository\Typora\image-20230318124815652.png)

![image-20230318124844566](D:\Repository\Typora\image-20230318124844566.png)

![image-20230318124942914](D:\Repository\Typora\image-20230318124942914.png)

![image-20230318125434789](D:\Repository\Typora\image-20230318125434789.png)

![image-20230318125441387](D:\Repository\Typora\image-20230318125441387.png)

![image-20230318125507704](D:\Repository\Typora\image-20230318125507704.png)

![image-20230318125513174](D:\Repository\Typora\image-20230318125513174.png)

## 多环境

![image-20230318125702760](D:\Repository\Typora\image-20230318125702760.png)

![image-20230318130522775](D:\Repository\Typora\image-20230318130522775.png)

![image-20230318130819004](D:\Repository\Typora\image-20230318130819004.png)

![image-20230318131123683](D:\Repository\Typora\image-20230318131123683.png)

![image-20230318131146479](D:\Repository\Typora\image-20230318131146479.png)

![image-20230318131216075](D:\Repository\Typora\image-20230318131216075.png)

![image-20230318131226382](D:\Repository\Typora\image-20230318131226382.png)

## 开发热部署

![image-20230606160623173](D:\Repository\Typora\image-20230606160623173.png)

![image-20230606160710587](D:\Repository\Typora\image-20230606160710587.png)

![image-20230606160747767](D:\Repository\Typora\image-20230606160747767.png)

![image-20230606161240802](D:\Repository\Typora\image-20230606161240802.png)



![image-20230318131255563](D:\Repository\Typora\image-20230318131255563.png)

## 整合Junit

![image-20230318131810901](D:\Repository\Typora\image-20230318131810901.png)

![image-20230318132214424](D:\Repository\Typora\image-20230318132214424.png)

## 整合Mybatis

![image-20230318132337595](D:\Repository\Typora\image-20230318132337595.png)

![image-20230318132344791](D:\Repository\Typora\image-20230318132344791.png)

![image-20230318133124309](D:\Repository\Typora\image-20230318133124309.png)

![image-20230318133131912](D:\Repository\Typora\image-20230318133131912.png)

![image-20230318133141959](D:\Repository\Typora\image-20230318133141959.png)

![image-20230318134821004](D:\Repository\Typora\image-20230318134821004.png)

### 路由映射

![image-20230606161740928](D:\Repository\Typora\image-20230606161740928.png)

![image-20230606161830416](D:\Repository\Typora\image-20230606161830416.png)

![image-20230606161855885](D:\Repository\Typora\image-20230606161855885.png)

## 静态资源

![image-20230617091109450](D:\Repository\Typora\image-20230617091109450.png)

## 文件上传

![image-20230617091252332](D:\Repository\Typora\image-20230617091252332.png)

![image-20230617091419462](D:\Repository\Typora\image-20230617091419462.png)

![image-20230617091448190](D:\Repository\Typora\image-20230617091448190.png)

## 拦截器

![image-20230617093522533](D:\Repository\Typora\image-20230617093522533.png)

![image-20230617093532816](D:\Repository\Typora\image-20230617093532816.png)

![image-20230617093703010](D:\Repository\Typora\image-20230617093703010.png)

## RestFul

![image-20230617095221174](D:\Repository\Typora\image-20230617095221174.png)

![image-20230617095253422](D:\Repository\Typora\image-20230617095253422.png)

![image-20230617095354618](D:\Repository\Typora\image-20230617095354618.png)

![image-20230617095441777](D:\Repository\Typora\image-20230617095441777.png)

![image-20230617095523834](D:\Repository\Typora\image-20230617095523834.png)

![image-20230617100351300](D:\Repository\Typora\image-20230617100351300.png)

![image-20230617102654817](D:\Repository\Typora\image-20230617102654817.png)

![image-20230617102833164](D:\Repository\Typora\image-20230617102833164.png)

![image-20230617102843409](D:\Repository\Typora\image-20230617102843409.png)

![image-20230617102849673](D:\Repository\Typora\image-20230617102849673.png)

![image-20230617103730925](D:\Repository\Typora\image-20230617103730925.png)

![image-20230617103842123](D:\Repository\Typora\image-20230617103842123.png)

# MyBatisPlus

![image-20230617104156434](D:\Repository\Typora\image-20230617104156434.png)

![image-20230617104241786](D:\Repository\Typora\image-20230617104241786.png)

![image-20230617104319814](D:\Repository\Typora\image-20230617104319814.png)

![image-20230617104402867](D:\Repository\Typora\image-20230617104402867.png)

![image-20230617104503332](D:\Repository\Typora\image-20230617104503332.png)

## 多表查询

![image-20230617105922503](D:\Repository\Typora\image-20230617105922503.png)

![image-20230617110433911](D:\Repository\Typora\image-20230617110433911.png)

用了results映射，即使字段一样，仍要用result一一对应。

## 分页查询

![image-20230617111322549](D:\Repository\Typora\image-20230617111322549.png)

![image-20230617111338774](D:\Repository\Typora\image-20230617111338774.png)

## 跨域

![image-20230617131735792](D:\Repository\Typora\image-20230617131735792.png)

使当前控制器使用默认跨域：

![image-20230617131814308](D:\Repository\Typora\image-20230617131814308.png)

