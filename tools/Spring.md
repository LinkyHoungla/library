# 概述

Spring Framework是一款强大的Java应用程序框架，致力于提供高效、可扩展的开发环境。结合轻量级容器、依赖注入功能，支持POJO容器配置、面向切面编程和AOP模块。

**核心特性**

+ 使用POJO进行容器配置
+ 面向切面编程的简便方法
+ 支持Android和iOS移动应用开发技术
+ 事务管理、对象/关系映射、JDBC、JMS等技术支持

**应用场景**

+ 适用于长时间运行、无法控制升级周期的大型企业应用
+ 可作为单一jar形式在云环境中运行
+ 也可用于独立应用程序（批处理或集成工作负载）

**模块划分**

+ 核心模块（Core）：包含配置模型和依赖注入机制
+ 支持不同应用架构的模块：消息传递、事务性数据、持久性、Web等
+ Spring MVC Web框架和Spring WebFlux响应式web框架

**Java模块化支持**

+ Spring框架的jar支持部署到JDK 9的模块路径（"Jigsaw"）
+ 使用"Automatic-Module-Name"清单项定义稳定的语言级模块名称
+ 在JDK 8和9+的classpath上都保持正常工作

**规范支持**

+ 支持JSR 330（依赖注入）和JSR 250（通用注解）规范
+ 开发人员可选择使用这些规范替代Spring专用机制

**Jakarta EE升级**

+ 从Spring Framework 6.0开始，升级到Jakarta EE 9级别
+ 基于`jakarta`命名空间替代传统的`javax`包
+ 准备为Jakarta EE API的进一步发展提供支持

**技术变迁**

+ 应用程序开发由部署到应用服务器转变为更云友好、开发者友好的方式（尤其在Spring Boot下）
+ Servlet容器变为嵌入式，易于变更
+ WebFlux应用程序可在非Servlet容器的服务器上运行（如Netty）

# 控制反转

**IoC（Inversion of Control）**，控制反转，也称为依赖注入（DI），是一种理论、概念、思想，将对象的创建、赋值、管理工作交给代码之外的容器实现，实现了对象的创建与解耦合。

+ **正转（正向控制）：** 手动完成对象的创建、赋值等操作，例如使用`new`关键字、`setXxx()`语句，耦合度高，不易维护。
+ **反转（控制反转）：** 通过容器实现对象的创建、赋值等操作，程序员只需修改容器的配置文件来适应程序功能的变化，降低耦合度。

它是一个过程，对象的依赖关系通过构造参数、工厂方法参数或对象实例化后设置的属性来定义。容器在创建bean时注入这些依赖关系，这是对象控制其依赖关系实例化的逆过程，因此被称为控制反转。

**Java中对象创建方式**

1. **构造方法：** `new ClassName()`
2. **反射：** 使用反射机制动态创建对象，`Class.newInstance()` 或 `Constructor.newInstance()`
3. **序列化：** 将对象转为字节流进行序列化，再从字节流反序列化为对象
4. **动态代理：** 使用`Proxy`类创建动态代理对象
5. **容器：** 使用IoC容器，如Tomcat容器、Spring IoC容器

**Spring IoC容器**

+ **基础包：** `org.springframework.beans` 和 `org.springframework.context`
+ **接口：**
  + [`BeanFactory`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/beans/factory/BeanFactory.html)：提供高级配置机制，管理任何类型的对象。
  + [`ApplicationContext`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/context/ApplicationContext.html)：是 `BeanFactory` 的子接口，增加了更多企业特定功能。
    + 更容易与Spring的AOP功能集成
    + Message resource 处理（用于国际化）
    + 事件发布
    + 应用层的特定上下文，如 `WebApplicationContext` 用于 web 应用
+ **Bean管理：** 在Spring中，被Spring IoC容器管理的构成应用程序骨干的对象被称为Bean。IoC容器负责实例化、组装和管理这些Bean。
+ **配置元数据：** Bean及其之间的依赖关系反映在IoC容器使用的配置元数据中，这些元数据指定了Bean的创建、组装及其与其他Bean之间的关系。

## 容器

Spring的 `ApplicationContext` 是负责管理Bean生命周期和依赖关系的核心接口。通过不同实现类以及多样化的配置元数据格式，Spring IoC容器提供了灵活的方式来管理和配置应用程序的组件。

**`org.springframework.context.ApplicationContext` 接口：**

+ 代表Spring IoC容器，负责实例化、配置和组装bean。
+ 通过读取配置元数据来确定实例化、配置和组装哪些对象。

**配置元数据：**

+ 表示指示容器如何处理对象的信息，可使用XML、Java注解或Java代码表示。
+ XML一直是传统格式，但可以使用少量XML配置启用对Java注解或代码的支持。

**`ApplicationContext` 接口实现**

+ 实现方式：
  + `ClassPathXmlApplicationContext`：用于独立应用程序，读取位于类路径下的XML配置文件。
  + `FileSystemXmlApplicationContext`：同样用于独立应用程序，读取文件系统中的XML配置文件。
+ 元数据格式支持：
  + 可以通过提供少量XML配置来使用Java注解或代码作为元数据格式，实现声明性配置。

下图显示了Spring工作方式的高层视图。你的应用程序类与配置元数据相结合，这样，在 `ApplicationContext` 被创建和初始化后，你就有了一个完全配置好的可执行系统或应用程序。

![container magic](https://springdoc.cn/spring/images/container-magic.png)

> 大多数场景下，不需要明确的用户代码来实例化Spring IoC容器。
>
> 在Web应用场景中，通常只需少量的模板式Web描述符（如`web.xml`文件）。

### 配置

Spring IoC容器消费一种配置元数据，这种数据告诉Spring容器如何实例化、配置和组装对象，为应用开发者提供了控制权。

配置元数据传统上以XML格式提供，但Spring IoC容器与配置元数据格式解耦。现在许多开发者选择[基于Java的配置](https://springdoc.cn/spring/core.html#beans-java)。

**其他配置元数据形式**

- [基于注解的配置](https://springdoc.cn/spring/core.html#beans-annotation-config): 使用注解定义Bean。
- [Java-based configuration](https://springdoc.cn/spring/core.html#beans-java): 使用Java定义外部Bean。使用的注解有 `@Configuration`, `@Bean`, `@Import`, 和 `@DependsOn`。

Spring的配置包括至少一个Bean定义，XML格式将Bean配置为 `<beans/>` 元素内的 `<bean/>` 元素。Java配置则使用 `@Configuration` 类中的 `@Bean` 注解的方法。

这些Bean定义对应于应用程序的实际对象，如服务层、持久层对象、表现对象和基础设施对象等。一般不在容器中配置细粒度的domain对象，这是repository和业务逻辑的责任。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">  
        <!-- 这个bean的合作者和配置在这里 -->
    </bean>

    <bean id="..." class="...">
        <!-- 这个bean的合作者和配置在这里 -->
    </bean>

    <!-- 更多bean定义在这里 -->

</beans>
```

+ `id`属性：用于识别单个Bean定义。
+ `class`属性：定义Bean的类型，使用类的全路径名。

#### 后处理器注册

`<context:annotation-config/>` 隐含地注册以下后处理器：

+ [`ConfigurationClassPostProcessor`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/context/annotation/ConfigurationClassPostProcessor.html)
+ [`AutowiredAnnotationBeanPostProcessor`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/beans/factory/annotation/AutowiredAnnotationBeanPostProcessor.html)
+ [`CommonAnnotationBeanPostProcessor`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/context/annotation/CommonAnnotationBeanPostProcessor.html)
+ [`PersistenceAnnotationBeanPostProcessor`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/orm/jpa/support/PersistenceAnnotationBeanPostProcessor.html)
+ [`EventListenerMethodProcessor`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/context/event/EventListenerMethodProcessor.html)

> `<context:annotation-config/>` 在定义它的同一应用上下文中寻找Bean的注解。比如放在 `DispatcherServlet` 的 `WebApplicationContext` 中，它只检查controller中的`@Autowired` Bean，不会检查service中的。

### 实例化

Spring 的 ApplicationContext 构造函数接受一条或多条资源路径字符串，用于从外部资源（如本地文件系统、Java CLASSPATH 等）加载配置元数据。

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```

**services.xml - 服务层配置**

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="petStore" class="org.springframework.samples.jpetstore.services.PetStoreServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <property name="itemDao" ref="itemDao"/>
        <!-- additional collaborators and configuration for this bean -->
    </bean>
    <!-- more bean definitions for services -->
</beans>
```

**daos.xml - 数据访问对象配置**

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="accountDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaAccountDao">
        <!-- additional collaborators and configuration for this bean -->
    </bean>
    <bean id="itemDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaItemDao">
        <!-- additional collaborators and configuration for this bean -->
    </bean>
    <!-- more bean definitions for data access objects -->
</beans>
```

在这些XML示例中，`services.xml` 定义了 `PetStoreServiceImpl` 类作为服务层，并引用了两个数据访问对象 `JpaAccountDao` 和 `JpaItemDao`。

#### 多文件配置与导入

你可以从多个XML文件加载Bean定义。使用`<import/>`元素可以轻松实现这一点。

```xml
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```

**所有位置路径都是相对于进行导入的定义文件**，`services.xml` 必须与导入文件在同一目录或classpath位置。被导入文件的内容必须是有效的XML Bean定义，根据Spring Schema。

> 避免使用相对 "../" 路径来引用父目录中的文件，这可能造成对当前应用程序之外的文件的依赖。推荐使用完全限定的资源位置，如 `file:C:/config/services.xml` 或 `classpath:/config/services.xml`。
>
> 然而，请注意，你正在将你的应用程序的配置与特定的绝对位置相耦合。一般来说，最好是为这种绝对位置保留一个指示 - 例如，通过 "${…}" 占位符，在运行时针对JVM系统属性（system properties）进行解析。

Spring的命名空间提供了导入指令的功能，并提供了更多配置功能，例如 `context` 和 `util` 命名空间。

### 使用

`ApplicationContext` 是一个高级工厂的接口，能够维护不同Bean及其依赖关系的注册表。通过使用方法 `T getBean(String name, Class<T> requiredType)`，你可以检索到Bean的实例。

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// 检索配置的实例
PetStoreService service = context.getBean("petStore", PetStoreService.class);
List<String> userList = service.getUsernameList();
```

`GenericApplicationContext`与reader delegate的结合提供了更灵活的方式读取Bean定义。

```java
GenericApplicationContext context = new GenericApplicationContext();
new XmlBeanDefinitionReader(context).loadBeanDefinitions("services.xml", "daos.xml");
context.refresh();
```

你可以混合和匹配不同的reader delegate在同一个`ApplicationContext`中读取bean定义。

尽管`ApplicationContext`提供了多种检索Bean的方法，但理想情况下，应用代码不应该直接调用`getBean()`方法。Spring的API不应该成为应用程序代码的依赖。例如，Spring与Web框架的集成为各种Web框架组件（如 controller 和JSF管理的Bean）提供了依赖注入，让你通过元数据（如autowiring注解）声明对特定Bean的依赖。

## Bean

Spring IoC容器管理着一个或多个Bean，这些Bean由提供给容器的配置元数据创建，通常以XML `<bean/>` 定义的形式呈现。

在容器中，Bean定义以 `BeanDefinition` 对象表示，包含以下元数据：

+ **全路径类名**：通常是被定义的Bean的实际实现类。
+ **Bean的行为配置元素**：描述了Bean在容器中的行为方式（如scope、生命周期回调等）。
+ **其他Bean的引用**：作为合作者或依赖关系，需要与该Bean一起工作。
+ **其他配置设置**：如pool的大小限制或在管理连接池的Bean中使用的连接数。

这个元数据转化为构成每个Bean定义的一组属性。

| 属性                     | 解释                                                         |
| :----------------------- | :----------------------------------------------------------- |
| Class                    | [实例化 Bean](https://springdoc.cn/spring/core.html#beans-factory-class) |
| Name                     | [Bean 命名](https://springdoc.cn/spring/core.html#beans-beanname) |
| Scope                    | [Bean Scope](https://springdoc.cn/spring/core.html#beans-factory-scopes) |
| Constructor arguments    | [依赖注入](https://springdoc.cn/spring/core.html#beans-factory-collaborators) |
| Properties               | [依赖注入](https://springdoc.cn/spring/core.html#beans-factory-collaborators) |
| Autowiring mode          | [注入协作者（Autowiring Collaborators）](https://springdoc.cn/spring/core.html#beans-factory-autowire) |
| Lazy initialization mode | [懒加载的Bean](https://springdoc.cn/spring/core.html#beans-factory-lazy-init) |
| Initialization method    | [初始化回调](https://springdoc.cn/spring/core.html#beans-factory-lifecycle-initializingbean) |
| Destruction method       | [销毁回调](https://springdoc.cn/spring/core.html#beans-factory-lifecycle-disposablebean) |

除了包含如何创建特定 Bean 的信息的 Bean 定义外，`ApplicationContext` 实现还允许注册在容器外（由用户）创建的现有对象。

通过 `getBeanFactory()` 方法访问 `ApplicationContext` 的 `BeanFactory` 来实现的，该方法返回 `DefaultListableBeanFactory` 实现。`DefaultListableBeanFactory` 通过 `registerSingleton(..)` 和 `registerBeanDefinition(..)` 方法支持这种注册。然而，典型的应用程序只与通过常规Bean定义元数据定义的Bean一起工作。

> Bean 元数据和手动提供的单体实例需要尽早注册，以便容器在自动注入和其它内省步骤中正确推导它们。虽然在某种程度上支持覆盖现有的元数据和现有的单体实例，但 **官方不支持在运行时注册新的Bean**（与对工厂的实时访问同时进行），这可能会导致并发访问异常、Bean容器中的不一致状态，或者两者都有。

### 命名

每个Bean在承载它的容器中都有一个或多个标识符，这些标识符必须是唯一的，通常一个Bean只有一个标识符，额外的标识符可以被视为别名。

**标识符类型**

+ **id属性**：允许精确指定一个id，通常是字母数字，也可以包含特殊字符。
+ **name属性**：用于指定Bean的一个或多个别名，可以用逗号、分号或空格分隔。

> 尽管 `id` 属性被定义为 `xsd:string` 类型，但 bean id 的唯一性是由容器强制执行的，尽管不是由 XML 解析器执行。

**标识符的生成规则**

+ **未明确提供标识符**：如果未提供明确的name或id，容器将为Bean生成一个唯一的名称。
+ **命名惯例**：推荐按照Java命名惯例给Bean命名，例如以小写字母开始并采用驼峰命名法，如`accountManager`、`userService`等。

你不需要为Bean提供一个 `name` 或 `id`。如果你不明确地提供 `name` 或 `id`，容器将为该 Bean 生成一个唯一的名称。然而，如果你想通过使用 `ref` 元素或服务定位器风格的查找来引用该 bean 的名称，你必须提供一个名称。不提供名字的动机与使用 [内部Bean](https://springdoc.cn/spring/core.html#beans-inner-beans) 和 [注入协作者（Autowiring Collaborators）](https://springdoc.cn/spring/core.html#beans-factory-autowire) 有关。

> 统一命名Bean使你的配置更容易阅读和理解。另外，如果你使用Spring AOP，在对一组按名称相关的Bean应用 advice 时，也有很大的帮助。

> 在classpath中的组件扫描（component scanning），Spring为未命名的组件生成Bean名称，遵循前面描述的规则：基本上，取简单的类名并将其初始字符变成小写。然而，在（不寻常的）特殊情况下，当有一个以上的字符，并且第一个和第二个字符都是大写时，原来的大小写会被保留下来。这些规则与 `java.beans.Introspector.decapitalize`（Spring在此使用）所定义的规则相同。

#### 别名

在 Bean 定义中，你可以为Bean提供一个以上的名字，通过使用由 `id` 属性指定的最多一个名字和 `name` 属性中任意数量的其他名字的组合。这些名字可以是同一个Bean的等效别名，在某些情况下很有用，比如让应用程序中的每个组件通过使用一个特定于该组件本身的Bean名字来引用一个共同的依赖关系。

然而，在实际定义Bean的地方指定所有别名并不总是足够的。有时，为一个在其他地方定义的Bean引入别名是可取的。这种情况通常发生在大型系统中，配置被分割到每个子系统中，每个子系统都有自己的对象定义集。在基于XML的配置元数据中，你可以使用 `<alias/>` 元素来实现这一点。下面的例子展示了如何做到这一点。

```xml
<alias name="fromName" alias="toName"/>
```

在这种情况下，一个名为 `fromName` 的bean（在同一个容器中）在使用这个别名定义后，也可以被称为 `toName`。

例如，子系统A的配置元数据可以引用一个名为 `subsystemA-dataSource` 的数据源。子系统B的配置元数据可以引用一个名为 `subsystemB-dataSource` 的数据源。当组成使用这两个子系统的主应用程序时，主应用程序以 `myApp-dataSource` 的名字来引用数据源。为了让这三个名字都指代同一个对象，你可以在配置元数据中添加以下别名定义。

```xml
<alias name="myApp-dataSource" alias="subsystemA-dataSource"/>
<alias name="myApp-dataSource" alias="subsystemB-dataSource"/>
```

每个组件和主应用程序都可以通过一个独特的名称来引用dataSource，并保证不与任何其他定义冲突（有效地创建了一个命名空间），但它们引用的是同一个bean。

### 实例化

Bean定义（definition）本质上是创建一个或多个对象的“配方”，容器根据这些定义所封装的配置元数据来创建（或获取）实际的对象。

**基于XML的Bean定义**

在XML的`<bean/>`元素中使用`class`属性指定要实例化的对象的类型（或class），相当于Java代码中的`new`操作符。

这个`class`属性可以指定两种方式：

+ **直接调用构造函数**：容器通过反射直接调用构造函数创建Bean。
+ **静态工厂方法**：不太常见，在容器中调用一个类上的`static`工厂方法创建bean，需要指定包含被调用的`static`工厂方法的实际类。

如果你想为一个嵌套类配置一个Bean定义（definition），你可以使用嵌套类的二进制名称或源（source）名称。

> 例如，如果你在 `com.example` 包中有一个叫做 `SomeThing` 的类，而这个 `SomeThing` 类有一个叫做 `OtherThing` 的静态嵌套类，它们可以用美元符号（`$`）或点（`.`）分开。所以在Bean定义中的 `class` 属性的值将是 `com.example.SomeThing$OtherThing` 或 `com.example.SomeThing.OtherThing`。



#### 构造函数

当你用构造函数的方法创建一个Bean时，所有普通的类都可以被Spring使用并与之兼容。也就是说，被开发的类不需要实现任何特定的接口，也不需要以特定的方式进行编码。只需指定Bean类就足够了。然而，根据你对该特定Bean使用的IoC类型，你可能需要一个默认（空）构造函数。

Spring IoC容器几乎可以管理任何你希望它管理的类。它并不局限于管理真正的JavaBean。大多数Spring用户更喜欢真正的JavaBean，它只有一个默认的（无参数）构造函数，以及按照容器中的属性建模的适当的setter和getter。你也可以在你的容器中拥有更多奇特的非bean风格的类。例如，如果你需要使用一个绝对不遵守JavaBean规范的传统连接池，Spring也可以管理它。

通过基于XML的配置元数据，你可以按以下方式指定你的bean类。

```xml
<bean id="exampleBean" class="examples.ExampleBean"/>

<bean name="anotherExample" class="examples.ExampleBeanTwo"/>
```

关于向构造函数提供参数（如果需要）和在对象被构造后设置对象实例属性的机制的详细信息，请参见 [依赖注入](https://springdoc.cn/spring/core.html#beans-factory-collaborators)。

#### 静态工厂

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

关于向工厂方法提供（可选）参数以及在对象从工厂返回后设置对象实例属性的机制，详见 [依赖和配置详解](https://springdoc.cn/spring/core.html#beans-factory-properties-detailed)。

现在考虑这个例子的一个变体，即不使用构造函数，而是让Spring调用一个 `static` 工厂方法来返回对象的实例。

```xml
<bean id="exampleBean" class="examples.ExampleBean" factory-method="createInstance">
    <constructor-arg ref="anotherExampleBean"/>
    <constructor-arg ref="yetAnotherBean"/>
    <constructor-arg value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

下面的例子显示了相应的 `ExampleBean` 类。

```java
public class ExampleBean {

    // a private constructor
    private ExampleBean(...) {
        ...
    }

    // a static factory method; the arguments to this method can be
    // considered the dependencies of the bean that is returned,
    // regardless of how those arguments are actually used.
    public static ExampleBean createInstance (
        AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {

        ExampleBean eb = new ExampleBean (...);
        // some other operations...
        return eb;
    }
}
```

`static` 工厂方法的参数由 `<constructor-arg/>` 元素提供，与实际使用的构造函数完全相同。被工厂方法返回的类的类型不一定与包含 `static` 工厂方法的类的类型相同（尽管在这个例子中，它是相同的）。实例（非静态）工厂方法可以以基本相同的方式使用（除了使用 `factory-bean` 属性而不是 `class` 属性），所以我们在此不讨论这些细节。

#### 实例工厂

与 [通过静态工厂方法进行的实例化](https://springdoc.cn/spring/core.html#beans-factory-class-static-factory-method) 类似，用实例工厂方法进行的实例化从容器中调用现有 bean 的非静态方法来创建一个新的 bean。要使用这种机制，请将 `class` 属性留空，并在 `factory-bean` 属性中指定当前（或父代或祖代）容器中的一个 Bean 的名称，该容器包含要被调用来创建对象的实例方法。用 `factory-method` 属性设置工厂方法本身的名称。下面的例子显示了如何配置这样一个Bean。

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

下面的例子显示了相应的类。

```java
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }
}
```

一个工厂类也可以容纳一个以上的工厂方法，如下例所示。

```xml
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>

<bean id="accountService"
    factory-bean="serviceLocator"
    factory-method="createAccountServiceInstance"/>
```

下面的例子显示了相应的类。

```java
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();

    private static AccountService accountService = new AccountServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }

    public AccountService createAccountServiceInstance() {
        return accountService;
    }
}
```

这种方法表明，工厂Bean本身可以通过依赖注入（DI）进行管理和配置。请看详细的 [依赖和配置](https://springdoc.cn/spring/core.html#beans-factory-properties-detailed)。

>  在Spring文档中，“factory bean” 是指在Spring容器中配置的Bean，它通过 [实例](https://springdoc.cn/spring/core.html#beans-factory-class-instance-factory-method) 或 [静态](https://springdoc.cn/spring/core.html#beans-factory-class-static-factory-method)工厂方法创建对象。相比之下，`FactoryBean`（注意大写字母）是指Spring特定的[`FactoryBean`](https://springdoc.cn/spring/core.html#beans-factory-extension-factorybean) 实现类。

### 运行时类型

要确定一个特定Bean的运行时类型是不容易的。在Bean元数据定义中指定的类只是一个初始的类引用，可能与已声明的工厂方法相结合，或者是一个 `FactoryBean` 类，这可能导致Bean的运行时类型不同，或者在实例级工厂方法的情况下根本没有被设置（而是通过指定的 `factory-bean` 名称来解决）。此外，AOP代理可能会用基于接口的代理来包装Bean实例，对目标Bean的实际类型（只是其实现的接口）的暴露有限。

要了解某个特定Bean的实际运行时类型，推荐的方法是对指定的Bean名称进行 `BeanFactory.getType` 调用。这将考虑到上述所有情况，并返回 `BeanFactory.getBean` 调用将为同一Bean名称返回的对象类型。

## 依赖

一个典型的企业应用程序并不是由单一的对象（或Spring术语中的bean）组成的。即使是最简单的应用也有一些对象，它们一起工作，呈现出最终用户所看到的连贯的应用。下一节将解释你如何从定义一些单独的Bean定义到一个完全实现的应用，在这个应用中，各对象相互协作以实现一个目标。

### 依赖注入

DI（Dependency Injection） :依赖注入， 只需要在程序中提供要使用的对象名称就可以， 至于对象如何在容器中创建， 赋值，查找都由容器内部实现。
DI是ioc技术的实现方式（即容器如何创建对象这一问题的实现方式）

依赖注入（DI）是一个过程，对象仅通过构造参数、工厂方法的参数或在对象实例被构造或从工厂方法返回后在其上设置的属性来定义它们的依赖（即与它们一起工作的其它对象）。然后，容器在创建 bean 时注入这些依赖。这个过程从根本上说是Bean本身通过使用类的直接构造或服务定位模式来控制其依赖的实例化或位置的逆过程（因此被称为控制反转）。

采用DI原则，代码会更干净，当对象被提供其依赖时，解耦会更有效。对象不会查找其依赖，也不知道依赖的位置或类别。因此，你的类变得更容易测试，特别是当依赖是在接口或抽象基类上时，这允许在单元测试中使用stub或mock实现。

DI有两个主要的变体。 [基于构造器的依赖注入](https://springdoc.cn/spring/core.html#beans-constructor-injection) 和 [基于setter的依赖注入](https://springdoc.cn/spring/core.html#beans-setter-injection)。

**IOC配置文件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--
        声明bean（告诉spring要创建某个类的对象）
        1、id：自定义名称，唯一值，spring通过该id的属性值找到对象
        2、class：要创建类的全限定类名
        3、下述的声明语句在spring底层类似与执行了以下代码：
            SomeService service = new SomeServiceImpl();
        4、对象的保存：
            spring将对象保存到内部的map中，map.put(id值，对象)
            map.put("someService",new SomeServiceImpl())
        5、一个bean标签声明一个java对象
        6、spring容器根据bean标签创建对象，尽管存在class属性相同的bean标签，只要是id值不同，
           spring容器就会创建该class的对象
    -->
    <bean id="someService" class="com.mms.service.impl.SomeServiceImpl"/>
    <bean id="someService2" class="com.mms.service.impl.SomeServiceImpl"/>

    <!--
        spring容器也可以创建非自定义类的对象，例如java.lang.String类的对象，只要指定了
        class属性，spring容器就可以创建该类的对象
    -->
    <bean id="myString" class="java.lang.String"/>
</beans>
```

#### 基于构造器

基于构造函数的 DI 是通过容器调用带有许多参数的构造函数来完成的，每个参数代表一个依赖。调用带有特定参数的 `static` 工厂方法来构造 bean 几乎是等价的，本讨论对构造函数的参数和 `static` 工厂方法的参数进行类似处理。下面的例子显示了一个只能用构造函数注入的依赖注入的类。

```java
public class SimpleMovieLister {

    // the SimpleMovieLister has a dependency on a MovieFinder
    private final MovieFinder movieFinder;

    // a constructor so that the Spring container can inject a MovieFinder
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // business logic that actually uses the injected MovieFinder is omitted...
}
```

请注意，这个类并没有什么特别之处。它是一个POJO，对容器的特定接口、基类或注解没有依赖。

##### 参数解析

构造函数参数解析匹配是通过使用参数的类型进行的。如果 bean 定义中的构造器参数不存在潜在的歧义，那么**构造器参数在 bean 定义中的定义顺序就是这些参数在 bean 被实例化时被提供给适当的构造器的顺序**。考虑一下下面这个类。

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

当引用另一个Bean时，类型是已知的，并且可以进行匹配（就像前面的例子那样）。当使用一个简单的类型时，比如 `<value>true</value>`，Spring不能确定值的类型，所以在没有帮助的情况下不能通过类型进行匹配。考虑一下下面这个类。

```java
package examples;

public class ExampleBean {

    // Number of years to calculate the Ultimate Answer
    private final int years;

    // The Answer to Life, the Universe, and Everything
    private final String ultimateAnswer;

    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

**构造函数参数类型匹配**

在前面的情况下，如果你通过使用 `type` 属性显式地指定构造函数参数的类型，容器就可以使用简单类型的类型匹配，如下例所示。

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```

**构造函数参数索引**

你可以使用 `index` 属性来明确指定构造函数参数的索引，如下例所示。

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```

除了解决多个简单值的歧义外，指定一个索引还可以解决构造函数有两个相同类型的参数的歧义。

>   索引（下标）从0开始。

**构造函数参数名**

你也可以使用构造函数的参数名称来进行消歧，如下面的例子所示。

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```

请记住，**要使这一方法开箱即用，你的代码在编译时必须启用debug标志，以便Spring能够从构造函数中查找参数名称**。如果你不能或不想用debug标志编译你的代码，你可以使用 [@ConstructorProperties](https://download.oracle.com/javase/8/docs/api/java/beans/ConstructorProperties.html) JDK注解来明确命名你的构造函数参数。这样一来，示例类就得如下。

```java
package examples;

public class ExampleBean {

    // Fields omitted

    @ConstructorProperties({"years", "ultimateAnswer"})
    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

#### 基于Setter

基于 Setter 的 DI 是通过容器在调用无参数的构造函数或无参数的 `static` 工厂方法来实例化你的 bean 之后调用 Setter 方法来实现的。

下面的例子显示了一个只能通过使用纯 setter 注入的类的依赖注入。这个类是传统的Java。它是一个POJO，对容器的特定接口、基类（base class）或注解没有依赖。

下面的例子将基于XML的配置元数据用于基于setter的DI。一个Spring XML配置文件的一小部分指定了一些Bean的定义，如下所示。

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- setter injection using the nested ref element -->
    <property name="beanOne">
        <ref bean="anotherExampleBean"/>
    </property>

    <!-- setter injection using the neater ref attribute -->
    <property name="beanTwo" ref="yetAnotherBean"/>
    <property name="integerProperty" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

下面的例子显示了相应的 `ExampleBean` 类。

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

`ApplicationContext` 支持它所管理的Bean的基于构造器和基于setter的DI。它还支持在一些依赖已经通过构造器方法注入后的基于setter的DI。你以 `BeanDefinition` 的形式配置依赖关系，你将其与 `PropertyEditor` 实例一起使用，将属性从一种格式转换为另一种。

然而，大多数Spring用户并不直接使用这些类（即以编程方式），而是使用XML Bean定义、注解组件（即用 `@Component`、 `@Controller` 等注解的类），或基于Java的 `@Configuration` 类中的 `@Bean` 方法。然后这些来源在内部被转换为 `BeanDefinition` 的实例，并用于加载整个Spring IoC容器实例。

> 由于你可以混合使用基于构造函数的DI和基于setter的DI，一个好的经验法则是对强制依赖使用构造函数，对可选依赖使用setter方法或配置方法。请注意，在setter方法上使用 [@Autowired](https://springdoc.cn/spring/core.html#beans-autowired-annotation) 注解可以使属性成为必须的依赖；然而，带有参数程序化验证的构造器注入是更好的。
>
> Spring团队通常提倡构造函数注入，因为它可以让你将应用组件实现为不可变的对象，并确保所需的依赖不为 `null`。此外，构造函数注入的组件总是以完全初始化的状态返回给客户端（调用）代码。顺便提一下，大量的构造函数参数是一种不好的代码气味，意味着该类可能有太多的责任，应该重构以更好地解决适当的分离问题。
>
> Setter注入主要应该只用于在类中可以分配合理默认值的可选依赖。否则，必须在代码使用依赖的所有地方进行非null值检查。Setter注入的一个好处是，Setter方法使该类的对象可以在以后重新配置或重新注入。因此，通过 [JMX MBean](https://springdoc.cn/spring/integration.html#jmx) 进行管理是setter注入的一个引人注目的用例。
>
> 对于一个特定的类，使用最合理的DI风格。有时，在处理你没有源代码的第三方类时，你会做出选择。例如，如果一个第三方类没有暴露任何setter方法，那么构造函数注入可能是唯一可用的DI形式。

### 解析过程

容器按如下方式执行 bean 依赖解析。

+ `ApplicationContext` 是用描述所有bean的配置元数据创建和初始化的。配置元数据可以由XML、Java代码或注解来指定。
+ 对于每个Bean来说，它的依赖是以属性、构造函数参数或静态工厂方法的参数（如果你用它代替正常的构造函数）的形式表达的。在实际创建Bean时，这些依赖被提供给Bean。
+ 每个属性或构造函数参数都是要设置的值的实际定义，或对容器中另一个Bean的引用。
+ 每个作为值的属性或构造函数参数都会从其指定格式转换为该属性或构造函数参数的实际类型。默认情况下，Spring 可以将以字符串格式提供的值转换为所有内置类型，如 `int`、`long`、`String`、`boolean` 等等。

当容器被创建时，Spring容器会验证每个Bean的配置。然而，在实际创建Bean之前，Bean的属性本身不会被设置。**当容器被创建时，那些具有单例作用域并被设置为预实例化的Bean（默认）被创建。作用域在 [Bean Scope](https://springdoc.cn/spring/core.html#beans-factory-scopes) 中定义。否则，Bean只有在被请求时才会被创建。**创建 bean 有可能导致创建 bean 图（graph），因为 bean 的依赖关系和它的依赖关系（等等）被创建和分配。请注意，这些依赖关系之间的解析不匹配可能会出现得很晚—也就是说，在第一次创建受影响的Bean时。

#### 循环依赖

如果你使用主要的构造函数注入，就有可能产生一个无法解决的循环依赖情况。

比如说。类A通过构造函数注入需要类B的一个实例，而类B通过构造函数注入需要类A的一个实例。如果你将A类和B类的Bean配置为相互注入，Spring IoC容器会在运行时检测到这种循环引用，并抛出一个 `BeanCurrentlyInCreationException`。

一个可能的解决方案是编辑一些类的源代码，使其通过setter而不是构造器进行配置。或者，避免构造器注入，只使用setter注入。换句话说，虽然不推荐这样做，但你可以用setter注入来配置循环依赖关系。

与典型的情况（没有循环依赖关系）不同，Bean A和Bean B之间的循环依赖关系迫使其中一个Bean在被完全初始化之前被注入到另一个Bean中（一个典型的鸡生蛋蛋生鸡的场景）。

一般来说，你可以相信Spring会做正确的事情。它在容器加载时检测配置问题，例如对不存在的bean的引用和循环依赖。在实际创建Bean时，Spring尽可能晚地设置属性和解析依赖关系。这意味着，当你请求一个对象时，如果在创建该对象或其某个依赖关系时出现问题，已经正确加载的Spring容器就会产生一个异常—例如，Bean由于缺少或无效的属性而抛出一个异常。这种对某些配置问题的潜在延迟可见性是 `ApplicationContext` 实现默认预置单例Bean的原因。在实际需要之前创建这些Bean需要付出一些前期时间和内存的代价，当 `ApplicationContext` 被创建时，你会发现配置问题，而不是后来。你仍然可以覆盖这个默认行为，这样单例Bean就会懒加载地初始化，而不是急切地预实例化。

如果不存在循环依赖关系，当一个或多个协作（Collaborate） Bean被注入到依赖Bean中时，每个协作Bean在被注入到依赖Bean中之前被完全配置。这意味着，如果Bean A对Bean B有依赖，Spring IoC容器会在调用Bean A的setter方法之前完全配置Bean B。换句话说，Bean被实例化（如果它不是预先实例化的单例），其依赖被设置，相关的生命周期方法（如 [配置的 init 方法](https://springdoc.cn/spring/core.html#beans-factory-lifecycle-initializingbean) 或 [InitializingBean 回调方法](https://springdoc.cn/spring/core.html#beans-factory-lifecycle-initializingbean)）被调用。

### 配置

你可以将Bean属性和构造函数参数定义为对其他托管Bean（协作者）的引用，或者定义为内联的值。Spring的基于XML的配置元数据支持 `<property/>` 和 `<constructor-arg/>` 元素中的子元素类型，以达到这个目的。

#### 字面值

字面值指基本类型和string。`<property/>` 元素的 `value` 属性将属性或构造函数参数指定为人类可读的字符串表示。Spring 的 [转换服务](https://springdoc.cn/spring/core.html#core-convert-ConversionService-API) 被用来将这些值从 `String` 转换成属性或参数的实际类型。下面的例子显示了各种值的设置。

```xml
<bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <!-- results in a setDriverClassName(String) call -->
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mydb"/>
    <property name="username" value="root"/>
    <property name="password" value="misterkaoli"/>
</bean>
```

下面的例子使用 [p-namespace](https://springdoc.cn/spring/core.html#beans-p-namespace) 来实现更简洁的XML配置。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource"
        destroy-method="close"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://localhost:3306/mydb"
        p:username="root"
        p:password="misterkaoli"/>

</beans>
```

前面的XML更简洁。然而，除非你使用的IDE（如 [IntelliJ IDEA](https://www.jetbrains.com/idea/) 或 [Spring Tools for Eclipse](https://spring.io/tools)）支持在你创建Bean定义时自动补全属性，否则错别字会在运行时而非设计时发现。强烈建议使用这样的IDE帮助。

你也可以配置一个 `java.util.Properties` 实例，如下所示。

```xml
<bean id="mappings"
    class="org.springframework.context.support.PropertySourcesPlaceholderConfigurer">

    <!-- typed as a java.util.Properties -->
    <property name="properties">
        <value>
            jdbc.driver.className=com.mysql.jdbc.Driver
            jdbc.url=jdbc:mysql://localhost:3306/mydb
        </value>
    </property>
</bean>
```

Spring容器通过使用 JavaBean 的 `PropertyEditor` 机制将 `<value/>` 元素中的文本转换为 `java.util.Properties` 实例。这是一个很好的捷径，也是Spring团队倾向于使用嵌套的 `<value/>` 元素而不是 `value` 属性风格的几个地方之一。

#### idref

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

第一种形式比第二种形式好，因为使用 `idref` 标签可以让容器在部署时验证被引用的、命名的 bean 是否真的存在。在第二种变体中，没有对传递给 `client` Bean 的 `targetName` 属性的值进行验证。只有在 `client` Bean实际被实例化时，才会发现错误（很可能是致命的结果）。如果 `client` Bean是一个 [prototype](https://springdoc.cn/spring/core.html#beans-factory-scopes) Bean，那么这个错别字和由此产生的异常可能只有在容器被部署后很久才能被发现。

>   4.0 版Bean XSD中不再支持 `idref` 元素上的 `local` 属性，因为它不再提供比普通 `bean` 引用更多的价值。在升级到4.0 schema时，将你现有的 `idref local` 引用改为 `idref bean`。

`<idref/>` 元素带来价值的一个常见地方（至少在早于Spring 2.0的版本中）是在 `ProxyFactoryBean` Bean定义中配置 [AOP interceptor（拦截器）](https://springdoc.cn/spring/core.html#aop-pfb-1)。当你指定拦截器名称时，使用 `<idref/>` 元素可以防止你把拦截器的ID拼错。

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

#### 内部Bean

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

#### 集合

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

#### 集合合并

Spring容器也支持合并集合。开发者可以定义一个父 <list/>、<map/>、<set/> 或 <props/> 元素，让子 <list/>、<map/>、<set/> 或 <props/> 元素继承和覆盖父集合的值。也就是说，子集合的值是合并父集合和子集合的元素的结果，子集合的元素覆盖父集合中指定的值。

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

#### 强类型集合

由于Java对泛型的支持，你可以使用强类型的 `Collection`。也就是说，我们可以声明一个 `Collection` 类型，使其只能包含（例如）`String` 元素。如果你使用Spring将一个强类型的 `Collection` 依赖性注入到Bean中，你可以利用Spring的类型转换支持，这样你的强类型 `Collection` 实例的元素在被添加到集合中之前就被转换为适当的类型。下面的Java类和Bean定义展示了如何做到这一点。

```java
public class SomeClass {

    private Map<String, Float> accounts;

    public void setAccounts(Map<String, Float> accounts) {
        this.accounts = accounts;
    }
}
```

```xml
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

#### Null and Empty String Values

Spring将属性等的空参数视为空字符串。下面这个基于XML的配置元数据片段将 `email` 属性设置为空字符串值（""）。

```xml
<bean class="ExampleBean">
    <property name="email" value=""/>
</bean>
```

前面的例子相当于下面的Java代码。

```java
exampleBean.setEmail("");
```

`<null/>` 元素处理 `null` 值。下面的列表显示了一个例子。

```xml
<bean class="ExampleBean">
    <property name="email">
        <null/>
    </property>
</bean>
```

前面的配置等同于以下Java代码。

```java
exampleBean.setEmail(null);
```

#### 命名空间

##### p 命名

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

##### c 命名

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

#### 复合属性

当你设置Bean属性时，你可以使用复合或嵌套的属性名，只要路径中除最终属性名外的所有组件不为 `null`。考虑一下下面的Bean定义。

```xml
<bean id="something" class="things.ThingOne">
    <property name="fred.bob.sammy" value="123" />
</bean>
```

`something` Bean有一个 `fred` 属性，它有一个 `bob` 属性，它有一个 `sammy` 属性，最后的 `sammy` 属性被设置为 `123` 的值。为了使这个方法奏效，`something` 的 `fred` 属性和 `fred` 的 `bob` 属性在构建 bean 后不能为 `null`。否则就会抛出一个 `NullPointerException`。

### 依赖声明

如果一个Bean是另一个Bean的依赖，这通常意味着一个Bean被设置为另一个Bean的一个属性。通常，你可以通过基于XML的配置元数据中的元素来实现这一点。然而，有时Bean之间的依赖关系并不那么直接。一个例子是当一个类中的静态初始化器需要被触发时，比如数据库驱动程序的注册。`depends-on` 属性可以明确地强制一个或多个Bean在使用此元素的Bean被初始化之前被初始化。下面的例子使用 `depends-on` 属性来表达对单个 bean 的依赖性。 

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
<bean id="accountDao" class="x.y.jdbc.JdbcAccountDao" />
```

> `depends-on` 属性可以指定初始化时间的依赖关系，而在 [单例](https://springdoc.cn/spring/core.html#beans-factory-scopes-singleton) Bean的情况下，也可以指定相应的销毁时间的依赖关系。与给定Bean定义了 `depends-on` 的依赖Bean会在给定Bean本身被销毁之前被首先销毁。因此，`depends-on` 也可以控制关闭的顺序。

### 懒加载

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

### 协作者

Spring容器可以自动连接协作Bean之间的关系。你可以让Spring通过检查 `ApplicationContext` 的内容为你的Bean自动解决协作者（其他Bean）。自动注入有以下优点。 * 自动注入可以大大减少对指定属性或构造函数参数的需要。（其他机制，如 [本章其他地方](https://springdoc.cn/spring/core.html#beans-child-bean-definitions) 讨论的 bean template，在这方面也很有价值）。 * 自动注入可以随着你的对象的发展而更新配置。例如，如果你需要给一个类添加一个依赖，这个依赖可以自动满足，而不需要你修改配置。因此，自动在开发过程中可能特别有用，而不会否定在代码库变得更加稳定时切换到显式注入的选择。

当使用基于XML的配置元数据时（见 [依赖注入](https://springdoc.cn/spring/core.html#beans-factory-collaborators)），你可以用 `<bean/>` 元素的 `autowire` 属性来指定bean定义的自动注入模式。自动注入功能有四种模式。你可以为每个Bean指定自动注入，从而选择哪些要自动注入。下表描述了四种自动注入模式。

| 模式          | 解释                                                         |
| :------------ | :----------------------------------------------------------- |
| `no`          | （默认）没有自动注入。Bean引用必须由 `ref` 元素来定义。对于大型部署来说，不建议改变默认设置，因为明确指定协作者会带来更大的控制力和清晰度。在某种程度上，它记录了一个系统的结构。 |
| `byName`      | 通过属性名称进行自动注入。Spring寻找一个与需要自动注入的属性同名的Bean。例如，如果一个Bean定义被设置为按名称自动注入，并且它包含一个 `master` 属性（也就是说，它有一个 `setMaster(..)` 方法），Spring会寻找一个名为 `master` 的Bean定义并使用它来设置该属性。 |
| `byType`      | 如果容器中正好有一个 property 类型的 bean 存在，就可以自动注入该属性。如果存在一个以上的bean，就会抛出一个致命的 exception，这表明你不能对该bean使用 `byType` 自动注入。如果没有匹配的 bean，就不会发生任何事情（该属性没有被设置）。 |
| `constructor` | 类似于 `byType`，但适用于构造函数参数。如果容器中没有一个构造函数参数类型的bean，就会产生一个致命的错误。 |

通过 `byType` 或 `constructor` 自动注入模式，你可以给数组（array）和泛型集合（collection）注入。在这种情况下，容器中所有符合预期类型的自动注入候选者都被提供来满足依赖。如果预期的key类型是 `String`，你可以自动注入强类型的 `Map` 实例。自动注入的 `Map` 实例的值由符合预期类型的所有 bean 实例组成，而 `Map` 实例的key包含相应的 bean 名称。

#### 缺陷

当自动注入在整个项目中被一致使用时，它的效果最好。如果自动注入没有被普遍使用，那么只用它来注入一个或两个Bean定义可能会让开发者感到困惑。

考虑自动注入的限制和弊端。

+ `property` 和 `constructor-arg` 设置中的明确依赖关系总是覆盖自动注入。你不能自动注入简单的属性，如基本数据、`String` 和 `Class`（以及此类简单属性的数组）。这个限制是设计上的。
+ 自动注入不如显式注入精确。尽管正如前面的表格中所指出的，Spring很小心地避免在模糊不清的情况下进行猜测，这可能会产生意想不到的结果。你的Spring管理的对象之间的关系不再被明确地记录下来。
+ 对于可能从Spring容器中生成文档的工具来说，注入信息可能无法使用。
+ 容器中的多个Bean定义可以与setter方法或构造参数指定的类型相匹配，以实现自动注入。对于数组、集合或 `Map` 实例，这不一定是个问题。然而，对于期待单一值的依赖关系，这种模糊性不会被任意地解决。如果没有唯一的Bean定义，就会抛出一个异常。

在后一种情况下，你有几种选择。

+ 放弃自动注入，改用明确注入。
+ 通过将bean定义的 `autowire-candidate` 属性设置为 `false` 来避免bean定义的自动注入，如 [下一节](https://springdoc.cn/spring/core.html#beans-factory-autowire-candidate) 所述。
+ 通过将 `<bean/>` 元素的 `primary` 属性设置为 `true`，将单个Bean定义指定为主要候选者。
+ 实现基于注解的配置所提供的更精细的控制，如 [基于注解的容器配置](https://springdoc.cn/spring/core.html#beans-annotation-config) 中所述。

#### 排除

在每个bean的基础上，你可以将一个bean排除在自动注入之外。在Spring的XML格式中，将 `<bean/>` 元素的 `autowire-candidate` 属性设置为 `false`。容器使特定的Bean定义对自动注入基础设施不可用（包括注解式配置，如 `@Autowired`）。

> `autowire-candidate` 属性被设计为只影响基于类型的自动注入。它不影响通过名称的显式引用，即使指定的 bean 没有被标记为 autowire 候选者，它也会被解析。因此，如果名称匹配，通过名称进行的自动注入还是会注入一个Bean。

你也可以根据对Bean名称的模式匹配来限制autowire候选人。顶层的 `<beans/>` 元素在其 `default-autowire-candidates` 属性中接受一个或多个模式。例如，要将自动注入候选状态限制在名称以 `Repository` 结尾的任何 bean，请提供 `*Repository` 的值。要提供多个模式，请用逗号分隔的列表定义它们。Bean定义的 `autowire-candidate` 属性的明确值为 `true` 或 `false`，总是优先考虑。对于这样的Bean，模式匹配规则并不适用。

这些技术对于那些你永远不想通过自动注入注入到其他 Bean 中的 Bean 是很有用的。这并不意味着排除在外的 Bean 本身不能通过使用 autowiring 进行配置。相反，Bean本身不是自动注入其他 Bean 的候选人。

### 方法注入

在大多数应用场景中，容器中的大多数Bean是 [单例](https://springdoc.cn/spring/core.html#beans-factory-scopes-singleton)。当一个单例Bean需要与另一个单例Bean协作或一个非单例Bean需要与另一个非单例Bean协作时，你通常通过将一个Bean定义为另一个Bean的一个属性来处理这种依赖关系。当Bean的生命周期不同时，问题就出现了。假设单例Bean A需要使用非单例（prototype）Bean B，也许是在A的每个方法调用上。容器只创建一次单例Bean A，因此只有一次机会来设置属性。容器不能在每次需要Bean B的时候为Bean A提供一个新的实例。

一个解决方案是放弃一些控制的反转。你可以通过实现 `ApplicationContextAware` 接口 [给 bean A 注入容器](https://springdoc.cn/spring/core.html#beans-factory-aware)，并在 bean A 需要时让 [容器的 `getBean("B")` 调用](https://springdoc.cn/spring/core.html#beans-factory-client) 询问（一个典型的 new）bean B 实例。下面的例子展示了这种方法。

```java
package fiona.apple;

// Spring-API imports
import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;

/**
 * A class that uses a stateful Command-style class to perform
 * some processing.
 */
public class CommandManager implements ApplicationContextAware {

    private ApplicationContext applicationContext;

    public Object process(Map commandState) {
        // grab a new instance of the appropriate Command
        Command command = createCommand();
        // set the state on the (hopefully brand new) Command instance
        command.setState(commandState);
        return command.execute();
    }

    protected Command createCommand() {
        // notice the Spring API dependency!
        return this.applicationContext.getBean("command", Command.class);
    }

    public void setApplicationContext(
            ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }
}
```

前面的情况是不可取的，因为业务代码知道并耦合到Spring框架。方法注入（Method Injection）是Spring IoC容器的一个高级功能，可以让你干净地处理这种用例。

#### 查找方法

查询方法注入是指容器能够覆盖容器管理的 bean 上的方法并返回容器中另一个命名的 bean 的查询结果。这种查找通常涉及到一个原型（prototype）Bean，就像 [上一节](https://springdoc.cn/spring/core.html#beans-factory-method-injection)中描述的情景。Spring框架通过使用CGLIB库的字节码生成来实现这种方法注入，动态地生成一个覆盖该方法的子类。

> 为了实现此动态子类化功能，被 Spring bean 容器子类化的类不能为 `final`，要被复写的方法也不能为 `final`。对一个包含 `abstract` 方法的类进行单元测试，需要你自己对这个类进行子类化，并提供一个 `abstract` 方法的 stub 实现。体的方法对于组件扫描也是必要的，这需要具体的类来接续。另一个关键的限制是，查找方法对工厂方法不起作用，特别是对配置类中的 `@Bean` 方法不起作用，因为在这种情况下，容器不负责创建实例，因此不能即时创建运行时生成的子类。

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

每当需要一个新的 `myCommand` Bean的实例时，被识别为 `commandManager` 的bean就会调用它自己的 `createCommand()` 方法。你必须注意将 `myCommand` Bean部署为一个原型（prototype），如果这确实是需要的。如果它是一个 [单例](https://springdoc.cn/spring/core.html#beans-factory-scopes-singleton)，每次都会返回同一个 `myCommand` Bean实例。

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

>   访问不同 scope 的目标Bean的另一种方式是 `ObjectFactory`/`Provider` 注入点。请看 [作为依赖的 Scope Bean](https://springdoc.cn/spring/core.html#beans-factory-scopes-other-injection)。你可能还会发现 `ServiceLocatorFactoryBean`（在 `org.springframework.beans.factory.config` 包中）很有用。

#### 任意方法

与查找方法注入相比，方法注入的一个不太有用的形式是用另一个方法实现替换托管Bean中的任意方法的能力。你可以安全地跳过本节的其余部分，直到你真正需要这个功能。

通过基于XML的配置元数据，你可以使用 `replaced-method` 元素来替换现有的方法实现，为已部署的Bean提供另一种方法。考虑一下下面这个类，它有一个我们想要覆盖的名为 `computeValue` 的方法。

```java
public class MyValueCalculator {

    public String computeValue(String input) {
        // some real code...
    }

    // some other methods...
}
```

实现 `org.springframework.beans.factory.support.MethodReplacer` 接口的类提供了新的方法定义，如下例所示。

```java
/**
 * meant to be used to override the existing computeValue(String)
 * implementation in MyValueCalculator
 */
public class ReplacementComputeValue implements MethodReplacer {

    public Object reimplement(Object o, Method m, Object[] args) throws Throwable {
        // get the input value, work with it, and return a computed result
        String input = (String) args[0];
        ...
        return ...;
    }
}
```

部署原始类并指定方法覆写的bean定义将类似于下面的例子。

```xml
<bean id="myValueCalculator" class="x.y.z.MyValueCalculator">
    <!-- arbitrary method replacement -->
    <replaced-method name="computeValue" replacer="replacementComputeValue">
        <arg-type>String</arg-type>
    </replaced-method>
</bean>

<bean id="replacementComputeValue" class="a.b.c.ReplacementComputeValue"/>
```

你可以在 `<replaced-method/>` 元素中使用一个或多个 `<arg-type/>` 元素来表示被重载方法的方法签名。只有当方法被重载并且在类中存在多个变体时，参数的签名才是必要的。为了方便起见，参数的类型字符串可以是全路径类型名称的子串。例如，下面这些都符合 `java.lang.String`。

```java
java.lang.String
String
Str
```

因为参数的数量往往足以区分每个可能的选择，这个快捷方式可以节省大量的输入，让你只输入符合参数类型的最短字符串。

## 生命周期

当你创建一个Bean定义时，你创建了一个“配方”，用于创建该Bean定义（definition）是所定义的类的实际实例。Bean定义（definition）是一个“配方”的想法很重要，因为它意味着，就像一个类一样，你可以从一个“配方”中创建许多对象实例。

你不仅可以控制各种依赖和配置值，将其插入到从特定Bean定义创建的对象中，还可以控制从特定Bean定义创建的对象的scope。这种方法是强大而灵活的，因为你可以通过配置来选择你所创建的对象的scope，而不是在Java类级别上烘托出一个对象的scope。Bean可以被定义为部署在若干scope中的一个。Spring框架支持六个scope，其中四个只有在你使用Web感知（aware）的 `ApplicationContext` 时才可用。你也可以创建 [一个自定义 scope](https://springdoc.cn/spring/core.html#beans-factory-scopes-custom)。

下表描述了支持的 scope。

| Scope                                                        | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [singleton](https://springdoc.cn/spring/core.html#beans-factory-scopes-singleton) | （默认情况下）为每个Spring IoC容器将单个Bean定义的Scope扩大到单个对象实例。 |
| [prototype](https://springdoc.cn/spring/core.html#beans-factory-scopes-prototype) | 将单个Bean定义的Scope扩大到任何数量的对象实例。              |
| [request](https://springdoc.cn/spring/core.html#beans-factory-scopes-request) | 将单个Bean定义的Scope扩大到单个HTTP请求的生命周期。也就是说，每个HTTP请求都有自己的Bean实例，该实例是在单个Bean定义的基础上创建的。只在Web感知的Spring `ApplicationContext` 的上下文中有效。 |
| [session](https://springdoc.cn/spring/core.html#beans-factory-scopes-session) | 将单个Bean定义的Scope扩大到一个HTTP `Session` 的生命周期。只在Web感知的Spring `ApplicationContext` 的上下文中有效。 |
| [application](https://springdoc.cn/spring/core.html#beans-factory-scopes-application) | 将单个Bean定义的 Scope 扩大到 `ServletContext` 的生命周期中。只在Web感知的Spring `ApplicationContext` 的上下文中有效。 |
| [websocket](https://springdoc.cn/spring/web.html#websocket-stomp-websocket-scope) | 将单个Bean定义的 Scope 扩大到 `WebSocket` 的生命周期。仅在具有Web感知的 Spring `ApplicationContext` 的上下文中有效。 |

> 一个 thread scope 是可用的，但默认情况下没有注册。欲了解更多信息，请参阅 [`SimpleThreadScope`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/context/support/SimpleThreadScope.html) 的文档。关于如何注册这个或任何其他自定义 scope 的说明，请参见 [使用自定义 Scope](https://springdoc.cn/spring/core.html#beans-factory-scopes-custom-using)。

### Singleton

只有一个单例 Bean 的共享实例被管理，所有对具有符合该Bean定义的ID的Bean的请求都会被Spring容器返回该特定的Bean实例。

换句话说，当你定义了一个Bean定义（define），并且它被定义为 singleton，Spring IoC容器就会为该Bean定义的对象创建一个确切的实例。这个单一的实例被存储在这种单体Bean的缓存中，所有后续的请求和对该命名Bean的引用都会返回缓存的对象。下面的图片显示了 singleton scope 是如何工作的。

![singleton](https://springdoc.cn/spring/images/singleton.png)

Spring 的 singleton Bean概念与Gang of Four（GoF）模式书中定义的singleton模式不同。GoF singleton模式对对象的范围进行了硬编码，即每个ClassLoader创建一个且仅有一个特定类的实例。Spring单例的范围最好被描述为每个容器和每个bean。这意味着，如果你在一个Spring容器中为一个特定的类定义了一个Bean，Spring容器就会为该Bean定义的类创建一个且只有一个实例。Singleton scope 是Spring的默认 scope。要在XML中把一个Bean定义为singleton，你可以像下面的例子那样定义一个Bean。

```xml
<bean id="accountService" class="com.something.DefaultAccountService"/>

<!-- the following is equivalent, though redundant (singleton scope is the default) -->
<bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
```

### Prototype

Bean 部署的非 singleton prototype scope 导致每次对该特定Bean的请求都会创建一个新的Bean实例。也就是说，该 bean 被注入到另一个 bean 中，或者你通过容器上的 `getBean()` 方法调用来请求它。作为一项规则，你应该对所有有状态的 bean 使用 prototype scope，对无状态的 bean 使用 singleton scope。

下图说明了Spring prototype scope。

![prototype](https://springdoc.cn/spring/images/prototype.png)

（数据访问对象(DAO)通常不被配置为 prototype，因为典型的DAO并不持有任何对话状态。对我们来说，重用 singleton 图的核心是比较容易的）。

下面的例子在XML中定义了一个 prototype bean。

```xml
<bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
```

与其他scope相比，Spring并不管理 prototype Bean的完整生命周期。容器对prototype对象进行实例化、配置和其他方面的组装，并将其交给客户端，而对该prototype实例没有进一步的记录。因此，尽管初始化生命周期回调方法在所有对象上被调用，而不考虑scope，但在prototype的情况下，配置的销毁生命周期回调不会被调用。客户端代码必须清理prototype scope 内的对象，并释放原prototype Bean持有的昂贵资源。为了让Spring容器释放由 prototype scopeBean 持有的资源，可以尝试使用自定义 [Bean后处理器](https://springdoc.cn/spring/core.html#beans-factory-extension-bpp)，它持有对需要清理的Bean的引用。

在某些方面，Spring容器在 prototype scope Bean 方面的作用是替代Java的 `new` 操作。所有超过该点的生命周期管理必须由客户端处理。（关于Spring容器中Bean的生命周期的详细信息，请参见 [生命周期回调](https://springdoc.cn/spring/core.html#beans-factory-lifecycle)）。

> 当你使用对 prototype Bean 有依赖的 singleton scope Bean时，请注意依赖关系是在实例化时解析的。因此，如果你将一个 prototype scope 的Bean依赖性注入到一个 singleton scope 的Bean中，一个新的 prototype Bean 被实例化，然后被依赖注入到 singleton Bean中。prototype 实例是唯一提供给 singleton scope Bean的实例。
>
> 然而，假设你想让 singleton scope 的Bean在运行时反复获得 prototype scope 的Bean的新实例。你不能将 prototype scope 的Bean 依赖注入到你的 singleton Bean中，因为这种注入只发生一次，当Spring容器实例化 singleton Bean 并解析和注入其依赖关系时。如果你在运行时需要一个新的 prototype Bean 实例不止一次，请参阅 [方法注入](https://springdoc.cn/spring/core.html#beans-factory-method-injection)。

### 其他

`request`、`session`、`application` 和 `websocket` scope只有在你使用Web感知的Spring `ApplicationContext` 实现（如 `XmlWebApplicationContext`）时才可用。如果你将这些scope与常规的Spring IoC容器（如 `ClassPathXmlApplicationContext`）一起使用，就会抛出一个 `IllegalStateException`，抱怨有未知的Bean scope。

#### 配置

为了支持Bean在 `request`、 `session`、`application` 和 `Websocket` 级别的scope（Web scope 的Bean），在你定义Bean之前，需要一些小的初始配置。（对于标准作用域（`singleton` 和 `prototype`）来说，这种初始设置是不需要的）。

你如何完成这个初始设置取决于你的特定Servlet环境。

如果你在Spring Web MVC中访问 scope 内的Bean，实际上是在一个由Spring `DispatcherServlet` 处理的请求（request）中，就不需要进行特别的设置。 `DispatcherServlet` 已经暴露了所有相关的状态。

如果你使用Servlet Web容器，在Spring的 `DispatcherServlet` 之外处理请求（例如，在使用JSF时），你需要注册 `org.springframework.web.context.request.RequestContextListener` `ServletRequestListener`。这可以通过使用 `WebApplicationInitializer` 接口以编程方式完成。或者，在你的Web应用程序的 `web.xml` 文件中添加以下声明。

```xml
<web-app>
    ...
    <listener>
        <listener-class>
            org.springframework.web.context.request.RequestContextListener
        </listener-class>
    </listener>
    ...
</web-app>
```

另外，如果你的监听器（listener）设置有问题，可以考虑使用Spring的 `RequestContextFilter`。过滤器（filter）的映射取决于周围的Web应用配置，所以你必须适当地改变它。下面的列表显示了一个Web应用程序的过滤器部分。

```xml
<web-app>
    ...
    <filter>
        <filter-name>requestContextFilter</filter-name>
        <filter-class>org.springframework.web.filter.RequestContextFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>requestContextFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    ...
</web-app>
```

`DispatcherServlet`、`RequestContextListener` 和 `RequestContextFilter` 都做了完全相同的事情，即把HTTP请求对象绑定到为该请求服务的 `Thread`。这使得 request scope 和 session scope 的Bean可以在调用链的更远处使用。列表显示了Web应用程序的过滤器部分。

#### Request

考虑以下用于Bean定义的XML配置。

```xml
<bean id="loginAction" class="com.something.LoginAction" scope="request"/>
```

Spring容器通过为每一个HTTP请求使用 `loginAction` Bean定义来创建 `LoginAction` Bean的新实例。也就是说，`loginAction` Bean在HTTP请求层面上是有 scope 的。你可以随心所欲地改变被创建的实例的内部状态，因为从同一个 `loginAction` Bean定义中创建的其他实例不会看到这些状态的变化。它们是针对单个请求的。当请求完成处理时，该请求所涉及的Bean会被丢弃。

当使用注解驱动（annotation-driven）的组件或Java配置时，`@RequestScope` 注解可以用来将一个组件分配到 `request` scope。下面的例子展示了如何做到这一点。

```java
@RequestScope
@Component
public class LoginAction {
    // ...
}
```

#### Session

考虑以下用于Bean定义的XML配置。

```xml
<bean id="userPreferences" class="com.something.UserPreferences" scope="session"/>
```

Spring容器通过使用 `userPreferences` Bean定义，在单个HTTP `Session` 的生命周期内创建一个新的 `UserPreferences` Bean实例。换句话说，`userPreferences` Bean在HTTP `Session` 级别上是有效的scope。与 request scope 的Bean一样，你可以随心所欲地改变被创建的实例的内部状态，要知道其他HTTP `Session` 实例也在使用从同一个 `userPreferences` Bean定义中创建的实例，它们不会看到这些状态的变化，因为它们是特定于单个HTTP `Session`。当HTTP `Session` 最终被丢弃时，作用于该特定HTTP `Session` 的bean也被丢弃。

当使用注解驱动（annotation-driven）的组件或Java配置时，你可以使用 `@SessionScope` 注解来将一个组件分配到 `session` scope。

```java
@SessionScope
@Component
public class UserPreferences {
    // ...
}
```

#### Application

考虑以下用于Bean定义的XML配置。

```xml
<bean id="appPreferences" class="com.something.AppPreferences" scope="application"/>
```

Spring容器通过为整个Web应用程序使用一次 `appPreferences` Bean定义来创建 `AppPreferences` Bean的新实例。也就是说，`appPreferences` Bean是在 `ServletContext` 级别上的scope，并作为常规的 `ServletContext` 属性存储。这有点类似于Spring的 singleton Bean，但在两个重要方面有所不同。它是每个 `ServletContext` 的单例，而不是每个Spring `ApplicationContext`（在任何给定的Web应用程序中可能有几个），而且它实际上是暴露的，因此作为 `ServletContext` 属性可见。

当使用注解驱动（annotation-driven）的组件或Java配置时，你可以使用 `@ApplicationScope` 注解来将一个组件分配到 `application` scope。下面的例子显示了如何做到这一点。

```java
@ApplicationScope
@Component
public class AppPreferences {
    // ...
}
```

#### WebSocket

WebSocket scope 与WebSocket会话的生命周期相关，适用于通过 WebSocket 实现的 STOMP 应用程序，详情请参见 [WebSocket scope](https://springdoc.cn/spring/web.html#websocket-stomp-websocket-scope)。

#### 作为依赖

Spring IoC容器不仅管理对象（Bean）的实例化，而且还管理协作者（或依赖）的连接。如果你想把（例如）一个HTTP request scope 的Bean注入到另一个时间较长的scope的Bean中，你可以选择注入一个AOP代理来代替这个 scope 的Bean。也就是说，你需要注入一个代理对象，它暴露了与 scope 对象相同的公共接口，但它也可以从相关的scope（如HTTP request）中检索到真正的目标对象，并将方法调用委托给真正的对象。

> 你也可以在 scope 为 `singleton` 的Bean之间使用 `<aop:scoped-proxy/>`，引用会经过一个可序列化的中间代理，因此能够在反序列化时重新获得目标 singleton Bean。当针对scope 为 `prototype` 的Bean声明 `<aop:scoped-proxy/>` 时，对共享代理的每个方法调用都会导致创建一个新的目标实例，然后调用被转发到该实例。另外，scope代理并不是以生命周期安全的方式从较短的scope访问Bean的唯一方法。你也可以将你的注入点（也就是构造器或设置器参数或自动注入的字段）声明为 `ObjectFactory<MyTargetBean>`，允许在每次需要时通过 `getObject()` 调用来检索当前的实例—而不需要保留实例或单独存储它。作为一个扩展变量，你可以声明 `ObjectProvider<MyTargetBean>`，它提供了几个额外的访问变体，包括 `getIfAvailable` 和 `getIfUnique`。JSR-330 的变体被称为 `Provider`，并在每次检索时使用 `Provider<MyTargetBean>` 声明和相应的 `get()` 调用。关于JSR-330整体的更多细节，请看 [这里](https://springdoc.cn/spring/core.html#beans-standard-annotations)。

下面的例子中的配置只有一行，但理解其背后的 "为什么" 以及 "如何" 是很重要的。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- an HTTP Session-scoped bean exposed as a proxy -->
    <bean id="userPreferences" class="com.something.UserPreferences" scope="session">
        <!-- instructs the container to proxy the surrounding bean -->
        <aop:scoped-proxy/> 
    </bean>

    <!-- a singleton-scoped bean injected with a proxy to the above bean -->
    <bean id="userService" class="com.something.SimpleUserService">
        <!-- a reference to the proxied userPreferences bean -->
        <property name="userPreferences" ref="userPreferences"/>
    </bean>
</beans>
```

要创建这样的代理，你需要在一个 scope Bean定义中插入一个子 `<aop:scoped-proxy/>` 元素（参见 [选择要创建的代理类型](https://springdoc.cn/spring/core.html#beans-factory-scopes-other-injection-proxies) 和 [基于 XML Schema 的配置](https://springdoc.cn/spring/core.html#core.appendix.xsd-schemas)）。为什么在 `request`、`session` 和自定义 scope 层次上的Bean定义需要 `<aop:scoped-proxy/>` 元素？请考虑下面的 singleton Bean 定义，并与你需要为上述 scope 定义的内容进行对比（注意，下面的 `userPreferences` Bean定义是不完整的）。

```xml
<bean id="userPreferences" class="com.something.UserPreferences" scope="session"/>

<bean id="userManager" class="com.something.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
```

在前面的例子中，singleton Bean（`userManager`）被注入了对HTTP `Session` scope Bean（`userPreferences`）的引用。这里突出的一点是 `userManager` Bean是一个 singleton：它在每个容器中只被实例化一次，它的依赖关系（在这种情况下只有一个，即 `userPreferences` Bean）也只被注入一次。这意味着 `userManager` Bean只对完全相同的 `userPreferences` 对象（也就是它最初被注入的对象）进行操作。

当把一个生命周期较短的 scope Bean 注入一个生命周期较长的 scope Bean时，这不是你想要的行为（例如，把一个HTTP `Session` scope的协作Bean作为依赖关系注入 singleton Bean）。相反，你需要一个单例的 `userManager` 对象，而且，在 HTTP `Session` 的生命周期内，你需要一个特定于 HTTP `Session` 的 `userPreferences` 对象。因此，容器创建一个与 `UserPreferences` 类完全相同的公共接口的对象（最好是一个 `UserPreferences` 实例的对象），它可以从 scope 机制（HTTP request、`Session` 等）中获取真正的 `UserPreferences` 对象。容器将这个代理对象注入到 `userManager` Bean中，而 `userManager` Bean并不知道这个 `UserPreferences` 引用是一个代理。在这个例子中，当 `UserManager` 实例调用依赖注入的 `UserPreferences` 对象上的方法时，它实际上是调用了代理上的方法。然后，代理从（在这种情况下）HTTP `Session` 中获取真正的 `UserPreferences` 对象，并将方法调用委托给检索到的真正 `UserPreferences` 对象。

因此，在将 `request` scope 和 `session` scope 的Bean注入协作对象时，你需要以下（正确和完整的）配置，正如下面的例子所示。

```xml
<bean id="userPreferences" class="com.something.UserPreferences" scope="session">
    <aop:scoped-proxy/>
</bean>

<bean id="userManager" class="com.something.UserManager">
    <property name="userPreferences" ref="userPreferences"/>
</bean>
```

### 自定义

Bean的Scope机制是可扩展的。你可以定义你自己的Scope，甚至重新定义现有的Scope，尽管后者被认为是不好的做法，你不能覆盖内置的 `singleton` 和 `prototype` scope。

#### 创建

为了将你的自定义scope集成到 Spring 容器中，你需要实现 `org.springframework.beans.factory.config.Scope` 接口，本节将介绍该接口。要了解如何实现你自己的scope，请参阅 Spring 框架本身提供的 `Scope` 实现，以及 [`Scope`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/beans/factory/config/Scope.html) javadoc，其中更详细地解释了你需要实现的方法。

`Scope` 接口有四个方法来从scope中获取对象，从scope中移除对象，以及让对象被销毁。

例如，session scope的实现会返回 session scope 的 bean（如果它不存在，该方法会返回一个新的 bean 实例，在把它绑定到 session 上供将来引用）。下面的方法从底层scope返回对象。

```java
Object get(String name, ObjectFactory<?> objectFactory)
```

例如，session scope的实现是将session scope的Bean从底层session中移除。该对象应该被返回，但是如果没有找到指定名称的对象，你可以返回 `null`。下面的方法将对象从底层scope中删除。

```java
Object remove(String name)
```

下面的方法注册了一个callback，当scope被销毁或scope中的指定对象被销毁时，该callback应该被调用。

```java
void registerDestructionCallback(String name, Runnable destructionCallback)
```

请参阅 [javadoc](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/beans/factory/config/Scope.html#registerDestructionCallback) 或Spring scope 的实现，以了解更多关于销毁callback的信息。

下面的方法获得底层scope的conversation id。

```java
String getConversationId()
```

这个 id 对每个 scope 都是不同的。对于一个 session scope 的实现，这个 id 可以是 session id。

#### 使用

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

>   当你把 `<aop:scoped-proxy/>` 放在 `FactoryBean` 实现的 `<bean>` 声明中时，是 factory bean 本身被限定了scope，而不是从 `getObject()` 返回的对象。

#### 性质

## 继承

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

> `ApplicationContext` 默认预设了所有的singleton。因此，重要的是（至少对于singleton Bean来说），如果你有一个（父）Bean定义，你打算只作为模板使用，并且这个定义指定了一个类，你必须确保将 *abstract* 属性设置为 `true`，否则应用上下文将实际（试图）预实化 `abstract` Bean。

## 容器扩展点

### `BeanPostProcessor`

`BeanPostProcessor` 接口定义了回调方法，你可以实现这些方法来提供你自己的（或覆盖容器的默认）实例化逻辑、依赖性解析逻辑等。如果你想在Spring容器完成实例化、配置和初始化Bean之后实现一些自定义逻辑，你可以插入一个或多个自定义 `BeanPostProcessor` 实现。

你可以配置多个 `BeanPostProcessor` 实例，你可以通过设置 `order` 属性控制这些 `BeanPostProcessor` 实例的运行顺序。只有当 `BeanPostProcessor` 实现了 `Ordered` 接口时，你才能设置这个属性。如果你编写自己的 `BeanPostProcessor`，你也应该考虑实现 `Ordered` 接口。关于进一步的细节，请参阅 [`BeanPostProcessor`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/beans/factory/config/BeanPostProcessor.html) 和 [`Ordered`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/core/Ordered.html) 接口的 javadoc。也请参见关于 [`BeanPostProcessor` 实例的程序化注册](https://springdoc.cn/spring/core.html#beans-factory-programmatically-registering-beanpostprocessors) 的说明。

> 更多操作`BeanPostProcessor` 实例对Bean（或对象）实例进行操作。也就是说，Spring IoC容器实例化一个Bean实例，然后由 `BeanPostProcessor` 实例来完成其工作。`BeanPostProcessor` 实例是按容器范围的。这只有在你使用容器层次结构时才有意义。如果你在一个容器中定义了一个 `BeanPostProcessor`，它只对该容器中的Bean进行后处理。换句话说，在一个容器中定义的 `BeanPostProcessor` 不会对另一个容器中定义的 `BeanPostProcessor` 进行后处理，即使两个容器都是同一层次结构的一部分。要改变实际的Bean定义（即定义Bean的蓝图），你需要使用 `BeanFactoryPostProcessor`，如 [用 `BeanFactoryPostProcessor` 定制配置元数据](https://springdoc.cn/spring/core.html#beans-factory-extension-factory-postprocessors) 中所述。

`org.springframework.beans.factory.config.BeanPostProcessor` 接口正好由两个回调方法组成。当这样的类被注册为容器的后处理器时，对于容器创建的每个 bean 实例，后处理器在容器初始化方法（如 `InitializingBean.afterPropertiesSet()` 或任何已声明的 `init` 方法）被调用之前和任何 bean 初始化回调之后都会从容器获得一个回调。后处理程序可以对Bean实例采取任何行动，包括完全忽略回调。bean类后处理器通常会检查回调接口，或者用代理来包装bean类。一些Spring AOP基础设施类被实现为Bean后处理器，以提供代理封装逻辑。

`ApplicationContext` 会自动检测在配置元数据中定义的实现 `BeanPostProcessor` 接口的任何Bean。`ApplicationContext` 将这些 bean 注册为后处理器，以便以后在 bean 创建时可以调用它们。Bean后处理器可以像其他Bean一样被部署在容器中。

请注意，通过在配置类上使用 `@Bean` 工厂方法来声明 `BeanPostProcessor` 时，工厂方法的返回类型应该是实现类本身，或者至少是 `org.springframework.beans.factory.config.BeanPostProcessor` 接口，明确表示该 Bean 的后处理性质。否则，`ApplicationContext` 无法在完全创建它之前按类型自动检测它。由于 `BeanPostProcessor` 需要尽早被实例化，以便应用于上下文中其他Bean的初始化，所以这种早期的类型检测是至关重要的。

> 以编程方式注册 `BeanPostProcessor` 实例虽然推荐的 `BeanPostProcessor` 注册方法是通过 `ApplicationContext` 自动检测（如前所述），但你可以通过使用 `addBeanPostProcessor` 方法，针对 `ConfigurableBeanFactory` 以编程方式注册它们。当你需要在注册前评估条件逻辑或甚至在一个层次结构中跨上下文复制Bean Post处理器时，这可能很有用。然而，请注意，以编程方式添加的 `BeanPostProcessor` 实例并不尊重 `Ordered` 接口。这里，是注册的顺序决定了执行的顺序。还要注意的是，以编程方式注册的 `BeanPostProcessor` 实例总是在通过自动检测注册的实例之前被处理，而不考虑任何明确的顺序。

> `BeanPostProcessor` 实例和AOP自动代理实现了 `BeanPostProcessor` 接口的类是特殊的，会被容器区别对待。所有 `BeanPostProcessor` 实例和它们直接引用的Bean在启动时被实例化，作为 `ApplicationContext` 特殊启动阶段的一部分。接下来，所有的 `BeanPostProcessor` 实例被分类注册，并应用于容器中的所有其他Bean。因为AOP的自动代理是作为 `BeanPostProcessor` 本身实现的，所以 `BeanPostProcessor` 实例和它们直接引用的Bean都不符合自动代理的条件，因此，没有切面被织入进去。对于任何这样的Bean，你应该看到一个信息性的日志消息：`Bean someBean is not eligible for getting processed by all BeanPostProcessor interfaces (for example: not eligible for auto-proxying)`。如果你通过使用自动注入或 `@Resource`（可能会退回到自动注入）将 `BeanPostProcessor` 连接起来，Spring在搜索类型匹配的依赖候选者时可能会访问意想不到的Bean，因此，使它们没有资格进行自动代理或其他种类的Bean后处理。例如，如果你有一个用 `@Resource` 注解的依赖，其中字段或setter的名称不直接对应于Bean的声明名称，并且没有使用name属性，Spring会访问其他Bean以通过类型匹配它们。



## 路径扫描

本章中的大多数例子都使用XML来指定配置元数据，在Spring容器中产生每个 `BeanDefinition`。上一节（[基于注解的容器配置](https://springdoc.cn/spring/core.html#beans-annotation-config)）演示了如何通过源级注解提供大量的配置元数据。然而，即使在这些例子中，"基础" Bean定义也是在XML文件中明确定义的，而注解只驱动依赖性注入。本节描述了一个通过扫描classpath来隐式检测候选组件的选项。候选组件是与过滤器标准相匹配的类，并且有一个在容器中注册的相应的bean定义。这样就不需要使用 XML 来执行 bean 注册了。相反，你可以使用注解（例如 `@Component`）、AspectJ类型表达式或你自己的自定义过滤标准来选择哪些类在容器中注册了Bean定义。

### @Component

`@Repository` 注解是任何满足 repository（也被称为数据访问对象或 DAO）角色或 stereotype 的类的标记。这个标记的用途包括异常的自动翻译，如 [异常翻译](https://springdoc.cn/spring/data-access.html#orm-exception-translation) 中所述。

Spring提供了更多的 stereotype 注解。`@Component`, `@Service`, 和 `@Controller`。 `@Component` 是一个通用的stereotype，适用于任何Spring管理的组件。`@Repository`、 `@Service` 和 `@Controller` 是 `@Component` 的特殊化，用于更具体的使用情况（分别在持久层、服务层和表现层）。因此，你可以用 `@Component` 来注解你的组件类，但是，通过用 `@Repository`、`@Service` 或 `@Controller` 来注解它们，你的类更适合于被工具处理或与切面关联。例如，这些stereotype注解是指向性的理想目标。在Spring框架的未来版本中，`@Repository`、`@Service` 和 `@Controller` 还可以携带额外的语义。因此，如果你要在服务层使用 `@Component` 或 `@Service` 之间进行选择，`@Service` 显然是更好的选择。同样地，如前所述，`@Repository` 已经被支持作为持久层中自动异常翻译的标记。

### 自动检测

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

>   使用 `<context:component-scan>` 就隐含地实现了 `<context:annotation-config>` 的功能。在使用 `<context:component-scan>` 时，通常不需要包括 `<context:annotation-config>` 元素。

>   扫描classpath包需要classpath中存在相应的目录项。当你用Ant构建JAR时，确保你没有激活JAR任务的 files-only 开关。另外，在某些环境中，根据安全策略，classpath目录可能不会被暴露—例如，JDK 1.7.0_45及以上版本的独立应用程序（这需要在清单中设置 'Trusted-Library'，见 https://stackoverflow.com/questions/19394570/java-jre-7u45-breaks-classloader-getresources）。在JDK 9的模块路径（Jigsaw）上，Spring的classpath扫描一般都能按预期工作。但是，请确保你的组件类在你的 `module-info` 描述符中被导出。如果你希望Spring调用你的类中的非public成员，请确保它们是 "开放的"（也就是说，它们在你的 `module-info` 描述符中使用 `opens` 声明而不是 `exports` 声明）。

此外，当你使用组件扫描元素时，`AutowiredAnnotationBeanPostProcessor` 和 `CommonAnnotationBeanPostProcessor` 都被隐含地包括在内。这意味着这两个组件被自动检测并连接在一起—所有这些都不需要在XML中提供任何bean配置元数据。

> 你可以通过包含值为 `false` 的 `annotation-config` 属性来禁用 `AutowiredAnnotationBeanPostProcessor` 和 `CommonAnnotationBeanPostProcessor` 的注册。

### 自定义扫描

默认情况下，用 `@Component`、`@Repository`、`@Service`、`@Controller`、 `@Configuration` 注解的类，或者本身用 `@Component` 注解的自定义注解是唯一被检测到的候选组件。然而，你可以通过应用自定义 filter 来修改和扩展这种行为。将它们作为 `@ComponentScan` 注解的 `includeFilters` 或 `excludeFilters` 属性（或作为XML配置中 `<context:component-scan>` 元素的 `<context:include-filter />` 或 `<context:exclud-filter />` 子元素）。每个 filter 元素都需要 `type` 和 `expression` 属性。下表描述了过滤选项。

| Filter Type | 示例表达式                   | 说明                                                         |
| :---------- | :--------------------------- | :----------------------------------------------------------- |
| 注解 (默认) | `org.example.SomeAnnotation` | 一个注解在目标组件中的类型级别是 *present* 或 *meta-present*。 |
| 可指定      | `org.example.SomeClass`      | 目标组件可分配给（继承或实现）的一个类（或接口）。           |
| aspectj     | `org.example..*Service+`     | 要被目标组件匹配的 AspectJ type 表达式。                     |
| regex       | `org\.example\.Default.*`    | 一个与目标组件的类名相匹配的 regex expression。              |
| 自定义      | `org.example.MyTypeFilter`   | `org.springframework.core.type.TypeFilter` 接口的自定义实现。 |

下面的例子显示了配置忽略了所有 `@Repository` 注解，而使用 “stub” repository。

```java
@Configuration
@ComponentScan(basePackages = "org.example",
        includeFilters = @Filter(type = FilterType.REGEX, pattern = ".*Stub.*Repository"),
        excludeFilters = @Filter(Repository.class))
public class AppConfig {
    // ...
}
```

下面的列表显示了等效的XML。

```xml
<beans>
    <context:component-scan base-package="org.example">
        <context:include-filter type="regex"
                expression=".*Stub.*Repository"/>
        <context:exclude-filter type="annotation"
                expression="org.springframework.stereotype.Repository"/>
    </context:component-scan>
</beans>
```

> 你也可以通过在注解上设置 `useDefaultFilters=false` 或者提供 `use-default-filters="false"` 作为 `<component-scan/>` 元素的属性来禁用默认过滤器。这将有效地禁止对用 `@Component`、`@Repository`、`@Service`、`@Controller`、 `@RestController` 或 `@Configuration` 注解或元注解的类进行自动检测。

## 环境

[`Environment`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/core/env/Environment.html) 接口是一个集成在容器中的抽象，它对 application environment 的两个关键方面进行建模：[配置文件（profiles）](https://springdoc.cn/spring/core.html#beans-definition-profiles) 和 [属性（properties）](https://springdoc.cn/spring/core.html#beans-property-source-abstraction)。

profile是一个命名的、逻辑上的bean定义组，只有在给定的profile处于活动状态时才会在容器中注册。无论是用 XML 定义的还是用注解定义的，Bean 都可以被分配给一个profile。`Environment` 对象在profile方面的作用是确定哪些profile（如果有的话）是当前活动（active）的，以及哪些profile（如果有的话）应该是默认活动的。

属性（Properties）在几乎所有的应用程序中都扮演着重要的角色，它可能来自各种来源：properties 文件、JVM系统属性、系统环境变量、JNDI、Servlet上下文参数、特设的 `Properties` 对象、`Map` 对象等等。与属性有关的 `Environment` 对象的作用是为用户提供一个方便的服务接口，用于配置属性源并从它们那里解析属性。

### Bean自定义

Bean定义配置（Bean definition profiles） 在核心容器中提供了一种机制，允许在不同的环境中注册不同的bean。“环境”这个词对不同的用户来说意味着不同的东西，而这个功能可以帮助许多用例，包括。

+ 在开发中针对内存中的数据源工作，而在QA或生产中从JNDI查找相同的数据源。
+ 仅在将应用程序部署到 performance 环境中时才注册监控基础设施。
+ 为 customer A 与 customer B 的部署注册定制的bean实现。

考虑一下实际应用中的第一个用例，它需要一个 `DataSource`。在一个测试环境中，配置可能类似于以下。

```java
@Bean
public DataSource dataSource() {
    return new EmbeddedDatabaseBuilder()
        .setType(EmbeddedDatabaseType.HSQL)
        .addScript("my-schema.sql")
        .addScript("my-test-data.sql")
        .build();
}
```

现在考虑如何将这个应用程序部署到QA或production环境中，假设该应用程序的数据源是在生产应用服务器的JNDI目录下注册的。我们的 `dataSource` bean现在看起来如下。

```java
@Bean(destroyMethod = "")
public DataSource dataSource() throws Exception {
    Context ctx = new InitialContext();
    return (DataSource) ctx.lookup("java:comp/env/jdbc/datasource");
}
```

问题是如何根据当前环境在使用这两种变化之间切换。随着时间的推移，Spring用户已经设计出了许多方法来完成这一工作，通常是依靠系统环境变量和包含 `${placeholder}` 标记的XML `<import/>` 语句的组合，根据环境变量的值解析到正确的配置文件路径。Bean定义配置是一个核心的容器功能，为这个问题提供了一个解决方案。

如果我们把前面的例子中显示的环境特定的Bean定义的用例进行概括，我们最终需要在某些情况下注册某些Bean定义，但在其他情况下不需要。你可以说，你想在情况A中注册某种类型的bean定义，而在情况B中注册另一种类型。

#### @Profile

[`@Profile`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/context/annotation/Profile.html) 注解让你表明当一个或多个指定的配置文件处于活动状态时，一个组件就有资格注册。使用我们前面的例子，我们可以重写 `dataSource` 配置如下。

```java
@Configuration
@Profile("development")
public class StandaloneDataConfig {

    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.HSQL)
            .addScript("classpath:com/bank/config/sql/schema.sql")
            .addScript("classpath:com/bank/config/sql/test-data.sql")
            .build();
    }
}
```

```java
@Configuration
@Profile("production")
public class JndiDataConfig {

    @Bean(destroyMethod = "") 
    public DataSource dataSource() throws Exception {
        Context ctx = new InitialContext();
        return (DataSource) ctx.lookup("java:comp/env/jdbc/datasource");
    }
}
```

>    `@Bean(destroyMethod = "")` 禁用默认的销毁方法推理。

> 如前所述，对于 `@Bean` 方法，你通常会选择使用程序化的JNDI查找，通过使用Spring的 `JndiTemplate`/`JndiLocatorDelegat` helper 或前面显示的直接使用JNDI `InitialContext`，但不使用 `JndiObjectFactoryBean` 的变体，这将迫使你将返回类型声明为 `FactoryBean` 类型。

profile 字符串可以包含一个简单的 profile 名称（例如，`production`）或一个 profile 表达式。profile 表达式允许表达更复杂的 profile 逻辑（例如，`production & us-east`）。profile 表达式中支持以下运算符。

+ `!`: profile的 `NOT` 逻辑
+ `&`: profile的 `AND` 的逻辑
+ `|`: profile的 `OR` 的逻辑

>   你不能在不使用括号的情况下混合使用 `&` 和 `|` 运算符。例如，`production & us-east | eu-central` 不是一个有效的表达。它必须表示为 `production & (us-east | eu-central)`。

你可以使用 `@Profile` 作为 [元注解](https://springdoc.cn/spring/core.html#beans-meta-annotations)，以创建一个自定义的组成注解。下面的例子定义了一个自定义的 `@Production` 注解，你可以把它作为 `@Profile("production")` 的直接替换。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Profile("production")
public @interface Production {
}
```

>   如果一个 `@Configuration` 类被标记为 `@Profile`，所有与该类相关的 `@Bean` 方法和 `@Import` 注解都会被绕过，除非一个或多个指定的 profiles 处于激活状态。如果一个 `@Component` 或 `@Configuration` 类被标记为 `@Profile({"p1", "p2"})`，该类不会被注册或处理，除非 profiles "p1" 或 "p2" 已经被激活。如果一个给定的profiles前缀为NOT操作符（`!`），那么只有在该profiles没有激活的情况下，才会注册被注解的元素。例如，给定 `@Profile({"p1", "!p2"})`，如果profile 'p1' 被激活或 profile 'p2' 未被激活，注册将发生。

`@Profile` 也可以在方法层面上声明，以便只包括一个配置类的一个特定Bean（例如，对于一个特定Bean的备选变体），正如下面的例子所示。

```java
@Configuration
public class AppConfig {

    @Bean("dataSource")
    @Profile("development") 
    public DataSource standaloneDataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.HSQL)
            .addScript("classpath:com/bank/config/sql/schema.sql")
            .addScript("classpath:com/bank/config/sql/test-data.sql")
            .build();
    }

    @Bean("dataSource")
    @Profile("production") 
    public DataSource jndiDataSource() throws Exception {
        Context ctx = new InitialContext();
        return (DataSource) ctx.lookup("java:comp/env/jdbc/datasource");
    }
}
```

> `StandaloneDataSource` 方法只在 `development` profile 中可用。
>
> `jndiDataSource` 方法只在 `production` profile 中可用。

> 对于 `@Bean` 方法的 `@Profile`，一个特殊的情况可能适用。在同一Java方法名的重载 `@Bean` 方法的情况下（类似于构造函数重载），`@Profile` 条件需要在所有重载的方法上一致声明。如果条件不一致，那么在重载的方法中，只有第一个声明的条件才是重要的。因此，`@Profile` 不能被用来选择具有特定参数签名的重载方法而不是另一个。同一个Bean的所有工厂方法之间的解析遵循Spring在创建时的构造函数解析算法。如果你想定义具有不同概况条件的备选Bean，请使用不同的Java方法名，通过使用 `@Bean` name 属性指向同一个Bean名称，如前面的例子所示。如果参数签名都是一样的（例如，所有的变体都有无参数的工厂方法），这是首先在一个有效的Java类中表示这种安排的唯一方法（因为一个特定名称和参数签名的方法只能有一个）。

#### xml 配置

XML的对应部分是 `<beans>` 元素的 `profile` 属性。我们前面的配置样本可以用两个XML文件重写，如下。

```xml
<beans profile="development"
    xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:jdbc="http://www.springframework.org/schema/jdbc"
    xsi:schemaLocation="...">

    <jdbc:embedded-database id="dataSource">
        <jdbc:script location="classpath:com/bank/config/sql/schema.sql"/>
        <jdbc:script location="classpath:com/bank/config/sql/test-data.sql"/>
    </jdbc:embedded-database>
</beans>
<beans profile="production"
    xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:jee="http://www.springframework.org/schema/jee"
    xsi:schemaLocation="...">

    <jee:jndi-lookup id="dataSource" jndi-name="java:comp/env/jdbc/datasource"/>
</beans>
```

也可以避免这种分割，在同一个文件中嵌套 `<beans/>` 元素，如下例所示。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:jdbc="http://www.springframework.org/schema/jdbc"
    xmlns:jee="http://www.springframework.org/schema/jee"
    xsi:schemaLocation="...">

    <!-- other bean definitions -->

    <beans profile="development">
        <jdbc:embedded-database id="dataSource">
            <jdbc:script location="classpath:com/bank/config/sql/schema.sql"/>
            <jdbc:script location="classpath:com/bank/config/sql/test-data.sql"/>
        </jdbc:embedded-database>
    </beans>

    <beans profile="production">
        <jee:jndi-lookup id="dataSource" jndi-name="java:comp/env/jdbc/datasource"/>
    </beans>
</beans>
```

`spring-bean.xsd` 已经被限制了，只允许在文件中最后出现这样的元素。这应该有助于提供灵活性，而不会在XML文件中产生混乱。

> 对应的XML不支持前面描述的 profile 表达式。然而，可以通过使用 `!` 操作符来否定一个 profile。也可以通过嵌套 profile 来应用逻辑 “and”，如下面的例子所示。`<beans xmlns="http://www.springframework.org/schema/beans"    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"    xmlns:jdbc="http://www.springframework.org/schema/jdbc"    xmlns:jee="http://www.springframework.org/schema/jee"    xsi:schemaLocation="...">     <!-- other bean definitions -->     <beans profile="production">        <beans profile="us-east">            <jee:jndi-lookup id="dataSource" jndi-name="java:comp/env/jdbc/datasource"/>        </beans>    </beans> </beans>`在前面的例子中，如果 `production` 和 `us-east` profiles都处于活动状态，那么 `dataSource` Bean就会被暴露。

#### 激活

现在我们已经更新了我们的配置，我们仍然需要指示Spring哪个profile是激活的。如果我们现在启动我们的示例应用程序，我们会看到一个 `NoSuchBeanDefinitionException` 被抛出，因为容器找不到名为 `dataSource` 的Spring Bean。

激活一个 profile 可以通过几种方式进行，但最直接的是以编程方式对环境API进行激活，该API可以通过 `ApplicationContext` 获得。下面的例子显示了如何做到这一点。

```java
AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
ctx.getEnvironment().setActiveProfiles("development");
ctx.register(SomeConfig.class, StandaloneDataConfig.class, JndiDataConfig.class);
ctx.refresh();
```

此外，你还可以通过 `spring.profiles.active` 属性声明性地激活profiles，它可以通过系统环境变量、JVM系统属性、`web.xml` 中的servlet上下文参数，甚至作为JNDI中的一个条目来指定（参见 [`PropertySource` 抽象](https://springdoc.cn/spring/core.html#beans-property-source-abstraction)）。在集成测试中，可以通过使用 `spring-test` 模块中的 `@ActiveProfiles` 注解来声明活动profiles（见 [environment profiles 的 context 配置](https://springdoc.cn/spring/testing.html#testcontext-ctx-management-env-profiles)）。

请注意，profiles 不是一个 "非此即彼" 的命题。你可以同时激活多个profiles。在程序上，你可以向 `setActiveProfiles()` 方法提供多个profiles名称，该方法接受 `String…` 可变参数。下面的例子激活了多个profiles。

```java
ctx.getEnvironment().setActiveProfiles("profile1", "profile2");
```

在声明上，`spring.profiles.active` 可以接受一个用逗号分隔的 profile 名称列表，正如下面的例子所示。

```shell
-D spring.profiles.active="profile1,profile2"
```

#### 默认

默认 profile 代表默认启用的 profile。考虑一下下面的例子。

```java
@Configuration
@Profile("default")
public class DefaultDataConfig {

    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.HSQL)
            .addScript("classpath:com/bank/config/sql/schema.sql")
            .build();
    }
}
```

如果没有激活profile，就会创建 `dataSource`。你可以把它看作是为一个或多个Bean提供默认定义的一种方式。如果任何profile被启用，默认的profile就不应用。

你可以通过在环境中使用 `setDefaultProfiles()` 来改变默认配置文件的名称，或者通过声明性地使用 `spring.profiles.default` 属性。

###  `PropertySource` 

Spring的 `Environment` 抽象提供了对可配置的属性源层次结构的搜索操作。考虑一下下面的列表。

```java
ApplicationContext ctx = new GenericApplicationContext();
Environment env = ctx.getEnvironment();
boolean containsMyProperty = env.containsProperty("my-property");
System.out.println("Does my environment contain the 'my-property' property? " + containsMyProperty);
```

在前面的片段中，我们看到了一种询问Spring的高级方式，即询问 `my-property` 属性是否为当前环境所定义。为了回答这个问题，`Environment` 对象在一组 [`PropertySource`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/core/env/PropertySource.html) 对象上执行搜索。`PropertySource` 是对任何key值对来源的简单抽象，Spring的 [`StandardEnvironment`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/core/env/StandardEnvironment.html) 配置了两个 `PropertySource` 对象—一个代表JVM系统属性集合（`System.getProperties()`），一个代表系统环境变量集合（`System.getenv()`）。

> 这些默认的属性源存在于 `StandardEnvironment` 中，用于独立的应用程序中。 [`StandardServletEnvironment`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/web/context/support/StandardServletEnvironment.html) 被填充了额外的默认属性源，包括servlet config、servlet context参数，以及 [`JndiPropertySource`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/jndi/JndiPropertySource.html)（如果JNDI可用）。

具体来说，当你使用 `StandardEnvironment` 时，如果运行时存在 `my-property` 系统属性或 `my-property` 环境变量，调用 `env.containsProperty("my-property"`) 会返回 `true`。

>   执行的搜索是分层次的。默认情况下，系统属性（system properties）比环境变量有优先权。因此，如果在调用 `env.getProperty("my-property")` 时，`my-property` 属性恰好在两个地方都被设置了，那么系统属性值 "胜出" 并被返回。请注意，属性值不会被合并，而是被前面的条目完全覆盖。对于一个普通的 `StandardServletEnvironment` 来说，完整的层次结构如下，最高优先级的条目在顶部。`ServletConfig` 参数（如果适用 - 例如，在 `DispatcherServlet` 上下文的情况下）。`ServletContext` 参数（web.xml的context-param条目）.JNDI环境变量（`java:comp/env/` 条目）。JVM系统属性（`-D` 命令行参数）。JVM系统环境（操作系统环境变量）.

最重要的是，整个机制是可配置的。也许你有一个自定义的属性源，你想集成到这个搜索中。要做到这一点，实现并实例化你自己的 `PropertySource`，并将其添加到当前环境的 `PropertySources` 集合中。下面的例子展示了如何做到这一点。

```java
ConfigurableApplicationContext ctx = new GenericApplicationContext();
MutablePropertySources sources = ctx.getEnvironment().getPropertySources();
sources.addFirst(new MyPropertySource());
```

在前面的代码中，`MyPropertySource` 在搜索中被加入了最高优先级。如果它包含 `my-property` 属性，该属性将被检测并返回，而不是任何其他 `PropertySource` 中的任何 `my-property` 属性。 [`MutablePropertySources`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/core/env/MutablePropertySources.html) API暴露了许多方法，允许精确地操作属性源（property sources）的集合。

#### 占位符解析

历史上，元素中占位符的值只能通过JVM系统属性或环境变量来解析。现在的情况不再是这样了。因为 `Environment` 抽象被集成到整个容器中，所以很容易通过它来解决占位符的问题。这意味着你可以以任何你喜欢的方式配置解析过程。你可以改变通过系统属性和环境变量搜索的优先级，或者完全删除它们。你也可以酌情将你自己的属性源添加到组合中。

具体来说，无论 `customer` 属性在哪里定义，只要它在 `Environment` 中是可用的，下面的语句就能发挥作用。

```xml
<beans>
    <import resource="com/bank/service/${customer}-config.xml"/>
</beans>
```

## `LoadTimeWeaver`

`LoadTimeWeaver` 被Spring用来在类被加载到Java虚拟机（JVM）时进行动态转换。

要启用 load-time 织入，你可以把 `@EnableLoadTimeWeaving` 添加到你的一个 `@Configuration` 类中，如下例所示。

```java
@Configuration
@EnableLoadTimeWeaving
public class AppConfig {
}
```

另外，对于XML配置，你可以使用 `context:load-time-weaver` 元素。

```xml
<beans>
    <context:load-time-weaver/>
</beans>
```

一旦为 `ApplicationContext` 配置，该 `ApplicationContext` 中的任何Bean都可以实现 `LoadTimeWeaverAware`，从而接收对 load-time 织入实例的引用。这在与 [Spring的JPA支持](https://springdoc.cn/spring/data-access.html#orm-jpa) 相结合时特别有用，因为JPA类的转换可能需要 load-time 织入。请参考 [`LocalContainerEntityManagerFactoryBean`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/orm/jpa/LocalContainerEntityManagerFactoryBean.html) javadoc以了解更多细节。关于AspectJ load-time 织入的更多信息，请参见 [在Spring框架中用AspectJ进行加载时织入（Load-time Weaving）](https://springdoc.cn/spring/core.html#aop-aj-ltw)。



# 面向切面

面向切面的编程（AOP）通过提供另一种思考程序结构的方式来补充面向对象的编程（OOP）。OOP中模块化的关键单位是类，而AOP中模块化的单位是切面。切面使跨越多种类型和对象的关注点（如事务管理）模块化。（这样的关注点在AOP文献中通常被称为 "交叉（crosscutting）" 关注点）。

Spring的关键组件之一是AOP框架。虽然Spring IoC容器并不依赖于AOP（意味着如果你不想使用AOP，就不需要使用），但AOP补充了Spring IoC，提供了一个非常有能力的中间件解决方案。

Spring AOP 和 AspectJ pointcuts

Spring通过使用 [基于schema的方法](https://springdoc.cn/spring/core.html#aop-schema) 或 [`@AspectJ` 注解风格](https://springdoc.cn/spring/core.html#aop-ataspectj)，提供了简单而强大的编写自定义切面的方法。这两种风格都提供了完全类型化的advice，并使用 AspectJ 的 pointcut 语言，同时仍然使用Spring AOP进行织入。

本章讨论了基于 schema 和 `@AspectJ` 的AOP支持。[下一章](https://springdoc.cn/spring/core.html#aop-api) 将讨论低级别的AOP支持。

AOP在Spring框架中被用于：

+ 提供声明式的企业服务。最重要的此类服务是 [声明式事务管理](https://springdoc.cn/spring/data-access.html#transaction-declarative)。
+ 让用户实现自定义切面，用AOP补充他们对OOP的使用。

**功能**

Spring AOP是用纯Java实现的。不需要特殊的编译过程。Spring AOP不需要控制类加载器的层次结构，因此适合在Servlet容器或应用服务器中使用。

Spring AOP目前只支持方法执行连接点（advice 在Spring Bean上执行方法）。字段拦截没有实现，尽管可以在不破坏Spring AOP核心API的情况下增加对字段拦截的支持。如果你需要advice字段访问和更新连接点，可以考虑使用AspectJ这样的语言。

Spring AOP的AOP方法与其他大多数AOP框架不同。其目的不是提供最完整的AOP实现（尽管Spring AOP的能力很强）。相反，其目的是在AOP实现和Spring IoC之间提供一个紧密的整合，以帮助解决企业应用中的常见问题。

因此，例如，Spring框架的AOP功能通常是与Spring IoC容器一起使用的。切面是通过使用正常的Bean定义语法来配置的（尽管这允许强大的 "自动代理" 功能）。这是与其他AOP实现的一个重要区别。你不能用Spring AOP轻松或有效地做一些事情，比如advice非常细粒度的对象（通常是domain对象）。在这种情况下，AspectJ是最好的选择。然而，我们的经验是，Spring AOP为企业级Java应用中的大多数问题提供了一个很好的解决方案，这些问题是可以用AOP来解决的。

Spring AOP从未努力与AspectJ竞争，以提供一个全面的AOP解决方案。我们认为，Spring AOP等基于代理的框架和AspectJ等完整的框架都很有价值，它们是互补的，而不是竞争的。Spring将Spring AOP和IoC与AspectJ无缝集成，以便在基于Spring的应用架构中实现AOP的所有用途。这种整合并不影响Spring AOP API或AOP联盟的API。Spring AOP仍然是向后兼容的。关于Spring AOP API的讨论，请参见 [下一章](https://springdoc.cn/spring/core.html#aop-api)。

>   Spring框架的核心原则之一是非侵入性。这是一个想法，即你不应该被迫在你的业务或domain模型中引入框架特定的类和接口。然而，在某些地方，Spring框架确实让你选择将Spring框架特定的依赖关系引入你的代码库。给你这样的选项的理由是，在某些情况下，用这样的方式来阅读或编码一些特定的功能可能会更容易。然而，Spring框架（几乎）总是为你提供选择：你可以自由地做出明智的决定，哪种选择最适合你的特定用例或情景。与本章相关的一个选择是，选择哪种AOP框架（以及哪种AOP风格）。你可以选择AspectJ、Spring AOP或两者。你还可以选择 `@AspectJ` 注解式方法或Spring XML配置式方法。本章选择首先介绍 `@AspectJ` 风格的方法，这不应该被认为是Spring团队倾向于 `@AspectJ` 注解风格的方法，而不是Spring XML配置风格。参见 “[选择使用哪种AOP声明风格](https://springdoc.cn/spring/core.html#aop-choosing)”，以了解对每种风格的优缺点的更完整讨论。

## 概念

让我们首先定义一些核心的AOP概念和术语。这些术语并不是针对Spring的。不幸的是，AOP的术语并不是特别直观。然而，如果Spring使用自己的术语，那就更令人困惑了。

+ Aspect（切面）: 一个跨越多个类的关注点的模块化。事务管理是企业级Java应用中横切关注点的一个很好的例子。在Spring AOP中，切面是通过使用常规类（基于 [schema 的方法](https://springdoc.cn/spring/core.html#aop-schema)）或使用 `@Aspect` 注解的常规类（[@AspectJ](https://springdoc.cn/spring/core.html#aop-ataspectj) 风格）实现的。
+ Join point: 程序执行过程中的一个点，例如一个方法的执行或一个异常的处理。在Spring AOP中，一个连接点总是代表一个方法的执行。
+ Advice: 一个切面在一个特定的连接点采取的行动。不同类型的advice包括 "around"、"before" 和 "after" 的advice（Advice 类型将在后面讨论）。许多AOP框架，包括Spring，都将advice建模为一个拦截器，并在连接点（Join point）周围维护一个拦截器链。
+ Pointcut: 一个匹配连接点的谓词（predicate）。advice与一个切点表达式相关联，并在切点匹配的任何连接点上运行（例如，执行一个具有特定名称的方法）。由切点表达式匹配的连接点概念是AOP的核心，Spring默认使用AspectJ的切点表达式语言。
+ Introduction: 代表一个类型声明额外的方法或字段。Spring AOP允许你为任何 advice 的对象引入新的接口（以及相应的实现）。例如，你可以使用引入来使一个bean实现 `IsModified` 接口，以简化缓存。（介绍在AspectJ社区中被称为类型间声明）。
+ Target object: 被一个或多个切面所 advice 的对象。也被称为 "advised object"。由于Spring AOP是通过使用运行时代理来实现的，这个对象总是一个被代理的对象。
+ AOP proxy: 一个由AOP框架创建的对象，以实现切面契约（advice 方法执行等）。在Spring框架中，AOP代理是一个JDK动态代理或CGLIB代理。
+ Weaving（织入）: 将aspect与其他应用程序类型或对象连接起来，以创建一个 advice 对象。这可以在编译时（例如，使用AspectJ编译器）、加载时或运行时完成。Spring AOP和其他纯Java AOP框架一样，在运行时进行织入。

Spring AOP包括以下类型的advice。

+ Before advice: 在连接点之前运行的Advice ，但它不具备以下能力 阻止执行流进行到 join point 的能力（除非它抛出一个异常）。
+ After returning advice: 在一个连接点正常完成后运行的Advice （例如，如果一个方法返回时没有抛出一个异常）。
+ After (finally) advice: 无论连接点以何种方式退出（正常或特殊返回），都要运行该advice。
+ Around advice: 围绕一个连接点的advice，如方法调用。这是最强大的一种advice。Around advice可以在方法调用之前和之后执行自定义行为。它还负责选择是否继续进行连接点或通过返回自己的返回值或抛出一个异常来缩短advice方法的执行。

Around advice 是最通用的一种advice。由于Spring AOP和AspectJ一样，提供了完整的advice类型，我们建议你使用功能最少的advice类型来实现所需的行为。例如，如果你只需要用一个方法的返回值来更新缓存，那么你最好实现一个 after returning advice，而不是一个 around advice，尽管 around advice 可以完成同样的事情。使用最具体的 advice 类型可以提供一个更简单的编程模型，减少错误的可能性。例如，你不需要在用于 around advice 的 `JoinPoint` 上调用 `proceed()` 方法，因此，你不能失败地调用它。

所有的advice参数都是静态类型的，因此你可以使用适当类型的advice参数（例如，方法执行的返回值的类型）而不是 `Object` 数组。

由 `pointcuts` 匹配的连接点的概念是AOP的关键，它区别于只提供拦截的旧技术。点切使advice能够独立于面向对象的层次结构而成为目标。例如，你可以将提供声明性事务管理的 around advice 应用于跨越多个对象的一组方法（如 service 层的所有业务操作）。

## AOP 代理

Spring AOP默认使用标准的JDK动态代理进行AOP代理。这使得任何接口（或一组接口）都可以被代理。

Spring AOP也可以使用CGLIB代理。这对于代理类而不是接口来说是必要的。默认情况下，如果一个业务对象没有实现一个接口，就会使用CGLIB。由于对接口而不是类进行编程是很好的做法，业务类通常实现一个或多个业务接口。在那些（希望是罕见的）需要向没有在接口上声明的方法提供advice的情况下，或者在需要将代理对象作为具体类型传递给方法的情况下，可以 [强制使用 CGLIB](https://springdoc.cn/spring/core.html#aop-proxying)。

掌握Spring AOP是基于代理的事实是很重要的。请参阅 “[了解AOP代理](https://springdoc.cn/spring/core.html#aop-understanding-aop-proxies)”，以深入研究这一实现细节的实际含义。

Spring AOP是基于代理的。在你编写自己的切面或使用Spring框架提供的任何基于Spring AOP的切面之前，你必须掌握最后一句话的语义，这一点至关重要。

首先考虑这样一种情况：你有一个普通的、没有代理的、没什么特别的、直接的对象引用，正如下面的代码片段所示。

```java
public class SimplePojo implements Pojo {

    public void foo() {
        // this next method invocation is a direct call on the 'this' reference
        this.bar();
    }

    public void bar() {
        // some logic...
    }
}
```

如果你在一个对象引用上调用一个方法，该方法就会直接在该对象引用上被调用，正如下面的图片和列表所示。

![aop proxy plain pojo call](https://springdoc.cn/spring/images/aop-proxy-plain-pojo-call.png)

```java
public class Main {

    public static void main(String[] args) {
        Pojo pojo = new SimplePojo();
        // this is a direct method call on the 'pojo' reference
        pojo.foo();
    }
}
```

当客户端代码拥有的引用是一个代理时，情况会略有变化。请看下面的图和代码片段。

![aop proxy call](https://springdoc.cn/spring/images/aop-proxy-call.png)

```java
public class Main {

    public static void main(String[] args) {
        ProxyFactory factory = new ProxyFactory(new SimplePojo());
        factory.addInterface(Pojo.class);
        factory.addAdvice(new RetryAdvice());

        Pojo pojo = (Pojo) factory.getProxy();
        // this is a method call on the proxy!
        pojo.foo();
    }
}
```

这里需要理解的关键是，在 `Main` 类的 `main(..)` 方法里面的客户代码有一个对代理的引用。这意味着，对该对象引用的方法调用就是对代理的调用。因此，代理可以委托给与该特定方法调用相关的所有拦截器（advice）。然而，一旦调用最终到达目标对象（本例中的 `SimplePojo` 引用），它可能对自身进行的任何方法调用，如 `this.bar()` 或 `this.foo()`，都将被调用到 `this` 引用，而不是代理。这有很重要的意义。这意味着自我调用不会导致与方法调用相关的advice得到运行的机会。

好吧，那么对此该怎么做呢？最好的方法（这里的 "最好" 一词用得很宽泛）是重构你的代码，使自我调用不会发生。这确实需要你做一些工作，但这是最好的、最不具侵入性的方法。下一个方法绝对是可怕的，我们不愿意指出它，正是因为它是如此可怕。你可以（对我们来说很痛苦）将你的类中的逻辑完全与Spring AOP联系起来，正如下面的例子所示。

```java
public class SimplePojo implements Pojo {

    public void foo() {
        // this works, but... gah!
        ((Pojo) AopContext.currentProxy()).bar();
    }

    public void bar() {
        // some logic...
    }
}
```

这完全将你的代码与Spring AOP联系在一起，而且它使类本身意识到它是在AOP上下文中使用的，这与AOP背道而驰。它还需要在创建代理时进行一些额外的配置，正如下面的例子所示。

```java
public class Main {

    public static void main(String[] args) {
        ProxyFactory factory = new ProxyFactory(new SimplePojo());
        factory.addInterface(Pojo.class);
        factory.addAdvice(new RetryAdvice());
        factory.setExposeProxy(true);

        Pojo pojo = (Pojo) factory.getProxy();
        // this is a method call on the proxy!
        pojo.foo();
    }
}
```

最后，必须指出的是，AspectJ不存在这种自我调用的问题，因为它不是一个基于代理的AOP框架。

### @AspectJ

`@AspectJ` 指的是将aspect作为带有注解的普通Java类来声明的一种风格。`@AspectJ` 风格是由 [AspectJ项目](https://www.eclipse.org/aspectj) 引入的，作为AspectJ 5版本的一部分。Spring解释了与AspectJ 5相同的注解，使用AspectJ提供的库进行 pointcut 解析和匹配。不过，AOP运行时仍然是纯粹的Spring AOP，而且不依赖AspectJ编译器或织入器（weaver）。

#### 启动

要在Spring配置中使用 `@AspectJ` 切面，你需要启用Spring支持，以根据 `@AspectJ` 切面配置Spring AOP，并根据Bean是否被这些切面advice而自动代理。通过自动代理，我们的意思是，如果Spring确定一个Bean被一个或多个切面所advice，它就会自动为该Bean生成一个代理来拦截方法调用，并确保 advice 在需要时被运行。

`@AspectJ` 支持可以通过XML或Java风格的配置来启用。在这两种情况下，您还需要确保AspectJ的 `aspectjweaver.jar` 库在你应用程序的classpath上（1.9或更高版本）。该库可在AspectJ发行版的 `lib` 目录中找到，也可从Maven Central仓库中找到。

要用Java `@Configuration` 启用 `@AspectJ` 支持，请添加 `@EnableAspectJAutoProxy` 注解，如下例所示。

```java
@Configuration
@EnableAspectJAutoProxy
public class AppConfig {
}
```

要用基于XML的配置启用 `@AspectJ` 支持，请使用 `aop:aspectj-autoproxy` 元素，如下例所示。

```xml
<aop:aspectj-autoproxy/>
```

这假定你使用了 [基于XML Schema的配置](https://springdoc.cn/spring/core.html#core.appendix.xsd-schemas)中描述的模式支持。关于如何导入 `aop` 命名空间中的标签，请参见 [AOP schema](https://springdoc.cn/spring/core.html#core.appendix.xsd-schemas-aop)。

#### 声明

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

> 通过组件扫描自动检测切面你可以通过 `@Configuration` 类中的 `@Bean` 方法，将aspect类注册为Spring XML配置中的普通Bean，或者让Spring通过classpath扫描来自动检测它们—与任何其他Spring管理的Bean一样。然而，请注意，`@Aspect` 注解并不足以实现classpath中的自动检测。为此，你需要添加一个单独的 `@Component` 注解（或者，根据Spring组件扫描器的规则，添加一个符合条件的自定义 stereotype 注解）。

>   advice切面与其他切面？在Spring AOP中，切面本身不能成为其他切面的advice的目标。类上的 `@Aspect` 注解将其标记为一个切面，因此，将其排除在自动代理之外。

## 机制

Spring AOP使用JDK动态代理或CGLIB来为特定的目标对象创建代理。JDK动态代理是内置于JDK中的，而CGLIB是一个普通的开源类定义库（重新打包到 `spring-core` 中）。

如果要代理的目标对象至少实现了一个接口，就会使用JDK动态代理。目标类型实现的所有接口都被代理了。如果目标对象没有实现任何接口，就会创建一个CGLIB代理。

如果你想强制使用CGLIB代理（例如，代理为目标对象定义的每个方法，而不仅仅是其接口实现的方法），你可以这样做。然而，你应该考虑以下问题。

+ 使用CGLIB，`final` 方法不能被advice，因为它们不能在运行时生成的子类中被重写。
+ 从Spring 4.0开始，代理对象的构造器不再被调用两次，因为CGLIB代理实例是通过 `Objenesis` 创建的。只有当你的JVM不允许绕过构造器时，你才可能看到双重调用和Spring的AOP支持的相应debug日志条目。

要强制使用 CGLIB 代理，请将 `<aop:config>` 元素的 `proxy-target-class` 属性的值设为 `true`，如下所示。

```xml
<aop:config proxy-target-class="true">
    <!-- other beans defined here... -->
</aop:config>
```

当你使用@AspectJ自动代理支持时，要强制CGLIB代理，请将 `<aop:aspectj-autoproxy>` 元素的 `proxy-target-class` 属性设置为 `true`，如下所示。

```xml
<aop:aspectj-autoproxy proxy-target-class="true"/>
```

> 多个 `<aop:config/>` 部分在运行时被折叠成一个统一的自动代理创建者，它应用任何 `<aop:config/>` 部分（通常来自不同的 XML Bean 定义文件）指定的最强代理设置。这也适用于 `<tx:annotation-driven/>` 和 `<aop:aspectj-autoproxy/>` 元素。明确地说，在 `<tx:annotation-driven/>`, `<aop:aspectj-autoproxy/>`, 或 `<aop:config/>` 元素上使用 `proxy-target-class="true"` 将强制使用 CGLIB 的代理。

# 资源

# 数据

