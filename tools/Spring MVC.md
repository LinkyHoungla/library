# 概述

Spring Web MVC 是基于 Servlet API 构建的原始 Web 框架，从一开始就包含在 Spring 框架中。正式名称“Spring Web MVC”来自其源模块的名称 ( [`spring-webmvc`](https://github.com/spring-projects/spring-framework/tree/main/spring-webmvc))，但更常见的名称是“Spring MVC”。

与 Spring Web MVC 并行，Spring Framework 5.0 引入了一个反应式堆栈 Web 框架，其名称“Spring WebFlux”也基于其源模块 ( [`spring-webflux`](https://github.com/spring-projects/spring-framework/tree/main/spring-webflux))。

Spring Web MVC是构建在原始的Servlet API 上的Web 框架，并且从一开始就包含在 Spring Framework中，是Spring的核心组件。它正式名称"Spring Web MVC"是来自Spring的源码模块（spring-webmvc，[github.com/spring-proj…](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fspring-projects%2Fspring-framework%2Ftree%2Fmaster%2Fspring-webmvc%EF%BC%89%E7%9A%84%E5%90%8D%E7%A7%B0%EF%BC%8C%E4%BD%86%E5%AE%83%E6%9B%B4%E9%80%9A%E5%B8%B8%E8%A2%AB%E7%A7%B0%E7%AE%80%E7%A7%B0%E4%B8%BA%22Spring) MVC"。

也就是说，Spring MVC框架本身就是基于Servlet规范的，但是它对原始的Servlet API进行了封装，屏蔽了底层原始的Servlet方法，提供了更加高级的开发模式和注解的支持，用于方便开发者快速开发基于Servlet API并部署到Servlet容器的Web应用程序。

# 使用

**我们只需要添加一个spring-webmvc依赖，即可实现最简单的Spring MVC项目的测试，spring-webmvc依赖包含了spring-web依赖以及Spring框架的核心依赖，比如spring-core、spring-context等等。**

```xml
<dependencies>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring-framework.version}</version>
    </dependency>
</dependencies>
```

# 架构

![img](https://pic2.zhimg.com/80/v2-921fb541e2a88a5582bd537f5bea56c9_720w.webp)

Spring MVC基于MVC设计模式。在B/S架构中，系统标准的三层架构包括：表现层、业务层、持久层。三层架构在我们的实际开发中使用的非常多，属于宏观的解决方案。而MVC全名是Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写，是一种用于设计创建 Web 应用程序表现层的模式：

1. 表现层，框架有比如SpringMVC和struts2。

   1. 也就是我们常说的web层。它负责接收客户端请求，向客户端响应结果，通常客户端使用HTTP协议请求web层，web需要接收HTTP请求，完成HTTP响应。表现层依赖业务层，接收到客户端请求一般会调用业务层进行业务处理，并将业务层返回的处理结果响应给客户端。

   2. 表现层的设计一般都使用

      ```
      MVC模型
      ```

      。（MVC是表现层的设计模型，和其他层没有关系）：

      1. `V即View视图`，是指用户看到并与之交互的界面。比如由html元素组成的网页界面，或者软件的客户端界面。视图层仅仅是展示数据，并且提供给用户的对应的操作界面。位于最上层。
      2. `C即Controller控制器`，是指控制器接受用户的输入并调用模型和视图去完成用户的需求，控制器本身不输出任何东西和做任何处理。它只是接收请求并决定调用哪个model模型构件去处理请求，然后再确定用哪个视图来显示返回的数据。简单的说，Controller负责转发请求和响应，它将View和Model联系起来。Controller位于第二层。
      3. `M即Model模型`，包括业务功能编写（例如算法实现）、数据库设计以及数据存取操作实现。在MVC的三个部件中，模型拥有最多的处理任务，Model位于最下层。

   3. `表现层也被成为Controller层`，因为在前后端分离开发之后流行，后端一般不需要返回HTML页面了，而是直接返回JSON数据给前端即可，数据的解析和页面展示展示由前端框架负责。而在基于三层架构的Web应用中，Model也可以指全部的单纯的数据模型，也就是JavaBean对象，而业务功能由业务层承担了。也就是说MVC中的，View和Model要么很少使用了，要么有了新的意义。

2. 业务层，业务层一般不需要框架，因为它主要是一些业务逻辑代码。

   1. 也就是我们常说的`service层`。它负责业务逻辑处理，和我们开发项目的需求息息相关。web层依赖业务层，但是业务层不依赖web层。
   2. 业务层在业务处理时可能会依赖持久层，比如操作数据库数据。

3. 持久层，框架有比如mybatis和hibernate。

   1. 也就是我们是常说的`dao层或者mapper层`。负责对于数据的访问和操作，以及数据持久化，数据库是对数据进行持久化的载体，持久层是业务层和数据库进行交互的接口，业务层需要通过持久层来访问和操作数据库数。通俗的讲，持久层就是和数据库交互，对数据库表进行曾删改查的。

**总的来说，MVC模式与三层架构结合使用时的关系（采用SSM框架，未实现前后端分离）：**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d90b5676746049c58fb291b3f3c7aed4~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

如果是基于前后端分离的项目，所有请求都是返回JSON字符串给前端，那么由于项目不再需要JSP、freemarker等技术以及前端资源，不再需要响应HTML页面，因此对应的View视图层或许也可以不需要了，或者说View的工作由前端框架来实现了。

## Model

Model 对象负责在控制器和展现数据的视图之间传递数据。

模型封装了数据及对数据的操作，可以直接对数据库进行访问，不依赖视图和控制器，也就是说模型并不关注数据如何展示，只负责提供数据。GUI 程序模型中数据的变化一般会通过观察者模式通知视图，而在 web 中则不会这样。

实际上，放到 Model 属性中的数据将会复制到 Servlet Response 的属性中，这样视图就能在这里找到它们了。

从广义上来说，Model 指的是 MVC 中的 M，即 Model(模型)。从狭义上讲，Model 就是个 key-value 集合。

Model 是一个接口，它的实现类为 ExtendedModelMap，继承 ModelMap 类

```scala
public class ExtendedModelMap extends ModelMap implements Model
```

**示例**

```kotlin
package com.example.tacocloud.controller;

import com.example.tacocloud.bean.Order;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@Slf4j
@RequestMapping("/orders")
public class OrderController {
    /**
     * orderForm() 方法本身非常简单，
     * 只是返回了一个名为 orderForm 的逻辑视图名。
     */
    @GetMapping("/current")
    public String orderForm(Model model){
        model.addAttribute("order", new Order());
        return "orderForm";
    }
}
```

上面这段代码中，方法 `orderForm`的参数是 `Model model` ，这表明使用了 `Model`,

并调用了 `Model` 的`addAttribute`方法，以键-值的形式传入参数 `"order", new Order()`

最后，`return "orderForm";`表示返回名为`orderForm`的逻辑视图，也就意味着，我们要在 `\resources\templates`目录下，建立对应的 `orderForm.html` 文件

`Model`接口的源码如下：

```typescript
public interface Model {
    Model addAttribute(String var1, @Nullable Object var2);

    Model addAttribute(Object var1);

    Model addAllAttributes(Collection<?> var1);

    Model addAllAttributes(Map<String, ?> var1);

    Model mergeAttributes(Map<String, ?> var1);

    boolean containsAttribute(String var1);

    @Nullable
    Object getAttribute(String var1);

    Map<String, Object> asMap();
}
```

可以看出，使用 `Model`,主要就是调用它的`addAttribute()`方法，或者`addAllAttributes()`方法。

`addAttribute()`方法，就是传入单个的键值对

`addAllAttributes()`方法，就是以 集合或者字典的形式，传入一组键值对。

Model是一个接口，ModelMap是一个接口实现，作用是将model数据填充到request域。

在Web应用程序的上下文（context）中，数据绑定涉及HTTP请求参数（即表单数据或查询参数）与模型对象及其嵌套对象中的属性的绑定。

只有遵循 [JavaBeans命名约定](https://www.oracle.com/java/technologies/javase/javabeans-spec.html) 的 `public` 属性才会被暴露出来用于数据绑定—例如， `firstName` 属性的 `public String getFirstName()` 和 `public void setFirstName(String)` 方法。

> 模型（model）对象及其嵌套对象图，有时也被称为命令对象、表单支持对象或POJO（Plain Old Java Object）。

默认情况下，Spring允许绑定到模型对象图中的所有 public 属性。这意味着你需要仔细考虑模型有哪些 public 属性，因为客户端可以针对任何 public 属性路径，甚至是一些在特定用例中不被期望的目标。

例如，给定一个HTTP表单数据端点，一个恶意的客户端可以为模型对象图中存在的属性提供数值，但不是浏览器中呈现的HTML表单的一部分。这可能导致模型对象及其任何嵌套对象上的数据被设置，而这些数据并不期望被更新。

推荐的方法是使用一个专门的模型对象，只公开与表单提交相关的属性。

如果你不能或不想为每个数据绑定用例使用一个专门的模型对象，你必须限制允许数据绑定的属性。理想情况下，你可以通过 `WebDataBinder` 的 `setAllowedFields()` 方法注册允许的字段 pattern 来实现这一点。

例如，为了在你的应用程序中注册允许的字段 pattern ，你可以在 `@Controller` 或 `@ControllerAdvice` 组件中实现一个 `@InitBinder` 方法，如下所示：

```java
@Controller
public class ChangeEmailController {

    @InitBinder
    void initBinder(WebDataBinder binder) {
        binder.setAllowedFields("oldEmailAddress", "newEmailAddress");
    }

    // @RequestMapping methods, etc.

}
```

除了注册允许的 pattern 外，还可以通过 `DataBinder` 及其子类中的 `setDisallowedFields()` 方法注册不允许的字段 pattern 。然而，请注意，"allow list" 比 "deny list" 更安全。因此，`setAllowedFields()` 应该比 `setDisallowedFields()` 更受欢迎。

请注意，与允许的字段 pattern 匹配是区分大小写的；而与不允许的字段 pattern 匹配是不区分大小写的。此外，与不允许的 pattern 相匹配的字段将不被接受，即使它也恰好与允许列表中的 pattern 相匹配。

## View



# 核心组件

Spring MVC是基于**组件式**的Web框架，一个不同的功能点都由一个不同的组件负责，这样做的好处之一就是：**各个组件分工明确、相互配合完成请求的处理和响应工作。而且每一个组件都是独立的扩展点，我们可以很容易对其进行扩展而不必特别关心其他组件，相当灵活（在自定义组件之前我们还是有必要了解这些组件的关系的）。**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/601d9a12a742422e978c1df9dc6bc4ac~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**执行流程**

1. 请求到达DispatcherServlet。
2. 确定当前请求的处理器——handler。通过遍历handlerMappings，依次调用每个HandlerMapping的getHandler方法获取HandlerExecutionChain对象，只要有一个HandlerMapping的getHandler方法返回值不为null，就返回该返回值，就不会继续向后查找，如果在所有的handlerMapping中都没找到，则返回null。对于 RequestMappingHandlerMapping，他返回的handler就是HandlerMethod
   1. 找到的handler实际上是一个`HandlerExecutionChain`对象，它包含了`真正的handler以及适用于当前handler的HandlerInterceptor拦截器链`。
   2. 如果没有找到任何handler（handler返回null），那么通常返回`404`响应码或者抛出`NoHandlerFoundException`异常。
3. **根据handler确定当前请求的处理器适配器——HandlerAdapter**。通过遍历handlerAdapters，依次调用每个HandlerAdapter的`supports`方法只要有一个HandlerAdapter的supports方法返回true，就返回该HandlerAdapter，就不会继续向后查找，如果在所有的HandlerAdapter中都没找到，则直接抛出`ServletException`。对于`HandlerMethod`，它的适配器就是`RequestMappingHandlerAdapter`。
4. **顺序应用拦截器链中所有拦截器的PreHandle预处理方法**，只要有一个拦截器不通过，那么该请求就不会继续处理，而是直接从当前拦截器倒序执行`afterCompletion`方法，最后`返回`。如果都通过，那么`继续后续处理`。
5. 通过HandlerAdapter来执行给定的handler，将返回一个ModelAndView结果对象。
   1. 不同的handler的实际工作流程差别非常大，对于`HandlerMethod`来说，它是通过`RequestMappingHandlerAdapter`来调用的，最终目的就会尝试执行对应的`@ReuqestMapping方法`，也就是业务逻辑，其中还包括`参数反序列化、数据绑定、检验、返回值处理、序列化`等等步骤。
   2. `ModelAndView`对象的`Model`部分是业务返回的模型数据，它是一个map，`View`部分为视图的逻辑视图名，即`ViewName`。
   3. 返回的ModelAndView用以渲染视图，如果不需要渲染视图，则返回`null`，此情况通常是已将返回的数据序列化为JSON字符串写入response中了，比如`application/json请求`（通常是前后端分离的项目）。
6. **handler正常处理完毕时，倒序应用拦截器链中的所有拦截器的postHandle后处理方法。**
7. **处理handler执行过程中抛出的异常（如果有），将会通过注册的HandlerExceptionResolvers确定处理错误的ModelAndView，借此我们可以对异常转发到统一配置的错误页面！**
8. 如果ModelAndView不为null，则根据ModelAndView（无论是正产返回的还是错误处理的）渲染视图。（对于前后端分离的项目，通常是基于JSON的`application/json请求`，因此是没有渲染视图这一步的）。
   1. 首先可能会**调用ViewResolver尝试将给定的视图名称解析为一个View对象**（待呈现，内部保存了经过逻辑视图名viewName解析出来的物理视图资源URL路径）。
   2. 随后调用View本身的render方法，真正的执行渲染的工作。但是，如果更加严格的话，View 更多的是用于在将模型数据移交给特定视图技术之前进行数据准备，而不是真正的进行视图渲染，视图渲染仍然依靠其该类型视图本身的视图渲染技术！
      1. 对于一个简单的转发请求或者包含请求，如果目标是JSP视图，则View就是`InternalResourceView`，在渲染时首先就是将model数据存储到request域属性中，然后通过`RequestDispatcher`直接将请求`include`包含或者`forward`转发到物理视图路径URL对应的JSP资源，而include/forward方法则是我们在之前就见过的`原始的Servlet API`，最终还是通过response响应给用户的（对JSP来说就是将JSP渲染之后通过response输出为HTML文本）。
      2. **从上面JSP视图的渲染和输出可以看出来，View中仅仅是进行了数据的封装和准备（比如将model数据存储到request域属性中），并没有进行实际渲染视图的工作（仅仅是调用include/forward方法），对于JSP视图来说，真正的视图渲染和数据填充以及返回响应仍然是依赖最原始JSP技术，与Spring MVC无关，与View无关。**
9. **最后，无论请求处理成功还是失败抛出异常，都会倒序应用拦截器链中的所有拦截器的afterCompletion处理方法，表示请求处理完毕。**

如果不需要渲染给定的模型和视图，那么也就不需要ViewResolver和View的工作了，即第8步不需要了。比如，如果是返回的结果是JSON数据，那么在Handler执行handle方法的时候，也就是第5步时，执行返回的JSON结果就已经被写入response响应体中了，并且执行完毕之后的返回的ModelAndView为null。

所以说，如果是前后端分离的项目，统一使用JSON数据交互，那么表现层虽然仍然是MVC模式，但是通常Model和View并没有被用到，表现层也被称为Controller层！

上面的步骤仅仅是大概的步骤！还有些细节没有讲到，比如如果支持JSON数据交互，那么在Handler执行的时候会首先将JSON数据转换为对象，执行完毕之后还会将返回的对象转换为JSON数据。后面当我们学习了Spring MVC的源码之后，我们将会更加清晰！

另外，在处理器适配器HandlerAdapter执行handler的过程中（handle方法中），还会涉及到请求和响应相关的数据转换、格式化、检验的工作，比如JSON序列化和反序列化，以及数据类型转换、参数条件校验、方法参数的封装等，具体的逻辑还是很复杂的！

对于REST风格的请求与响应来说，核心类就是`HttpMessageConverter`，它用于完成从请求体到请求方法参数，以及从返回结果到响应体的转换。

## 分发器

与其他许多Web框架一样，**Spring MVC围绕分派控制器模式进行设计**，在该模式下， `DispatcherServlet`提供了**用于请求处理和分发的整体逻辑，而实际上的请求处理的工作是由各个可配置的组件来完成的。该模型非常灵活，并支持多种工作流程。DispatcherServlet也被称作前端控制器。**

与任何Servlet一样，都需要根据`Servlet规范`使用JavConfig或在web.xml中声明和映射DispatcherServlet（我们在前面就学习过了）。反过来，`DispatcherServlet`将会使用Spring的配置发现、调用请求映射，视图解析，异常处理等委托组件。

简单的说，**用户请求到达DispatcherServlet，然后由它调用其它组件处理用户的请求，DispatcherServlet是整个请求处理的流程控制中心，DispatcherServlet的存在降低了组件之间的耦合性。**

**这些不同功能的组件，在框架中对应着不同的顶级接口，它们的实现类都可以看作是由Spring 管理一些特殊的bean，DispatcherServlet委托给不同的特殊 bean 以处理请求并呈现适当的响应。通常这些组件（接口）都有默认的实现，也就是说，这些特殊bean不需要我们去配置，但我们也可以自定义其属性并扩展或替换它们。**

`DispatcherServlet` 委托给特殊的Bean来处理请求并呈现适当的响应。我们所说的 "特殊Bean" 是指实现框架契约的Spring管理的 `Object` 实例。这些实例通常有内置的约定，但你可以自定义它们的属性，并扩展或替换它们。

> Spring Boot遵循不同的初始化顺序。**Spring Boot不是挂入Servlet容器的生命周期，而是使用Spring配置来启动自己和嵌入式Servlet容器**。`Filter` 和 `Servlet` 声明在Spring配置中被检测到，并在Servlet容器中注册。

**DispatcherServlet将会委托的特殊bean（接口、组件）有下面几种**（来自[Spring官网](https://link.juejin.cn?target=https%3A%2F%2Fdocs.spring.io%2Fspring-framework%2Fdocs%2F5.2.8.RELEASE%2Fspring-framework-reference%2Fweb.html%23mvc-servlet-special-bean-types)）：

| HandlerMapping                        | 处理器映射器，用于查找能够处理请求的Handler，将请求映射为HandlerExecutionChain 对象（包含一个Handler处理器对象、多个 HandlerInterceptor 拦截器）对象。 |
| ------------------------------------- | ------------------------------------------------------------ |
| HandlerAdapter                        | 处理器适配器，帮助 DispatcherServlet调用请求映射到的Handler，但是不管Handler实际如何调用。将会返回一个ModelAndView对象，其中model是一个Map结构，存放了我们返回的所有数据，view是逻辑视图名，即ViewName。 |
| HandlerExceptionResolver              | 异常解析器，如果在前面执行handler的过程中抛出了某个异常，将会走异常解析器的方法！在异常解析器中可以将此错误映射到其他handler、HTML 错误视图（错误页面）或统一抛出自己的异常。 |
| ViewResolver                          | 视图解析器，ViewResolver根据handler执行之后返回的ModelAndView中的String类型的逻辑视图名解析成物理视图名，即具体的资源地址，再生成对应的View视图对象。但是具体的事务解析以及数据填充工作由View视图自己完成（View. render方法）。 |
| LocaleResolver, LocaleContextResolver | 区域解析器，用户的区域也称为Locale，Locale信息是可以由前端直接获取的，可以根据不同的用户区域展示不同的视图，比如为不同区域的用户可以设置不同的语言和时区，也就是提供国际化视图支持。 |
| ThemeResolver                         | 主题解析器，主题就是系统的整体样式或风格，可通过Spring MVC框架提供的主题（theme）设置应用不同的整体样式风格，提高用户体验。Spring MVC的主题就是一些静态资源的集合，即包括样式及图片，用来控制应用的视觉风格。主题也支持国际化，同一个主题不同区域也可以显示不同的风格。 |
| MultipartResolver                     | 多部件解析器，用于处理上传请求，文件上传时就可以使用MultipartResolver来解析上传请求中的文件数据，方便快捷的实现文件上传！ |
| FlashMapManager                       | 存储并检索FlashMap，这些FlashMap可用于将属性从一个请求传递到另一个请求，通常是用在重定向中。也就是说FlashMap主要用在redirect中传递参数，而FlashMapManager则用于管理这些FlashMap。 |

## HandlerMapping

### Handler

**Handler，即处理器，它对应着MVC中的C也就是Controller，在Spring MVC框架中，它的实现有很多种，比如：**

1. `实现Servlet接口或者继承HttpServlet类`等方法，这是最传统的方式，在使用框架之后，此方式几乎不再使用了。
2. `实现了Coltroller接口或者继承Coltroller的实现类`。
3. `实现了HttpRequestHandler接口或者继承HttpRequestHandler的实现类`。
4. 标注了`@RequestMapping注解以及使用@RequestMapping 作为元注解的注解（比如@GetMapping、@PostMapping、@PutMapping、@DeleteMapping、@PatchMapping等）的方法`。

Handler中就包含我们的业务代码逻辑！

一个Controller的实现被封装为一个对应类型的Handler对象。其中，采用@RequestMapping注解以及使用@RequestMapping 作为元注解的注解实现的Controller将被封装为HandlerMethod，内部保存了Controller方法。这种方式也最为常用，只需要在方法前加上@RequestMapping注解或者以@RequestMapping注解为元注解的注解，该方法就被作为Controller，对于该方法所属的具体类没有任何要求，因此一个Controller类中可以包含多个Controller方法，可以处理不同的请求，这种方法级别的Controller实现节省了大量类文件的编写，让应用更加轻量级。

Handler有多种类型，但是没有统一的接口，因此在源码中使用`Object`类型来保存。

### HandlerMapping

在Spring MVC中每个请求都需要一个对应的Handler来处理，那么当接收到一个请求之后到底使用哪个Handler进行处理呢？这需要通过HandlerMapping来查找了。

HandlerMapping即处理器映射器，它由DispatcherServlet调用，被用来根据请求（request）查找对应的Handler，并将请求映射为HandlerExecutionChain 对象（包含一个Handler处理器对象、多个用于预处理和后处理的HandlerInterceptor 拦截器）对象。

```java
interface HandlerMapping {

    /**
     * 返回包含匹配此请求的处理程序Handler对象和全部HandlerInterceptor拦截器链的HandlerExecutionChain。
     * 可根据请求 URL、会话状态或实现类选择的任何因素进行选择。
     * <p>
     * DispatcherServlet将查询所有已注册的HandlerMapping的bean 以查找匹配项，如果未找到，则返回null。这不是错误。
     *
     * @return 一个包含handler对象和全部interceptor拦截器的HandlerExecutionChain对象，如未找到映射，那么返回null
     */
    @Nullable
    HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception;

}
```

可以看到，HandlerMapping接口包含唯一的一个getHandler方法，这个方法就是通过request找到HandlerExecutionChain，而HandlerExecutionChain包装了一个Handler和一组Interceptor拦截器。

### 实现原理

请求映射的详细规则因HandlerMapping的不同实现而各不相同，web项目中多个映射器可以共存互不影响，并且可以排序。在查找映射的时候，DispatcherServlet将遍历所有在当前容器中已注册的HandlerMapping的bean以查找匹配项，找到第一个即停止查找。

如果在配置文件中指定HandlerMapping的实现，那么`Spring5.2.8.RELEASE`版本默认将加载`BeanNameUrlHandlerMapping、RequestMappingHandlerMapping以及RouterFunctionMapping（Spring 5.2新增的用于支持RouterFunctions的映射器）`这三个HandlerMapping。如果手动配置了HandlerMapping，那么不会加载默认的HandlerMapping。

一般我们不需要指定HandlerMapping，直接采用默认配置即可！

#### BeanNameUrlHandlerMapping

beanName方式。BeanNameUrlHandlerMapping，利用Controller的beanname来作为URL映射查找对应的Handler。BeanNameUrlHandlerMapping只会查找采用实现了Coltroller接口或者继承Coltroller的实现类，以及实现了HttpRequestHandler接口或者继承HttpRequestHandler的实现类这两种方式实现的Handler。

BeanNameUrlHandlerMapping将会默认配置（在没有手动配置其他HandlerMapping时），因此我们只需要配置Handler。

```java
public class BeanNameUrlController implements Controller {

    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        //创建ModelAndView对象
        ModelAndView mv = new ModelAndView();
        //跳转到哪个页面    mvc会走视图解析器
        mv.setViewName("index.jsp");
        return mv;
    }
}
```

采用XML的方式如下：

```xml
<!-- 注册 Handler(实现Controller接口)，name表示url路径，name前要加'/' -->
<bean name="/beanNameUrl" class="com.spring.mvc.controller.BeanNameUrlController"/>
```

#### SimpleUrlHandlerMapping

url方式。SimpleUrlHandlerMapping，根据配置的URL路径和Controller映射来查找对应的Handler，该方式可以将多个URL集中配置。SimpleUrlHandlerMapping只会查找采用实现了Coltroller接口或者继承Coltroller的实现类，以及实现了HttpRequestHandler接口或者继承HttpRequestHandler的实现类这两种方式实现的Handler。

SimpleUrlHandlerMapping默认不会被Spring自动配置，因此需要手动配置。

```xml
<!-- 注册 HandlerMapping，基于SimpleUrlHandlerMapping的方式 -->
<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
    <property name="mappings">
        <!--配置请求url和handler的beanName的映射关系-->
        <props>
            <prop key="/simpleUrl1">simpleUrlController1</prop>
            <prop key="/simpleUrl2">simpleUrlController2</prop>
            <!--通过*配置一个通用的Handler-->
            <!--那么 比如/simpleUrl3  /simpleUrl4等请求都会被转发到simpleUrlController3的Handler-->
            <prop key="/simpleUrl*">simpleUrlController3</prop>
        </props>
    </property>
</bean>
```

#### RequestMappingHandlerMapping

注解方式。RequestMappingHandlerMapping是最常用的一个HandlerMapping，因为它支持查找通过@RequestMapping注解或者@RequestMapping作为元注解的方式实现的Handler。从spring3.1版本开始，废除了DefaultAnnotationHandlerMapping的使用。

RequestMappingHandlerMapping在构建HandlerExecutionChain对象时，会在内部将对象内的handler属性的类型设置成HandlerMethod类型。

RequestMappingHandlerMapping将会默认配置（在没有手动配置其他HandlerMapping时），因此我们只需要配置Handler。而Handler的案例就不用说了吧，这个太常见了，就我们最常用的在方法上标注@RequestMapping注解以及使用@RequestMapping 作为元注解的注解（比如@GetMapping, @PostMapping, @PutMapping, @DeleteMapping, @PatchMapping）的方式！

## HanderAdapter

处理器适配器。帮助 DispatcherServlet调用找到的Handler，但是不管Handler实际如何调用。

在HandlerMapping找到对应的Handler之后，需要调用Handler，但是我们之前说过，Handler的实现形式有很多种，如果通过if else来判断类型并调用，那么可能随时涉及到修改此前的代码，这样就不符合开闭原则。通过`HandlerAdapter对Handler`进行执行，这是`适配器模式`的应用，通过扩展HandlerAdapter可以对更多类型的处理器进行执行而不修改修改源码。

当HandlerMapping获取到执行请求的Handler时，DispatcherServlte将会依次调用所有已注册的HandlerAdapter（可以排序），如果当前HandlerAdapter能够对当前的Handler进行执行，那么就通过当前HandlerAdapter来执行该Handler，这样就大大的减少了通过DispatcherServlet直接调用Handler的难度，同时这样可以使得程序的扩展性大大增强！

HandlerAdapter会通过反射调用具体Handler的方法，但是方法具体是怎么执行的，HandlerAdapter不会关心！执行完毕之后，HandlerAdapter将返回`ModelAndView`对象，其中model是一个Map结构，其实就是存放了我们返回给请求的所有结果值，view是逻辑视图名，即ViewName。`如果不需要渲染视图，则可能返回null，比如application/json请求。`

HandlerAdapter的接口方法如下，通过`supports`方法判断是否辅助执行该handler，通过`handle`方法对Handler进行执行。

```java
public interface HandlerAdapter {

    /**
     * 给定一个handler实例，返回此 HandlerAdapter 是否可以支持它。
     * 通常一个类型的HandlerAdapter仅支持一个类型的handler。
     *
     * @param handler 给定的handler处理器
     * @return 此HandlerAdapter是否可以使用给定的handler
     */
    boolean supports(Object handler);


    /**
     * 使用给定的handler来处理此请求。
     *
     * @param request  当前 HTTP 请求
     * @param response 当前 HTTP 响应
     * @param handler  要使用的handler。此对象之前必须传递给此接口的supports方法，并且该方法必须返回true
     * @return 一个ModelAndView模型和视图对象，具有视图的名称和所需的模型数据，如果请求已直接处理，则返回null
     */
    ModelAndView handle(HttpServletRequest request, HttpServletResponse response,
                        Object handler) throws Exception;

    // 获取当前请求的最后更改时间，主要用于供给浏览器判断当前请求是否修改过，
    // 从而判断是否可以直接使用之前缓存的结果

    /**
     * Same contract as for HttpServlet's getLastModified method. Can simply return -1 if there's no support in the handler class.
     * 与HttpServlet的getLastModified方法相同的作用。如果handler中没有支持，只需返回 -1 即可。
     *
     * @param request 当前 HTTP 请求
     * @param handler 要使用的handler
     * @return 给定handler的最后修改值
     */
    long getLastModified(HttpServletRequest request, Object handler);

}
```

Handler的执行是一个复杂的过程，其中可能涉及到参数类型的转换和封装，JSON的返序列化和序列化等过程！

### 实现

通常一种类型的Handler需要一种对应的的HandlerAdapter。

1. `RequestMappingHandlerAdapter`主要是`适配注解处理器`，注解处理器就是我们经常使用@RequestMapping注解及其作为元注解的方法处理器。
2. `HttpRequestHandlerAdapter`主要是适配静态资源处理器，静态资源处理器就是实现了HttpRequestHandler接口或HttpRequestHandler接口子类的类处理器，这一类处理器的作用是处理通过Spring MVC来访问的静态资源的请求。
3. `SimpleControllerHandlerAdapter`是Controller处理适配器，适配实现了Controller接口或Controller接口子类的处理器。
4. `SimpleServletHandlerAdapter`是Servlet处理适配器，适配实现了Servlet接口或Servlet的子类的处理器，我们不仅可以在web.xml里面配置Servlet，其实也可以用SpringMVC来配置Servlet，不过这个适配器很少用到，而且Spring MVC默认的适配器没有它。

`Spring5.2.8.RELEASE`版本中，默认配置的HandlerAdapter为`HttpRequestHandlerAdapter、SimpleControllerHandlerAdapter、RequestMappingHandlerAdapter，以及HandlerFunctionAdapter（Spring 5.2新增的用于支持RouterFunctions的处理器适配器）`。

同样，一般不会指定HandlerMapping，直接采用默认配置！

## HandlerExceptionResolver

异常解析器，如果在前面执行handler的过程中抛出了某个异常，将会走异常解析器的方法！注意，HandlerExceptionResolver只能处理在handler执行完毕之前抛出的异常，如果是在handler执行完毕之后的步骤中抛出的异常，比如页面渲染过程中的异常，它是不能处理的。

异常解析器可以定义各种解决异常的策略，可能将请求映射到其他handler、HTML 错误视图（错误页面）或统一抛出自己的异常。自定义的异常解析器必须实现HandlerExceptionResolver接口，并实现resolveException方法，然后将该异常解析器配置到容器中！

一个大型且完善的项目通常都会有自己的异常解析器，我们后面会详细讲！

HandlerExceptionResolver只有一个`resolveException`方法，该方法返回一个`ModelAndView`，其中可以包含要返回的数据和视图名称，也就是说，比如在`HandlerAdapter.handle()`方法执行过程中抛出异常，那么转而会执行`HandlerExceptionResolver.resolveException()`方法，最终将返回并渲染该方法的返回值。

```java
public interface HandlerExceptionResolver {

    /**
     * 尝试解决在处理程序执行期间抛出的给定异常，返回表示特定错误的ModelAndView视图（比如一个统一的异常页面）
     * <p>
     * 返回的ModelAndView可能是一个空的兑现(ModelAndView.isEmpty()返回true)
     * 表示异常已成功解决，但不会呈现任何视图，例如通过设置状态代码。
     *
     * @param request  当前 HTTP 请求
     * @param response 当前 HTTP 响应
     * @param handler  执行的handler
     * @param ex       在handler执行期间抛出的异常
     * @return 相应的要转发到的ModelAndView，或null
     */
    @Nullable
    ModelAndView resolveException(
            HttpServletRequest request, HttpServletResponse response, @Nullable Object handler, Exception ex);

}
```

## ViewResolver

### View

View，视图，Spring MVC框架提供了很多类型的View视图支持，包括：JstlView、FreemarkerView、PdfView等，我们最常用的视图就是jsp，它对应着InternalResourceView。

View对象中保存了经过逻辑视图名解析出来的物理视图资源URL路径，通过`View.render`方法可将返回的model数据（需要展示的数据）填充（渲染）到对应的物理视图中的对应位置，并将最终渲染完毕的页面通过response响应给客户端（比如返回拼接的HTML文本）。

但是，如果更加严格的话，View 更多的是用于在将模型数据移交给特定视图技术之前进行数据准备，而不是真正的进行视图渲染，视图渲染仍然依靠其该类型视图本身的视图渲染技术！

比如对于一个简单的JSP的View来说，比如InternalResourceView，在渲染时首先就是将model数据存储到request域属性中，然后通过RequestDispatcher直接将请求include包含或者forward转发到物理视图路径URL对应的JSP资源，而include/forward方法则是我们在之前就见过的原始的Servlet API，最终还是通过response响应给用户的（对JSP来说就是将JSP渲染之后通过response输出为HTML文本）。

从上面JSP视图的渲染和输出可以看出来，View中仅仅是进行了数据的封装和准备（比如将model数据存储到request域属性中），并没有进行实际渲染视图的工作（仅仅是调用include/forward方法），对于JSP视图来说，真正的视图渲染和数据填充以及返回响应仍然是依赖最原始JSP技术，与Spring MVC无关，与View无关。

```java
public interface View {

    /**
     * @return Content-Type 字符串，可能包含charset
     */
    @Nullable
    default String getContentType() {
        return null;
    }

    /**
     * 根据给定的model数据渲染当前的view
     *
     * @param model    一个Map，名称作为key，值作为value
     * @param request  当前请求
     * @param response 响应对象
     */
    void render(@Nullable Map<String, ?> model, HttpServletRequest request, HttpServletResponse response)
            throws Exception;
}
```

### ViewResolver

视图解析器，ViewResolver根据handler执行之后返回的ModelAndView中的String类型的逻辑视图名解析成物理视图名，即具体的资源URL地址，再生成对应的View视图对象，具体的视图渲染和数据填充的工作由View视图自己完成，实际上View中也仅仅是进行了数据准备工作，最终仍然是通过调用对应的视图的API来进行视图渲染和数据填充的，而这些方法都是各种视图模版或者技术规范自身的API，并非是View实现的。

ViewResolver接口只有一个`resolveViewName`方法，用于根据给定的逻辑视图名viewName和区域（用于国际化）locale查找对应的物理视图URL位置，并将其解析为一个`View`视图对象！

```java
public interface ViewResolver {

    /**
     * 按给定的viewName解析为View视图。
     * <p>
     *
     * @param viewName 要解析的视图的名称
     * @param locale   解析视图的区域设置，用于支持国际化
     * @return View 对象，如果找不到对应的视图，那么返回null
     * @throws Exception 如果视图无法解析（通常在创建实际View视图对象时出现问题）
     */
    @Nullable
    View resolveViewName(String viewName, Locale locale) throws Exception;

}
```

Spring为我们提供了非常多的视图解析器的默认实现，例如：

1. `BeanNameViewResolver`：在当前应用程序上下文中将逻辑视图名称解析为 bean名称，随后查找对应的名称的bean实例并返回。也就是说对应名称的bean应该是一个View视图对象。
2. `FreeMarkerViewResolver`：将逻辑视图名称解析为FreeMarkerView对象，说白了就是对FreeMarker模版的支持！
3. `InternalResourceViewResolver`：使用的最广泛的一个视图解析器。将逻辑视图名称解析为InternalResourceView对象，而InternalResourceView则会将返回的Model模型的属性都存放到对应的request属性中，然后通过RequestDispatcher在服务器端把请求forword到目标URL。我们知道/WEB-INF/下面的内容是不能直接通过请求的方式请求到的，为了安全性考虑，我们通常会把jsp等资源文件放在WEB-INF目录下，而InternalResourceViewResolver的自动转发机制就能即安全又方便（不需要写转发代码了）的访问这些资源了。

默认的ViewResolver就是`InternalResourceViewResolver`，指定的ViewResolver同样支持Order排序！

### 配置

**Spring MVC的很多组件都不需要我们配置，但是ViewResolver则可能需要！通常我们这样配置InternalResourceViewResolver：**

```xml
<!--配置视图解析器-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <!--要访问文件所在的相对目录-->
    <property name="prefix" value="/WEB-INF/pages/"/>
    <!--文件的后缀名-->
    <property name="suffix" value=".jsp"/>
</bean>
```

对于没有前后端分离的项目，通常各种视图比如JSP、CSS、JS文件是放在WEB-INF目录下的，因为/WEB-INF/下面的内容是不能通过直接请求的方式访问到，它需要服务器内部转发，因此我们通常配置InternalResourceViewResolver。

ViewResolver默认解析的视图文件位置根路径为webapp，WEB-INF目录也在这个下面，因此如果我们需要找到一个/WEB-INF/pages/index.jsp文件，那么我们返回的逻辑视图名就必须是“/WEB-INF/pages/index.jsp”。

此时我们可以像上面的案例那样设置InternalResourceViewResolver的prefix和suffix属性，prefix表示视图文件的前缀路径，而suffix则表示视图文件的后缀。此时我们返回的逻辑视图名可以直接返回“index”，InternalResourceViewResolver在解析时会自动拼接为“/WEB-INF/pages/index.jsp”，这样代码也更加简洁！

## 默认组件配置

我们可以在项目配置文件中配置上面的表格中列出来的各种基础bean的实现。DispatcherServlet将在当前WebApplicationContext容器中检查每一个特殊的bean，如果某个特殊bean我们并没有手动配置，那么DispatcherServlet将会加载位于org\springframework\web\servlet\DispatcherServlet.properties配置文件中该类型bean的默认配置，否则不会加载。

默认情况下，这些组件是在DispatcherServlet的onRefresh()方法中创建容器的时候被初始化的，并且会被配置到DispatcherServlet实例内部的对应属性中保存起来方便后续直接使用！另外，自动加载的bean仅存放于DispatcherServlet内部的集合属性中，并没有交给Spring 容器管理，这是要注意的地方！

在`Spring 5.2.8.RELEASE`版本中，`DispatcherServlet.properties`的配置如下：

```properties
# Default implementation classes for DispatcherServlet's strategy 
interfaces.
# Used as fallback when no matching beans are found in the DispatcherServlet context.
# Not meant to be customized by application developers.

org.springframework.web.servlet.LocaleResolver=org.springframework.web.servlet.i18n.AcceptHeaderLocaleResolver

org.springframework.web.servlet.ThemeResolver=org.springframework.web.servlet.theme.FixedThemeResolver

org.springframework.web.servlet.HandlerMapping=org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping,\
   org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping,\
   org.springframework.web.servlet.function.support.RouterFunctionMapping

org.springframework.web.servlet.HandlerAdapter=org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter,\
   org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter,\
   org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter,\
   org.springframework.web.servlet.function.support.HandlerFunctionAdapter


org.springframework.web.servlet.HandlerExceptionResolver=org.springframework.web.servlet.mvc.method.annotation.ExceptionHandlerExceptionResolver,\
   org.springframework.web.servlet.mvc.annotation.ResponseStatusExceptionResolver,\
   org.springframework.web.servlet.mvc.support.DefaultHandlerExceptionResolver

org.springframework.web.servlet.RequestToViewNameTranslator=org.springframework.web.servlet.view.DefaultRequestToViewNameTranslator

org.springframework.web.servlet.ViewResolver=org.springframework.web.servlet.view.InternalResourceViewResolver

org.springframework.web.servlet.FlashMapManager=org.springframework.web.servlet.support.SessionFlashMapManager
```

需要注意的是，通过`DispatcherServlet.properties`默认加载的组件仅会创建对象实例，仅提供最基本的mvc功能而并没有激活其他默认配置，而如果通过`@EnableWebMvc注解或者< mvc:annotation-driven/>标签开启mvc配置`之后，除了会注册这些组件之外，还会加载一些默认配置，并且支持自定义配置。

比如，如果不开启MVC配置，那么mvc就不支持`application/json`请求和响应（即使有jackson的依赖也不会自动注册MappingJackson2HttpMessageConverter），也就无法提供REST风格的请求、响应和异常处理，没有默认的conversionService！

# 父子容器

对于许多应用程序，有一个单一的 `WebApplicationContext` 是简单的，也是足够的。也可以有一个上下文层次结构，一个根 `WebApplicationContext` 被多个 `DispatcherServlet`（或其他 `Servlet`）实例共享，每个实例都有自己的子 `WebApplicationContext` 配置。参见 [`ApplicationContext` 的附加功能](https://springdoc.cn/spring/core.html#context-introduction) ，以了解更多关于上下文层次结构的特性。

根（root） `WebApplicationContext` 通常包含基础设施Bean，例如需要在多个 `Servlet` 实例中共享的数据存储库和业务服务。这些Bean有效地被继承，并且可以在 `Servlet` 特定的子 `WebApplicationContext` 中被重写（也就是重新声明），该 `WebApplicationContext` 通常包含给定 `Servlet` 的本地Bean。下面的图片显示了这种关系：

![mvc context hierarchy](https://springdoc.cn/spring/images/mvc-context-hierarchy.png)

## 初始化

我们此前学过并知道普通的Spring应用有自己的ApplicationContext容器，而如果是基于Spring的web应用，那么它的容器有所不同，将会使用WebApplicationContext。

DispatcherServlet 需要WebApplicationContext容器（Web应用程序上下文，扩展了ApplicationContext（普通应用程序上下文））来进行自己的配置，因为WebApplicationContext具有获取ServletContext的getServletContext方法，并且Spring的WebApplicationContext容器同样与web应用的ServletContext相关联，因此我们在web应用程序中可以直接使用RequestContextUtils的静态方法通过当前请求查找WebApplicationContext。

在学习Spring源码的时候，我们知道Spring支持父子容器，但是我们并没有用过，实际上对于许多web应用程序来说，拥有单个WebApplicationContext就足够了，当然也可以有一个有层次的上下文结构，对于比咋的应用程序或许会更好，其中一个Root（根）  WebApplicationContext 在多个调度器服务（或其他 Servlet）实例之间共享，每个实例都有其自己的Child（子） WebApplicationContext 配置。

Root WebApplicationContext 通常包含web应用中的基础结构 bean，例如需要跨多个Servlet实例共享的Dao、数据库配置bean、Service等服务bean，也就是三层架构中的业务层和持久层的bean，这些 bean可以在特定Servlet 的子 WebApplicationContext 中重写（即重新声明）。Child WebApplicationContext则用于存放三层架构中的表现层的bean，比如Controller、ViewResolver、HandlerMapping等Spring MVC的组件bean，因此也被称为Servlet WebApplicationContext。

Spring MVC 依托于 Servlet 规范。Servlet 规范中 ，所有的 Servlet 具有一个相同的上下文 ServletContext，ServletContext 将优先于 Servlet 初始化，Spring 利用了这个特性，在 ServletContext 初始化时创建父容器，并将其绑定到 ServletContext 的属性中，然后在每个 DispatcherServlet 初始过程中创建子容器并将 ServletContext 中的容器设置为父容器。

`DispatcherServlet` 期望有一个 `WebApplicationContext`（普通 `ApplicationContext` 的扩展）用于自己的配置。`WebApplicationContext` 有一个与 `ServletContext` 和与之相关的 `Servlet` 的链接。它也被绑定到 `ServletContext`，这样应用程序就可以使用 `RequestContextUtils` 上的静态方法来查询 `WebApplicationContext`，如果他们需要访问它的话。

+ ContextLoaderListener 会被优先初始化时，根据指定的 Spring 的配置文件路径，创建出应用的根上下文`Root WebApplicationContext`。
+ DispatcherServlet 是 SpringMVC 的核心组件。在初始化时，根据指定的 SpringMVC 的配置文件路径，创建出 SpringMVC 的独立上下文`Web WebApplicationContext`。该上下文在创建过程中，会判断`Root WebApplicationContext`是否存在，如果存在就将其设置为自己的 parent 上下文。这就是父子上下文（父子容器）的概念。

父子容器的作用在于，当我们尝试从子容器（`Servlet WebApplicationContext`）中获取一个 bean 时，如果找不到，则会委派给父容器（`Root WebApplicationContext`）进行查找。

### 父容器初始化

Servlet 容器会在 ServletContext 初始化时触发事件，然后由 ServletContextListener 监听，Spring 提供了这个接口的实现 ContextLoaderListener 并在初始化时创建父容器。

```java
public class ContextLoaderListener extends ContextLoader implements ServletContextListener {

	@Override
	public void contextInitialized(ServletContextEvent event) {
		initWebApplicationContext(event.getServletContext());
	}
}
```

ContextLoaderListener 仅调用了父类 ContextLoader 中的 `initWebApplicationContext`方法创建容器，该方法会将创建后的容器存至名为`WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE` 的属性中。

```xml
<!-- Spring 配置 -->
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
<!-- 指定 Spring 核心配置文件路径，用于初始化 Root WebApplicationContext 容器 -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
</context-param>
```

上述配置中，ContextLoaderListener 读取 context-param 中的 contextConfigLocation 指定的配置文件，初始化`Root WebApplicationContext`容器。下面查看初始化该容器的源码：

```java
public WebApplicationContext initWebApplicationContext(ServletContext servletContext) {
    //(1) 如果已经存在 Root WebApplicationContext，则抛出异常；
    // 在整个 web 应用中，只能有一个 Root WebApplicationContext 容器
    if (servletContext.getAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE) != null) {
        throw new IllegalStateException("Cannot initialize context because there is already a root application context present - check whether you have multiple ContextLoader* definitions in your web.xml!");
    } else {
        //(2) 打印日志
        servletContext.log("Initializing Spring root WebApplicationContext");
        Log logger = LogFactory.getLog(ContextLoader.class);
        if (logger.isInfoEnabled()) {
            logger.info("Root WebApplicationContext: initialization started");
        }
		// 记录开始时间
        long startTime = System.currentTimeMillis();

        try {
            if (this.context == null) {
                //(3) 创建 WebApplicationContext 对象
                this.context = this.createWebApplicationContext(servletContext);
            }
			//(4) 如果是 ConfigurableWebApplicationContext 的子类，如果未刷新，则进行配置和刷新
            if (this.context instanceof ConfigurableWebApplicationContext) {
                ConfigurableWebApplicationContext cwac = (ConfigurableWebApplicationContext)this.context;
                if (!cwac.isActive()) {  // (4.1)未刷新(激活)
                    if (cwac.getParent() == null) {  //(4.2) 无父容器，则进行加载和设置
                        ApplicationContext parent = this.loadParentContext(servletContext);
                        cwac.setParent(parent);
                    }
					// (4.3) 配置 context 对象，并进行刷新
                    this.configureAndRefreshWebApplicationContext(cwac, servletContext);
                }
            }

//(5) 将 context 放进 servletContext 中，记录为 Root WebApplicationContext
servletContext.setAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, this.context);
            //(6) 将 context 和当前线程绑定，这样可以更加方便得到 context
            ClassLoader ccl = Thread.currentThread().getContextClassLoader();
            if (ccl == ContextLoader.class.getClassLoader()) {
                currentContext = this.context;
            } else if (ccl != null) {
                currentContextPerThread.put(ccl, this.context);
            }
			//(7) 打印日志
            if (logger.isInfoEnabled()) {
                long elapsedTime = System.currentTimeMillis() - startTime;
                logger.info("Root WebApplicationContext initialized in " + elapsedTime + " ms");
            }
			//(8) 返回 context				
            return this.context;
        } catch (Error | RuntimeException var8) {
            //(9) 当发生异常，将异常放进 servletContext，不再重新初始化
            logger.error("Context initialization failed", var8);
            servletContext.setAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, var8);
            throw var8;
        }
    }
}
```

上述的 (4.3) 过程调用了 configureAndRefreshWebApplicationContext() 方法对 Context 进行配置，源码如下：

```java
protected void configureAndRefreshWebApplicationContext(ConfigurableWebApplicationContext wac, ServletContext sc) {
    String configLocationParam;
    //(1) 如果 wac 使用了默认编号，则重新设置 id 属性
   	// 注意这里的 wac 就是前一个方法创建的 WebApplicationContext 对象
    if (ObjectUtils.identityToString(wac).equals(wac.getId())) {
        // 情况一，使用 contextId 属性
        configLocationParam = sc.getInitParameter("contextId");
        if (configLocationParam != null) {
            wac.setId(configLocationParam);
        // 情况二，自动生成    
        } else {
            wac.setId(ConfigurableWebApplicationContext.APPLICATION_CONTEXT_ID_PREFIX + ObjectUtils.getDisplayString(sc.getContextPath()));
        }
    }
	//(2) 让 context 关联上 ServletContext
    wac.setServletContext(sc);
    //(3) 设置 context 的配置文件路径，该路径就是在 web.xml 中配置的 Spring 配置文件路径
    configLocationParam = sc.getInitParameter("contextConfigLocation");
    if (configLocationParam != null) {
        wac.setConfigLocation(configLocationParam);
    }
	//(4) 忽略
    ConfigurableEnvironment env = wac.getEnvironment();
    if (env instanceof ConfigurableWebEnvironment) {
        ((ConfigurableWebEnvironment)env).initPropertySources(sc, (ServletConfig)null);
    }
	//(5) 执行自定义初始化
    this.customizeContext(sc, wac);
    // 刷新 context ，执行初始化
    wac.refresh();
}
```

最后给出 ContextLoaderListener 初始化`Root WebApplicationContext`容器的时序图：

[![img](https://img2020.cnblogs.com/blog/1606446/202008/1606446-20200823004847820-1408075676.jpg)](https://img2020.cnblogs.com/blog/1606446/202008/1606446-20200823004847820-1408075676.jpg)

### 子容器初始化

子容器由 DispatcherServlet 进行初始化，简化后的类图如下所示。
![在这里插入图片描述](https://img-blog.csdnimg.cn/c5ea1de1d0ec4d7a8d2ccba6e821a28a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5aSn6bmPY29vbA==,size_20,color_FFFFFF,t_70,g_se,x_16)

1. 首先 Spring 提供的 HttpServletBean 继承 HttpServlet 类，重写了 init 方法，并调用了抽象方法 initServletBean。
2. 然后 HttpServletBean 的子类 FrameworkServlet 重写了 initServletBean 方法，并调用了 initWebApplicationContext 方法，这个方法将会完成子容器的初始化工作。子容器初始化时从 ServletContext 属性中取出并设置父容器。
3. DispatcherServlet 直接使用了父类 FrameworkServlet 的初始化方法。

```xml
<!-- SpringMVC 配置 -->
<servlet>
    <servlet-name>DispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- 指定 SpringMVC 核心配置文件路径，用于初始化 Root WebApplicationContext 容器 -->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc-config.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>DispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

DispatcherServlet 在 Web 容器创建之后，会读取 init-param 中 contextConfigLocation 指定的配置文件，初始化`Servlet WebApplicationContext`容器。下面查看初始化该容器的源码：

```java
protected WebApplicationContext initWebApplicationContext() {
    //(1) 通过工具类 WebApplicationContextUtils 来获取 Root WebApplicationContext 对象
    // 内部是从 ServletContext 中取出该对象的（前面方法将它放进了 ServletContext）
    WebApplicationContext rootContext = WebApplicationContextUtils.getWebApplicationContext(this.getServletContext());
    //(2) 初始化 WebApplicationContext 对象 wac
    WebApplicationContext wac = null;
    // 第一种情况，如果构造方法已经传入 webApplicationContext 属性，则直接使用
    if (this.webApplicationContext != null) {
        // 赋值给 wac 变量
        wac = this.webApplicationContext;
        // 如果是 ConfigurableWebApplicationContext 类型，并且未激活，则进行初始化
        if (wac instanceof ConfigurableWebApplicationContext) {
            ConfigurableWebApplicationContext cwac = (ConfigurableWebApplicationContext)wac;
            if (!cwac.isActive()) {   // 未激活
                if (cwac.getParent() == null) {
                    // 设置 rootContext 对象为 wac 的父上下文
                    cwac.setParent(rootContext);
                }
				// 配置和初始化 wac
                this.configureAndRefreshWebApplicationContext(cwac);
            }
        }
    }
	// 第二种情况，从 ServletContext 获取已有的 WebApplicationContext 对象
    if (wac == null) {
        wac = this.findWebApplicationContext();
    }
	// 第三种情况，创建一个 WebApplicationContext 对象，并将 rootContext 设置为parent
    if (wac == null) {
        wac = this.createWebApplicationContext(rootContext);
    }
	//(3) 如果未触发刷新事件，则主动触发刷新事件
    if (!this.refreshEventReceived) {
        synchronized(this.onRefreshMonitor) {
            this.onRefresh(wac);
        }
    }
	//(4) 将 context 放进 ServletContext 中，记录为 Servlet WebApplicationContext
    if (this.publishContext) {
        String attrName = this.getServletContextAttributeName();
        this.getServletContext().setAttribute(attrName, wac);
    }
	// 返回初始化后的 webApplicationContext
    return wac;
}
```

上述 (2) 步骤中调用了 configureAndRefreshWebApplicationContext() 方法对 Context 进行配置，源码如下：

```java
protected void configureAndRefreshWebApplicationContext(ConfigurableWebApplicationContext wac) {
    //(1) 如果 wac 使用了默认编号，则重新设置 id 属性
    // 注意这里的 wac 就是前一个方法获取到的 context 对象
    if (ObjectUtils.identityToString(wac).equals(wac.getId())) {
        // 情况一，使用 contextId 属性
        if (this.contextId != null) {
            wac.setId(this.contextId);
        // 情况二，自动生成    
        } else {
 wac.setId(ConfigurableWebApplicationContext.APPLICATION_CONTEXT_ID_PREFIX + ObjectUtils.getDisplayString(this.getServletContext().getContextPath()) + '/' + this.getServletName());
        }
    }
	//(2) 设置 wac 的 servletContext、servletConfig、namespace 属性
    wac.setServletContext(this.getServletContext());
    wac.setServletConfig(this.getServletConfig());
    wac.setNamespace(this.getNamespace());
    //(3) 添加监听器 SourceFilteringListener 到 wac 中
    wac.addApplicationListener(new SourceFilteringListener(wac, new FrameworkServlet.ContextRefreshListener()));
    //(4) 忽略
    ConfigurableEnvironment env = wac.getEnvironment();
    if (env instanceof ConfigurableWebEnvironment) {
        ((ConfigurableWebEnvironment)env).initPropertySources(this.getServletContext(), this.getServletConfig());
    }
	//(5) 执行处理完 WebApplicationContext 后的逻辑
    this.postProcessWebApplicationContext(wac);
    //(6) 执行处理完 WebApplicationContext 后的逻辑
    this.applyInitializers(wac);
    //(7) 刷新 wac ，从而初始化 wac
    wac.refresh();
}
```

大体上，和`Root WebApplicationContext`的配置过程是一样的。最后给出 DispatcherServlet 初始化`Servlet WebApplicationContext`容器的时序图：

[![img](https://img2020.cnblogs.com/blog/1606446/202008/1606446-20200823004930530-578606401.jpg)](https://img2020.cnblogs.com/blog/1606446/202008/1606446-20200823004930530-578606401.jpg)

## 配置

使用 Spring 需要将 bean 注入 Spring 容器，针对 web 环境，配置父容器需要使用 ContextLoaderListener，配置子容器需要使用 DispatcherServlet。

如果是基于XML的配置，那么Root WebApplicationContext通过ContextLoaderListener去加载名为“contextConfigLocation”的context-param参数来配置。而Servlet WebApplicationContext则是通过spring mvc中提供的DispatchServlet的名为“contextConfigLocation”的init-param参数初始化来加载配置。

```xml
<web-app>
    <display-name>Archetype Created Web Application</display-name>

    <!--配置contextConfigLocation初始化参数，指定父容器Root WebApplicationContext的配置文件 -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring-config.xml</param-value>
    </context-param>
    <!--监听contextConfigLocation参数并初始化父容器-->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>


    <!-- 配置spring mvc的前端核心控制器 -->
    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet
        </servlet-class>
        <!-- 配置contextConfigLocation初始化参数，指定子容器的配置文件并创建子容器 Servlet WebApplicationContext -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc-config.xml</param-value>
        </init-param>
        <!--配置Servlet的对象的创建时间点：取值如果为非负整数，表示应用加载时创建，值越小，servlet的优先级越高，就越先被加载，如果取值为负数，表示在第一次使用时才加载-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--配置映射路径-->
    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <url-pattern>/app1/*</url-pattern>
    </servlet-mapping>

</web-app>
```

ContextLoaderListener 被配置到监听器列表，ServletContext 初始化时会使用 context-param 中参数名为 contextConfigLocation 的值作为配置文件路径初始化容器。

DispatcherServlet 也需要添加到 Servlet 列表，DispatcherServlet 同时也会使用初始化参数 contextConfigLocation 作为配置文件的路径初始化容器。

在上面的配置中，`ContextLoaderListener`作为监听器会被优先初始化，随后ServletContext会被初始化并且会将`context-param`参数设置设置进去，而ContextLoaderListener它实际上是一个`ServletContextListener`监听器实现，它将会监听ServletContext的创建事件并调用对应的`contextInitialized方法`，在该方法中将会获取ServletContext配置的`contextConfigLocation`参数（这里面就有我们配置的配置文件路径，默认路径为`/WEB-INF/applicationContext.xml`）并且初始化`Root WebApplicationContext`实例，也就是父容器，实际类型为`XmlWebApplicationContext`。

随后会将父容器通过setAttribute方法设置到ServletContext中，属性的key为”org.springframework.web.context.WebApplicationContext.ROOT”，最后的”ROOT"字样表明这是一个 Root WebApplicationContext，而WebApplicationContext中也会保留ServletContext的引用，这样WebApplicationContext和ServletContext就关联起来了。

随后`DispatcherServlet`会被实例化并且设置初始化参数，在创建完毕之后的`init()`回调方法（该方法在其父类HttpServletBean中）中，将会获取它自己的`contextConfigLocation`参数，并且根据指定的配置文件在`initServletBean()`方法中创建`Servlet WebApplicationContext，也就是子容器`。同时，其会调用ServletContext的getAttribute方法来判断是否存在Root WebApplicationContext。如果存在，则将其设置为自己的parent。

**需要注意1：**由于`Root WebApplicationContext`是早于`Servlet WebApplicationContext`创建和初始化的，所以父容器的 bean 无法访问子容器的 bean，因为子容器还未初始化；而子容器的 bean 可以访问父容器的 bean，访问方式就是前面说的委派查询。说通俗点就是，在 Controller 层里可以注入 Service 对象，而 Service 层里无法注入 Controller 对象（编译可以通过，但是运行会出错）。

**需要注意2**：如果我们没有配置 ContextLoaderListener 来创建`Root WebApplicationContext`容器，那么`Servlet WebApplicationContext`的父上下文就是 null，也就是没有父容器。

注意：如果配置了ContextLoaderListener，那么一定要配置`名为contextConfigLocation的context-param参数`，即指定Spring配置文件位置，如果没有配置，那么默认查找的路径为`/WEB-INF/applicationContext.xml`，找不到对应路径的配置文件就会抛出异常。如果配置了DispatcherServlet，那么一定要配置`名为contextConfigLocation的init-param参数`，即指定Spring MVC配置文件位置，如果没有配置，那么默认查找的路径为`"/WEB-INF/"+容器nameSpace+ ".xml"（默认namespace为servletName+"-servlet"）`，找不到对应路径的配置文件就会抛出异常。

⭐️为什么需要父子容器

父子容器存在的一个意义其实就是分层，如前面那张图所示：

+ `Root WebApplicationContext`容器注册各种非 Web 组件的 bean，例如 Services、Repositories。
+ `Web WebApplicationContext`容器注册各种 Web 组件的 bean，例如 Controllers、ViewResolver 和 HandlerMapping。

> 关于controller 的配置不能定义在父容器，例如convert，信息转换等。

所以，这要求我们编写 Spring 和 SpringMVC 配置文件时，Spring 文件配置非 Web 组件的 bean，而 SpringMVC 文件配置 Web 组件的 bean。除此之外，也是为了防止同一组件在不同容器中分别注册初始化，出现两个 bean，这会导致一些错误的出现。

Spring 配置文件中的注解扫描：

```xml
<!-- 配置 IoC 容器注解扫描的包路径 -->
<context:component-scan base-package="com.example">
    <!-- 制定扫包规则，不扫描 @Controller 注解修饰的 Java 类，其它还是要扫描 -->
    <context:exclude-filter type="annotation"
                            expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

SpringMVC 配置文件中的注解扫描：

```xml
<!-- 配置 IoC 容器的注解扫描的包路径 -->
<context:component-scan base-package="com.example" use-default-filters="false">
    <!-- 制定扫包规则，只扫描使用 @Controller 注解修饰的 Java 类 -->
    <context:include-filter type="annotation"
                            expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

> 如果不需要 application context 层次结构，应用程序可以只配置一个 “root” context，并将 `contextConfigLocation` Servlet参数留空。

**在一个传统的Spring web项目中，通常情况下引入的不同组件都有不同的XML配置文件，这样的好处是可以将这些配置分开，而父子容器的作用大概同样是为了划分框架边界而区分的吧，并且实际上可以配置多个子容器共享一个父容器。**

当然，我们可以仅配置一个容器！

**对于XML的配置来说，如果不需要配置父容器，那么我们可以直接不配置context-param和listener，将所有的业务层和持久层的配置都写在spring  MVC的配置文件中即可，如果不需要配置子容器，那么我们在DispatcherServlet的contextConfigLocation参数的param-value中不填写任何值就行了，也就是让它是空着（这表示没有配置文件，如果不添加该属性，将会使用默认配置文件）。**

## 常见问题

在父子容器的配置中，子容器可以引用父容器中的bean，但是父容器不能够引用子容器中的bean的，并且各个子容器中定义的bean是互不可见的。在查找bean时，如果子容器存在该bean，那么将直接从子容器获取，否则才会尝试从父容器中获取！

**如果父子容器的配置中有相同的bean，比如两个配置中扫描包的路径有重叠，那么这个bean将会被初始化两次，注意，这不是覆盖，而是在父子的容器各自初始化两一次并且保存在各自的容器中，这将带来很多问题！**

1. 这样导致在两个父子IOC容器中生成大量的相同bean，这就会造成内存资源的浪费。
2. 对于某些配置bean，比如Mq的消费者，如果加载了两次，那么带来很大的问题。
3. 可能导致某些隐性的问题，比如如果子容器和父容器都加载了Service的bean，但是AOP的配置只是在父容器中被应用，那么这样回到这Service的AOP配置失效，因为，子容器将直接使用自己内部的没有经过AOP增强的Service对象。
   1. 一个避免方法就是使用注解配置，比如@Transactional，但是最好的办法就是：要么所有配置都在子容器中加载，要么父子容器加载的类通过不同的包路径彻底分开！

**如果基于XML配置：**

1. **Servlet WebApplicationContext子容器是一定会被初始化的**，因为它是随着`DispatcherServlet`的创建而默认创建，而DispatcherServlet一般都需要配置。只不过你可以选择不在子容器中初始化你的配置，也就是将param-value参数空着。
2. **Root WebApplicationContext父容器不一定会被初始化**，因为它被`ContextLoaderListener`创建，而ContextLoaderListener可以不配置，也就是说父容器可以真正的不存在。
3. 如果同时配置了父子容器，那么请求将会先到达子容器，而在@ReqestMapping的解析过程中，只是对Servlet WebApplicationContext容器中的bean进行处理的，并没有去查找父容器的bean（实际上是只查找当前DispatcherServlet关联的容器）。**因此如果Controller被配置到父容器中，那么因为不会对父容器中含有@RequestMapping注解的函数进行处理，更不会生成相应的handler。那么对应的请求过来的时候由于找不到对应的hander，将无法访问该Controller内部的@ReqestMapping对应的资源。**

**如果基于JavaConfig配置：**

1. 如是通过实现`WebApplicationInitializer`接口来进行配置的项目（比如入门案例中的JavaConfig配置），此时将只有一个容器，虽然它的displayName为Root WebApplicationContext，但是由于它和DispatcherServlet是发生了关联，因此实际上可以看作Servlet WebApplicationContext容器，因此可以解析该容器的Controller实例内部的@ReqestMapping注解，也就是说可以正常使用。

2. 如是通过继承

   ```
   AbstractAnnotationConfigDispatcherServletInitializer
   ```

   抽象类来进行配置的项目：

   1. **Servlet WebApplicationContext子容器一定会被初始化，即使你的getServletConfigClasses方法返回null，那只是表示子容器将不会加载你的任何其他配置而已。**
   2. **如果getRootConfigClasses方法返回null，那么父容器将不会被初始化，也就是说此时项目只有一个Servlet WebApplicationContext。**

3. 如果同时配置了父子容器，那么请求将会先到达子容器。而在@ReqestMapping的解析过程中，只是对Servlet WebApplicationContext容器中的bean进行处理的，并没有去查找父容器的bean。因此如果Controller被配置到父容器中，那么因为不会对父容器中含有@RequestMapping注解的函数进行处理，更不会生成相应的handler。那么对应的请求过来的时候由于找不到对应的hander，将无法访问该Controller内部的@ReqestMapping对应的资源。

另外需要注意的是，`Spring boot`采用了不同的初始化顺序和策略，使用Spring配置来引导自身和嵌入式Servlet容器（tomcat）。Filter和 Servlet 在 Spring 配置中声明，并在嵌入式的Servlet 容器中注册。

**如果只配置“一个”容器，那么建议只配置Servlet WebApplicationContext，也就是和DispatcherServlet关联的容器。**

# 注解 Controller

**Spring MVC 提供了基于注解的编程模型，`@Controller和@RestController`组件内部可以使用注解来表示请求映射、请求输入、异常处理等功能。注解提供了方法级别的Controller实现，从而不必扩展基类或实现特定的接口，减少了代码量、优化了项目结构！**

一个常见的基于注解定义的控制器案例如下：

```java
@Controller
public class HelloController {

    @GetMapping("/hello")
    public String handle(Model model) {
        model.addAttribute("message", "Hello World!");
        return "index.jsp";
    }
}
```

## 声明

我们可以通过在 Servlet 的WebApplicationContext中使用标准Spring bean 的定义方式来定义Controller控制器bean。

@Controller注解采用@Component作为元注解，支持@ComponentScan和`<context:component-scan/>`的组件扫描，同时@Controller注解还会指示标注的类被作为 Web 组件的角色，如果采用@Service、@Component等其他组件扫描注解，那么内部的方法级别的控制器不会生效！

@RestController 是一个组合注解，它组合使用了 @Controller 和 @ResponseBody元注解，指示其类中的每种方法都继承了类级别的@ResponseBody注解，因此返回的数据将直接写入response响应正文中，而不会使用ViewSolvsolver和View进行视图模版解析和渲染。

注意，如果需要写入response响应正文中是JSON数据并且能自动将返回的对象格式化为JSON，那么还需要JSON转换的依赖：

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.9.8</version>
</dependency>
```

## AOP 代理

Controller控制器中支持AOP 代理，例如在类上添加@Transactional进行事务控制，但这需要Spring MVC的配置文件定义了事务的支持！

由于Controller控制器中的方法一般都不是从接口中继承的，因此建议使用基于类的动态代理，但是如果Controller实现了除了容器回调接口（例如InitializingBean、DisposableBean、Closeable、AutoCloseable接口以及Aware接口包括其子接口）、标志性接口、内部接口的的其他合理接口，那么为了让控制机方法的AOP配置生效，可能需要显式配置基于类的代理，通常是配置`<tx:annotation-driven proxy-target-class="true"/>`或者`@EnableTransactionManagement(proxyTargetClass = true)`。

## 请求映射

我们可以使用注解@RequestMapping将请求映射到不同的控制器方法。它具有各种属性，可按 URL、HTTP 方法、请求参数、请求头和媒体类型匹配。可以在类级别使用它来表示共享映射，也可以在方法级别使用它来缩小到特定的终结点映射。

更常见的还有用于筛选特定HTTP方法的@RequestMapping变体：@GetMapping、@PostMapping、@PutMapping、@DeleteMapping、@PatchMapping。

上面的这些基于特定HTTP方法的注解是一种组合注解，在RESTFUL风格的项目中，大多数的控制器方法都应该映射到特定的HTTP请求方法，因此更多的使用这些特性化的注解，而不是使用@RequestMapping，后者默认情况下**与所有类型的HTTP方法匹配**。但是，在类级别仍可以使用@RequestMapping来表示共享映射。

**下面示例要求具体HTTP请求方法的方法级映射：**

```java
@RestController
@RequestMapping("/persons")
class PersonController {

    /**
     * 查询的方法，采用get请求
     */
    @GetMapping("/{id}")
    public Person get(@PathVariable Long id) {
        // ...
    }

    /**
     * 插入的方法，采用post请求
     */
    @PostMapping
    public void add(@RequestBody Person person) {
        // ...
    }

    /**
     * 更新的方法，采用put请求
     */
    @PutMapping
    public void update(@RequestBody Person person) {
        // ...
    }

    /**
     * 删除的方法，采用delete请求
     */
    @DeleteMapping("/{id}")
    public void delete(@PathVariable Long id) {
        // ...
    }
}
```

@GetMapping和@RequestMapping(method=RequestMethod.GET)透明的支持HTTP HEAD 方法进行请求映射，控制器方法不需要更改。response中的Content-Length头将会设置为将要写入的字节数而无需实际写入响应。也就是说，HTTP HEAD 请求的处理就像是 HTTP GET 一样，只是，不写入响应正文，而是计算响应字节数并设置Content-Length标头。HEAD请求一般用来测试超链接的有效性，可用性和最近修改。

对于HTTP OPTIONS请求，默认情况下将对应的控制器注解中的method属性（表示支持的HTTP 方法）的值设置为“Allow”响应头的值，其中RequestMethod.GET支持GET,HEAD,OPTIONS方法，其他的类型则支持本类型方法和OPTIONS方法，也可以通过response手动设置“Allow”头信息！

对于没有HTTP方法声明的@RequestMapping，Allow头将被设置为GET，HEAD，POST，PUT，PATCH，DELETE，OPTIONS。但是，控制器方法应始终声明支持的HTTP方法（例如通过使用HTTP方法特定的变体：@GetMapping，@PostMapping等）。

### URI

我们可以使用 global模式和通配符来映射请求，简单的说，请求的URL路径支持模式匹配！Spring MVC 使用 PathMatcher 协定和来自spring-core的 AntPathMatcher 实现进行 URI 路径匹配。

**下面是Pattern以及对应的匹配规则：**

| Pattern       | Description                                                  | Example                                                      |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ?             | 匹配一个字符                                                 | "/pages/t?st.html" 匹配 "/pages/test.html" 和"/pages/t3st.html" |
| *             | 匹配路径段中的零个或多个字符                                 | "/resources/.png" 匹配 "/resources/file.png"。"/projects/*/versions" 匹配 "/projects/spring/versions" 不匹配 "/projects/spring/boot/versions" |
| **            | 匹配零个或多个路径段，直到路径结束                           | "/resources/**" 匹配 "/resources/file.png" 和 "/resources/images/file.png" |
| {name}        | 匹配路径段，并捕获它作为名为"name"的控制器方法的参数变量的值 | "/projects/{project}/versions" 匹配" /projects/spring/versions" 和 captures project=spring |
| {name:[a-z]+} | 匹配满足"[a-z]+"正则表达式的路径段，并捕获它作为名为"name"的控制器方法的参数变量的值 | "/projects/{project:[a-z]+}/versions" 匹配"/projects/spring/versions" 不匹配 "/projects/spring1/versions" |

#### @PathVariable

捕获的 URI 中的变量可以通过@PathVariable注解注入到方法参数中进行访问，默认情况下参数名和URI中的变量名一致，如下例所示，

```java
@GetMapping("/owners/{ownerId}/pets/{petId}")
public Pet findPet(@PathVariable Long ownerId, @PathVariable Long petId) {
    // ...
}
```

实际上参数名和URI中的变量名不一致也可以，此时需要在@PathVariable注解中指明使用的变量：

```java
@GetMapping("/owners/{ownerId}")
public void findPet(@PathVariable("ownerId") Long id) {
    // ...
}
```

我们可以同时在类和方法级别中声明 URI 变量并使用，如下例所示：

```java
@Controller
@RequestMapping("/owners/{ownerId}")
public class OwnerController {

    @GetMapping("/pets/{petId}")
    public Pet findPet(@PathVariable Long ownerId, @PathVariable Long petId) {
        // ...
    }
}
```

URI变量将自动转换为参数相应的类型，如果不能转换将引发TypeMismatchException异常。默认情况下支持简单类型（int、long、Date 等），您可以注册对任何其他数据类型的支持，可以通过 WebDataBinder（DataBinder）或通过FormattingConversionService注册格式器Formatters来自定义类型转换规则。

#### 正则表达式

**语法`[varName:regex]`声明带有正则表达式的URI变量，在一个路径段中,可以多次使用该语法。例如，以下方法可以提取URI路径段中的名称、版本和文件扩展名：**

```java
java复制代码@RestController
public class RegexController {

    @GetMapping("/{name:[a-z-]+}-{version:\\d\\.\\d\\.\\d}{ext:\\.[a-z]+}")
    public void handle(@PathVariable String name, @PathVariable String version, @PathVariable String ext) {
        System.out.println(name);
        System.out.println(version);
        System.out.println(ext);
    }
}
```

访问/spring-web-3.0.5.jar，将会得到： ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1078e195a4fd4386809c795db96e60f8~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

#### 占位符

URI路径模式还可以支持嵌入${…}占位符，这些占位符在启动时通过PropertyPlaceHolderConfigurer针对本地、系统system、环境environment和其他属性源中的属性来解析。因此我们可以使用它根据某些外部配置对基本 URL 进行参数化。

URI配置文件UriPlaceholder.properties（需要加载到容器中）如下：

```java
uri1=tx
uri2=xt
uri3=xxx
```

Controller如下：

```java
@RestController
@RequestMapping("/${uri1}")
class PlaceholderController {

    @GetMapping("/${uri2}/{${uri3}}/{id}")
    public void handle(@PathVariable Long id, @PathVariable String xxx) {
        System.out.println(id);
        System.out.println(xxx);
    }
}
```

访问/tx/xt/aaa/111，结果如下：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cecedcd0d0074a00994777a0d56709c3~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

#### 后缀匹配

默认情况下，Spring MVC支持“.*”的后缀匹配，比如映射到 /person 的控制器也隐式映射到/person.*。这样的好处是，可以通过文件扩展名用于来表示请求的内容类型（content type）以用于响应（而不是使用Accept请求头） - 例如/person.pdf，/person.xml等。

但是后来文件扩展名的使用已经以各种方式证明是有问题的。使用 URI 变量、路径参数和 URI 编码时，可能会导致歧义，还可能引起RFD攻击，目前AcceptHeader是首选的内容协商判定依据。

如果要完全禁用文件扩展名匹配，必须设置以下两项：

1. `PathMatchConfigurer.setUseSuffixPatternMatch(false)。`
2. `ContentNegotiationConfigurer.preferredPathExtension(false）。`

### Content-Type

我们可以根据请求的`Content-Type（客户端发送的实体数据类型）`缩小请求映射范围，如下案例：

```java
@PostMapping(path = "/pets", consumes = "application/json")
public void addPet(@RequestBody Pet pet) {
    // ...
}
```

`consumes`属性是一个String数组，可以传递一个或多个media type媒体类型字符串，其中至少一个与请求的Content-Type匹配！

此外，consumes支持`否定表达式`，例如!text/plain表示除text/plain之外的任何Content-Type。

同理，我们可以在`类级别`声明共享的consumes属性。但是，与大多数其他请求映射属性不同，在类级别使用时，方法级使用该同名属性将会重写，而不是扩展类级声明，简单的说就是**如果方法上也有consumes属性，那么就不会采用、合并类级别上的consumes属性。**

**在设置consumes的媒体类型值的时候，可以使用MediaType类，该类为常用媒体类型（如`APPLICATION_JSON_VALUE`和`APPLICATION_XML_VALUE`）提供了可使用的字符串常量。**

### Accept

**我们可以根据请求的`Accept（客户端希望接受的数据类型）`缩小请求映射范围，如下案例：**

```java
@GetMapping(path = "/pets/{petId}", produces = "application/json")
@ResponseBody
public Pet getPet(@PathVariable String petId) {
    // ...
}
```

`produces`属性是一个String数组，可以传递一个或多个media type媒体类型字符串，其中至少一个与请求的Accept匹配！

此外，produces支持`否定表达式`，例如!text/plain表示除text/plain之外的任何Accept。 同理，我们可以在`类级别`声明共享的produces属性。但是，与大多数其他请求映射属性不同，在类级别使用时，方法级使用该同名属性将会重写，而不是扩展类级声明，简单的说就是**如果方法上也有produces属性，那么就不会采用、合并类级别上的produces属性。**

**在设置produces的媒体类型值的时候，可以使用MediaType类，该类为常用媒体类型（如`APPLICATION_JSON_VALUE`和`APPLICATION_XML_VALUE`）提供了可使用的字符串常量。**

### Params

**我们可以根据请求参数条件缩小请求映射范围。可以测试存在请求参数（myParam）、缺少请求参数（!myParam）或特定值（myParam=myValue）是否存在。**

下面的示例演示如何测试特定参数值：

```java
@GetMapping(path = "/pets/{petId}", params = "myParam=myValue")
public void findPet(@PathVariable String petId) {
    // ...
}
```

**上面的案例中，除了URL路径必须匹配之外，还必须存在myParam参数并且值等于myValue，请求才会映射到该控制器方法！**

### Headers

**我们可以根据请求头缩小请求映射范围。可以测试存在请求头（myHeader）、缺少请求头（! myHeader）或特定值（myHeader=myValue）是否存在。**

下面的示例演示如何测试特定请求头值：

```java
@GetMapping(path = "/pets", headers = "myHeader=myValue")
public void findPet(@PathVariable String petId) {
    // ...
}
```

上面的案例中，除了URL路径必须匹配之外，还必须存在myHeader请求头并且值等于myValue，请求才会映射到该控制器方法！

由于Content-Type和Accept都是请求头参数，因此它们也可以使用headers属性匹配，但是最好还是使用consumes和produces属性。

## 方法参数

方法支持JDK 8 的 java.util.Optional作为方法参数，并可以结合具有required属性的注解（例如@RequestParam、@RequestHeader ），采用Optional包装的参数等效于required=false。

**下表描述了受支持的控制器方法参数和注解，任何参数都不支持Reactive相关类型。**

| 控制器方法参数                                               | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| WebRequest, NativeWebRequest                                 | 用于对请求参数、请求（request）和会话（session）属性的通用访问，而无需直接使用 Servlet API。 |
| javax.servlet.ServletRequest,  javax.servlet.ServletResponse | 请求和响应，可以指定特性的类型，比如HttpServletRequest、MultipartHttpServletRequest |
| javax.servlet.http.HttpSession                               | session，如果使用了该参数，那么永远不会为null。请注意，session访问不是线程安全的。如果允许多个请求同时访问session，请考虑将RequestMappingHandlerAdapter实例的synchronizeOnSession标志设置为 true。 |
| javax.servlet.http.PushBuilder                               | Servlet 4.0 的PushBuilder用于实现服务器资源推送。如果客户端不支持 HTTP/2 ，则注入的 PushBuilder 实例可能为 null。 |
| java.security.Principal                                      | 当前经过身份验证的用户 – 如果已知，可能是特定的Principal实现类。 |
| HttpMethod                                                   | 当前请求的 HTTP 方法。                                       |
| java.util.Locale                                             | 当前请求的区域设置，由可用的LocaleResolver区域解析器确定。   |
| java.util.TimeZone, java.time.ZoneId                         | 与当前请求关联的时区，由LocaleContextResolver确定。          |
| java.io.InputStream, java.io.Reader                          | 原始请求体，通过 Servlet API 获取的。                        |
| java.io.OutputStream, java.io.Writer                         | 原始响应体，通过 Servlet API 获取的。                        |
| @PathVariable                                                | 用于访问 URI 模版变量，在前面的请求映射部分就讲过了。        |
| @MatrixVariable                                              | 用于访问 URI 路径段中的name-value键值对。                    |
| @RequestParam                                                | 用于访问 Servlet 请求参数，支持多部分文件参数类型，参数值将自动转换为声明的方法参数类型。注意，对于简单类型的参数，可以不使用@RequestParam注解！ |
| @RequestHeader                                               | 用于访问请求头中的信息。请求头的值将自动转换为声明的方法参数类型。 |
| @CookieValue                                                 | 用于访问 Cookie。Cookie值将转换为声明的方法参数类型。        |
| @RequestBody                                                 | 用于访问 HTTP 请求正文（请求体）。正文内容转换为声明的方法参数类型，通过使用HttpMessageConverter来实现转换，比较重要。 |
| HttpEntity                                                   | 用于访问请求头和正文。正文类型使用HttpMessageConverter转换。 |
| @RequestPart                                                 | 用于访问multipart/form-data多部分请求中的一个部分，使用HttpMessageConverter转换类型。对于解析同时上传多中数据的请求的解析方便 |
| java.util.Map, org.springframework.ui.Model,  org.springframework.ui.ModelMap | 用于访问、设置model参数，model中的数据将可能用于视图渲染。   |
| RedirectAttributes                                           | 指定用于重定向时的属性。一个专门用于重定向之后还能带参数跳转的的工具类。 |
| @ModelAttribute                                              | 用于访问模型中的现有属性（如果不存在，则进行实例化），并应用数据绑定和验证。 |
| Errors, BindingResult                                        | 用于访问来自命令对象（即@ ModelAttribute参数）的数据验证和数据绑定中的错误或来自@RequestBody或@RequestPart参数的验证中的错误。必须在经过验证的方法参数后声明一个Errors或BindingResult参数。 |
| SessionStatus + class-level @SessionAttributes               | SessionStatus#setComplete方法可以触发通过类级注解@SessionAttributes存储的session属性的清理，但是不会清理真正session中的属性。 |
| UriComponentsBuilder                                         | 用于构建相对于当前请求的host，port，scheme，context  path以及servletmapping的URL。 |
| @SessionAttribute                                            | 用于访问已经存储的session域中的会话属性。                    |
| @RequestAttribute                                            | 用于访问已创建的、预先存在的request域中的属性                |
| 任何其他参数                                                 | 如果方法参数不与此表中任何上面的类型匹配，并且它是一种简单类型（由  BeanUtils#isSimpleProperty确定），则它是作为一个@RequestParam，并且requir=false。否则，它将作为一个@ModelAttribute。 |

### Matrix Variables

根据URI规范RFC 3986，请求URL的路径段中支持键值对。 Spring MVC根据Tim Berners-Lee的一篇[旧文章](https://link.juejin.cn?target=https%3A%2F%2Fwww.w3.org%2FDesignIssues%2FMatrixURIs.html)将这些键值对称为“矩阵变量（matrix variables）”，但是它们也可以被称为URI路径参数。

矩阵变量可以出现在任何路径段中，每个变量用分号分隔，多个值用逗号分隔（例如，/cars;color=red,green;year=2012）。也可以通过重复的变量名称指定多个值（例如，color=red;color=green;color=blue）。

如果 URL 预期包含矩阵变量，则控制器方法的请求映射必须使用 URI 变量来屏蔽该矩阵变量内容，并确保请求可以独立于矩阵变量的顺序和存在成功匹配。

我们首先需要启用矩阵变量的使用。在JavaConfig配置中，您需要设置一个 UrlPathHelper bean，并设置removeSemicolonContent=false。在 MVC的XML 命名空间中，可以设置`<mvc:annotation-driven enable-matrix-variables="true"/>`。

下面示例如何简单的使用矩阵变量：

```java
@GetMapping("/matrix/{id}")
public void handle(@PathVariable String id, @MatrixVariable int q, @MatrixVariable int r) {
    System.out.println(id);
    System.out.println(q);
    System.out.println(r);
    //…………
}
```

访问/matrix/11;q=22;r=33，得到如下结果：

```java
11
22
33
```

**鉴于所有路径段可能都包含矩阵变量，并且不同的路径段中可能具有同名的矩阵变量，有时可能需要消除矩阵变量预期到底位于哪个路径变量中的歧义。**

**我们可以通过`@MatrixVariable 的name和pathvar属性`来区分，name表示矩阵变量名，pathVar表示当前矩阵变量所在的URI路径变量名！**

```java
@GetMapping("/matrix/{path1}/{path2}")
public void handle(@PathVariable int path1, @PathVariable int path2,
                    @MatrixVariable(name = "q", pathVar = "path1") int q1,
                    @MatrixVariable(name = "q", pathVar = "path2") int q2) {
    System.out.println(path1);
    System.out.println(path2);
    System.out.println(q1);
    System.out.println(q2);
    //…………
}
```

访问/matrix/11;q=22/33;q=44，结果如下：

```java
11
33
22
44
```

**矩阵变量如果不存在，默认将会抛出异常，当然可以通过`required`属性定义为可选变量以及通过defaultValue指定默认值，如下例所示：**

```java
java复制代码@GetMapping("/matrix/req/{id}")
public void handle(@MatrixVariable(required = false, defaultValue = "110") int q) {
    System.out.println(q);
    //…………
}
```

访问/matrix/req/11，结果如下：

```java
110
```

**若要获取所有矩阵变量，可以使用`MultiValueMap`，如下例所示：**

```java
@GetMapping("/matrix/all/{all1}/{all2}")
public void handle(@MatrixVariable MultiValueMap<String, String> allmatrixVars,
                   @MatrixVariable(pathVar = "all1") MultiValueMap<String, String> all1MatrixVars,
                   @MatrixVariable(pathVar = "all2") MultiValueMap<String, String> all2MatrixVars
) {
    System.out.println(allmatrixVars);
    System.out.println(all1MatrixVars);
    System.out.println(all2MatrixVars);
    //…………
}
```

访问/matrix/all/11;q=22;r=33/44;q=55;s=66，结果如下：

```java
{q=[22, 55], r=[33], s=[66]}
{q=[22], r=[33]}
{q=[55], s=[66]}
```

### @RequestParam

我们可以使用@RequestParam注解将 Servlet 请求参数（即查询参数或表单数据）绑定到控制器方法参数上。如果目标方法参数类型不是String，则自动应用类型转换。

@RequestParam 注解的name或者value属性表示绑定的请求参数名称，如果不设置，那么默认查找和变量名同名的请求参数，如下所示：

```java
@RestController
public class RequestParamController {

    @GetMapping("/requestParam1")
    public void handle(@RequestParam String str) {
        System.out.println(str);
        //…………
    }
}
```

访问/requestParam1?str=test，结果如下：

```java
test
```

**默认情况下，使用此注解标注的方法参数是必需的，如果没有对应的变量，将抛出异常，但可以通过将@RequestParam 注解的`required`标志设置为 false 或使用Java8的`java.util.Optional`包装原始参数类型来指定方法参数是可选的。**

```java
@GetMapping("/requestParam2")
public void handle2(@RequestParam Optional<String> str) {
    //是否存在该参数
    System.out.println(str.isPresent());
    //如果存在该参数，那么输出
    str.ifPresent(System.out::println);
    //…………
}
```

访问/requestParam2，结果如下：

```java
false
```

**required=false时，如果没有对应的请求参数，控制器方法对应的参数会被赋值为null，`如果参数是基本类型，此时会抛出异常，因为基本类型不能赋值为null`。可以使用基本类型的包装类，或者`java.util.Optional`，或者通过`defaultValue`属性设置默认值！**

```java
@GetMapping("/requestParam3")
public void handle3(@RequestParam(required = false,defaultValue = "123") long i) {
    System.out.println(i);
    //…………
}
```

访问/requestParam3，结果如下：

```java
123
```

**如果将参数类型声明为数组或列表则表示允许解析同一参数名称的多个参数值。**

```java
@GetMapping("/requestParam4")
public void handle4(@RequestParam long[] longs) {
    System.out.println(Arrays.toString(longs));
    //…………
}
```

访问/requestParam4?longs=11&longs=22，结果如下：

```java
[11, 22]
```

当 @RequestParam 注解标注在一个`Map<String, String>`或`MultiValueMap<String, String>`上，并且没在注解中指定的参数名称时，那么每个给定参数名称和对应的参数值都将被填充到该Map中。

```java
@GetMapping("/requestParam5")
public void handle5(@RequestParam MultiValueMap<String, String> map) {
    System.out.println(map);
    //…………
}
```

访问/requestParam5?a=11&a=22&b=33，结果如下：

```java
{a=[11, 22], b=[33]}
```

@RequestParam是可选的。默认情况下，任何简单值类型的参数（由 `BeanUtils#isSimpleProperty `确定），并且不由任何其他参数解析器解析，将被视为使用了@RequestParam 注解。

```java
@GetMapping("/requestParam6")
public void handle5(int a, String b) {
    System.out.println(a);
    System.out.println(b);
    //…………
}
```

访问/requestParam6?a=11&b=bb，结果如下：

```java
11
bb
```

### @RequestHeader

可以使用@RequestHeader注解将请求头的数据绑定到控制器中的方法参数中。如果目标方法参数类型不是String，则自动应用类型转换。

name和value属性用于指定请求头参数名，如果不指定，那么将方法参数名作为请求头参数名。required用于指示该请求头是否是必须的，默认为true，required=false时，defaultValue用于在不存在该请求头时指定默认值。

```java
@RestController
public class RequestHeaderController {

    @GetMapping("/header")
    public void handle(@RequestHeader(name = "Accept-Encoding", required = false) String acceptEncoding,
                       @RequestHeader(name = "Keep-Alive", required = false) String keepAlive) {
        System.out.println(acceptEncoding);
        System.out.println(keepAlive);
        //…………
    }
}
```

当 @RequestParam 注解标注在一个`Map<String, String>`或`MultiValueMap<String, String>`或`HttpHeaders`类型的参数上时，那么每个请求头的数据都将被填充到该Map中。

```java
@GetMapping("/header/all")
public void handle2(@RequestHeader Map<String, String> map,
                    @RequestHeader MultiValueMap<String, String> multiValueMap,
                    @RequestHeader HttpHeaders httpHeaders) {
    System.out.println(map);
    System.out.println(multiValueMap);
    System.out.println(httpHeaders);
    //…………
}
```

**Spring MVC内置支持可用于将逗号分隔的字符串转换为数组或字符串集合或类型转换系统已知的其他类型，这样就避免了手动转换操作。例如，用@RequestHeader("Accept")注解的方法参数可以是String类型，也可以是`String[]`或`List<String>`类型。**

### @CookieValue

可以使用`@CookieValue`注解将 `HTTP Cookie` 的值绑定到控制器中的方法参数中。如果目标方法参数类型不是String，则自动应用类型转换。

`name`和`value`属性用于指定`cookie名`，如果不指定，那么将`方法参数名`作为cookie名。`required`用于指示该cookie是否是必须的，默认为true，required=false时，`defaultValue`用于在不存在该cookie时指定默认值。

```java
@RestController
public class CookieValueController {

    @GetMapping("/setCookie")
    public void handle(HttpServletResponse resp) throws IOException {
        //生成一个随机字符串
        String uuid = UUID.randomUUID().toString();
        System.out.println("setCookie: " + uuid);
        //创建Cookie对象，指定名字和值。Cookie类只有这一个构造器
        Cookie cookie = new Cookie("uuid", uuid);
        //在响应中添加Cookie对象
        resp.addCookie(cookie);
    }

    @GetMapping("/cookieValue")
    public void handle1(@CookieValue String uuid) {
        System.out.println("cookieValue: " + uuid);
        //…………
    }
}
```

首先访问/setCookie，设置一个uuid的cookie，然后访问/cookieValue尝试获取，结果如下：

```java
setCookie: 1ad719b2-fdfd-489b-8f29-ee1e85eb2696
cookieValue: 1ad719b2-fdfd-489b-8f29-ee1e85eb2696
setCookie: 24646fa8-e033-411f-8f6c-b379830666c0
cookieValue: 24646fa8-e033-411f-8f6c-b379830666c0
```

### @ModelAttribute

#### 作用于方法参数

**可以在方法参数上使用@ModelAttribute注解来注入参数属性，如果没找到该属性，则将其实例化，同时，还会使用属性名称与HTTP Servlet请求参数中的同名字段名称的值来进行填充属性，这称为数据绑定**！默认的name属性根据参数类型推断而不是参数名。

默认情况下，任何不是简单值类型的参数（由 BeanUtils#isSimpleProperty 确定），并且不由任何其他参数解析器解析，都将被视为使用了@ModelAttribute 注解。

**@ModelAttribute的工作大概分为两步：**

1. 第一个步是获取参数属性实例，获取规则为：
   1. 从model存储的数据中查找；
   2. 从@SessionAttributes存储的数据中查找；
   3. 通过Converter依次从URI路径变量、URL参数、Form表单中查找并尝试转换为对应参数类型（查找的参数名基于参数类型设置，通常是类名的小写），找到就返回；无法强制转化为Date参数。
   4. 调用默认构造器器创建对象。
      1. 如果有多个构造器且没有空构造器，将抛出异常，建议只提供一个构造函数！
      2. 如果是基本类型，将抛出异常！
2. 在找到实例之后，第二步是通过WebDataBinder进行属性绑定和校验：
   1. 依次从URI路径变量、Form表单、URL参数中查找，如果找到同名属性的变量，那么该属性将赋值为找到的值（通过setter方法，并且设置到Converter的类型转换），否则，将使用空属性。URI路径变量、Form表单、URL参数中如果存在同名参数，则后面值的覆盖前面的值！
   2. 可以通过对参数添加 javax.validation.valid 注解或 Spring 的@Validated注解，在数据绑定后自动应用验证。

#### 作用于方法

被@ModelAttribute注解标注的方法会在控制器方法执行之前先执行，同时，@ModelAttribute方法可以和控制器方法一样的绑定、获取各种参数！通常用于辅助初始化model参数，实际项目中用的并不多。

如果用在@Controller类中，那么作用域就是当前Controller的控制器方法，如果用在@ControllerAdvice类中，那么作用域就是所有Controller的控制器方法。

**修饰没有返回值的方法**，此时可以在参数上声明一个Map、Model、ModelMap类型的参数，可以将数据存入model中，model中的数据使用范围是当前request。

**修饰有返回值的方法**。返回值会放到model中。此model属性的名称没有指定，它由返回值类型经过计算来定义，如方法返回User类型，那么属性值是User对象，属性的名称是user，即简单类名（实际上最后会通过Introspector#decapitalize方法得到），也可以通过@ModelAttribute(value="myModelName")指定名称。

**如果将@ModelAttribute和@RequestMapping标注在同一个方法上，那么方法返回值将被作为model属性！**

```java
@ModelAttribute
public MyModel model() {
    System.out.println("---ModelAttribute方法执行---");
    MyModel myModel = new MyModel();
    myModel.setModelId(123L);
    myModel.setModelType(321);
    myModel.setModelName("ModelAttribute");
    System.out.println(myModel.hashCode());
    return myModel;
}

@GetMapping("/modelAttribute")
public void handle5(MyModel myModel) {
    System.out.println("---处理器方法支持---");
    System.out.println(myModel);
    System.out.println(myModel.hashCode());
}
```

### @SessionAttribute

@SessionAttributes是一个类级别的注释，标注在某个控制器类上之后，该控制器中存入model的属性将存入session中，并且其他的控制器方法将可以共享的使用的session中的model属性。

即，如果属性只存入model中，那么当前request的调用链之间的方法可以共享数据，加了@SessionAttributes之后，存入model中的数据实际上会属性存入session中，不同request请求之间可以共享数据。

@SessionAttributes的name或者value属性是一个String数组，用于指定要存入session的model数据的名称，也可以设置types属性，它是一个Class数组，用于指定要存入session的model数据的类型。

如果想要清除当前类中通过@SessionAttributes存储到session中的数据，可以将通过org.springframework.web.bind.support.SessionStatus作为参数注入控制器方法，并调用setComplete方法，该方法不会清理真正的session中的数据！

在方法参数上使用 @SessionAttribute 注解可以访问预先存在的session域中的会话属性，无论是通过传统Session对象设置的还是通过@SessionAttributes设置的！

如果需要添加或删除指定的session属性，可以将 org.springframe. web.context.request.WebRequest或 javax.servlet.httpSession作为参数注入控制器方法。

```java
@RestController
@SessionAttributes({"myModel", "model"})
public class SessionAttributesController {

    /**
     * 通过@ModelAttribute存入的数据
     */
    @ModelAttribute
    public MyModel model() {
        MyModel myModel = new MyModel();
        myModel.setModelId(111L);
        myModel.setModelType(111);
        myModel.setModelName("ModelAttribute1");
        return myModel;
    }

    @GetMapping("/setsessionAttributes")
    public void handle1(Model model, HttpServletRequest request) {
        //手动存入的model数据
        MyModel myModel = new MyModel();
        myModel.setModelId(222L);
        myModel.setModelType(222);
        myModel.setModelName("ModelAttribute2");
        model.addAttribute("model", myModel);

        //手动存入session的数据
        HttpSession session = request.getSession();
        UUID uuid = UUID.randomUUID();
        System.out.println(uuid);
        session.setAttribute("id", uuid);
    }

    /**
     * 清理session
     */
    @GetMapping("/cleansessionAttributes")
    public void handle2(SessionStatus sessionStatus) {
        sessionStatus.setComplete();
    }
}
```

```java
@RestController
public class SessionAttributesController2 {

    /**
     * @param myModel 尝试获取通过@ModelAttribute存入的数据
     * @param model   尝试获取手动存入的model数据
     * @param id      尝试获取手动存入session的数据
     */
    @GetMapping("/getsessionAttributes")
    public void handle1(@SessionAttribute(name = "myModel",required = false) MyModel myModel,
                        @SessionAttribute(name = "model",required = false) MyModel model,
                        @SessionAttribute String id) {
        System.out.println(myModel);
        System.out.println(model);
        System.out.println(id);
    }
}
```

首先访问/sessionAttributes1，随后访问/sessionAttributes2，将可以获取session中的model数据！

```java
d8783090-da1e-495f-ac48-32a554009e20
MyModel{modelId=111, modelType=111, modelName='ModelAttribute1'}
MyModel{modelId=222, modelType=222, modelName='ModelAttribute2'}
d8783090-da1e-495f-ac48-32a554009e20
```

然后访问/cleansessionAttributes清除session，再次访问/sessionAttributes2，结果如下：

```java
null
null
d8783090-da1e-495f-ac48-32a554009e20
```

### @RequestAttribute

**与`@SessionAttribute`类似，可以使用`@RequestAttribute`注解访问之前创建的预先存在的request域中的属性。**

```java
@Controller
public class RequestAttributeController {

    @GetMapping("/requestAttribute1")
    public String handle1(HttpServletRequest request) {
        request.setAttribute("ra", "requestAttributeTest");
        return "/requestAttribute2";
    }


    @GetMapping("/requestAttribute2")
    @ResponseBody
    public void handle2(@RequestAttribute String ra) {
        System.out.println(ra);
    }

}
```

### 重定向参数

默认情况下，所有的model属性中的基本类型的属性，或者基本类型的数组、集合将自动追加为请求参数。

```java
@Controller
public class RedirectAttributesController {

    @GetMapping("/redirectAttributes1")
    public String handle1(Model model) {
        model.addAttribute("a", "aaa");
        model.addAttribute("b", 111);
        model.addAttribute("c", new Object());
        model.addAttribute("d", new int[]{1, 2, 3});
        return "redirect:/redirectAttributes2";
    }

    @GetMapping("/redirectAttributes2")
    @ResponseBody
    public void handle2(@RequestParam MultiValueMap<String, String> map, HttpServletRequest request) {
        System.out.println(map);
        Map<String, String[]> parameterMap = request.getParameterMap();
        System.out.println(parameterMap);
    }
}
```

访问/redirectAttributes1，转发后的URL如下：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6bc9f37e549e4501872878f3191f3096~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

很明显，model中可能包含其他不需要追加到URL中的参数，因此这不是一个好方法。为此，处理器方法中可以声明类型为RedirectAttributes 的参数，并用它的addAttribute方法来指定要提供给重定向视图的确切属性，如果控制器方法参数中存在RedirectAttributes参数，并且该方法确实进行了重定向，则使用RedirectAttributes的内容，否则，将使用model的内容。

```java
@GetMapping("/redirectAttributes3")
public String handle3(ModelMap model, RedirectAttributes redirectAttributes) {
    model.addAttribute("a", "aaa");
    model.addAttribute("b", 111);
    model.addAttribute("c", new Object());
    model.addAttribute("d", new int[]{1, 2, 3});

    redirectAttributes.addAttribute("a", "a");
    redirectAttributes.addAttribute("b", 222);
    redirectAttributes.addAttribute("d", new int[]{4, 5});
    return "redirect:/redirectAttributes4";
}

@GetMapping("/redirectAttributes4")
@ResponseBody
public void handle4(@RequestParam MultiValueMap<String, String> map, HttpServletRequest request) {
    Map<String, String[]> parameterMap = request.getParameterMap();
    System.out.println(parameterMap);
    System.out.println(map);
}
```

`RequestMappingHandlerAdapter`提供了一个名为`ignoreDefaultModelOnRedirect`的标志位属性，将此标志设置为 true 可确保在重定向中永远不会使用默认model属性，即使控制器方法中未声明`RedirectAttributes`参数也是如此。将其设置为 false 意味着如果控制器方法不声明RedirectAttributes参数，则默认model可能在重定向中使用，默认值为false。该参数还可以通过`<mvc:annotation-driven>`标签的`ignore-default-model-on-redirect`属性来配置。

`RedirectAttributes的addAttribute`方法同样会将属性追加到URL后面，对于隐私性不友好，并且对于复杂对象类型无法传递，为此我们可以使用另一个`addFlashAttribute`方法，`flash attributes`将会保存在`HTTP session`中（因此参数不会出现在URL中）。

```java
@GetMapping("/redirectAttributes5")
public String handle5(ModelMap model, RedirectAttributes redirectAttributes) {
    model.addAttribute("a", "aaa");
    model.addAttribute("b", 111);
    model.addAttribute("c", new Object());
    model.addAttribute("d", new int[]{1, 2, 3});

    Object o = new Object();
    System.out.println(o);
    redirectAttributes.addFlashAttribute("a", "aaa");
    redirectAttributes.addFlashAttribute("b", 111);
    redirectAttributes.addFlashAttribute("c", o);
    redirectAttributes.addFlashAttribute("d", new int[]{1, 2, 3});
    return "redirect:/redirectAttributes6";
}

@GetMapping("/redirectAttributes6")
@ResponseBody
public void handle6(@RequestParam MultiValueMap<String, String> map, HttpServletRequest request,
                    @ModelAttribute("a") String a, @ModelAttribute("c") Object c, @ModelAttribute("d") int[] d) {
    //无法获取到参数
    Map<String, String[]> parameterMap = request.getParameterMap();
    System.out.println(parameterMap);
    System.out.println(map);
    //可以通过ModelAttribute获取到参数
    System.out.println(a);
    System.out.println(c);
    System.out.println(Arrays.toString(d));
}
```

访问/redirectAttributes5，结果如下：

```java
java.lang.Object@612eabb7
{}
{}
aaa
java.lang.Object@612eabb7
[1, 2, 3]
```

并且重定向的URL中不再带有参数：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d28c3536806c405aa31e03b0eb55fa52~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

另外，如果重定向到一个页面视图中，那么能直接用el表达式获取对应的值！

如果存在URI变量，那么在重定向的URL中可以直接使用同名变量！

```java
@PostMapping("/files/{path}")
public String upload(...) {
    // ...
    return "redirect:files/{path}";
}
```

### Flash Attributes

`Flash attributes`为一个请求提供了一种存储用于另一个请求的属性的方法。这在重定向时最常用，例如Post-Redirect-Get模式。`Flash attributes`在重定向之前被临时保存（通常在session中），以便在重定向之后可供请求使用，并立即被删除。

Spring MVC有两个主要的抽象类来支持Flash attributes。 一个是`FlashMap`，它用于保存Flash属性，而另一个是`FlashMapManager`，它用于存储，检索和管理FlashMap实例。

Flash attributes的支持始终处于“打开”状态，无需显式启用。但是，如果不使用它，则永远不会导致HTTP session的创建。在每个请求上，都有一个“input（输入）” FlashMap，其属性是从上一个请求（如果有）传递过来的，而“output（输出）” FlashMap的属性是为后续请求保存的。这两个FlashMap实例都可以通过RequestContextUtils中的getInputFlashMap和getOutputFlashMap静态方法在Spring MVC中的任何位置访问。

基于注解的控制器通常不需要直接使用FlashMap。相反，@RequestMapping方法可以接受RedirectAttributes类型的参数，并使用它为重定向方案添加Flash attributes。通过RedirectAttributes添加的Flash attributes会自动传播到“output（输出）” FlashMap。同样，重定向到目标后，来自“input（输入）” FlashMap的属性会自动添加到服务于目标URL的控制器的model中，这就是在上面的案例中可以使用@ModelAttribute绑定属性以及可以在模型页面中直接通过el获取属性的原因！

根据上面的描述，我们还可以在控制器方法中通过RequestContextUtils来获取RedirectAttributes传递的属性：

```java
@GetMapping("/redirectAttributes7")
@ResponseBody
public void handle7(HttpServletRequest request,@ModelAttribute("a") String a, @ModelAttribute("c") Object c, @ModelAttribute("d") int[] d) {
    Map<String, ?> inputFlashMap = RequestContextUtils.getInputFlashMap(request);
    System.out.println(inputFlashMap);
    //可以通过ModelAttribute获取到参数
    System.out.println(a);
    System.out.println(c);
    System.out.println(Arrays.toString(d));
}
```

我们让/redirectAttributes5的控制器方法重定向到访问/redirectAttributes7，访问/redirectAttributes5之后，结果如下：

```java
java.lang.Object@618c3937
FlashMap [attributes={a=aaa, b=111, c=java.lang.Object@618c3937, d=[I@6d2cd428}, targetRequestPath=/mvc/redirectAttributes7, targetRequestParams={}]
aaa
java.lang.Object@618c3937
[1, 2, 3]
```

可以看到，input的FlashMap中正好存储着传递的属性！

### Multipart

在启用MultipartResolver后，带有multipart/form-data的POST请求的内容将被解析并作为常规请求参数进行访问，并且支持使用MultipartFile参数接收上传的文件数据。如下案例：

```java
@Controller
public class FileUploadController {

    @PostMapping("/form")
    public String handleFormUpload(@RequestParam("name") String name,
                                   @RequestParam("file") MultipartFile file) {

        if (!file.isEmpty()) {
            byte[] bytes = file.getBytes();
            // store the bytes somewhere
            return "redirect:uploadSuccess";
        }
        return "redirect:uploadFailure";
    }
}
```

将参数类型声明为`List<MultipartFile>`则表示允许解析相同参数名称的多个文件。

当@RequestParam 注解声明为`Map<String, MultipartFile>`或`MultiValueMap<String, MultipartFile>`时，在注解中未指定参数名称的情况下，将填充映射本次请求中的所有给定参数名称的multipart files。

在使用 Servlet 3.0 的`StandardServletMultipartResolver`而不是Apache的`CommonsMultipartResolver`时，还可以声明`javax.servlet.http.part`而不是 Spring 的`MultipartFile`来作为方法参数或集合值类型。

multipart多部件内容还可以数据绑定到`命令对象（即通过@ModelAttribute查找并绑定的参数对象）`的某些属性上。

将`@RequestPart`与` javax.validation.Valid`或Spring 的 `@Validated`一起使用，可用于多部分文件的校验！

### @RequestBody

可以使用@RequestBody注解通过HttpMessageConverter读取请求正文（通常是JSON字符串）并反序列化为控制器方法的参数对象。在前端分离开发的项目中，通常是采用JSON数据交互，因此该注解非常常用！我们通过mvc配置自定义HttpMessageConverter。

另外，如果在@ModelAttribute方法中通过@RequestBody从输入流中获取了数据，那么在真正的控制器方法中便不能再次获取，因为`ServletInputStream`已在@ModelAttribute方法中被使用并被关闭。

如果是采用JSON数据交互，那么我们应该引入相关`JSON依赖`，Spring MVC默认依赖`jackson`来实现JSON的序列化和反序列化，如果存在该jar包，则Spring MVC会自动创建一个`MappingJackson2HttpMessageConverter`，而无需我们手动注册！

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson
-databind -->
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.9.8</version>
</dependency>
```

一个Controller：

```java
@RestController
public class RequestBodyController {

    @PostMapping("/requestBody")
    public void handle(@RequestBody User user) {
        System.out.println(user);
        //…………
    }
}
```

```java
public class User {

    private int age;
    private String name;

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "age=" + age +
                ", name='" + name + '\'' +
                '}';
    }
}
```

访问/requestBody，并且请求体Content-Type为Content-Type格式，请求体中传递{"age":12,"name":"名字"}，结果如下：

```java
User{age=12, name='名字'}
```

成功的进行了JSON数据的反序列化和封装！

可以将@RequestBody注解标注的参数继续标注`javax.validation.Valid `或 Spring 的 `@Validated`注解，这两种注解都会导致应用标准 Bean 验证。默认情况下，验证错误会导致`MethodArgumentNotValidException`，该异常将转换为 400（BAD_REQUEST）响应。或者，可以通过`Errors或BindingResult参数`在控制器方法内处理异常。关于参数校验，后面会单独讲解！

### HttpEntity

**HttpEntity参数可以实现@RequestBody的功能，将请求体反序列化为对应泛型类型的对象实例，但是它还提供获取请求头HttpHeaders的功能！**

```java
@RestController
public class HttpEntityController {

    @PostMapping("/httpEntity")
    public void handle(HttpEntity<User> httpEntity) {
        //获取请求体
        User body = httpEntity.getBody();
        System.out.println(body);
        //获取请求头
        HttpHeaders headers = httpEntity.getHeaders();
        System.out.println(headers);
        //…………
    }
}
```

访问/httpEntity，并且请求体Content-Type为Content-Type格式，请求体中传递{"age":12,"name":"名字"}，结果如下（我采用postman测试）：

```java
User{age=12, name='名字'}
[content-type:"application/json", user-agent:"PostmanRuntime/7.26.5", accept:"*/*", cache-control:"no-cache", pos
```

### @ResponseBody

我们可以在方法上使用@ResponseBody注解，表示将会通过HttpMessageConverter将控制器方法返回的实体进行序列化并且设置为响应体，而不会走视图解析的逻辑。

我们此前说的前后端分离以及JSON数据交互的开发方式中，就是通过@RequestBody将前端发送的请求的请求体中的JSON数据反序列化为控制器方法的参数对象实例，通过@ResponseBody将后端控制器方法返回的对象实例序列化为JSON数据然后写入相应体返回给前端！序列化和反序列化都依赖于HttpMessageConverter。

@ResponseBody也支持在@Controller类上使用，在这种情况下，该类的所有控制器方法都继承该注解。这和使用一个@RestController有同样的效果，它的内部将@Controller和@ResponseBody作为元注解。

简单的前后端分离的开发方式如下：

```java
@RestController
public class ResponseBodyController {

    @PostMapping("/responseBody")
    public User handle(@RequestBody User user) {
        System.out.println(user);
        user.setAge(999);
        user.setName("responseBody");
        //返回的实体将会被序列化并且写入响应体，而不会走视图解析的逻辑
        return user;
    }
}
```

实际开发中，项目应该统一返回一个结果对象，可能包括响应码、消息、数据这三部分，而不仅仅是数据部分！

@ResponseBody同样适用于反应式类型的返回结果，比如Servlet3.0的异步请求返回的`DeferredResult`。

### ResponseEntity

ResponseEntity返回值可以实现@ResponseBody的功能，将返回的实体序列化为JSON数据并写入响应体，但是它还提供设置响应码和响应头的功能！

```java
@RestController
public class ResponseEntityController {

    @PostMapping("/responseEntity")
    public ResponseEntity<User> handle(@RequestBody User user) {
        System.out.println(user);
        user.setAge(888);
        user.setName("responseEntity");
        return ResponseEntity.ok().header("header", "header").body(user);
    }
}
```

ResponseEntity同样适用于反应式类型的返回结果，比如Servlet3.0的异步请求返回的DeferredResult。

### Json View

**Spring默认了对Jackson JSON库的支持。**

首先我们需要引入jackson的依赖：

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson
-databind -->
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.9.8</version>
</dependency>
```

Spring MVC 为`Jackson的序列化视图（https://www.baeldung.com/jackson-json-view-annotation）`提供内置支持，该视图可以实现只序列化对象中的指定字段。

若要将它与@ResponseBody或ResponseEntity控制器方法一起使用，可以使用 `Jackson 的 @JsonView `注解来激活序列化视图类，如下例所示：

```java
/**
 * @author lx
 */
@RestController
public class JsonViewController {

    /**
     * 在方法上使用@JsonView注解
     */
    @GetMapping("/client")
    @JsonView(Client.WithoutPasswordView.class)
    public Client getUser() {
        return new Client("eric", "7!jd#h23");
    }
}
```

```java
/**
 * @author lx
 */
public class Client {

    /**
     * 该接口用于指示返回的JSON视图没有password字段
     */
    public interface WithoutPasswordView {
    }

    /**
     * 该接口用于指示返回的JSON视图具有password字段
     */
    public interface WithPasswordView extends WithoutPasswordView {
    }

    /**
     * 在字段或者getter方法上标注@JsonView注解
     */
    @JsonView(WithoutPasswordView.class)
    private String username;
    @JsonView(WithPasswordView.class)
    private String password;


    public String getUsername() {
        return this.username;
    }

    public String getPassword() {
        return this.password;
    }

    public Client() {
    }

    public Client(String username, String password) {
        this.username = username;
        this.password = password;
    }
}
```

**上面的案例中，对于实体的字段或者getter方法使用`@JsonView`注解，并且指定一个类型，在控制器方法上，同样使用`@JsonView`注解，该注解中具有一个类型，它表示对于实体中所有具有该类型及其父类型的`@JsonView`注解标注的字段或者getter方法对应的字段都进行序列化，其他字段则不进行序列化！**

我们访问/client，结果如下：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/49abea19fa5c4ebbb4871df2376f3723~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

可以看到，password字段并没有被序列化，如果我们将控制其方法的@JsonView的类型改为Client.WithPasswordView.class，那么结果如下：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8b11bb6ad32b460db9828184336cb9af~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

可以看到，password字段已被被序列化！

实际上@JsonView注解允许传递一个视图类Class数组，即指定多个视图类，但每个控制器方法只能指定一个@JsonView注解。

## ControllerAdvice

通常，`@ExceptionHandler，@InitBinder和@ModelAttribute`方法在声明它们的@Controller类（或类层次结构）中生效。如果要使此这些方法更全局地应用（跨不同的控制器），则可以在标有`@ControllerAdvice或@RestControllerAdvice`的类中声明它们。

@ControllerAdvice注解采用`@Component`作为元注解诶，这意味着可以通过组件扫描将此类注册为Spring Bean。 @RestControllerAdvice则将`@ControllerAdvice和@ResponseBody`作为元注解，这意味着`@ExceptionHandler`方法的返回值可以通过HttpMessageConverter进行序列化（比如转换为`JSON字符串`）并添加到响应体。

在启动时，具有@RequestMapping和@ExceptionHandler方法的类将会检测@ControllerAdvice类型的Spring bean，然后在运行时应用其方法。`全局@ExceptionHandler`方法（来自@ControllerAdvice）在`本地@ExceptionHandler`方法（来自@Controller）之后应用。相比之下，`全局@ModelAttribute和@InitBinder`方法则在`本地@ModelAttribute和@InitBinder`方法之前应用。

多个@ControllerAdvice或@RestControllerAdvice类支持`order排序`，可实现Ordered、PriorityOrdered接口，或者采用@Order、@Priority注解。比较优先级为PriorityOrdered>Ordered>@Order>@Priority，如果没有order值，那么将返回Integer.MAX_VALUE，即最低优先级。

默认情况下，@ControllerAdvice方法可以应用于每个请求（就是所有控制器的所有方法），但可以使用注解上的属性将其缩小应用范围：

1. @ControllerAdvice具有`annotations`属性，它是一个Annotation类型的Class数组，表示生效的目标是所有至少使用了其中一个给定注解的控制器！
2. @ControllerAdvice具有`value和basePackages`属性，它是一个String类型的数组，表示生效的目标是所有至少属于其中一个包以及子包路径下的控制器！
3. @ControllerAdvice具有`assignableTypes`属性，它是一个Class类型的数组，表示生效的目标是所有至少属于其中一个类型的控制器！
4. 如果声明了`多个选择器`，那么只要某个控制器只需要符合其中一个。另外，选择器检查在运行时执行，因此添加许多选择器可能会对性能产生负面影响并增加复杂性。

**@ExceptionHandler用于异常处理，@InitBinder用于数据绑定时的类型转换**。

# 数据绑定

在数据绑定过程中，Spring [MVC框架](https://so.csdn.net/so/search?q=MVC框架&spm=1001.2101.3001.7020)会通过数据绑定组件（DataBinder）将请求参数串的内容进行类型转换，然后将转换后的值赋给控制器类中方法的形参，这样后台方法就可以正确绑定并获取客户端请求携带的参数了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513091336952.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTczMTU5,size_16,color_FFFFFF,t_70)

1. Spring MVC将ServletRequest对象传递给DataBinder;
2. 将处理方法的入参对象传递给DataBinder;
3. DataBinder调用ConversionService组件进行数据类型转换、数据格式化等工作，并将ServletRequest对象中的消息填充到参数对象中;
4. 调用Validator组件对已经绑定了请求消息数据的参数对象进行数据合法性校验;
5. 校验完成后会生成数据绑定结果BindingResut对象，Spring MVC会将BindingResult对象中的内容赋给处理方法的相应参数。

1. springMVC框架将ServletRequest对象及方法的如参实例传递给WebDataBinderFactory石丽以创建DataBinder对象。
2. DataBinder调用装配在springMVC上下文的ConversionService组件进行数据类型转换、数据格式化(类型转换和格式化是一起的一会来看源码)，将Servlet中的请求信息填充到如参的对象中。
3. 调用Validator组件对已经完成绑定了的请求消息的入参对象进行数据合法性校验，把最终生成数据绑定结果BindingData对象
4. 如果在数据类型转换、格式化和校验的过程中出现错误的话，会把处理的结果放到BindingResult对象中，将它们赋给处理方法的响应入参。

```java
public final Object resolveArgument(
		MethodParameter parameter, ModelAndViewContainer mavContainer,
		NativeWebRequest request, WebDataBinderFactory binderFactory)
		throws Exception {
 
	String name = ModelFactory.getNameForParameter(parameter);
	Object attribute = (mavContainer.containsAttribute(name)) ?
			mavContainer.getModel().get(name) : createAttribute(name, parameter, binderFactory, request);
 
	WebDataBinder binder = binderFactory.createBinder(request, attribute, name);
	if (binder.getTarget() != null) {
		bindRequestParameters(binder, request);
		validateIfApplicable(binder, parameter);
		if (binder.getBindingResult().hasErrors()) {
			if (isBindExceptionRequired(binder, parameter)) {
				throw new BindException(binder.getBindingResult());
			}
		}
	}
 
	// Add resolved attribute and BindingResult at the end of the model
 
	Map<String, Object> bindingResultModel = binder.getBindingResult().getModel();
	mavContainer.removeAttributes(bindingResultModel);
	mavContainer.addAllAttributes(bindingResultModel);
 
	return binder.getTarget();
}
```

10行，创建了一个WebDataBinder对象。传入了request，目标方法的参数，还有属性名(目标方法中没有加@ModelAttribute修饰，为目标方法如参类名第一个字母小写)。

12，13行，分别进行数据的绑定和校验。

这个对象中就包括了数据的转换格式化(conversionService，从名字上就能看出来，包含了Formatting和Conversion)，使用数据校验(validators)组件完成的数据效验，如果在数据转换格式化或者是娇艳过程中出现错误会把结果放在绑定结果(bindingResult)中。



在开发中前后台交互的数据无非是下面几种：

+ 基本类型（int、double、Integer、String 等）
+ 对象（类）类型（自定义的实体类）
+ 日期类型（java.util.Date）
+ 复杂类型（对象数组、List、Set、Map 等）
+ 特殊文本类型（JSON、XML 等）

下面就总结一下这些数据在 SpringMVC 中如何绑定到方法形参中。

使用 Maven 来搭建项目，所有的代码都已上传到 GitHub 上，有需要的小伙伴可以前往[下载](https://github.com/weizhiwen/springmvc-databind)，也欢迎你 star 该仓库哦！

在给方法加上 @ResponseBody 注解后，直接将处理好的数据输出到响应流中，没有了试图解析过程，也就是返回的是 JSON 类型。SpringMVC 这里使用了适配器模式来处理数据转换，当我们使用 Jackson 作为解析 JSON 工具，这里注意一个大坑，Jackson 内默认的编码为 ISO-8859-1（大坑），这就会导致在输出中文时乱码，这点可以通过浏览器的控制台查看，解决方法有以下几种。

1、在每个方法上加上编码设置`@RequestMapping(value = "basetype3.do", produces = "application/json; charset=utf-8")`

2、在 SpringMVC 配置文件中修改 Jackson 的默认编码为 UTF-8，注意要放在 `<mvc:annotation-driven/>` 前面，放在内部是不生效的。

3、更改 JSON 解析工具，推荐使用阿里的  fastjson，默认编码就是 UTF-8，解析速度也比 Jackson 快。

```xml
<!--特别注意：必须放在mvc:annotation-driven前面，放在内部是不会生效的-->
    <!--json装换器使用 Jackson 默认编码是 ISO-8859-1 需要重新设置编码-->
    <!--<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">-->
        <!--<property name="messageConverters">-->
            <!--<list>-->
                <!--<bean class="org.springframework.http.converter.StringHttpMessageConverter">-->
                    <!--<property name="supportedMediaTypes">-->
                        <!--<list>-->
                            <!--<value>text/plain;charset=UTF-8</value>-->
                            <!--<value>text/html;charset=UTF-8</value>-->
                            <!--<value>applicaiton/json;charset=UTF-8</value>-->
                        <!--</list>-->
                    <!--</property>-->
                <!--</bean>-->
            <!--</list>-->
        <!--</property>-->
    <!--</bean>-->
    <!-- json装换器使用 fastjson，默认就是 UTF-8 编码，不需要再重新设置编码-->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
        <property name="messageConverters">
            <list>
                <bean class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter"/>
            </list>
        </property>
    </bean>
    <!--配置注解驱动-->
    <mvc:annotation-driven/>
```

## 简单类型

“在执行程序时，Spring MVC会根据客户端请求参数的不同，将请求消息中的信息以一定的方式转换并绑定到控制器类的方法参数中。这种将请求消息数据与后台方法参数建立连接的过程就是Spring MVC中的数据绑定。

SpringMVC里面，所谓的数据绑定就是将请求带过来的表单数据绑定到执行方法的**参数变量**。

实际开发中，SpringMVC作为表现层框架，肯定会接受前台页面传递过来的参数，SpringMVC提供了丰富的接受参数的方法。

SpringMVC 有一些数据是不能自动绑定，需要我们使用它提供的注解强制绑定。

　　遇到需要强制绑定的几种情况：

　　（1）默认参数自动绑定的是表单数据，如果数据不是来自表单，那么需要强制绑定

　　（2）数据是来自表单的，但是参数名不匹配，那么也需要强制绑定

　　（3）数据是来自表单的，但是需要将数据绑定在 Map 对象里面，需要强制绑定

当前端请求的参数比较简单时，可以在后台方法的形参中直接使用Spring MVC提供的默认参数类型进行数据绑定。如下所示：

+ **HttpServletRequest**：通过request对象获取请求信息；
+ **HttpServletResponse**：通过response处理响应信息；
+ **HttpSession**：通过session对象得到session中存放的对象；
+ **Model/ModelMap**：Model是一个接口，ModelMap是一个接口实现，作用是将model数据填充到request域。

使用以下代码获取前端的数据

```java
	@RequestMapping(value = "/firstController1")
	public String SelectUser(HttpServletRequest request) {
		String id = request.getParameter("id");
		if (id!=null) {
			System.out.println(id);
		}
		return "first";
	}
12345678
```

运行项目，打开对应的网址，在后面使用问号传递参数 id=78 `http://localhost:8080/DataBanding/hello/firstController1?id=78`，随后在控制台当中查看。可以看到一条输出语句，打印了id的值。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513092228679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTczMTU5,size_16,color_FFFFFF,t_70)
在这里使用的是HttpServletRequest，当然还可以使用其他的对象：HttpServletResponse、HttpSession、Model/ModelMap不一 一列举。

当我们的命名比较同意的话就可以不使用到对象进行获取值，直接获取Integer对象的值：

```java
	@RequestMapping(value = "/firstController2")
	public String SelectUser1(Integer id) {
		if (id!=null) {
			System.out.println("firstController2="+id);
		}
		return "first";
	}
1234567
```

相同的。我们重新传递这个值：`http://localhost:8080/DataBanding/hello/firstController2?id=78`，很显然这样也是可以的，而且相比之下较为简便。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513093009754.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTczMTU5,size_16,color_FFFFFF,t_70)
当使用到的变量名不是特别一致的时候，或者前端请求中参数名和后台控制器类方法中的形参名不一样，那么这样就会导致后台无法正确绑定并接收到前端请求的参数。这要如何实现呢？可以考虑使用Spring MVC提供的@RequestParam注解类型来进行间接数据绑定。@RequestParam注解的属性声明如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513095012688.png)
定义方法，添加@RequestParam即可，如下是给id添加一个别名，user_id

```java
	@RequestMapping(value = "/firstController3")
	public String SelectUser3( @RequestParam(value="user_id")Integer id) {
		if (id!=null) {
			System.out.println("firstController3/user_id="+id);
		}
		return "first";
	}
1234567
```

调用这个方法传递参数进行测试：`http://localhost:8080/DataBanding/hello/firstController3?user_id=78`传递的参数是user_id=78，随后在控制台查看输出的id：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513095130597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTczMTU5,size_16,color_FFFFFF,t_70)

## POJO类型

在使用简单数据类型绑定时，可以很容易的根据具体需求来定义方法中的形参类型和个数，然而在实际应用中，客户端请求可能会传递多个不同类型的参数数据，如果还使用简单数据类型进行绑定，那么就需要手动编写多个不同类型的参数，这种操作显然比较繁琐。 针对多类型、多参数的请求，可以使用[POJO](https://so.csdn.net/so/search?q=POJO&spm=1001.2101.3001.7020)类型进行数据绑定。POJO类型的数据绑定就是将所有关联的请求参数封装在一个POJO中，然后在方法中直接使用该POJO作为形参来完成数据绑定。其中 POJO（Plain Ordinary Java Object）是指简单的Java对象，实际就是普通JavaBeans。

.比如：定义一个表单提交，需要提交用户名和密码。register.jsp文件。

```java
	<form action="${pageContext.request.contextPath }/hello/registerUser"
		method="post">
		用户名：<input type="text" name="username" /><br />
		密&nbsp;&nbsp;&nbsp;码：<input type="password" name="password" /><br />
		<input type="submit" value="注册" />
	</form>
123456
```

新定义一个com.lzq.po 的包，在下面新建一个User类，把需要传递的数据进行定义，并且getter/setter封装。在使用POJO类型数据绑定时，前端请求的参数名(指如上form表单内各元素的name属性值)必须与要绑定的POJO类中的属性一样，这样才会自动将请求数据绑定到POJO对象中，否则后台接收的参数值为null。

```java
	 String username;
	 Integer password;
12
```

随后在前文相同的地方定义路由：

```java
	@RequestMapping(value = "/toRegister")
	public String toRegister() {
		return "register";
	}

	@RequestMapping(value = "/registerUser")
	public String toRegisterUser(User user) {
		String username = user.getUsername();
		Integer password = user.getPassword();
		System.out.println("username = " + username + ", password = " + password);
		return "register";
	}
123456789101112
```

由toRegister路由进入，先跳到jsp页面进行输入数据，在jsp当中的action这个属性进行再次跳转，到/registerUser路由，进行接收。并且将数据进行打印输出，`http://localhost:8080/DataBanding/hello/registerUser`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513102346526.png)
在控制台当中进行查看：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513102455160.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTczMTU5,size_16,color_FFFFFF,t_70)
但是在这里，会发现当用户名输入中文的时候会导致乱码。如何修改呢？我们只需要将编码都设置为utf-8即可，在web.xml文件当中添加代码即可。

```java
<filter>
        <filter-name>CharacterEncodingFilter</filter-name>		
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
                   <param-name>encoding</param-name>
                   <param-value>UTF-8</param-value>
        </init-param>
</filter>
<filter-mapping>
       <filter-name>CharacterEncodingFilter</filter-name>
       <url-pattern>/*</url-pattern>
</filter-mapping>
123456789101112
```

上述代码中，通过`<filter-mapping>`元素的配置会拦截前端页面中的所有请求，并交由名称CharacterEncodingFilter 的编码过滤器类进行处理 filter 元素中，首先配置了编码过滤器类`org.spring.framework.web.filter.CharacterEncodingFilter`，然后通过初始化参数设置统一的编码为 UTF-8 这样所有的请求信息内容都会以 UTF-8 的编码格式进行解析。再进行测试，输入中文，在控制台进行查看：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513104314180.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0OTczMTU5,size_16,color_FFFFFF,t_70)

## 封装POJO

所谓的包装POJO，就是在一个POJO中包含另一个简单POJO。例如，在订单对象中包含用户对象。这样在使用时，就可以通过订单查询到用户信息。 使用包装POJO类型数据绑定时，如果前端条件参数是包装类中POJO的子属性，则参数名必须为`对象.属性`；如果查询条件参数是包装类的直接基本属性，则参数名直接用对应的`属性名`

## 复杂类型

复杂类型包括数组和集合类型，像 List、Set、Map。

数组类型用于传入多个参数名称相同的值，如接收页面上的复选框参数时。

SpringMVC 对于复杂类型的支持并不是很好，因为对于复杂类型，我们更多都是使用 JSON、XML等数据格式来传参。对于 List、Set、Map 这些类型，还需要单独设置一个包装类，属性设置为对应的集合类型，方法的参数为包装类型，比较繁琐。SpringMVC 对复杂类型的数据绑定的功能，基本上就是鸡肋。

类说明：

+ UserList 为 User 对应的 List 集合包装类，只有一个属性，`private List<User> users;`。
+ UserList 为 User 对应的 Set 集合包装类，只有一个属性，`private Set<User> users = new HashSet<>();`，并且需要在该类的构造函数中初始化 Set 集合的大小，不能动态改变 Set 集合大小，在传值时，对象的个数不能超过这个大小。
+ UserList 为 User 对应的 Map 集合包装类，只有一个属性，`private Map<String, User> users;`。



```java
// http://localhost:8080/springmvc/complextype1.do?ids=1&ids=2
@RequestMapping(value = "complextype1.do")
@ResponseBody
public String objecttype1(String[] ids) {
    System.out.println(ids.length);
    StringBuilder stringBuilder = new StringBuilder();
    for(String id : ids) {
        stringBuilder.append(id + " ");
    }
    return stringBuilder.toString();
}

// http://localhost:8080/springmvc/complextype2.do?users%5B0%5D.name=Tom&users%5B1%5D.name=Lucy 注意特殊字符[]的转义，不然会报错
// http://localhost:8080/springmvc/complextype2.do?users%5B0%5D.name=Tom&users%5B1%5D.name=Lucy&users%5B6%5D.name=Mary 注意特殊字符[]的转义，不然会报错
@RequestMapping(value = "complextype2.do")
@ResponseBody
public String objecttype2(UserList userList) {
    return userList.toString();
}

// http://localhost:8080/springmvc/complextype2.do?users%5B0%5D.name=Tom&users%5B1%5D.name=Lucy&users%5B2%5D.name=Mary 注意特殊字符[]的转义，不然会报错
@RequestMapping(value = "complextype3.do")
@ResponseBody
public String objecttype3(UserSet userSet) {
    System.out.println(userSet.getUsers().size());
    return userSet.toString();
}

// http://localhost:8080/springmvc/complextype4.do?users%5B%270%27%5D.name=Tom&users%5B%271%27%5D.name=Lucy&users%5B%272%27%5D.name=Mary
@RequestMapping(value = "complextype4.do")
@ResponseBody
public String objecttype4(UserMap userMap) {
    System.out.println(userMap.getUsers().size());
    return userMap.toString();
}
```





找到或者创建属性实例之后，会进行数据绑定，依次从URI路径变量、Form表单、URL参数中查找数据，如果找到同名变量，那么将赋值为找到的值，否则，将使用空属性。在有必要的情况下，同样对属性应用Converter转换。

URI路径变量、Form表单、URL参数中如果存在同名参数，则后面的覆盖前面的！

可以通过将@ModelAttribute的binding属性设置为false来禁止数据绑定!

一个简单的案例如下，一个MyModel类：

```java
/**
 * @author lx
 */
public class MyModel {
    private Long modelId;
    private Integer modelType;

    private String modelName;

    public void setModelId(Long modelId) {
        this.modelId = modelId;
    }

    public void setModelType(Integer modelType) {
        this.modelType = modelType;
    }

    public void setModelName(String modelName) {
        this.modelName = modelName;
    }


    @Override
    public String toString() {
        return "MyModel{" +
                "modelId=" + modelId +
                ", modelType=" + modelType +
                ", modelName='" + modelName + '\'' +
                '}';
    }
}
```

一个Controller：

```java
@RestController
public class ModelAttributeController {

    @PostMapping("/modelAttribute/{modelId}/{modelType}")
    public void handle(@ModelAttribute MyModel myModel) {
        System.out.println(myModel);
    }
}
```

发送一个post请求，访问/modelAttribute/123/321，表单参数为“modelName:myModel”，结果如下：

```java
MyModel{modelId=123, modelType=321, modelName='myModel'}
```

数据绑定可能会导致错误。默认情况下将抛出BindException给客户端。若要在控制器方法中检查此类错误并做出处理，可以在@ModelAttribute参数旁边的后一个参数添加BindingResult参数，该参数可以校验绑定是否成功！

在数据绑定之后，还可以通过添加@javax.validation.valid注解或者Spring的@Validated注解以及各种校验注解对数据进行非常方便的校验，这部分后面单独讲解！



对于你提到的这段代码：

```java
@GetMapping("/len")
ApiResponse<Test> getTime(Test name) {
    return ApiResponse.success(name);
}
```

在 Spring MVC 中，`@GetMapping` 注解对应着 HTTP 的 GET 请求，而方法中的 `Test name` 参数表示将请求路径中的参数自动绑定到 `Test` 类型的对象 `name` 上。

Spring MVC 会尝试根据请求路径中的参数名和 `Test` 类型的字段名进行自动匹配，并将请求参数映射到 `Test` 类型的对象上。这种自动的请求参数映射到对象的过程是 Spring MVC 的一个特性，它利用了 JavaBean 的规范，通过对象的 setter 方法或者字段来自动匹配请求参数。

假设请求路径为 `/len?param1=value1&param2=value2`，如果 `Test` 类型中包含了名为 `param1` 和 `param2` 的字段，并提供了相应的 setter 方法或公共字段，Spring MVC 就会尝试将请求参数 `param1` 和 `param2` 自动映射到 `Test` 类型的对象上。

这种行为是 Spring MVC 默认的参数绑定方式，如果请求路径中的参数名与对象的字段名一致，Spring MVC 就会尝试自动绑定。因此，在你的代码中，GET 请求路径中的参数能够被正确地映射到 `Test` 对象上。

这个知识点涉及到 Spring MVC 中的参数绑定和请求处理的部分。具体来说，这属于 Spring MVC 的请求参数绑定功能。

Spring MVC 的请求参数绑定是框架的一个特性，它可以将请求中的参数自动映射到控制器方法的参数上，甚至是映射到方法中的自定义对象上。这样的功能使得开发者可以在控制器方法中直接使用对象来处理请求参数，减少了手动处理请求参数的繁琐工作。

在你的示例中，通过在控制器方法的参数中直接使用 `Test name` 这样的对象，Spring MVC 会尝试根据请求路径中的参数名来自动映射参数到 `Test` 对象的属性上，从而简化了获取请求参数的过程。

这种请求参数自动绑定到方法参数或对象的功能属于 Spring MVC 的请求处理和参数解析的一部分。

当客户端发起一个 HTTP 请求时，其中的参数通常包含在 URL 中（对于 GET 请求）或者请求体中（对于 POST、PUT 等请求）。在 Spring MVC 中，有一个参数解析器（Argument Resolver）的机制，能够将这些请求参数映射到控制器方法的参数上，这就是请求参数绑定。

## 参数绑定过程

1. **控制器方法定义：** 在控制器类的方法中，你可以直接声明方法参数来接收请求中的参数。比如：

   ```
   javaCopy code@GetMapping("/example")
   public String handleRequest(@RequestParam("param1") String param1, @RequestParam("param2") int param2) {
       // 使用 param1 和 param2 处理请求
   }
   ```

   这种方式下，`@RequestParam` 注解将会尝试根据参数名（比如 "param1"、"param2"）将请求参数映射到方法参数上。

2. **对象参数接收：** 除了直接接收单个参数外，你也可以使用对象来接收多个相关参数。比如：

   ```
   javaCopy codepublic class MyRequestObject {
       private String param1;
       private int param2;
       // Getters and setters
   }
   
   @GetMapping("/example")
   public String handleRequest(MyRequestObject requestObject) {
       // 使用 requestObject 中的参数
   }
   ```

   在这个例子中，Spring MVC 会尝试将请求参数自动映射到 `MyRequestObject` 类的属性中，从而实现请求参数到对象的自动转换。

3. **自动映射原理：** Spring MVC 使用了 JavaBean 的规范来进行参数绑定。对于方法参数中的对象，它会寻找对象中的字段或者对应的 setter 方法，并尝试将请求参数根据名称自动映射到这些字段或者方法上。

4. **绑定方式：** 请求参数绑定可以通过注解（比如 `@RequestParam`、`@PathVariable`、`@RequestBody`）或者直接声明方法参数为对象的方式来实现。Spring MVC 在后台根据请求参数名和方法参数或对象的字段名进行匹配，自动进行参数的绑定。

在 Spring MVC 中，请求参数绑定的底层原理涉及到处理器适配器（HandlerAdapter）和处理器方法参数解析器（HandlerMethodArgumentResolver）。

### 底层原理：

1. **处理器适配器：** 当一个请求到达时，处理器适配器负责将请求映射到对应的控制器方法上。处理器适配器选择合适的处理器（即控制器方法），并调用处理器来处理请求。
2. **处理器方法参数解析器：** 处理器方法参数解析器负责将请求中的参数解析并映射到控制器方法的参数上。Spring MVC 中有多种内置的参数解析器，包括用于处理请求参数的 `RequestParamMethodArgumentResolver`、用于处理路径变量的 `PathVariableMethodArgumentResolver` 等。
3. **参数解析流程：** 当请求到达时，处理器适配器选择相应的处理器方法，并调用参数解析器来解析方法中的参数。参数解析器会根据方法参数的类型和注解等信息，尝试从请求中获取对应的参数值，并将其映射到方法参数中。比如，`@RequestParam` 注解告诉 Spring MVC 使用请求参数来填充方法参数，`@PathVariable` 则用于路径变量的映射，而对象参数则会自动尝试将请求参数映射到对象的属性中。
4. **参数绑定策略：** Spring MVC 的参数绑定策略基于 JavaBean 规范。当方法参数为对象时，Spring MVC 会查找对象中的属性或对应的 setter 方法，并尝试将请求参数与属性名进行匹配，从而实现参数的自动绑定。
5. **自定义参数解析器：** 你也可以实现自己的参数解析器来处理特定的参数映射逻辑。通过实现 `HandlerMethodArgumentResolver` 接口，你可以定义自定义的参数解析器，并注册到 Spring MVC 中，使得 Spring MVC 能够根据你的需求来解析特定类型的参数。

请求参数绑定和 Jackson 的消息转换属于 Spring MVC 的不同部分，但它们之间有一定的联系。

+ **请求参数绑定：** 是 Spring MVC 中负责将请求中的参数映射到控制器方法参数上的机制。它根据方法参数的类型、注解以及请求中的参数名进行匹配，并尝试将参数值映射到方法参数或对象的属性上。
+ **Jackson 的消息转换：** 则是指 Spring MVC 中利用 Jackson 库来实现请求和响应中 JSON 格式数据与 Java 对象之间的转换。它负责将请求体中的 JSON 数据转换为 Java 对象，或者将 Java 对象转换为 JSON 数据返回给客户端。

这两个功能是相对独立的，但在某些场景下可能会有联系。比如，当你使用对象来接收请求参数时，请求参数绑定会尝试将请求参数的字符串值转换为对象属性对应的类型。在这个过程中，Spring MVC 可能会使用内置的类型转换器来将参数字符串转换为对应的类型。如果遇到无法自动转换的情况，可以自定义类型转换器来处理。

所以，请求参数绑定并不直接依赖于 Jackson 的消息转换机制，但在使用对象接收请求参数时，可能会涉及到类型转换的问题，此时可以考虑使用自定义类型转换器来解决。同时，Jackson 的消息转换主要负责 JSON 数据和 Java 对象之间的转换，不直接参与请求参数的绑定。

## DataBinder

**和此前介绍的`@ModelAttribute`方法类似，Spring MVC的`@InitBinder`方法同样在控制器方法之前执行，并且同样支持与@RequestMapping控制器方法相同的许多参数。但是，它们通常使用`WebDataBinder`参数（用于注册类型转换器）和并且返回值为void。**

**和此前介绍的@ModelAttribute方法类似，@Controller类和@ControllerAdvice类中都可以定义@InitBinder方法，@Controller类中的@InitBinder方法默认支持范围是当前类的请求，@ControllerAdvice类中方法默认支持范围是所有的请求！**

**@InitBinder方法有如下作用：**

1. 将请求参数（即表单或查询数据）绑定到model对象，这和@ModelAttribute差不多，但不是它的主要功能。
2. 通过注册类型转换器，用于将基于字符串的请求值（例如请求参数，路径变量，请求头，Cookie等）转换为控制器方法参数的目标类型。

每次的请求，在HandlerAdapter的handler方法内都会创建一个WebDataBinder对象：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c1f418c360c745db98d3ba6519f78f16~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**创建WebDataBinder之后，会尝试添加预配置的`conversionService`，如果`开启了Spring mvc的配置（通过@EnableWebMvc注解或者<mvc:annotation-driven/>标签`，这个conversionService的配置我们在上面就讲过了）并且没有更改，那么默认就是一个D`efaultFormattingConversionService`，其内部封装了常见的全局可用的Converter和Formarter（Formatter会被转换为Converter），这个conversionService中的转换器是`全局可用的`！**

随后回调所有符合规则的@InitBinder方法，然后我们可以在该方法中对WebDataBinder对象中注册适用于当前请求的自定义的PropertyEditor和Formatter转换器（Formatter会被转换为PropertyEditor)，但是这里注册的转换器将值绑定到该Binder对象本身，也就是说每一次请求中通过WebDataBinder的@InitBinder方法注册的转换器是不可复用的。

**在解析时，首先使用注册的局部PropertyEditor来解析，然后在使用全局的conversionService来解析！**

如下案例，有一个Controller：

```java
java复制代码@RestController
public class InitBinderController {

    @GetMapping("/initBinder")
    public void handle(Date date) {
        System.out.println(date);
    }
}
```

如果我们访问/initBinder?date=2012/12/12，结果如下：

```java
java
复制代码Wed Dec 12 00:00:00 CST 2012
```

**可以发现，可以实现字符串到时间的转换，这是因为默认的conversionService提供了这种格式的字符串到Date类型的Converter，如果我们尝试换一种格式呢？**

如果我们访问/initBinder?date=2012-12-12，结果如下：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1c9fd941f0024ba1b8bcba644db48283~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**此时直接抛出异常了！因为字符串格式不匹配，此时我们可以自定义类型转换器，如上面所讲，我们可以定义三种转换器，如果在@InitBinder方法中注册，那么我们可以注册PropertyEditor和Formatter这两种类型的转换器！**

**对于这种时间字符串转换为Date的转换器，PropertyEditor和Formatter都已经提供了各自的实现，我们只需要传入给定的字符串模式即可，当然也可以自己实现！**

如果我们注册PropertyEditor，那么如下声明：

```java
java复制代码@RestController
public class InitBinderController {

    @InitBinder
    public void initBinder(WebDataBinder binder) {
        System.out.println("----initBinder------");
        //注册一个自定义的PropertyEditor
        //第一个参数表示转换后属性的类型，第二个参数是自定义的PropertyEditor的实例
        //这个CustomDateEditor是Spring内置的专门用于格式化时间的PropertyEditor，我们只需要设置时间字符串的模式即可
        binder.registerCustomEditor(Date.class, new CustomDateEditor(new SimpleDateFormat("yyyy-MM-dd"), false));
    }

    @GetMapping("/initBinder")
    public void handle(Date date) {
        System.out.println(date);
    }
}
```

再次访问/initBinder?date=2012-12-12，结果如下：

```java
java复制代码----initBinder------
Wed Dec 12 00:00:00 CST 2012
```

成功的进行了转换！

如果我们注册Formatter，那么如下声明：

```java
java复制代码@InitBinder
public void initBinder(WebDataBinder binder) {
    System.out.println("----initBinder------");
    //注册一个自定义的Formatter
    //这个DateFormatter是Spring内置的专门用于格式化时间的Formatter
    //只需要在构造器参数中设置时间字符串的模式即可，这里并没有设置，因为DateFormatter内置了本地模式支持解析yyyy-MM-dd
    binder.addCustomFormatter(new DateFormatter());
}
```

再次访问/initBinder?date=2012-12-12，结果如下：

```java
java复制代码----initBinder------
Wed Dec 12 00:00:00 CST 2012
```

同样成功的进行了转换！

# 类型转换

`BeanWrapper`是一个Spring的内部体系，主要用于在创建bean实例之后填充bean的依赖，我们在此前`Spring IOC的源码`的部分已经讲过了，`BeanWrapper`对于大部分开者这来说都是无感知的，被Spring内部使用，属于一个底层的对象。

**Spring中的类型转换主要发生在两个地方：**

1. **`Spring创建bean实例时`，在底层的BeanWrapper中注入Bean的属性依赖的时候，如果对于找到的依赖类型（给定的常量值或者找到依赖对象）如果不符合属性的具体类型，那么需要转换为对应的属性类型；**
2. **Spring MVC中，在执行处理器方法之前，可能需要把HTTP请求的数据通过DataBinder绑定到控制器`方法的给定参数`上，然而HTTP参数到后端时都是String类型，而方法参数可能是各种类型，这就可能涉及到从String到给定类型的转换。**

**Spring提供了三种最常见的数据类型转换器PropertyEditor、Formatter、Converter，无论是Spring MVC的 DataBinder和底层的BeanWrapper都支持使用这些转换器进行数据转换:**

1. **`PropertyEditor`是JDK自带的类型转换接口。主要用于实现String到其他类型的转换。Spring已经提供了用于常见类型转换的PropertyEditor实现。**
2. **`Formatter`是Spring 3.0时提供的接口，只能转换String到其他类型，支持SPI机制。通常对于Spring MVC的参数绑定时的类型转换使用Formatter就可以了。Spring已经提供了用于常见类型转换的Formatter实现。**
3. **`Converter`是Spring 3.0时提供的接口，可以提供从一个对象类型到另一个对象类型的转换，支持SPI机制，当需要实现通用类型转换逻辑时应该使用Converter。Spring已经提供了用于常见类型转换的Converter实现。**

## PropertyEditor

**在最开始，Spring 使用PropertyEditor（属性编辑器）的概念来实现对象和字符串之间的转换，PropertyEditor接口并非来自于Spring，而是来自Java的rt.jar核心依赖包，是JDK自带的。属性编辑器的作用我们在此前就说过了：**

1. 在Spring创建bean时将数据转换为bean的属性对应的类型。
2. 在 Spring MVC 框架中分析、转换HTTP 请求参数。

PropertyEditor接口的常见方法如下：

```java
java复制代码public interface PropertyEditor {

    /**
     * 设置（或更改）要编辑的对象。原始类型（如"int"）必须包装为相应的对象类型，如"java.lang.integer"
     *
     * @param value 要编辑的新目标对象。属性编辑器不应修改此对象，而属性编辑器应创建一个新对象来保存任何修改的值
     */
    void setValue(Object value);

    /**
     * 获取属性值。
     *
     * @return 属性的值。原始类型（如"int"）将包装为相应的对象类型，如"java.lang.integer"
     */
    Object getValue();

    /**
     * 为属性提供一个表示初始值的字符串，属性编辑器以此值作为属性的默认值
     */
    String getJavaInitializationString();

    /**
     * 获取属性值的可编辑的字符串表示形式
     *
     * @return 如果值不能表示为可编辑字符串，则返回 null。如果返回非 null 值，则属性编辑器应准备好在 setAsText()中分析该字符串
     */
    String getAsText();

    /**
     * 通过分析给定字符串来设置属性值。如果字符串格式错误或此类属性不能以文本形式表示，可能会引发 java.lang.IllegalArgumentException
     *
     * @param text 要解析的字符串。
     */
    void setAsText(String text) throws java.lang.IllegalArgumentException;
}
```

**虽然经常看见有文章说PropertyEditor仅用于支持从String到对象的转换。但是实际上在目前的Spring版本中，早已支持通过PropertyEditor实现从对象到对象的转换，典型的实现就是来自于spring-data-redis的各种PropertyEditor实现，比如ValueOperationsEditor，我们可以直接依赖ValueOperations，并且对其注入redisTemplate，Spring 在检查到类型不一致时，最终会在ValueOperationsEditor中通过注入的redisTemplate获取ValueOperations并返回。**

**`支持从对象转换为对象的核心方法就是PropertyEditor#setValue方法。`** ![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/19d0ffb9c77b46bca668d4042996ceb3~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

### 内置

**尽管现在如果在需要自定义转换器时，PropertyEditor被推荐使用Converter替代，但是我们仍然能够配置并且正常使用自定义的PropertyEditor，并且Spring内部就是用了很多默认PropertyEditor。**

**Spring 拥有许多内置的`PropertyEditor`实现。它们都位于 `org.springframework.beans.propertyeditors包中`。默认情况下，大多数（但不包括全部）由 `BeanWrapperImpl`来注册（位于`AbstractBeanFactory#initBeanWrapper`方法中，注册之后被BeanWrapper用于创建和填充Bean实例的类型转换）。很多默认属性编辑器实现也都是可配置的，比如`CustomDateEditor，就可以指定日期模式`。**

**下表列出了Spring提供的常见PropertyEditor：**

| 类型                    | 描述                                                         |
| ----------------------- | ------------------------------------------------------------ |
| ByteArrayPropertyEditor | 字节数组的编辑器。将String转换为相应的byte[]表示形式。默认情况下由 BeanWrapperImpl 注册。 |
| ClassEditor             | 支持表示类的字符串String解析与实际Class的相互转换。当找不到类时，将抛出IllegalArgumentException。默认情况下，由BeanWrapperImpl注册。 |
| CustomBooleanEditor     | boolean的可自定义属性编辑器，将指定的String解析为boolean值。默认情况下，由 BeanWrapperImpl 注册，但可以通过将其自定义实例注册为自定义编辑器来覆盖。 |
| CustomCollectionEditor  | 集合的可自定义属性编辑器，将任何源字符串或者集合转换为给定的目标集合类型。默认情况下，由 BeanWrapperImpl 注册，但可以通过将其自定义实例注册为自定义编辑器来覆盖。 |
| CustomDateEditor        | java.util.Date 的可自定义属性编辑器，支持自定义 DateFormat。默认情况下未注册。必须由用户根据需要使用适当的格式进行手动注册。 |
| CustomNumberEditor      | 任何Number的子类（如Integer、  Long、 Float或 Double）的可自定义属性编辑器。默认情况下，由BeanWrapperImpl注册，但可以通过将其自定义实例注册为自定义编辑器来覆盖。 |
| FileEditor              | 将字符串解析为 java.io.File 对象。默认情况下，由 BeanWrapperImpl 注册。 |
| InputStreamEditor       | 将一个String通过中间的ResourceEditor和Resource生成一个InputStream。默认用法不会关闭inputstream。默认情况下，由BeanWrapperImpl注册。 |
| LocaleEditor            | 可以实现String和Locale对象的相互转换，字符串格式为[国家/地区]，与Locale的 toString()方法相同）。默认情况下，由 BeanWrapperImpl 注册。 |
| PatternEditor           | 可以实现String和java.util.regex.Pattern对象的相互转换        |
| PropertiesEditor        | 可以将字符串转换为Properties对象（使用java.util.Properties类的javadoc中定义的格式格式化）。默认情况下，由BeanWrapperImpl注册。 |
| StringTrimmerEditor     | 修剪字符串的属性编辑器，还可以（可选）将空字符串转换为null值。默认情况下未注册，必须是用户手动注册的。 |
| URLEditor               | 可以将URL字符串解析为实际 URL 对象。默认情况下，由 BeanWrapperImpl 注册。 |

**`BeanWrapperImpl自动注册的PropertyEditor位于PropertyEditorRegistrySupport#createDefaultEditors方法中，并且是在需要转类型但是其他自定义转换器中无法找到合适的转换器的时候才会注册的（convertIfNecessary方法内的findDefaultEditor方法），属于延迟初始化！`**

### PropertyEditorManager

**Spring使用`java.beans.PropertyEditorManager`来注册、搜索可能的需要的PropertyEditor。搜索路径还包括rt.jar包中的sun.bean.editors，其中包括用于Font、Color和大多数基本类型的PropertyEditor实现。**

**`另外，如果某个类型的类型转换器与该类型的Class位于同一个包路径，并且命名为ClassName+Editor，那么当需要转换为该类型时，将会自动发现该转换器，而无需手动注册，如下实例：`**

```java
java复制代码com
  chank
    pop
      Something         // Something类
      SomethingEditor // 用于Something类的类型转换，将会自动发现
```

**一个管理默认属性编辑器的管理器：PropertyEditorManager，该管理器内保存着一些常见类型的属性编辑器， 如果某个JavaBean的常见类型属性没有通过BeanInfo显式指定属性编辑器，IDE将自动使用PropertyEditorManager中注册的对应默认属性编辑器。**

**`实际上我们前面说的spring-data-redis的各种PropertyEditor实现就是采用的这个机制取发现的，它们并没有手动注册：`**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/de22ef16f20f4ca687e48e1ed98a6d03~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**当然，我们也可以使用标准的BeanInfo  JavaBeans机制显式的指定某个类与某个属性编辑器的关系，如下实例：**

```java
com
  chank
    pop
      Something
      SomethingBeanInfo
```

### 自定义

**Spring 预注册了许多自定义属性编辑器实现（例如，将表示为字符串的类名转换为Class对象）。此外，Java 的标准 JavaBeans PropertyEditor查找机制允许对类的PropertyEditor进行适当命名，并放置在与它所支持的类相同的包中，以便可以自动找到它（上面讲过了）。**

**Spring提供了PropertyEditor的一个核心实现类`PropertyEditorSupport`，如果我们要编写自定义的属性编辑器，只需要继承这个类即可，PropertyEditorSupport实现了`PropertyEditor`接口的所有方法，我们继承PropertyEditorSupport之后只需要重写自己需要的方法即可，更加方便！**

**如果需要注册其他自定义PropertyEditors，可以使用几种机制：**

1. **最不建议、不方便是使用`ConfigurableBeanFactory接口的registerCustomEditor()方法`，因为这需要我们获取BeanFactory的引用。该方法将自定义的PropertyEditor直接注册到`AbstractBeanFactory的customEditors`缓存中，等待后续BeanWrapper的获取。**
2. **另一种（稍微方便一点）的机制是使用`CustomEditorConfigurer`，它是一个特殊的`BeanFactoryPostProcessor`，可以将自定义的PropertyEditor或者PropertyEditorRegistrar实现存入其内部的`customEditors和propertyEditorRegistrars`属性中，启动项目之后，它的`postProcessBeanFactory`方法会在所有普通bean实例化和初始化之前（创建BeanWrapper之前）调用beanFactory来将这些`PropertyEditor和propertyEditorRegistrars`注册到`AbstractBeanFactory的customEditors和propertyEditorRegistrars`缓存。**

**基于以上的配置，在Spring bean对应的BeanWrapper初始化时，会自动从`AbstractBeanFactory的customEditors和propertyEditorRegistrars`缓存中将自定义的PropertyEditor注册到自己内部（位于`AbstractBeanFactory#initBeanWrapper`方法中），之后被`BeanWrapper用于创建和填充 Bean 实例的类型转换。`**

**`但是请注意，这种配置不适用于Spring MVC的数据绑定，因为DataBinder默认不会查找这里注册到AbstractBeanFactory中的customEditors和propertyEditorRegistrars缓存，数据绑定时需要的自定义Editor必须在org.springframework.validation.DataBinder中手动注册（通过Spring MVC的@InitBinder方法）。`**

如下案例，自定义了一个PropertyEditor，日期格式为“yyyy-MM-dd”：

```java
/**
 * @author lx
 */
public class DateEditor extends PropertyEditorSupport {
    private String formatter = "yyyy-MM-dd";

    @Override
    public void setAsText(String text) throws IllegalArgumentException {
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat(formatter);
        try {
            Date date = simpleDateFormat.parse(text);
            System.out.println("-----DateEditor-----");
            //转换后的值设置给PropertyEditorSupport内部的value属性
            setValue(date);
        } catch (ParseException e) {
            throw new IllegalArgumentException(e);
        }
    }

    public DateEditor() {
    }

    public DateEditor(String formatter) {
        this.formatter = formatter;
    }
}
```

一个实体，需要将“2020-12-12”的字符串转换为Date类型的属性：

```java
@Component
public class TestDate {
    @Value("2020-12-12")
    private Date date;

    @PostConstruct
    public void test() {
        System.out.println(date);
    }
}
```

**将自定义的PropertyEditor注册到CustomEditorConfigurer的customEditors属性中，该属性是Map<Class<?>, Class<? extends PropertyEditor>类型，即都是Class类型：**

```xml
<bean 
 class="org.springframework.beans.factory.config.CustomEditorConfigurer">
    <property name="customEditors">
        <map>
            <entry key="java.util.Date" value="com.spring.mvc.config.DateEditor"/>
        </map>
    </property>
</bean>
```

**通过在`CustomEditorConfigurer的customEditors属性`中直接添加自定义编辑器的类型确实可以注册，但是这种方法的缺点是无法为`自定义的PropertyEditor`指定初始化参数。**

**实际上在早期的Spring版本中，Map中的value类型是一个实例，因此它是支持自定义初始化参数的，但是因为PropertyEditor是有状态的，如果多个BeanWrapper共用同一个PropertyEditor实例，那么可能造成难以察觉的问题。因此，在新版本中customEditors属性的Map的Value是Class类型，并且每个BeanWrapper在设置转换器是都会创建属于自己的PropertyEditor实例。如果想要需要控制PropertyEditor的实例化过程，比如设置初始化参数，那么我们需要使用PropertyEditorRegistrar去注册它们。**

**还有一个缺点是，`基于customEditors属性配置的PropertyEditor无法与Spring MVC的数据绑定共享同样的配置方式，即使它们都需要配置某个同样的PropertyEditor`。**

### 使用

**`PropertyEditorRegistrar`，顾名思义，它是一个PropertyEditor的“注册表”，Spring中还有很多Registrar结尾的类，这种类通用作用就是用于注册类名去除“Registrar”之后的数据，比如PropertyEditorRegistrar就是用于注册PropertyEditor，它还有一个可选的特性就是，可以在一次方法调用中注册多个实例并且更加灵活！**

**另外，PropertyEditorRegistrar实例与名为PropertyEditorRegistry的接口配合使用，而该接口又被Spring的BeanWrapper和 DataBinder都实现了，因此PropertyEditorRegistrar中的PropertyEditor配置很容易的被BeanWrapper和 DataBinder共享！**

**Spring为我们提供了一个PropertyEditorRegistrar的实现ResourceEditorRegistrar，如果我们要实现自己的PropertyEditorRegistrar，那么可以参数该类，特别是它的registerCustomEditors方法。实际上ResourceEditorRegistrar将会被Spring自动默认注册到容器中（位于prepareBeanFactory方法中），因此该类中的PropertyEditor通常会被所有的beanWarpper使用！**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c28436abde644741bf98e59332280d35~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

下面是一个自己的PropertyEditorRegistrar：

```java
java复制代码/**
 * @author lx
 */
public final class CustomPropertyEditorRegistrar implements PropertyEditorRegistrar {

    private String formatter;


    /**
     * 传递一个PropertyEditorRegistry的实现，使用给定的PropertyEditorRegistry注册自定义PropertyEditor
     * BeanWrapperImpl和DataBinder都实现了PropertyEditorRegistry接口，传递的通常是 BeanWrapper 或 DataBinder。
     * <p>
     * 该方法仅仅是定义了注册的流程，只有当某个BeanWrapper 或 DataBinder实际调用时才会真正的注册
     *
     * @param registry 将要注册自定义PropertyEditor的PropertyEditorRegistry
     */
    @Override
    public void registerCustomEditors(PropertyEditorRegistry registry) {

        // 预期将创建新的属性编辑器实例，可以自己控制创建流程
        registry.registerCustomEditor(Date.class, new DateEditor(formatter));

        // 可以在此处注册尽可能多的自定义属性编辑器...
    }


    public String getFormatter() {
        return formatter;
    }

    public void setFormatter(String formatter) {
        this.formatter = formatter;
    }
}
```

下面是如何配置CustomEditorConfigurer并注入CustomPropertyEditorRegistrar实例：

```xml
xml复制代码<bean 
class="org.springframework.beans.factory.config.CustomEditorConfigurer">
    <!--propertyEditorRegistrars是一个数组，可以传递多个自定义的PropertyEditorRegistrar-->
    <property name="propertyEditorRegistrars">
        <array>
            <ref bean="customPropertyEditorRegistrar"/>
        </array>
    </property>
</bean>

<!--自定义的CustomPropertyEditorRegistrar-->
<bean id="customPropertyEditorRegistrar" class="com.spring.mvc.config.CustomPropertyEditorRegistrar">
    <property name="formatter" value="yyyy-MM-dd"/>
</bean>
```

启动项目，同样成功转换：

```xml
xml复制代码-----DateEditor-----
Sat Dec 12 00:00:00 CST 2020
```

**注意，Spring的目的就是让每个BeanWarpper和DataBinder都初始化自己的PropertyEditor实例，这是为了防止多个实例共用一个有状态的PropertyEditor导致数据异常，如果你确定没问题的话，也可以在PropertyEditorRegistrar中配置同一个PropertyEditor实例。**

### 共享配置

**配置PropertyEditorRegistrar之后，想要将这些PropertyEditor配置应用在Spring MVC的DataBinder中非常简单，如下案例：**

```java
java复制代码@Controller
public class RegistrarController {
    @Resource
    private CustomPropertyEditorRegistrar customPropertyEditorRegistrar;

    @InitBinder
    public void init(WebDataBinder binder) {
        //调用registerCustomEditors方法向当前DateBinder注册PropertyEditor
        customPropertyEditorRegistrar.registerCustomEditors(binder);
    }

    //其他控制器方法
}
```

**只需要在控制器中引入`customPropertyEditorRegistrar`实例，然后在`@ initBinder方法`中调用`registerCustomEditors`方法并传入`DataBinder`，即可将内部配置的PropertyEditor注册到当前DataBinder中。**

**这种类型的PropertyEditor注册方式可以产生简洁的代码（注册多个PropertyEditor的实现只有一行代码），并允许将公共的PropertyEditor注册代码封装在一个类中，然后根据需要在多个Controllers之间共享。**

## Converter

**Spring 3.0引入了`core.convert`包，它提供了一般类型的转换系统，作为` JavaBeans PropertyEditors`属性编辑器的替代服务。**

依靠参数转换器Converter<String,T>可以从URI路径变量、URL参数、Form表单中获取同名参数并转换为参数对象，当然Converter还支持属性绑定时的类型转换！

查找顺序为URI路径变量、URL参数、Form表单参数，找到之后立即返回，不再向后查找！

如下案例，我们定义一个StringToMyModelConverter，它实现了org.springframework.core.convert.converter.Converter接口，并且实现了convert方法，该方法就是类型转换的方法，可以约定前端传递的数据的格式，然后后端按照格式解析！这里，我们约定传递的格式为JSON数据。

```java
/**
 * 把字符串转换MyModel
 */
@Component
public class StringToMyModelConverter implements Converter<String, MyModel> {

    /**
     * String source    传入进来字符串
     *
     * @param source 传入的要被转换的字符串
     * @return 转换后的格式类型
     */
    @Override
    public MyModel convert(String source) {
        ObjectMapper mapper = new ObjectMapper();
        MyModel myModel = null;
        try {
            myModel = mapper.readValue(source, MyModel.class);
            System.out.println("-----------------");
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("转换异常！");
        }
        return myModel;
    }
}
```

在Spring MVC配置文件中配置类型转换器，将自定义的转换器注册到类型转换服务中去：

```xml
<!--conversion-service指定在字段绑定期间用于类型转换的转换服务的 bean 名称。 -->
<!--如果不指定，则表示注册默认常见类型转换器DefaultFormattingConversionService-->
<mvc:annotation-driven enable-matrix-variables="true" conversion-service="conversionService"/>

<!--配置类型转换服务工厂，它除了可以注入自定义类型转换器之外，还会默认创建DefaultConversionService-->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <!--注入自定义转换器实例-->
            <ref bean="stringToMyModelConverter"/>
        </set>
    </property>
</bean>
```

控制器方法：

```java
@GetMapping("/modelAttribute/{myModel}")
public void handle2(@ModelAttribute MyModel myModel) {
    System.out.println(myModel);
}
```

访问/modelAttribute/{"modelId":"9999","modelType":"999","modelName":"666"}，结果如下：

```java
-----------------
MyModel{modelId=9999, modelType=999, modelName='666'}
```

### 接口

**相比于复杂的PropertyEditor接口，`org.springframework.core.convert.converter.Converter`是一个用于类型转换的非常简单且强大的SPI接口，翻译成中文就是“转换器”。`Converter`提供了核心的转换行为！**

```java
java复制代码@FunctionalInterface
public interface Converter<S, T> {

    /**
     * 将 S 类型的源对象转换为目标类型 T 对象
     *
     * @param source 要转换的源对象，它必须是 S 类型的实例（从不为null）
     * @return 转换后的对象，它必须是 T 类型的实例（可能为null）
     * @throws IllegalArgumentException 如果源对象无法转换为所需的目标类型
     */
    @Nullable
    T convert(S source);

}
```

**要想创建自己的`Converter`，只需要实现Converter接口，其中S表示要转换的类型，T表示要转换到的类型。**

**`convert(S)`方法的每次调用，都应该保证输入参数不为null。如果转换失败，转换器可能会引发任何非受检异常。在抛出异常时应该包裹在一个IllegalArgumentException中，同时我们必须保证Converter是`线程安全`的！**

**和PropertyEditor类似，为了方便起见，`Spring在core.convert.support `包中已经提供了非常多的`Converter转换器实现`，其中包括从字符串到数字的转换器和其它常见类型。**

**下面是一个典型的Converter转换器实现：**

```java
public final class StringToInteger implements Converter<String, Integer> {

    @Override
    public Integer convert(String source) {
        return Integer.valueOf(source);
    }
}
```

### 使用

#### ConverterFactory

**Converter的类型转换是明确的，倘若对有同一父类或接口的多个子类型需要进行类型转化，为每个类型都写一个Converter显然是十分不理智的，当需要集中管理整个类层次结构的转换逻辑时，可以实现`ConverterFactory`接口：**

```java
java复制代码/**
 * @param <S> 源类型
 * @param <R> 目标类型的超类型
 */
public interface ConverterFactory<S, R> {

    /**
     * 获取转换器从 S 转换为目标类型 T，其中 T 也是 R 的子类型。
     *
     * @param <T>        目标类型
     * @param targetType 要转换为的目标类型的Class
     * @return 从 S 到 T 的转换器
     */
    <T extends R> Converter<S, T> getConverter(Class<T> targetType);

}
```

**参数S是需要转换的类型，R是要转换到的类的基类。然后实现`getConverter(Class)`方法，其中T是R的子类型。`ConverterFactory`用于实现一种类型到N种类型的转换。**

**Spring已经提供了基本的`ConverterFactory的实现`：**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5c1be03655194602a5607ac969ca83de~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

#### GenericConverter

**当需要定义`复杂的Converter实现`时，可以使用`GenericConverter`接口，GenericConverter并不是Converter的子接口，而是一个`独立的顶级接口`，翻译成中文就是“通用转换器”！**

**与Converter相比，GenericConverter更灵活并且没有强类型的签名，它支持在多个源类型和目标类型之间进行转换，用于实现N种类型到N种类型的转换。此外，GenericConverter提供了可用的源和目标字段上下文（`TypeDescriptor`），你可以在实现转换逻辑时使用它们。这样的上下文允许类型转换由字段注解或字段签名上声明的泛型信息驱动类型转换。**

**下面是GenericConverter的接口定义：**

```java
java复制代码/**
 * 用于在两种或多种类型之间转换的通用转换器接口。
 * <p>
 * 这是最灵活的转换器SPI接口，也是最复杂的。它的灵活性在于GenericConverter可能支持在多个源/目标类型对之间转换
 * 此外，GenericConverter的实现在类型转换过程中可以访问源/目标字段上下文。
 * 这允许解析源和目标字段元数据，如注解和泛型信息，这些元数据可用于影响转换逻辑。
 */
public interface GenericConverter {

    /**
     * 返回所有此转换器可以转换的源类型和目标类型的ConvertiblePair
     * 每个ConvertiblePair都表示一组可转换的源类型以及目标类型。
     * <p>
     * 对于ConditionalConverter，此方法可能会返回 null 以指示应考虑所有ConvertiblePair
     */
    @Nullable
    Set<ConvertiblePair> getConvertibleTypes();

    /**
     * 将源对象转换为TypeDescriptor（类型描述符）描述的目标类型。
     *
     * @param source     要转换的源对象（可能是null）
     * @param sourceType 正在转换的字段的类型描述符
     * @param targetType 要转换为的字段的类型描述符
     * @return 转换的对象
     */
    @Nullable
    Object convert(@Nullable Object source, TypeDescriptor sourceType, TypeDescriptor targetType);


    /**
     * 源类型到目标类型对的持有者
     */
    final class ConvertiblePair {

        private final Class<?> sourceType;

        private final Class<?> targetType;

        /**
         * 创建一个新的ConvertiblePair
         *
         * @param sourceType 源类型
         * @param targetType 目标类型
         */
        public ConvertiblePair(Class<?> sourceType, Class<?> targetType) {
            Assert.notNull(sourceType, "Source type must not be null");
            Assert.notNull(targetType, "Target type must not be null");
            this.sourceType = sourceType;
            this.targetType = targetType;
        }

        public Class<?> getSourceType() {
            return this.sourceType;
        }

        public Class<?> getTargetType() {
            return this.targetType;
        }


        /*用于判断是否已存在某个源类型到目标类型的组*/

        @Override
        public boolean equals(@Nullable Object other) {
            if (this == other) {
                return true;
            }
            if (other == null || other.getClass() != ConvertiblePair.class) {
                return false;
            }
            ConvertiblePair otherPair = (ConvertiblePair) other;
            return (this.sourceType == otherPair.sourceType && this.targetType == otherPair.targetType);
        }

        @Override
        public int hashCode() {
            return (this.sourceType.hashCode() * 31 + this.targetType.hashCode());
        }

        @Override
        public String toString() {
            return (this.sourceType.getName() + " -> " + this.targetType.getName());
        }
    }

}
```

**`GenericConverter`中拥有一个内部类`ConvertiblePair`，这个内部类用于封装一种可转换的源类型与目标类型对，一个GenericConverter可以拥有多个ConvertiblePair。**

**要实现GenericConverter，需要重写`getConvertibleTypes()方法`以返回受转换支持的源类型到目标类型对，也就是ConvertiblePair。然后实现`convert(Object, TypeDescriptor, TypeDescriptor)方法`，该方法包含转换的逻辑。源 TypeDescriptor（字段描述符）提供对保存了要转换值的源字段的访问。目标 TypeDescriptor提供对要设置转换值的目标字段的访问。**

**`TypeDescriptor`作为类型描述符，保存了对应参数、字段的元数据，可以从其中获取对应参数、字段的名字、类型、注解、泛型的信息！**

**Spring已经提供了基本的GenericConverter的实现，一个很好的例子就是在Java数组和集合之间转换的转换器，比如`ArrayToCollectionConverter`，首先线它会创建对应的集合类型，然后在将数组元素存入集合中时，如有必要，会尝试将数组元素类型转换为集合元素的泛型类型！**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/96efda1a46be479c9814116ad97d7b06~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

##### ConditionalGenericConverter

**如果觉得只通过源类型和目标类型是否匹配来判断能够支持转换的方式太过简单了，还需要在特定的条件成立时才支持转换，比如可能希望在目标字段上存在指定的注解时才表示可以转换，或者可能希望仅在目标字段的类型上定义了特定方法（如静态的valueOf方法）时才表示可以转换，此时我们可以使用`ConditionalGenericConverter`接口。**

**`ConditionalGenericConverter是GenericConverter 和ConditionalConverter的结合`，允许自定义匹配条件来确定是否能执行转换！**

```java
java复制代码/**
 * 条件转换器，允许有条件的执行转换
 * <p>
 * 通常用于根据字段或类级别的特征（如注解或方法）的存在有选择地匹配自定义转换逻辑。
 * 例如，从 String 字段转换为 Date 字段时，如果目标字段已使用@DateTimeFormat注解，则matches可能会返回true
 */
public interface ConditionalConverter {

    /**
     * 是否可以应用从源类型到当前正在考虑的目标类型的转换？
     *
     * @param sourceType 正在转换的字段的类型描述符
     * @param targetType 要转换为的字段的类型描述符
     * @return 如果应执行转换，则为 true，否则为 false
     */
    boolean matches(TypeDescriptor sourceType, TypeDescriptor targetType);
}

/**
 * ConditionalGenericConverter同时继承了GenericConverter和ConditionalConverter接口，支持更复杂的判断
 */
public interface ConditionalGenericConverter extends GenericConverter, ConditionalConverter {
}
```

**`Spring已经提供了基本的ConditionalGenericConverter的实现`，大部分的实现都是用来处理有集合或数组参与的转换，一个很好的例子同样是`ArrayToCollectionConverter`，它会在`getConvertibleTypes`方法判断源类型和目标类型满足条件之后，继续调用matches方法判断源数组元素类型到底能否转换为目标集合的元素类型，如果可以转换，那么才会真正的执行转换的逻辑！**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/10f37b8de9a24ca0a0a3d66acf993885~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

#### ConversionService API

**由于整个Conversion机制的复杂性，Spring提供了`ConversionService `接口，该接口定义了一系列统一的API方法供外部调用，用于在运行时执行类型转换，屏蔽了具体的内部调用逻辑。这是基于`门面（facade）设计模式`！**

```java
java复制代码/**
 * 用于类型转换的服务接口。
 * 这是进入转换系统的入口，通过调用convert(Object, Class) 来使用这个系统执行线程安全的类型转换。
 */
public interface ConversionService {

    /**
     * 如果源类型的对象可以转换为目标类型，则返回 true
     * <p>
     * 如果此方法返回 true，则意味着convert(Object, Class) 方法能够将源型的实例转换为目标类型。
     * <p>
     * 对于集合、数组和map类型之间的转换，此方法将返回 true，即使转换调用仍可能抛出ConversionException（如果基础元素不可转换）。
     * 调用者在使用集合和map时应处理此特殊情况。
     *
     * @param sourceType 要转换的源类型（如果源对象为 null，则可能为 null）
     * @param targetType 要转换为的目标类型（必需存在）
     * @return 如果可以执行转换，则为 true，如果不执行，则为 false
     * @throws IllegalArgumentException 如果targetType目标类型为null
     */
    boolean canConvert(@Nullable Class<?> sourceType, Class<?> targetType);

    /**
     * 如果源类型的对象可以转换为目标类型，则返回 true
     * <p>
     * 如果此方法返回 true，则意味着convert(Object, TypeDescriptor, TypeDescriptor)方法能够将源类型实例转换为目标类型。
     * <p>
     * 对于集合、数组和map类型之间的转换，此方法将返回 true，即使转换调用仍可能抛出ConversionException（如果基础元素不可转换）。
     * 调用者在使用集合和map时应处理此特殊情况。
     *
     * @param sourceType 有关要转换的源类型的上下文，也就是TypeDescriptor（如果源对象为 null，则可能为 null）
     * @param targetType 要转换为的目标类型的上下文，也就是TypeDescriptor（必需存在）
     * @return 如果可以在源类型和目标类型之间执行转换，则为 true，如果不执行，则为 false
     * @throws IllegalArgumentException 如果targetType目标类型为null
     */
    boolean canConvert(@Nullable TypeDescriptor sourceType, TypeDescriptor targetType);

    /**
     * 将给定的源对象转换为指定的目标类型。
     *
     * @param source     要转换的源对象（可能是 null ）
     * @param targetType 要转换为的目标类型（必需存在）
     * @return 转换的对象，目标类型的实例
     * @throws ConversionException      如果发生转换异常
     * @throws IllegalArgumentException 如果targetType目标类型为null
     */
    @Nullable
    <T> T convert(@Nullable Object source, Class<T> targetType);

    /**
     * 将给定的源对象转换为指定的目标类型。
     * TypeDescriptor提供有关发生转换的源和目标位置（通常是对象字段或属性位置）的其他上下文，从中可以获取到字段名、类型、注解、泛型等信息
     *
     * @param source     要转换的源对象（可能是 null ）
     * @param sourceType 有关要转换的源类型的上下文，也就是TypeDescriptor（如果源对象为 null，则可能为 null）
     * @param targetType 要转换为的目标类型的上下文，也就是TypeDescriptor（必需存在）
     * @return 转换的对象，目标类型的实例
     * @throws ConversionException      如果发生转换异常
     * @throws IllegalArgumentException 如果目标类型为null，或源类型为null，但源对象不是null
     */
    @Nullable
    Object convert(@Nullable Object source, @Nullable TypeDescriptor sourceType, TypeDescriptor targetType);

}
```

**ConversionService仅仅是一个用于类型转换的服务接口，大多数ConversionService的实现还实现了ConverterRegistry接口（一个提供了注册Converter方法的接口），它为注册Converter转换器提供了SPI机制。因此，`ConversionService的实现通常具有支持注册多个Converter转换器的方法。`**

```java
java复制代码/**
 * 用于使用类型转换系统注册转换器。
 */
public interface ConverterRegistry {

    /**
     * 向此注册表添加一个普通转换器，可转换的可转换源/目标类型对派生自转换器的泛型参数类型。
     */
    void addConverter(Converter<?, ?> converter);

    /**
     * 向此注册表添加一个普通转换器，已显示指定可转换的可转换源/目标类型对
     */
    <S, T> void addConverter(Class<S> sourceType, Class<T> targetType, Converter<? super S, ? extends T> converter);

    /**
     * 向此注册表添加通用转换器。
     */
    void addConverter(GenericConverter converter);

    /**
     * 将范围转换器工厂添加到此注册表。可转换源/目标类型对派生自转换器工厂的泛型参数类型。
     */
    void addConverterFactory(ConverterFactory<?, ?> factory);

    /**
     * 删除对应 源/目标类型对 的所有Converter
     */
    void removeConvertible(Class<?> sourceType, Class<?> targetType);

}
```

**通常，需要进行类型转换的时候，我们直接调用ConversionService的方法即可，而实际上的具体的转换逻辑将会被委托给其内部注册的转换器实例！** **Spring在`core.convert.support`包中提供了一系列健壮的conversionService实现和相关配置类。比如`GenericConversionService`是适用于大多数环境的通用实现，提供了配置Converter的功能。比如`ConversionServiceFactory`则是一个提供了为ConversionService注册Converter的服务的通用工厂。**

### 配置

ConversionService是一个无状态对象，被设计为在应用程序启动时实例化，然后可以在多个线程之间共享。在Spring应用程序中，通常为每个Spring容器（或ApplicationContext）配置一个ConversionService实例。Spring接收转换服务，并在框架需要执行类型转换时使用它。我们还可以将此ConversionService注入到任何bean中，并直接调用它的转换方法。

#### 配置BeanWarpper的ConversionService

**如果想要手动注册全局生效的默认ConversionService，那么需要将id命名为“`conversionService`”。在容器的`finishBeanFactoryInitialization`方法初始化全部普通bean实例之前，Spring容器会首先初始化这个`名为“conversionService”的ConversionService`，并且设置到`AbstractBeanFactory的conversionService属性`中。**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7e28055b4d9542e7a01f30ba7e2ed42d~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**在后续的`BeanWarpper`的初始化方法中（`AbstractBeanFactory#initBeanWrapper方法`中），会获取这个注册的`conversionService`并保存在自己的内部，用于后续的属性填充的类型转换服务：**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/08185755d23e4caeb052dc616a210ca6~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**对于的`普通spring项目`来说，不会注册默认的ConversionService，因此底层BeanWarpper的ConversionService为`null`，对于`boot项目`则会默认注册一个`ApplicationConversionService`服务。所以说，如果未在Spring中注册ConversionService，则BeanWarpper会使用基于`PropertyEditor`属性编辑器的原始转换系统。**

下面是注册一个默认ConversionService的示例：

```xml
xml复制代码<bean id="conversionService" class="org.springframework.context.support.Con
versionServiceFactoryBean"/>
```

**这里我们并没有直接配置ConversionService，而是配置的是一个`ConversionServiceFactoryBean`对象，它是一个`FactoryBean`类型的对象， 通过工厂模式来生产`ConversionService`，这也符合Spring的一贯作风，比较重且复杂的类构造起来一般采用工厂模式，它生产的对象实际类型就是`DefaultConversionService`。**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7811ac419e5140b7978c4d9eaf86dc72~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**在创建DefaultConversionService时，其默认会注册一些常用的Converter，如果要用自己的自定义转换器补充或重写默认转换器，那么可以设置`ConversionServiceFactoryBean的converters属性`，该属性值可以配置为`任何Converter、ConverterFactory或GenericConverter的实现`。**

如下是一个自定义的Converter实现：

```java
java复制代码/**
 * 把字符串转换Date
 *
 * @author lx
 */
@Component
public class StringToDateConverter implements Converter<String, Date> {

    /**
     * String source    传入进来字符串
     *
     * @param source 传入的要被转换的字符串
     * @return 转换后的格式类型
     */
    @Override
    public Date convert(String source) {
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
        Date parse;
        try {
            parse = dateFormat.parse(source);
            System.out.println("--------StringToDateConverter---------");
        } catch (ParseException e) {
            throw new IllegalArgumentException(e);
        }
        return parse;
    }
}
```

然后将其配置到ConversionServiceFactoryBean中即可：

```xml
xml复制代码<!--配置类型转换服务工厂，它会默认创建DefaultConversionService，并且支持注入自定义
类型转换器之外-->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <!--注入自定义转换器实例-->
            <ref bean="stringToDateConverter"/>
        </set>
    </property>
</bean>
```

#### 直接使用ConversionService

**我们可以直接在程序中以编程式的使用DefaultConversionService并且不需要创建DefaultConversionService的实例，因为DefaultConversionService实现了`懒加载的单例模式`，我们可以通过`getSharedInstance()`方法直接获取共享实例。**

```java
java复制代码public class DefaultConversionService extends GenericConversionService {

    @Nullable
    private static volatile DefaultConversionService sharedInstance;

    public static ConversionService getSharedInstance() {
        DefaultConversionService cs = sharedInstance;
        if (cs == null) {
            synchronized (DefaultConversionService.class) {
                cs = sharedInstance;
                if (cs == null) {
                    cs = new DefaultConversionService();
                    sharedInstance = cs;
                }
            }
        }
        return cs;
    }

    //…………
}
```

#### 配置DataBinder的ConversionService

**上面注册的默认全局ConversionService仅适用于`BeanWarpper`，对于`Spring MVC的DataBinder`无效，因为DataBinder初始化并在绑定ConversionService时不会使用`AbstractBeanFactory的conversionService属性`，DataBinder的ConversionService有另外的配置方法：**

1. 对于`XML配置`来说，配置`<mvc:annotation-driven>`标签即表示会默认使用一个`DefaultFormattingConversionService`实例，可以通过`conversion-service`属性指定某个`ConversionService`实例。
2. 对于`JavaConfig配置`来说，通过加入`@EnableWebMvc注解`也能注册一个默认的`DefaultFormattingConversionService`，而注册一个`id为mvcConversionService的ConversionService`即表示替代这个默认的`DefaultFormattingConversionService`。
3. **如果没有这两种配置，那么DataBinder同样没有ConversionService。**

如下配置，使得BeanWarpper和DataBinder都是用同一个conversionService：

```xml
xml复制代码<!--conversion-service属性指定在字段绑定期间用于类型转换的转换服务的bean名称。 -->
<!--如果不指定，则表示注册默认DefaultFormattingConversionService-->
<mvc:annotation-driven conversion-service="conversionService"/>

<!--配置类型转换服务工厂，它会默认创建DefaultConversionService，并且支持注入自定义类型转换器之外-->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <!--注入自定义转换器实例-->
            <ref bean="stringToDateConverter"/>
        </set>
    </property>
</bean>
```

关于`DefaultFormattingConversionService`，我们下面会介绍！

```xml
<!--conversion-service属性指定在字段绑定期间用于类型转换的转换服务的bean名称。 -->
<!--如果不指定，则表示注册默认DefaultFormattingConversionService-->
<mvc:annotation-driven conversion-service="conversionService"/>

<!--配置工厂，它会默认创建DefaultFormattingConversionService，并且支持注入自定义converters和formatters-->
<!--如果将它命名为conversionService，那么BeanWrapper和DataBinder 都共用此conversionService-->
<bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <!--注入自定义formatters，支持Formatter 和 AnnotationFormatterFactory的实例-->
    <property name="formatters">
        <set/>
    </property>
    <!--注入自定义converters，支持Converter、ConverterFactory、GenericConverter的实例-->
    <property name="converters">
        <set/>
    </property>
    <!--注入自定义的formatterRegistrars-->
    <property name="formatterRegistrars">
        <set>
            <ref bean="customFormatterRegistrar"/>
        </set>
    </property>
</bean>
```

**在解析时，首先使用注册的局部PropertyEditor来解析，然后在使用全局的conversionService来解析！**

如果你希望能够处理多种不同的时间格式，可以考虑使用 Spring 的 `@DateTimeFormat` 注解配合自定义的全局转换器来实现。下面是一个示例：

首先，创建一个自定义的全局转换器实现 `Converter<String, LocalDateTime>` 接口，用于处理多种时间格式：

```java
import org.springframework.core.convert.converter.Converter;
import org.springframework.stereotype.Component;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.Arrays;
import java.util.List;

@Component
public class StringToLocalDateTimeConverter implements Converter<String, LocalDateTime> {

    private final List<DateTimeFormatter> formatters = Arrays.asList(
            DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"),
            DateTimeFormatter.ofPattern("yyyy-MM-dd")
            // Add more formats if needed
    );

    @Override
    public LocalDateTime convert(String source) {
        for (DateTimeFormatter formatter : formatters) {
            try {
                return LocalDateTime.parse(source, formatter);
            } catch (Exception ignored) {
            }
        }
        throw new IllegalArgumentException("Unable to parse the date: " + source);
    }
}
```

然后，在你的 Spring MVC 配置中注册这个全局转换器：

```xml
<mvc:annotation-driven conversion-service="conversionService"/>

<bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <bean class="your.package.StringToLocalDateTimeConverter"/>
        </set>
    </property>
</bean>
```

现在，当你在 Spring MVC 的控制器方法中使用 `LocalDateTime` 类型的参数并标记为 `@RequestParam` 或 `@PathVariable` 时，Spring 将会尝试使用你定义的多个格式进行转换。例如：

```java
@GetMapping("/example")
public String example(@RequestParam("date") @DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss") LocalDateTime date) {
    // Your logic here
    return "example";
}
```

这样，Spring 将会根据你提供的多个时间格式来尝试解析参数中的日期时间字符串。

> 在使用 jackson 进行类和json的相互转换时，一定要具有get和set方法，否则会出现错误：No serializer found for class xxx and no properties discovered to create BeanSerializer。

## Fommarter

**`core.convert `是一种通用类型转换系统。它提供了一个统一的ConversionService API 以及一个强类型的Converter  SPI，用于实现从一种类型到另一种类型的转换逻辑，甚至提供了更多扩展功能的GenericConverter以及ConditionalGenericConverter，Spring通常建议使用此系统为beanWarpper绑定bean属性值。此外，Spring表达式语言（SPEL）和DataBinder都可以使用此系统绑定字段值。例如，当SPEL需要将Short强制转换为Long以完成expression.setValue(Object bean, Object value)操作时，core.convert系统将执行强制转换。**

**在`Spring MVC`中，HTTP中的源数据都是`String`类型，数据绑定需要将String转换为其他类型，同时也可能需要将数据转换为具有本地格式的字符串样式进行展示，core.convert中更通用的Converter SPI不能直接满足这种格式要求。为了解决这些问题，Spring 3 引入了一个方便的 `Formatter SPI`，用于在web环境下替代PropertyEditor。**

**通常，当需要实现通用类型转换逻辑时，可以使用Converter SPI，例如，在java.util.Date和Long之间转换。当在客户端环境（如Web应用程序）中工作并且需要解析和输出本地化字段值时，可以使用`Formatter SPI`。ConversionService为两个SPI提供统一的类型转换API。**

### Formatter SPI

**`org.springframework.format.Formatter`是一个用于实现字段格式化逻辑的非常简单并且是强类型的SPI接口。**

```java
java
复制代码public interface Formatter<T> extends Printer<T>, Parser<T> {}
```

**Formatter继承了Printer和Parser接口**，下面是这两个接口的定义：

```java
java复制代码@FunctionalInterface
public interface Printer<T> {

    /**
     * 打印类型 T 的对象以进行显示
     *
     * @param object 要打印的实例
     * @param locale 当前用户的区域（本地化）设置
     * @return 打印的文本字符串
     */
    String print(T object, Locale locale);

}

@FunctionalInterface
public interface Parser<T> {

    /**
     * 分析文本字符串以生成 T
     *
     * @param text   文本字符串
     * @param locale 当前用户区域设置
     * @return T 的实例
     * @throws ParseException           当 java.text 解析库中发生解析异常时
     * @throws IllegalArgumentException 当发生解析异常时
     */
    T parse(String text, Locale locale) throws ParseException;

}
```

**如果要创建自己的Formatter， 需要实现上面提到的`Formatter`接口以完成T类型对象的格式化和解析功能。**

**实现`print()`方法根据客户端的区域设置以打印T实例，实现 `parse()`方法根据客户端的区域设置以及文本字符串中分析 T 的实例。如果解析尝试失败，则 Formatter 应引发ParseException或IllegalArgumentException异常。注意确保 Formatter 实现是线程安全的。**

`org.springframework.format.support子包`提供了常见方便使用的Formatter实现。

`org.springframework.format.number子包`提供了NumberStyleFormatter、 CurrencyStyleFormatter和PercentStyleFormatter来格式化Number对象，其内部使用java.text.NumberFormat。

`org.springframework.format.number.money`提供了与JSR-354集成的针对货币的格式化器，比如CurrencyUnitFormatter、MonetaryAmountFormatter。

`org.springframework.format.datetime子包`提供了DateFormatter，内部使用java.text.DateFormat来格式化java.util.Date对象。org.springframework.format.datetime.joda子包基于joda时间库提供全面的datetime格式支持。

### 注解驱动格式化

**字段格式化可以按字段类型或注解进行配置，要绑定一个注解到Formatter，可以实现`AnnotationFormatterFactory`接口。**

**`BeanWarpper和DataBinder`都支持通过自定义注解驱动类型转换，但是请注意Spring MVC请求数据绑定时只能对某个完整变量（URI路径变量、请求参数、请求体数据）进行注解驱动类型转换，如果是`@RequestBody`之类的请求或者使用一个变量表示一个实体时，变量内部的数据不支持注解驱动类型转换。**

```java
java复制代码/**
 * 用于创建formatter以格式化使用特定注解的字段值的工厂。
 * <p>
 * 例如，DateTimeFormatAnnotationForMatterFactory 可能会创建一个formatter，该formatter对使用@DateTimeForma注解的字段格式化为Date类型
 *
 * @param <A> 应触发格式化的注解的类型
 */
public interface AnnotationFormatterFactory<A extends Annotation> {

    /**
     * 可使用A类型注解的字段类型。
     */
    Set<Class<?>> getFieldTypes();

    /**
     * 获取 Printer 以 print 具有指定注解的fieldType类型的字段值
     *
     * @param annotation 注解实例
     * @param fieldType  注解的字段类型
     * @return the printer
     */
    Printer<?> getPrinter(A annotation, Class<?> fieldType);

    /**
     * 获取 Parser 以 parse 有指定注解的fieldType类型的字段值
     *
     * @param annotation 注解实例
     * @param fieldType  注解的字段类型
     * @return the parser
     */
    Parser<?> getParser(A annotation, Class<?> fieldType);

}
```

**泛型A表示与格式化逻辑关联的注解，例如`org.springframework.format.annotation.DateTimeFormat`，`getFieldTypes()`方法返回可在其上使用注解的字段类型。`getprinter()`返回Printer以打印注解字段的值。`getParser()`返回一个Parser来解析注解字段的clientValue。**

`org.springframework.format.annotation`包中提供了`AnnotationFormatterFactory`关联的注解的实现：

1. **`@NumberFormat`用格式化Number类型的字段（比如Double、Long），对应NumberFormatAnnotationFormatterFactory、Jsr354NumberFormatAnnotationFormatterFactory。**
2. **`@DateTimeFormat `用于格式化java.util.Date、java.util.Calendar、Long（时间戳毫秒）以及JSR-310 java.time 和 Joda-Time 值类型。对应DateTimeFormatAnnotationFormatterFactory、JodaDateTimeFormatAnnotationFormatterFactory、Jsr310DateTimeFormatAnnotationFormatterFactory。**

**使用时也很简单，开启Spring mvc配置之后，Spring MVC默认会注册这些AnnotationFormatterFactory，我们可直接使用上面的注解。**

**下面的示例使用@DateTimeFormat将前端传递的yyyy-MM-dd类型的日期字符串参数格式化为Date！**

```java
java复制代码public class MyDate {
    @DateTimeFormat(pattern = "yyyy-MM-dd")
    private Date date;

    public Date getDate() {
        return date;
    }

    public void setDate(Date date) {
        this.date = date;
    }
}
```

一个控制器方法：

```java
java复制代码@RequestMapping("/dateTimeFormat/{date}")
@ResponseBody
public MyDate annotationFormatterFactory(MyDate date) {
    System.out.println(DateFormat.getDateTimeInstance().format(date.getDate()));
    return date;
}
```

访问/dateTimeFormat/2021-01-29，可以看到如下输出：

```java
java
复制代码2021-1-29 0:00:00
```

说明格式化成功，此时页面展示的JSON字符串样式如下：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/baf90663f211462b8dcb7bb8f729a6c5~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

### FormatterRegistry SPI

**`org.springframework.format.FormatterRegistry`是可用于注册formatters的SPI接口，它还继承了`ConverterRegistry`，因此同样可以注册converters**

```java
java复制代码public interface FormatterRegistry extends ConverterRegistry {

    void addPrinter(Printer<?> printer);

    void addParser(Parser<?> parser);

    void addFormatter(Formatter<?> formatter);

    void addFormatterForFieldType(Class<?> fieldType, Formatter<?> formatter);

    void addFormatterForFieldType(Class<?> fieldType, Printer<?> printer, Parser<?> parser);

    void addFormatterForFieldAnnotation(AnnotationFormatterFactory<? extends Annotation> annotationFormatterFactory);
}
```

**实际上，上面看到的FormatterRegistry提供的注册Formatter、Parser、Printer、AnnotationFormatterFactory的方法在内部会被转换为注册对应的Converter，因为它们的功能的本质都是相同的，并且Converter的功能涵盖了Formatter所有功能！因此Formatter和Converter可以看作是同一个底层类的不同表层展示：**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dd47b73db9534d3e9095271cad7f80d7~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**FormatterRegistry SPI 允许我们集中配置格式规则，而不是在控制器之间复制这样的配置。例如，可能希望强制所有日期字段以某种方式格式化，或者具有特定注解的字段以某种方式格式化。使用共享的FormatterRegistry，我们只需定义一次这些规则，这些规则在需要格式化时会自动使用。**

**FormattingConversionService是一个适用于大多数环境的FormatTerregistry的实现。并且因为FormattingConversionService还继承了GenericConversionService，因此可以直接使用FormattingConversionService将Converter和Formatter都配置在一起。**

**Spring MVC默认使用的`DefaultFormattingConversionService`就实现了FormattingConversionService。**

### FormatterRegistrar SPI

**`org.springframework.format.FormatterRegistrar`是一个用于为FormatterRegistry注册formatters和converters的通用SPI接口，它类似于前面学习的PropertyEditorRegistrar，可`一次性注册多个formatter和converter`，在为给定格式（如日期格式）注册多个相关converters和formatters时，FormatterRegistrar非常有用。**

```java
java复制代码public interface FormatterRegistrar {

    /**
     * 为FormatterRegistry注册formatters和converters
     *
     * @param registry 要使用的 FormatterRegistry 实例
     */
    void registerFormatters(FormatterRegistry registry);

}
```

### 全局配置

**我们在“配置DataBinder的ConversionService”的部分就说过了，在开启MVC配置之后，DataBinder会默认使用一个DefaultFormattingConversionService作为conversionService，当然我们也可以配置自定义的conversionService。**

**在学习Fromatter之后，我们可以使用`FormattingConversionServiceFactoryBean`工厂而不是ConversionServiceFactoryBean来作为一个真正的BeanWrapper和DataBinder 都共用的conversionService，因为它支持更多特性，比如同时支持注册formatters和converters！**

**下面我们提供一个简单的自定义的全局conversionService配置，其内部通过FormatterRegistrar注册两个AnnotationFormatterFactory实例，以期待实现基于注解的格式化转换！**

**自定义两个注解：**

```java
java复制代码/**
 * @author lx
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD, ElementType.FIELD, ElementType.PARAMETER, ElementType.ANNOTATION_TYPE})
public @interface PersonFormat {

}
/**
 * @author lx
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD, ElementType.FIELD, ElementType.PARAMETER, ElementType.ANNOTATION_TYPE})
public @interface SexFormat {

    String value() default "";

}
```

**Person实体，其内部的sex字段标注了@SexFormat注解，该类用来测试Spring MVC的DataBinder的数据绑定：**

```java
java复制代码/**
 * @author lx
 */
public class Person {
    private Long id;
    private String tel;
    private Integer age;
    @SexFormat
    private String sex;

    public Person(Long id, String tel, Integer age, String sex) {
        this.id = id;
        this.tel = tel;
        this.age = age;
        this.sex = sex;
    }

    public Person() {
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getTel() {
        return tel;
    }

    public void setTel(String tel) {
        this.tel = tel;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }
}
```

**PersonFormatter的实现，用于将前端传递的person字符串转换为Person，我们约定前段传递的person字符串使用“|”分隔：**

```java
java复制代码/**
 * @author lx
 */
public class PersonFormatter implements Formatter<Person> {

    @Override
    public Person parse(String text, Locale locale) throws ParseException {
        String[] split = text.split("\\|");
        if (split.length == 4) {
            return new Person(Long.valueOf(split[0]), split[1], Integer.valueOf(split[2]), split[3]);
        }
        throw new ParseException("参数格式不正确：" + text, 0);
    }

    @Override
    public String print(Person object, Locale locale) {
        return object.toString();
    }
}
```

SexFormatter的实现，用于将性别的数字转换为性别字符串：

```java
java复制代码public class SexFormatter implements Formatter<String> {
    private static final String MAN = "男";
    private static final String WOMAN = "女";
    private static final String OTHER = "未知";


    @Override
    public String parse(String text, Locale locale) {
        if ("0".equals(text)) {
            return MAN;
        }
        if ("1".equals(text)) {
            return WOMAN;
        }
        return OTHER;
    }

    @Override
    public String print(String object, Locale locale) {
        return object;
    }


    public static class WomanFormatter extends SexFormatter {

        @Override
        public String parse(String text, Locale locale) {
            return WOMAN;
        }
    }

    public static class ManFormatter extends SexFormatter {

        @Override
        public String parse(String text, Locale locale) {
            return MAN;
        }
    }
}
```

PersonAnnotationFormatterFactory的实现：

```java
java复制代码/**
 * @author lx
 */
@Component
public class PersonAnnotationFormatterFactory implements AnnotationFormatterFactory<PersonFormat> {

    Set<Class<?>> classSet = Collections.singleton(Person.class);

    @Override
    public Set<Class<?>> getFieldTypes() {
        return classSet;
    }

    @Override
    public Parser<?> getParser(PersonFormat annotation, Class<?> fieldType) {
        return configureFormatterFrom(annotation);
    }

    @Override
    public Printer<?> getPrinter(PersonFormat annotation, Class<?> fieldType) {
        return configureFormatterFrom(annotation);
    }

    private Formatter<Person> configureFormatterFrom(PersonFormat annotation) {
        return new PersonFormatter();
    }
}
```

SexAnnotationFormatterFactory的实现：

```java
java复制代码/**
 * @author lx
 */
@Component
public class SexAnnotationFormatterFactory implements AnnotationFormatterFactory<SexFormat> {

    Set<Class<?>> classSet = Collections.singleton(String.class);

    @Override
    public Set<Class<?>> getFieldTypes() {
        return classSet;
    }

    @Override
    public Parser<?> getParser(SexFormat annotation, Class<?> fieldType) {
        return configureFormatterFrom(annotation);
    }

    @Override
    public Printer<?> getPrinter(SexFormat annotation, Class<?> fieldType) {
        return configureFormatterFrom(annotation);
    }

    private Formatter<String> configureFormatterFrom(SexFormat annotation) {
        String value = annotation.value();
        if ("0".equals(value)) {
            return new SexFormatter.ManFormatter();
        }
        if ("1".equals(value)) {
            return new SexFormatter.WomanFormatter();
        }
        return new SexFormatter();
    }
    
}
```

**一个Controller控制器，用于测试Spring MVC的DataBinder，同时其内部有一个sex属性标注了@SexFormat注解，该类用来测试Spring 的BeanWrapper：**

```java
java复制代码/**
 * @author lx
 */
@RestController
public class AnnotationFormatterFactoryController {

    /*用于测试DataBinder的数据转换*/


    @RequestMapping("/annotationFormatterFactory/{person}")
    @ResponseBody
    public Person annotationFormatterFactory(@PersonFormat Person person, @SexFormat String sex) {
        System.out.println(sex);
        return person;
    }

    /*用于测试BeanWrapper的数据转换*/

    @SexFormat("2")
    @Value("1")
    private String sex;

    @PostConstruct
    public void test() {
        System.out.println(sex);
    }
}
```

**一个自定义的FormatterRegistrar，将两个自定义的AnnotationFormatterFactory注册到FormatterRegistry中：**

```java
java复制代码/**
 * @author lx
 */
@Component
public class CustomFormatterRegistrar implements FormatterRegistrar {

    @Resource
    private PersonAnnotationFormatterFactory personAnnotationFormatterFactory;
    @Resource
    private SexAnnotationFormatterFactory sexAnnotationFormatterFactory;
    
    @Override
    public void registerFormatters(FormatterRegistry registry) {
        registry.addFormatterForFieldAnnotation(personAnnotationFormatterFactory);
        registry.addFormatterForFieldAnnotation(sexAnnotationFormatterFactory);
    }
}
```

下面是配置文件，Spring的BeanWrapper和Spring MVC的DataBinder都支持该conversionService：

```xml
xml复制代码<!--conversion-service属性指定在字段绑定期间用于类型转换的转换服务的bean名称。 -->
<!--如果不指定，则表示注册默认DefaultFormattingConversionService-->
<mvc:annotation-driven conversion-service="conversionService"/>

<!--配置工厂，它会默认创建DefaultFormattingConversionService，并且支持注入自定义converters和formatters-->
<!--如果将它命名为conversionService，那么BeanWrapper和DataBinder 都共用此conversionService-->
<bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <!--注入自定义formatters，支持Formatter 和 AnnotationFormatterFactory的实例-->
    <property name="formatters">
        <set/>
    </property>
    <!--注入自定义converters，支持Converter、ConverterFactory、GenericConverter的实例-->
    <property name="converters">
        <set/>
    </property>
    <!--注入自定义的formatterRegistrars-->
    <property name="formatterRegistrars">
        <set>
            <ref bean="customFormatterRegistrar"/>
        </set>
    </property>
</bean>
```

**启动项目，我们即发现了输出了“女”字符，说我们配置的conversionService对于Spring BeanWrapper的属性数据转换成功生效！**

**访问/annotationFormatterFactory/1234134|123456|11|1，结果如下：**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/37745f87e09143108ff74475117c603e~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**成功的从指定格式的字符串转换Person参数实例，Spring MVC的DataBinder测试成功，但是我们发现其内部的sex属性却还是1，并没有转换为性别字符串，为什么呢？因为Spring MVC请求数据绑定时只能对某个完整变量（URI路径变量、请求参数、请求体数据）进行整体的注解驱动类型转换，比如上面的person路径变量就可以整体转换为Person对象成功，但是变量内部的部分数据不支持注解驱动类型转换，比如上面的性别数据标识符“1”位于person变量字符串内部，这样就不能应用注解驱动类型转换！**

**如果想要处理，那么可以添加一个单独的请求参数“sex”。访问/annotationFormatterFactory/1234134|123456|11|1?sex=1，结果如下：**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/639fbe5856eb4d02a26dcac6b9f755f5~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

# 信息转换

**HttpMessageConverter同样Spring 3.0加入的一个Converter，但是它不属于org.springframework.core.convert体系，而是位于org.springframework.http.converter包，它不会参与BeanWrapper、DataBinder、SPEL中的类型转换操作，它常被用在HTTP客户端（比如RestTemplate）和服务端（比如Spring MVC REST风格的controllers）的数据转换中，详细描述如下：**

1. 在Spring  MVC的控制器方法中，如果使用`@RequestBody、HttpEntity<B>、@RequestPart`等设置请求参数时，将会采用`HttpMessageConverter`完成请求正文（请求体）到方法参数的类型转换，并且这里的转换操作。
2. 在Spring MVC的控制器方法中，如果使用`@ResponseBody、HttpEntity<B>, ResponseBodyEmitter、SseEmitterResponseEntity<B>`等设置响应时，将会采用`HttpMessageConverter`对返回的实体对象执行转换操作并写入响应正文（响应体）。
3. 在通过`RestTemplate`进行远程HTTP调用的时候，将会通过`HttpMessageConverter`将调用返回的响应正文（响应体）转换为指定类型的对象，开发者可以直接获取转换后的对象。

**对于请求体的转换操作发生在DataBinder创建之前。某个请求体或者响应体使用什么哪种HttpMessageConverter来进行转换，与请求或者响应中的media type（MIME，媒体类型）有关，Spring MVC已经提供了主要的媒体类型的HttpMessageConverter实现，默认情况下，HttpMessageConverter在客户端的 RestTemplate和服务器端的 RequestMappingHandlerAdapter中注册。**

**所有转换器会支持各自默认的媒体类型，但可以通过设置supportedMediaTypes属性来覆盖它。**

下面是HttpMessageConverter的常见实现以及简介：

| MessageConverter                       | 描述                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| StringHttpMessageConverter             | 可以从 HTTP 请求和响应读取和写入字符串数据。默认情况下，此转换器支持所有媒体类型（*/*），并使用Content-Type为text/plain的内容类型进行写入。 |
| FormHttpMessageConverter               | 可以从 HTTP 请求和响应读取和写入表单数据。默认情况下，此转换器支持读取和写入application/x-www-form-urlencoded媒体类型MultiValueMap<String,  String>。默认还支持写“multipart/form-data”和"multipart/mixed"媒体类型的数据MultiValueMap<String,  Object>，但是无法读取这两个请求的数据，也就是说无法支持文件上传。如果想要支持multipart/form-data媒体类型的请求，请配置MultipartResolver组件 |
| ByteArrayHttpMessageConverter          | 可以从 HTTP 请求和响应读取和写入byte[]字节数组。默认情况下，此转换器支持所有媒体类型 （*/*），并使用Content-Type为application/octet-stream的内容类型进行写入。这可以通过设置受支持媒体类型属性来覆盖。 |
| MarshallingHttpMessageConverter        | 可以从 HTTP 请求和响应通过org.springframework.oxm包中的Marshaller和Unmarshaller来读取和写入XML数据。默认情况下，此转换器支持text/xml 和application/xml。 |
| MappingJackson2HttpMessageConverter    | 最常见的Converter。可以从 HTTP 请求和响应通过Jackson  2.x 的ObjectMapper读取和写入Json数据。可以使用Jackson提供的注解根据需要自定义 JSON 映射规则（比如@JsonView）。当需要进一步控制JSON序列化和反序列化规则时，可以自定义ObjectMapper的实现并通过该Converter的objectMapper属性注入。默认情况下，此转换器支持application/json。 |
| MappingJackson2XmlHttpMessageConverter | 可以从 HTTP 请求和响应通过通过Jackson XML 扩展的 XmlMapper 读取和写入 XML数据。可以使用JAXB或Jackson提供的注解根据需要自定义 XML 映射规则。当需要进一步控制XML序列化和反序列化规则时，可以自定义XmlMapper的实现并通过该Converter的objectMapper属性注入。默认情况下，此转换器支持application/xml。 |
| SourceHttpMessageConverter             | 可以从 HTTP 请求和响应读取和写入javax.xml.transform.Source数据，仅支持 DOMSource、SAXSource 和 StreamSource类型。默认情况下，此转换器支持text/xml和application/xml。 |
| BufferedImageHttpMessageConverter      | 可以从 HTTP 请求和响应读取和写入java.awt.image.BufferedImage数据。默认情况下，此转换器可以读取ImageIO#getReaderMIMETypes()方法返回的所有媒体类型，并且使用ImageIO#getWriterMIMETypes()方法返回的第一个可用的媒体类型进行写入。 |

## 配置

**在通过注解和JavaConfig开启MVC配置之后，我们可以通过重写`configureMessageConverters方法`来替换Spring MVC创建的默认转换器，比如配置FastJson转换器，或者重写`extendMessageConverters方法`来扩展或者修改转换器！**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ac3bc93fc7d94932bb3a009fec68500e~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**图中的`addDefaultHttpMessageConverters方法`用于注册默认的转换器，从该方法的源码中能够得知，如果存在jackson的依赖，那么会自动注册`MappingJackson2HttpMessageConverter`：**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/771be1be64684e0aba9a7e723a5bd4cf~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

如下JavaConfig配置：

```java
java复制代码/**
 * @author lx
 */
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        Jackson2ObjectMapperBuilder builder = new Jackson2ObjectMapperBuilder()
                .indentOutput(true)
                .dateFormat(new SimpleDateFormat("yyyy-MM-dd"));
        converters.add(new MappingJackson2HttpMessageConverter(builder.build()));
    }
}
```

**这表示我们自定义配置的`MappingJackson2HttpMessageConverter`在序列化和反序列化Date时仅支持“`yyyy-MM-dd`”格式。**

以下XML配置，可达到和JavaConfg配置同样的效果：

```xml
xml复制代码<mvc:annotation-driven>
    <!--配置自定义的消息转换器-->
    <mvc:message-converters>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <!--配置objectMapper-->
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean"
                      p:indentOutput="true"
                      p:simpleDateFormat="yyyy-MM-dd"/>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

推荐使用JavaConfg的配置！





# 数据校验

web项目中，后端对于前端传递的参数总是免不了进行校验，比如字符长度、字段大小、null校验等等，虽然有些校验前端也会去做，但是为了增加web应用的健壮性和安全性（比如，如果绕过前端发送的直接请求，这时参数就没法得到保证了），对于重要的接口，后端进行参数二次校验是非常有必要的！

在使用Spring MVC框架之后，在进行请求的数据绑定（data binding）成功之后，可以基于`javax.validation.Valid注解`或者`Spring的@Validated注解`进行自动化数据校验，并且支持配置全局和单个请求的校验器。使用起来非常方便和简单，大大减少了开发人员的负担！

在上面 “`Spring MVC参数属性校验`” 的部分中，通过MVC配置的默认校验，仅仅是针对Spring MVC的控制器方法绑定的参数对象的属性进行校验，如果还需要对于`方法参数本身`进行或者`方法的返回值`进行校验，比如校验方法参数和返回值本身不为null，或者需要对属于非控制器的方法进行同样的Bean validation校验，那么我们需要配置Spring 驱动方法校验。`Spring 驱动方法校验`的校验规则和上面的Spring MVC参数属性校验的校验规则都是一样的，只不过它的应用范围更加广泛，在普通项目中应该手动配置Spring 驱动方法校验。同样，如果是Spring Boot的web项目，那么Spring Boot为我们自动配置好了，我们无需任何配置！

## Bean Validation

**`Java Bean Validation 2.0`后改名为`Jakarta Bean Validation`，2018年之后的新版本依赖均以Jakarta开头，官网是`https://beanvalidation.org/`，目前来说，它们的maven依赖源码基本一致，最大的区别就是最新依赖命名为`jakarta.validation:jakarta.validation-api`，并且最新的版本都是此依赖，以前的`javax.validation`旧版本依赖不再更新！**

**Jakarta Bean Validation提供了一套`Java Bean`验证规范，也称为`JSR-303`规范，提出了一种声明性的Bean验证约束的功能，可以使用规范中注解帮绑定到模型属性，然后由运行时强制执行这些约束条件，还可以自定义约束注解，如下示例：**

```java
java复制代码public class PersonForm {

    @NotNull
    @Size(max=64)
    private String name;

    @Min(0)
    private int age;
}
```

**`Jakarta Bean Validation`仅仅是一套规范，并没提供任何实现，但是到目前有些厂商已提供了自己的实现，我们只需要引入他们提供的maven坐标即可使用它们的实现，最常见的就是`hibernate-validator`，`hibernate-validator和hibernate ORM框架`没啥联系，唯一的联系就是它们都是由`hibernate团队`开发的，官网是：`https://hibernate.org/validator/`。**

## 内置约束注解

**`hibernate-validator`提供了`Jakarta Bean Validation`的内置约束注解的全部实现，同时可以自定义约束条件！同时，`hibernate-validator`对`Jakarta Bean Validation`的内置约束注解的使用范围进行了扩展，比如`@Max`适用于字符串。参见：`https://docs.jboss.org/hibernate/validator/7.0/reference/en-US/html_single/#section-builtin-constraints`。**

**内置约束位于`jakarta.validation.constraints`包中！所有的注解都具有`message、groups、payload`属性：**

1. `message`：在违反约束时返回创建的错误消息，可以使用｛key｝设置默认的key，将会自动查找对应的value，如果没有对应的value，那么使用key的值作为默认错误消息。消息中可以通过{elementName}获取注解的元素值，也可以使用${validatedValue}获取注解标注的值，也可以使用其他el表达式。
2. `payload`：可以将自定义有效Payload对象分配给约束，通常未使用。

**此外它们还有如下特性：**

| 注解                            | 描述                                                         | 支持的类型（如果这些注解放在它们不支持的类型上，那么运行时将会抛出UnexpectedTypeException异常） |
| ------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| @AssertFalse                    | 检查注解的值是否为false。null值被认为验证通过。              | 支持Boolean和boolean类型。                                   |
| @AssertTrue                     | 检查注解的值是否为true。null值被认为验证通过。               |                                                              |
| @DecimalMax(value=, inclusive=) | 当inclusive=false，检查注解的值是否小于指定最大值，否则，检查该值是否小于或等于指定的最大值。参数value在内部会通过new BigDecimal(value)转换为的BigDecimal表现形式再进行比较。null值被认为校验通过。 | 支持BigDecimal、BigInteger、CharSequence，byte 、short 、int 、long以及它们的包装类型的验证。请注意，由于舍入错误，因此不支持double和float。 此外hv的实现还支持Number及其子类型，以及javax.money.MonetaryAmount类型（如果存在JSR 354 的依赖）。还支持double和float，但是可能出现比较结果不准确的情况。 |
| @DecimalMin(value=, inclusive=) | 当inclusive=false，检查注解的值是否大于指定最小值，否则，检查该值是否大于或等于指定的最小值。参数value在内部会通过new BigDecimal(value)转换为的BigDecimal表现形式再进行比较。null值被认为校验通过。 |                                                              |
| @Digits(integer=, fraction=)    | 检查注解的值是否是属于最多integer整数位和fraction小数位范围内的数字。null值被认为校验通过。 |                                                              |
| @Max(value=)                    | 检查注解的值是否小于或等于指定的最大值。null值被认为校验通过。 | 支持BigDecimal、BigInteger，byte 、short 、int 、long以及它们的包装类型的验证。请注意，由于舍入错误，不支持double和float。 此外，hv的实现还支持CharSequence及其子类型（由字符序列表示的数值计算），以及Number及其子类型以及javax.money.MonetaryAmount类型（如果存在JSR 354 的依赖）。还支持double和float，但是可能出现比较结果不准确的情况。 |
| @Min(value=)                    | 检查注解的值是否小于或等于指定的最大值。null值被认为校验通过。 |                                                              |
| @Email                          | 检查注解的值是否是有效的电子邮件地址。可选参数 regexp 和flags允许指定电子邮件必须匹配的附加正则表达式（包括正则表达式标志）。 | 支持CharSequence及其子类型。                                 |
| @Future                         | 检查注解的值是否是一个将来的时间。null值被认为校验通过。     | 支持java.util.Date, java.util.Calendar, java.time.Instant, java.time.LocalDate, java.time.LocalDateTime, java.time.LocalTime, java.time.MonthDay, java.time.OffsetDateTime, java.time.OffsetTime, java.time.Year, java.time.YearMonth, java.time.ZonedDateTime, java.time.chrono.HijrahDate, java.time.chrono.JapaneseDate, java.time.chrono.MinguoDate, java.time.chrono.ThaiBuddhistDate类型。 此外，hv的实现还支持Joda Time date/time类型（如果存在Joda Time依赖），以及支持ReadablePartial 和 ReadableInstant的任何实现。 |
| @FutureOrPresent                | 检查注解的值是否是当前时间或者将来时间。null值被认为校验通过。 |                                                              |
| @Past                           | 检查注解的值是否是一个过去的时间。null值被认为校验通过。     |                                                              |
| @PastOrPresent                  | 检查注解的值是否是当前时间或者过去时间。                     |                                                              |
| @NotBlank                       | 检查注解的值是否不为null并且必须至少包含一个非空白字符。     | 支持CharSequence及其子类型。                                 |
| @NotEmpty                       | 检查注解的值是否不为null并且不为空。                         | 支持CharSequence及其子类型（计算字符序列的长度，即不能为""），以及Collection、Map、Array（至少包含一个元素/键值对）。 |
| @NotNull                        | 检查注解的值是否不为null。                                   | 支持任何类型。                                               |
| @Null                           | 检查注解的值是否为null。                                     |                                                              |
| @Negative                       | 检查注解的值是否是严格的负数（0不通过）。null值被认为校验通过。 | 支持BigDecimal、BigInteger，byte 、short 、int 、long 、float、double以及它们的包装类型。 此外，hv的实现还支持CharSequence及其子类型（由字符序列表示的数值计算），以及Number及其子类型以及javax.money.MonetaryAmount类型（如果存在JSR 354 的依赖）。 |
| @NegativeOrZero                 | 检查注解的值是否是负数或0。null值被认为校验通过。null值被认为校验通过。 |                                                              |
| @Positive                       | 检查注解的值是否是严格的正数（0不通过）。null值被认为校验通过。 |                                                              |
| @PositiveOrZero                 | 检查注解的值是否是正数或0。null值被认为校验通过。            |                                                              |
| @Pattern(regex=, flags=)        | 检查注解的值是否与给定的正则表达式匹配（regex），可以考虑给定的标记匹配项（falgs）null值被认为校验通过。 | 支持CharSequence及其子类型。                                 |
| @Positive                       | 检查注解的值是否是严格的正数（如果是0.0则通过，其他0则不通过）。null值被认为校验通过。 | 支持BigDecimal、BigInteger，byte 、short 、int 、long 、float、double以及它们的包装类型。 此外，hv的实现还支持CharSequence及其子类型（由字符序列表示的数值计算），以及Number及其子类型以及javax.money.MonetaryAmount类型（如果存在JSR 354 的依赖）。 |
| @PositiveOrZero                 | 检查注解的值是否是正数或0。null值被认为校验通过。            |                                                              |
| @Size(min=, max=)               | 检查注解的值是否在给定的最小值（默认0）和最大值（默认Integer.MAX_VALUE）之间，包含两个端点值。null值被认为校验通过。 | 支持CharSequence及其子类型（计算字符序列的长度），以及Collection、Map、Array（计算元素/键值对数量）。 |

## 其他约束注解

**除了由`Jakarta Bean Validation API`定义的约束注解之外，`Hibernate Validator`还提供了几个有用的自定义约束注解，虽然可能用不到，但是可以看看以防不时之需！**

| 注解                                                         | 描述                                                         | 支持的类型                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| @CreditCardNumber(ignoreNonDigitCharacters=)                 | 检查注解的值是否通过 Luhn 校验和测试，即美国信用卡号的格式是否正确。ignoreNonDigitCharacters表示是否允许忽略非数字字符，默认值为 false。null值被认为校验通过。 | 支持CharSequence及其子类型。                                 |
| @Currency(value=)                                            | 检查注解的javax.money.MonetaryAmount的货币单位是否属于包含在指定的value单位数组中。null值被认为校验通过。 | 支持javax.money.MonetaryAmount及其子类型（如果存在JSR 354 的依赖）。 |
| @DurationMax(days=, hours=, minutes=, seconds=, millis=, nanos=, inclusive=) | 检查注解的java.time.Duration表示的时间差不大于注解指定的值，如果inclusive标志设置为 true，则允许相等。null值被认为校验通过。 | 支持java.time.Duration类型。                                 |
| @DurationMin(days=, hours=, minutes=, seconds=, millis=, nanos=, inclusive=) | 检查注解的java.time.Duration表示的时间差不小于注解指定的值，如果inclusive标志设置为 true，则允许相等。null值被认为校验通过。 |                                                              |
| @EAN                                                         | 检查注解的值是否是有效的EAN（商品用条形码）。type属性指定EAN类型，默认EAN-13。null值被认为校验通过。 | 支持CharSequence及其子类型。                                 |
| @ISBN                                                        | 检查注解的值是否是有效的ISBN（国际标准书号）。type属性指定ISBN类型，默认ISBN-13。null值被认为校验通过。 | 支持CharSequence及其子类型。                                 |
| @Length(min=, max=)                                          | 检查注解的值长度是否在最小值和最大值之间，包含两个端点值。null值被认为校验通过。 | 支持CharSequence及其子类型。                                 |
| @CodePointLength(min=, max=, normalizationStrategy=)         | 检查注解的值的代码点长度是否在最小值和最大值之间，包含两个端点值。如果设置了normalizationStrategy，则验证规范化值。null值被认为校验通过。 | 支持CharSequence及其子类型。                                 |
| @Range(min=, max=)                                           | 检查注解的值是否位于指定最小值和最大值之间，包含两个端点值。null值被认为校验通过。 | 支持BigDecimal、BigInteger，byte 、short 、int 、long 、float、double以及它们的包装类型。 |
| @UniqueElements                                              | 检查注解的集合是否仅包含唯一元素，通过equals()方法来比较元素是否相等。null值被认为校验通过。 | 支持Collection及其子类型。                                   |
| @URL(protocol=, host=, port=, regexp=, flags=)               | 根据 RFC2396 规范检查检查注解的值是否为有效的 URL。如果在注解中指定了任何可选参数：协议、主机或端口，则相应的 URL 片段必须与指定的值匹配。可选参数 regexp 和flags允许指定URL必须匹配附加的正则表达式（包括正则表达式标志）。默认情况下，此约束使用java.net.URL的构造器来验证给定字符串是否表示有效的 URL。 | 支持CharSequence及其子类型。                                 |

## 集成hibernate-validator

**Spring支持和hibernate-validator非常方便的集成，执行Bean校验的核心就是`javax.validation.Validator`，Validator需要我们引入`Bean Validation provider`的依赖，也就是具体的Bean Validation的实现，例如hibernate-validator。在运行时，Validator依赖具体的Bean Validation的实现来进行校验。**

**对于普通项目，我们需要引入`hibernate-validator`或者其他引入Bean校验的相关实现依赖，对于`Spring Boot`项目则会自动引入了相关依赖，我们无需引入！**

```xml
xml复制代码<!-- https://mvnrepository.com/artifact/org.hibernate.validator/hibernate-
validator -->
<!-- 引入hibernate-validator -->
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>6.0.18.Final</version>
</dependency>
<!-- https://mvnrepository.com/artifact/javax.el/javax.el-api -->
<!-- tomcat 7 可能需要该依赖 -->
<dependency>
    <groupId>org.glassfish</groupId>
    <artifactId>javax.el</artifactId>
    <version>3.0.1-b08</version>
</dependency>
```

**注意，hibernate-validator 7的版本的Bean validation API均从`javax.validation`换成了`jakarta.validation`，因此如果切换版本，可能需要重新导入API的类路径。**

## 配置

**如果我们想要配置能够在其他地方手动调用的Validator，则可以通过Spring提供的LocalValidatorFactoryBean来快速将Validator配置为Spring管理的Bean！**

**如果需要手动校验，那么由于LocalValidatorFactoryBean 实现了 javax.validation.validatorFactory 和 javax.validation.Validator，以及 Spring 的org.springframework.validation.Validator，因此我们可以将对其中任一接口的引用注入需要手动调用验证逻辑的bean中。**

```java
java复制代码@Bean
public LocalValidatorFactoryBean validator() {
    LocalValidatorFactoryBean localValidatorFactoryBean = new LocalValidatorFactoryBean();
    //即使不设置Provider，Spring也会自动查找Provider的实现
    //localValidatorFactoryBean.setProviderClass(HibernateValidator.class);
    return localValidatorFactoryBean;
}
```

## 使用

### 参数属性校验

**这里的配置仅仅是针对Spring MVC的控制器方法绑定的参数对象的属性进行校验，如果需要对于参数本身以及返回值进行校验，那么需要后面的介绍！**

**这里的数据校验是在数据绑定成功之后进行的，和数据绑定类似， Spring MVC 绑定参数的属性校验支持全局配置和局部配置。**

#### 全局和局部配置

**对于普通项目中Spring MVC的参数绑定校验，我们只需要通过`@EnableWebMvc`或者`<mvc:annotation-driven/>`打开MVC配置，Spring MVC会自动尝试查找类路径中引入的`ValidationProvider`的实现并将第一个找到的Provider配置全局的Validator。从而无需我们手动配置。如果只引入了`hibernate-validator`的依赖 ，那么只有一个Provider的实现——`HibernateValidator`。对于Spring Boot项目则会自动开启MVC配置，同样无需我们手动开启。**

**如果我们需要手动指定Spring MVC使用的全局Validator，那么如下配置：**

```java
java复制代码/**
 * @author lx
 */
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {
    /**
     * 提供自定义Validator验证器，而不是默认创建的验证器
     * 如果返回null，并且如果类路径中存在 JSR-303 Provider的实现，那么将会默认创建类型为
     * org.springframework.validation.beanvalidation.OptionalValidatorFactoryBean的Validator
     */
    @Override
    public Validator getValidator() {
        LocalValidatorFactoryBean localValidatorFactoryBean = new LocalValidatorFactoryBean();
        //即使不设置Provider，Spring也会自动查找类路径中的Provider的实现
        //localValidatorFactoryBean.setProviderClass(HibernateValidator.class);
        return localValidatorFactoryBean;
    }
}
```

下面的示例演示如何在 XML 中实现相同的配置：

```xml
xml复制代码<!--validator属性用于指定Validator bean，该属性不是必须的，除非想要自定义
validator-->
<!--如果未指定，那么自动查找类路径中的JSR-303 provider实现来配置一个类型为-->
<!--org.springframework.validation.beanvalidation.OptionalValidatorFactoryBean的Validator-->
<mvc:annotation-driven validator="mvcValidator"/>
```

当然，我们同样可以为某些请求在注册一个局部的Validator：

```java
java复制代码/**
 * 添加局部的Validator
 */
@InitBinder
public void initBinder(WebDataBinder dataBinder) {
    //如果使用预定义的Validator，那么需要手动调用afterPropertiesSet初始化
    //如果是使用自定义的Validator实现，那么需要supports方法支持要校验的参数类型否则抛出异常
    LocalValidatorFactoryBean optionalValidatorFactoryBean = new LocalValidatorFactoryBean();
    optionalValidatorFactoryBean.afterPropertiesSet();
    //可以添加多个Validator
    dataBinder.addValidators(optionalValidatorFactoryBean);
}
```

**如果同时添加了全局和局部的Validator，那么在校验时会形成validators链，其中全局Validator排在最前面，校验时会按照排序顺序依次调用每一个Validator进行校验，所有的Validator都通过才算通过！**

**在开启MVC配置之后，将会自动配置Spring MVC的全局Validator，我们直接进行测试！我们采用lmbok的@Data注解来省略setter、getter、toString方法，如果不会那么请手写相关方法。**

下**面是一个需要校验的实体，要求id、sex和name都不能为null，id要求为正数，sex要求为0或者1，name要求在1到是个字符之间：**

```java
java复制代码@Data
public class User1 {

    @Positive
    @NotNull
    private Long id;

    @Range(min = 0, max = 1)
    @NotNull
    private Byte sex;

    @Size(min = 1, max = 10)
    @NotBlank
    private String name;
}
```

**接下来是控制器方法，非常重要的一步就是，我们需要在对对象属性进行校验的对象参数前加上`@Validated或者@Valid注解`，`javax.validation.Valid`注解可以标记在属性、方法参数或方法返回类型上以表示对标记的对象进行级联校验，也就是校验对象内部的属性，而`org.springframework.validation.annotation.Validated`则是Spring提供的注解，它具有和@Valid一样的功能，并且还新增了可选验证组的功能，后面会讲！**

```java
java复制代码/**
 * @author lx
 */
@RestController
public class GlobalMvcValidationController {

    /**
     * 支持application/json请求的参数校验
     */
    @PostMapping("/pv1")
    @ResponseBody
    public User1 pv1(@Validated @RequestBody User1 user1) {
        System.out.println(user1);
        user1.setId(0L);
        return user1;
    }

    /**
     * 支持普通请求的参数校验
     */
    @GetMapping("/pv2")
    @ResponseBody
    public void pv2(@Valid User1 user1) {
        System.out.println(user1);
    }
}
```

### 级联校验

**在上面我们就说了@Valid支持级联校验，如果校验的对象内部的属性中同样标注了@Valid注解，那么validation engine验证引擎将会自动递归跟进！同样，级联校验的对象如果为null，那么被忽略（算作校验通过）。**

**如下实体，内部具有一个对象属性，我们采用@Valid注解进行级联校验：**

```java
java复制代码@Data
public class User2 {

    @Positive
    @NotNull
    private Long id;

    @Range(min = 0, max = 1)
    @NotNull
    private Byte sex;

    @Size(min = 1, max = 10)
    @NotBlank
    private String name;

    /**
     * 标注@Valid注解，对对象类型的属性进行级联校验
     */
    @Valid
    @NotNull
    private Address address;

    @Data
    public class Address {
        @NotBlank
        @Pattern(regexp = "\\d{6}")
        private String postcode;
        @NotBlank
        @Size(min = 10, max = 100)
        private String workAddress;
        @NotBlank
        @Size(min = 10, max = 100)
        private String homeAddress;

    }

}
```

控制器方法：

```java
java复制代码/**
 * 级联校验
 */
@PostMapping("/pv3")
@ResponseBody
public User2 pv3(@RequestBody @Valid User2 user2) {
    System.out.println(user2);
    return user2;
}
```

### 容器校验

**Java容器包括`Collection、Map、Array`等常见类型。**

**`@Valid`同样支持容器元素的内部属性校验，任何类型的容器属性可以加上@Valid 注解，这将导致校验每个元素进行级联校验（map包括key和value）。但是`hibernate validator`不推荐这么做，而是希望添加在容器的具体的泛型类型上，这样的好处是校验的目的更加明确。对于数组的校验似乎有一定的限制，比如目前测试无法校验null元素。**

**对容器元素进行校验时，任何元素不满足条件就算做校验不通过！**

如下实体：

```java
java复制代码@Data
public class User3 {

    /**
     * list至少包含两个元素
     * 元素不能为null且进行级联校验
     */
    @Size(min = 2)
    @NotNull
    @Valid
    private List<@NotNull InnerClass> user1s;

    /**
     * map不能为空
     * key的长度至少为2个字符且不是空白字符，value不能为null且进行级联校验
     */
    @NotEmpty
    private Map<@NotBlank @Size(min = 2) String, @NotNull @Valid InnerClass> stringUser1Map;

    /**
     * 数组不能为空，且对元素进行级联校验
     * 校验似乎有一定的限制，比如目前测试无法通过@NotNull校验null元素
     */
    @NotEmpty
    @Valid
    @NotNull
    private @NotNull InnerClass[] user2s;

    @Data
    public static class InnerClass {
        @NotNull
        @Min(1)
        private Long id;

        @NotBlank
        @Size(min = 5)
        private String name;
    }
}
```

一个控制器方法：

```java
java复制代码/**
 * 容器元素校验
 */
@PostMapping("/pv4")
@ResponseBody
public User3 pv4(@RequestBody @Valid User3 user3) {
    System.out.println(user3);
    return user3;
}
```

### 驱动方法校验

**在上面 “`Spring MVC参数属性校验`” 的部分中，通过MVC配置的默认校验，仅仅是针对Spring MVC的控制器方法绑定的参数对象的属性进行校验，如果还需要对于`方法参数本身`进行或者`方法的返回值`进行校验，比如校验方法参数和返回值本身不为null，或者需要对属于非控制器的方法进行同样的Bean validation校验，那么我们需要配置Spring 驱动方法校验。`Spring 驱动方法校验`的校验规则和上面的Spring MVC参数属性校验的校验规则都是一样的，只不过它的应用范围更加广泛，在普通项目中应该手动配置Spring 驱动方法校验。同样，如果是Spring Boot的web项目，那么Spring Boot为我们自动配置好了，我们无需任何配置！**

**下面，我们测试默认Spring MVC参数属性校验的局限性！**

如下`GlobalValidationController`控制器类，前两个方法需要接收String、Long类型的参数，实际上对于这种非自定义类型的参数，我们只能将校验的注解写在控制器方法的参数上，后两个方法虽然是我们自定义的User1类型的参数，但是第三个方法我们希望校验这个传递进来的User1对象本身不为null，对于第四个方法则是希望校验这个集合类型的参数本身的元素数量最少为2个等，这种情况下，校验注解同样只能写在控制器方法的参数上！

**为了让校验的适用性更加广泛，我们需要配置Spring驱动方法校验，配置的方法非常简单，只需要在Spring 容器中配置一个MethodValidationPostProcessor的bean即可：**

```java
java复制代码/**
 * @author lx
 */
@Configuration
public class SpringConfig {

    @Bean
    public MethodValidationPostProcessor validationPostProcessor() {
        return new MethodValidationPostProcessor();
    }

}
```

**如果开始了MVC配置并且开启了Spring MVC参数属性校验，那么对于控制器方法进行级联校验时将采用MVC配置中的校验器！**

**MethodValidationPostProcessor是一个方便的BeanPostProcessor实现，它委托给JSR-303 Provider对带注解的方法执行方法级校验。支持包括方法参数、参数内部的属性、方法返回值（标注可以标注在方法上）的校验，比如：**

```java
java
复制代码public @NotNull Object myValidMethod(@NotNull String arg1, @Max(10) int arg2)
```

**需要进行校验的方法所属的目标类/接口上应该标注Spring 的`@Validated`注解（指定`@Valid`无效），这样Spring才能搜索其内部的方法以进行方法校验。也可以指定@Validated验证组，默认情况下，`JSR-303` 将仅针对其默认组进行校验。**

**`MethodValidationPostProcessor的原理`很简单，在其本身初始化的时候会在内部创建一个`Validator`以及一个包含该Validator的一个Advisor通知器，由于它作为一个`BeanPostProcessor`，因此在`postProcessAfterInitialization()`方法中会判断每一个Spring管理的bean实例并且对于符合条件的bean传递其内部的`Advisor`通知器，随后创建代理bean对象并返回，实际上注入的是一个当代理对象的方法被调用的时候，将会调用Advisor通知器内部的`MethodValidationInterceptor`拦截器对目标方法进行拦截并且利用`Validator`对方法进行校验。**

**也就是说，最终还是通过`Spring AOP`的机制创建代理对象，通过在目标方法的执行前后增加了校验的逻辑来实现参数和返回值校验的，因此它与是否是控制器类无关，只要是被Spring管理的Bean都有继承成了校验的对象！**

**不过，既然是基于`Spring AOP`控制的参数校验，那么Spring AOP的限制在此同样有效：**

1. **AOP的机制导致了同一个AOP类中的需要校验的`方法互相调用`时，被调用方法的校验不会生效，因为Spring AOP的代理机制最终还是通过原始目标对象本身去调用目标方法的，这样被调用的方法就会因为是原始对象调用的而不被拦截，当然也有解决办法，那就是获取代理对象，通过代理对象去调用内层方法！**
2. **`无论是基于JDK动态代理还是CGLIB代理`，由于本身的缺陷，它们代理的方法的增强都具有限制。对于JDK的代理，目标类必须实现符合规则的接口（不是说只要是实现了接口就会使用JDK代理，具体规则在AOP源码部分有讲解），并且只能代理实现的接口的方法，而对于CGLIB的代理，目标类不能是final的，并且需要代理的方法也不能是private/final/static的。这些AOP代理的限制也是校验增强方法的限制。具体的代理方式是JDK代理优先，然后是尝试CGLIB的代理，我们在Spring AOP部分已经讲过了，目前无法手动指定。**
3. **和[@Async](https://link.juejin.cn?target=https%3A%2F%2Fblog.csdn.net%2Fweixin_43767015%2Farticle%2Fdetails%2F110135495)注解一样，Spring不能为`@Validated注解`标注的类解决setter方法和反射字段注解的`循环依赖注入（包括自己注入自己）`，将会抛出：“……This means that said other beans do not use the final version of the bean……”异常，根本因为这个AOP代理对象不是使用通用的AbstractAutoProxyCreator的方法创建的，而是使用MethodValidationPostProcessor后处理器来创建的，Spring目前没有解决这个问题。解决办法是在引入的依赖项上加一个@Lazy注解，原理就是再给它加一层AOP代理……。而其他的，Spring可以解决比如由于事物或者自定义的AOP机制创建的AOP代理的循环依赖。**

### 返回值校验

**返回值的校验注解可以标在方法上或者返回值的类型之前！另外需要注意的是，即使返回值校验不通过，此时的业务代码也肯定是被执行过了的！**

### 非控制器类

**因为Spring驱动方法校验基于Spring AOP，因此即使是非控制器类比如说service层的bean同样支持方法校验！**

**虽然controller层通常是没有接口的设计，但是如果web项目的其他层（比如service层）存在着接口-实现类的设计方式，那么有以下注意点：**

1. **参数上的约束注解应该定义在被重写的接口/父类方法上，或者即使重写的方法上的存在约束注解也应该和接口/父类方法上的约束注解完全一致，否则运行时将抛出ConstraintDeclarationException异常！**
2. **@Validated标注在实现类或者接口/父类上均可。标注在接口/父类上，表示任何实现类和子类都会开启方法校验，如果标注在某个实现类上标志只有该类会开启方法校验。**
3. **返回值的约束注解，标注在实现类或者接口/父类的方法上均可，如果都标注了注解（除非注解完全一致），那么这些注解将会全部进行校验，都满足才算满足！**

如下Service：

```java
java复制代码public interface OtherValidationService {

    void parameterValidation(@Valid @NotNull User1 user1);

    @Size(min = 5) String returnValidation();
}




@Service
@Validated
public class OtherValidationServiceImpl implements OtherValidationService {
    
    @Override
    public void parameterValidation(User1 user1) {
        System.out.println(user1);
    }

    @Override
    public @NotBlank @Size(min = 4) String returnValidation() {
        return "";
    }

}
```

控制器类：

```java
java复制代码@RestController
@Validated
public class OtherValidationController {

    @Resource
    private OtherValidationService otherValidationService;

    @GetMapping("/parameterValidation")
    public void user1() {
        otherValidationService.parameterValidation(null);
    }

    @GetMapping("/returnValidation")
    public String returnTest() {
        return otherValidationService.returnValidation();
    }
}
```

### 自定义约束

**内置的约束注解已经能够满足几乎所有的校验需要了，但是总有些特别的需求需要定义自己的校验规则，如果要创建自定义约束，那么也很简单，需要执行以下三个步骤：**

1. **创建自定义的约束注解，使用@Constraint作为元注解；**
2. **创建一个validator验证器，通常是实现javax.validation.ConstraintValidator接口，并与@Constraint关联；**
3. **定义默认错误消息；**

下面我们尝试创建自定义约束注解，为了方便，我将所有的实现都定义在一个注解中。

```java
java复制代码/**
 * 该自定义约束注解用于判断奇偶性，可以标注在方法、字段、参数、类型 上面
 * <p>
 * 基于规范，当被校验对象为null时，校验为通过
 *
 * @author lx
 */
@Target({METHOD, FIELD, PARAMETER, TYPE_USE})
@Retention(RUNTIME)
@Constraint(validatedBy = Odevity.MyConstraintValidator.class)
public @interface Odevity {

    /**
     * 在违反约束时返回创建错误消息的默认key
     */
    String message() default "{com.spring.mvc.config.Odevity.message}";

    /**
     * 允许此约束所属的规范验证组
     */
    Class<?>[] groups() default {};

    /**
     * 可以将自定义有效Payload对象分配给约束，通常未使用
     */
    Class<? extends Payload>[] payload() default {};

    /**
     * 设置校验的模式
     * ODD——奇数，EVEN——偶数
     */
    OdevityMode value();


    /**
     * Validator校验器的实现，真正的校验的逻辑
     */
    class MyConstraintValidator implements ConstraintValidator<Odevity, Long> {

        private OdevityMode odevityMode;

        /**
         * 初始化Validator校验器
         *
         * @param constraintAnnotation 当前Odevity注解实例
         */
        @Override
        public void initialize(Odevity constraintAnnotation) {
            odevityMode = constraintAnnotation.value();
        }

        /**
         * 执行校验的路基
         *
         * @param value   注解的数据的值
         * @param context 校验上下文
         * @return 是否校验通过，false不通过，true通过
         */
        @Override
        public boolean isValid(Long value, ConstraintValidatorContext context) {
            if (value == null) {
                return true;
            }
            boolean flag = value % 2 == 0;
            return flag && odevityMode == OdevityMode.EVEN ||
                    (!flag && odevityMode == OdevityMode.ODD);
        }
    }

    /**
     * 奇偶性的枚举常量
     */
    enum OdevityMode {
        /**
         * 奇数
         */
        ODD,

        /**
         * 偶数
         */
        EVEN;
    }

}
```

**@Odevity是我们自定义的一个约束注解，用于校验值是奇数还是偶数！注解之上有几个元注解：**

1. `@Target`：表示该注解可以标志的位置，包括方法、字段、参数、类型（比如返回值类型、泛型类型等等）这四个位置！
2. `@Retention`：表示在什么级别保存该注解的信息，RUNTIME的意思就是在编译时会将注解信息记录到class文件，运行时仍然保留注解，因此可以通过反射获取注解的信息。
3. `@javax.validation.Constraint`：这个注解是`Java Bean validation`提供的元注解，用来指示`@Odevity`注解是一个约束注解，并且指定用于校验使用`@Odevity`注解标注的数据的validator验证器的`Class`。如果约束注解可用于多个数据类型，则可以指定多个验证器的Class，每个校验器对应一个可验证的数据类型，对于不同的类型将会调用对应的校验器。如果两个校验器对应同一个可验证的数据类型，那么将会抛出异常。

**根据Jakarta Bean Validation API，任何自定义的约束注解，都应该包含一下三个元素：**

1. `message`：在违反约束时返回创建的错误消息，可以使用｛key｝设置默认的key，将会自动查找对应的value，如果没有对应的value，那么使用key的值作为默认错误消息。消息中可以通过{elementName}获取注解的元素值，也可以使用${validatedValue}获取注解标注的值，也可以使用其他el表达式。
2. `attribute`：允许此约束所属的规范验证组。
3. `payload`：可以将自定义有效Payload对象分配给约束，通常未使用。

**MyConstraintValidator是一个约束校验器，通常校验延器需要实现`Jakarta Bean Validation的javax.validation.ConstraintValidator接口`！通常校验器被单独定义为一个外部类，这里为了方便直接定义在注解中。**

**`ConstraintValidator`接口中具有`两个泛型参数`，`第一个参数`指定要校验的注解类型（Odevity），`第二个`指定一个要校验的数据类型，表示验证器可以处理这些类型，这里是Long类型，因此如果@Odevity标记在其它数据类型上，那么将会抛出`UnexpectedTypeException`异常：**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6d52db1a3bc34237854383efe062d366~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**校验器的实现非常简单。`initialize()`方法允许我们访问校验约束注解的属性值，我们可以将它们存储在自定义验证器的字段中，如示例中所示。** **`isValid()`方法包含实际的验证逻辑，第一个参数表示需要校验的数据的值。请注意，Jakarta Bean Validation 规范建议将 null 值视为有效值。如果 null 不是元素的有效值（视作校验通过)，则应显式使用@NotNull注解。**

**错误消息的生成依赖于`isValid()`方法返回true或者false，返回false表示没有通过校验！使用传递的第二个参数 ConstraintValidatorContext 对象，可以添加其他错误消息或完全禁用默认错误消息生成并仅定义自定义错误消息，一般不常用。**

**另外，实现了`ConstraintValidator`的类，会被自动加入到Spring容器中进行统一管理，因此我们无需手动初始化，也无须加上`@Compent或者其他组件注解`！**

**最后，我们在项目的`resources目录`下添加一个国际化的错误消息文件`ValidationMessages_zh_CN.properties`，其内部的key就是@Odevity中的message的默认值：**

```java
java复制代码# 消息中可以通过{elementName}获取注解的元素值，也可以使用${validatedValue}获取注解
标注的值，也可以使用其他el表达式
# 对 "校验值：${validatedValue}.不符合校验的要求：{value}"进行了中文unicode编码
com.spring.mvc.config.Odevity.message=\u6821\u9a8c\u503c：${validatedValue}.\u4e0d\u7b26\u5408\u6821\u9a8c\u7684\u8981\u6c42\uff1a{value}
```

**添加国际化消息文件不是必须的，因为大部分项目都没有国际化的需求，我们直接将错误消息定义为message元素的默认值即可！**

### 统一异常处理

**Spring异常处理校验如果不通过，通常会抛出三个异常，在执行统一异常处理的时候，可以将这三个异常捕获，并且提取里面的信息，封装为result返回给客户端：**

1. Spring 驱动方法校验异常：`ConstraintViolationException`。
2. Spring MVC的JSON请求参数对象内部属性校验异常：`MethodArgumentNotValidException`。
3. Spring MVC的普通请求参数对象内部属性校验异常：`BindException`。

# ViewResolver

**Spring MVC 定义了 ViewSolvsolver 和 View 接口，这些接口允许我们在Web应用中通过模型数据对视图模版进行渲染并返回给浏览器，而无需依赖特定的视图技术。**

**ViewResolver 提供逻辑视图名称到实际物理视图的映射，即将逻辑视图名解析为View视图对象，其中包含了真实物理视图的信息（比如url路径、locale国际化信息）。View 则用于在将模型数据移交给特定视图技术之前进行数据准备，随后调用对应的视图技术进行视图渲染。**

**ViewResolver的不同实现具有不同的逻辑视图名到物理视图的映射算法，常见实现的uml类图结构如下:**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0dbb3dcb944c44f2b9a6d013310cd163~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

## AbstractCachingViewResolver

**实现了ViewResolver接口的便捷抽象基类。继承了AbstractCachingViewResolver的子类将默认缓存它们已经解析了的View视图实例，这意味着无论初始View视图的检索成本有多高，视图解析都不会是性能问题，即缓存可以提高某些视图技术的性能。**

**我们可以通过将cache属性设置为false来关闭缓存。此外，如果必须在运行时刷新某个视图（例如，当修改FreeMarker模板时），则可以使用removeFromCache(String viewName，Locale loc)方法。** **默认最大缓存1024个，缓存的View的key为viewName和locale的组合：**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/364c039faaeb40c0959ed2008697ea92~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

## XmlViewResolver

**ViewResolver的一种实现，继承了AbstractCachingViewResolver，具有缓存View视图的能力。**

**XmlViewResolver支持接受XML配置文件，配置文件中存放的是自己配置的View bean，每一个View bean都有自己的id/name，XmlViewResolver将根据Controller处理器方法返回的逻辑视图名称到指定的XML配置文件中寻找对应名称的 View bean并返回，用于处理视图，这就是它的工作（将逻辑视图名解析为真正的View视图对象）原理！**

**默认配置文件是/WEB-INF/views.xml，如果不使用默认配置，那么可以在XmlViewResolver的location属性中指定它的位置。**

**XmlViewResolver指定的cachekey就是viewName：**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c2e3b8543b834ec5a8e0f6d69d502a2e~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

如下案例，一个Controller：

```java
java复制代码/**
 * @author lx
 */
@Controller
public class XmlViewResolverController {

    @RequestMapping("/xmlViewResolver")
    public ModelAndView handle() {
        ModelAndView modelAndView = new ModelAndView();
        //添加模型数据
        modelAndView.addObject("msg", "xmlViewResolver测试");
        //添加逻辑视图名
        modelAndView.setViewName("xvr");
        return modelAndView;
    }
}
```

spring-mvc-config.xml配置文件，我们手动注册了一个XmlViewResolver，并指定配置文件位置为类路径下面（resources下面）的views.xml文件！

```xml
xml复制代码<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd">
    
    <!--扫描包-->
    <context:component-scan base-package="com.spring.mvc"/>

    <!--手动配置一个XmlViewResolver-->
    <bean class="org.springframework.web.servlet.view.XmlViewResolver">
        <!--指定配置文件位置-->
        <property name="location" value="classpath:views.xml"/>
    </bean>
    
</beans>
```

views.xml，我们在其中配置一个View bean，bean的id为xvr，与Controller方法中设置的逻辑视图名一致！

```xml
xml复制代码<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--配置一个InternalResourceView类型的View bean，name或者id属性需要与逻辑视图名匹配-->
    <bean id="xvr" class="org.springframework.web.servlet.view.InternalResourceView">
        <!--配置jsp或者其他资源的url路径，请求将对转发到指定的资源-->
        <property name="url" value="/WEB-INF/jsp/xvr.jsp"/>
    </bean>
</beans>
```

/WEB-INF/jsp目录下配置一个jsp视图模版xvr.jsp：

```html
html复制代码<%@ page contentType="text/html;charset=UTF-8" %>
<html>
<head>
    <title>XmlViewResolver</title>
</head>
<body>
<h1>XmlViewResolver！</h1>
<br/>
<span style="color: #dc143c; ">${msg}</span>
<br/>
</body>
</html>
```

项目总体结构如下：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8410e199634646958c226bae5c1d73f1~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

启动项目，访问/xmlViewResolver，结果如下：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7da2874778054a63b8c175f5b5b71c70~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

## ResourceBundleViewResolver

**ViewResolver的一种实现，继承了AbstractCachingViewResolver，具有缓存View视图的能力。**

**ResourceBundleViewResolver支持接受properties配置文件，配置文件的编写和XmlViewResolver的XML配置文件的编写有很大区别。properties中key的编写就是通过viewname.(class)指定View视图的实现类，ket的前缀就是逻辑视图名，通过viewname.propertyName指定某个View视图的属性。**

**ResourceBundleViewResolver将根据Controller处理器方法返回的逻辑视图名称到指定的properties配置文件中寻找对应名称的 View bean并返回，用于处理视图，这就是它的工作（将逻辑视图名解析为真正的View视图对象）原理！**

**默认配置文件是classpath下面的views.properties，如果不使用默认配置，那么可以在ResourceBundleViewResolver的basename或者basenames属性中指定，这里只能指定配置文件前缀，，如指定的baseName是base，那么base.properties、baseabc.properties等等以base开始的属性文件都会被Spring当做ResourceBundleViewResolver解析视图的资源文件。**

**默认最大缓存1024个，缓存的View的key为viewName和locale的组合：**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6c9100e080d044119350d206c54f4203~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

如下案例，一个Controller：

```java
java复制代码/**
 * @author lx
 */
@Controller
public class ResourceBundleViewResolverController {

    @RequestMapping("/resourceBundleViewResolver")
    public ModelAndView handle() {
        ModelAndView modelAndView = new ModelAndView();
        //添加模型数据
        modelAndView.addObject("msg", "ResourceBundleViewResolver测试");
        //添加逻辑视图名
        modelAndView.setViewName("rbvr");
        return modelAndView;
    }
}
```

spring-mvc-config.xml配置文件中我们手动注册了一个ResourceBundleViewResolver，basename前缀为view。

```xml
xml复制代码<!--手动配置一个ResourceBundleViewResolver-->
<bean class="org.springframework.web.servlet.view.ResourceBundleViewResolver">
    <!--指定配置文件前缀-->
    <property name="basename" value="view"/>
</bean>
```

在resources目录下添加一个views.properties，我们在其中配置一个View， id为rbvr，与Controller方法中设置的逻辑视图名一致！

```java
java复制代码#指定视图名rbvr以及通过(class)指定View所属类型
rbvr.(class)=org.springframework.web.servlet.view.InternalResourceView
#指定某个视图名的类型的属性，这里指定rbvr视图的url属性
rbvr.url=/WEB-INF/jsp/rbvr.jsp
```

在/WEB-INF/jsp目录下配置一个jsp视图模版rbvr.jsp：

```xml
xml复制代码<%@ page contentType="text/html;charset=UTF-8" %>
<html>
<head>
    <title>ResourceBundleViewResolver</title>
</head>
<body>
<h1>ResourceBundleViewResolver！</h1>
<br/>
<span style="color: #dc143c; ">${msg}</span>
<br/>
</body>
</html>
```

启动项目，访问/xmlViewResolver，结果如下：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a01c13beb4f1423fb171eedd6da79929~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

## UrlBasedViewResolver

**ViewSolver的简单实现，继承了AbstractCachingViewResolver，具有缓存View视图的能力。**

**UrlBasedViewResolver，提供了一种通过拼接前后缀的方式来方便的获取逻辑视图对应的真实物理视图URL的功能。我们可以通过配置prefix属性指定一个URL前缀，通过suffix属性指定一个URL后缀，然后把返回的逻辑视图名称加上指定的前缀和后缀就是指定的物理视图URL了。比如prefix设置为“/WEB-INF/pages/”，suffix设置为“.jsp”，返回的逻辑视图为“index”，那么最终的物理视图URL就是“/WEB-INF/pages/index.jsp”默认情况下。prefix和suffix都是空字符串，设置为null同样被解析为空串！**

**配置UrlBasedViewResolver时，必须指定一个viewClass属性，该UrlBasedViewResolver会将所有的逻辑视图名解析为该类型的View对象。由于从逻辑视图到物理视图（路径）的映射就是拼接URL的规则，并且解析出来的viewClass指定为一种，因此我们必须要像XmlViewResolver和ResourceBundleViewResolver那样手动配置View的映射以及实现，而是由UrlBasedViewResolver自动创建！**

**UrlBasedViewResolver将尝试检查逻辑视图对应的物理资源是否确实存在。但是，如果viewClass为InternalResourceView，则通常无法确定目标资源是否真实存在。在这种情况下，UrlBasedViewResolver将始终返回一个任何给定逻辑视图名称的InternalResourceView视图对象，无论它是否真实存在，永远不会返回null。因此，应将它配置为解析器链中的最后一个视图解析器（如果存在多个解析器），以尽量防止后续因物理视图资源找不到而返回的404响应！**

**同时，可以为UrlBasedViewResolver设置viewNames属性，这是一个String数组，表示该UrlBasedViewResolver能解析的逻辑视图名，支持通配符匹配，比如\*，如果没有设置viewNames（将会是null），或者设置了但是没有匹配，那么resolveViewName将会返回null，进而由下一个ViewResolver解析。**

XmlViewResolver指定的cachekey就是viewName：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1c24bdf3d5c94c258f98ad7b25b4c74b~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

如下案例，一个Controller：

```java
java复制代码/**
 * @author lx
 */
@Controller
public class UrlBasedViewResolverController {

    @RequestMapping("/urlBasedViewResolver")
    public ModelAndView handle() {
        ModelAndView modelAndView = new ModelAndView();
        //添加模型数据
        modelAndView.addObject("msg", "UrlBasedViewResolver测试");
        //添加逻辑视图名
        modelAndView.setViewName("ubvr");
        return modelAndView;
    }
}
```

在spring-mvc-config.xml配置文件中我们手动注册一个UrlBasedViewResolver。

```xml
xml复制代码<!--手动配置一个UrlBasedViewResolver-->
<bean class="org.springframework.web.servlet.view.UrlBasedViewResolver">
    <!--指定物理视图路径前缀-->
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <!--指定物理视图路径后缀-->
    <property name="suffix" value=".jsp"/>
    <!--必须指定解析的View视图的类型-->
    <!--这里指定为InternalResourceView，那么将会将请求转发到物理视图的路径-->
    <property name="viewClass" value="org.springframework.web.servlet.view.InternalResourceView"/>
</bean>
```

在/WEB-INF/jsp目录下配置一个jsp视图模版ubvr.jsp：

```html
html复制代码<%@ page contentType="text/html;charset=UTF-8" %>
<html>
<head>
    <title>UrlBasedViewResolver</title>
</head>
<body>
<h1>UrlBasedViewResolver！</h1>
<br/>
<span style="color: #dc143c; ">${msg}</span>
<br/>
</body>
</html>
```

启动项目，访问/ urlBasedViewResolver，结果如下：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5810df08945444e9a52e155afbb1cad1~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

## InternalResourceViewResolver

**专门用于支持内部资源视图InternalResourceView（实际上，就是为了支持Servlet和JSP资源）及其子类（如 JstlView 和TilesView）的 UrlBasedViewResolver 的便捷子类。**

**相比于父类UrlBasedViewResolver，不需要指定viewClass属性，默认的解析得到的视图类型就是InternalResourceView，InternalResourceView的一个特性就是它的会把Controller处理器方法返回的model模型属性都存放到对应的request中，然后通过RequestDispatcher在服务器端把请求forword转发到目标物理资源视图URL。基于InternalResourceView的隐匿的转发机制，对于访问/WEB-INF/下面的受保护的资源来说是一种非常方便快捷且安全的方式了。**

**UrlBasedViewResolver通过设置viewClass为InternalResourceView的类型，也可以达到同样的效果！同样的，如果存在解析器链，InternalResourceViewResolver始终需要放在最后一个，因为它将尝试解析任何给定的逻辑视图名称并返回一个对应的InternalResourceView，无论对应的资源是否确实存在。**

如下案例，一个Controller：

```java
java复制代码/**
 * @author lx
 */
@Controller
public class InternalResourceViewResolverController {

    @RequestMapping("/internalResourceViewResolver")
    public ModelAndView handle() {
        ModelAndView modelAndView = new ModelAndView();
        //添加模型数据
        modelAndView.addObject("msg", "InternalResourceViewResolver测试");
        //添加逻辑视图名
        modelAndView.setViewName("irvr");
        return modelAndView;
    }
}
```

在spring-mvc-config.xml配置文件中我们手动注册一个InternalResourceViewResolver，注意此时我们应该将此前配置的UrlBasedViewResolver注释掉！

```xml
xml复制代码<!--手动配置一个InternalResourceViewResolver-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <!--指定物理视图路径前缀-->
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <!--指定物理视图路径后缀-->
    <property name="suffix" value=".jsp"/>
</bean>
```

在/WEB-INF/jsp目录下配置一个jsp视图模版irvr.jsp：

```html
html复制代码<%@ page contentType="text/html;charset=UTF-8" %>
<html>
<head>
    <title>InternalResourceViewResolver</title>
</head>
<body>
<h1>InternalResourceViewResolver！</h1>
<br/>
<span style="color: #dc143c; ">${msg}</span>
<br/>
</body>
</html>
```

启动项目，访问/internalResourceViewResolver，结果如下：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a9ca87638bf341288e0cb9ddfde40544~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

## FreeMarkerViewResolver

**专门用于支持FreeMarkerView的 UrlBasedViewResolver 的便捷子类，和InternalResourceViewResolver比较类似，FreeMarkerViewResolver默认会将逻辑视图名解析为FreeMarkerView视图。**

**FreeMarkerViewResolver将检查指定模板资源是否存在，并且仅在实际找到模板时返回非null的 View 对象。**

**虽然View仅仅是进行了数据的准备和API的调用，但是由于JSP视图的渲染和数据填充是Servlet容器和tomcat服务器天然支持和实现的，因此不必引入专门的依赖，而对于FreeMarker模版的渲染和数据填充技术则需要引入专门的FreeMarker依赖，这样FreeMarkerView才能正常工作！**

```xml
xml复制代码<!-- https://mvnrepository.com/artifact/org.freemarker/freemarker -->
<dependency>
  <groupId>org.freemarker</groupId>
  <artifactId>freemarker</artifactId>
  <version>2.3.30</version>
</dependency>
<!--用于引入FreeMarkerConfigurationFactory-->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context-support</artifactId>
  <version>5.2.8.RELEASE</version>
</dependency>
```

如下案例，一个Controller：

```java
java复制代码/**
 * @author lx
 */
@Controller
public class FreeMarkerViewResolverController {

    @RequestMapping("/freeMarkerViewResolver")
    public ModelAndView handle() {
        ModelAndView modelAndView = new ModelAndView();
        //添加模型数据
        modelAndView.addObject("msg", "FreeMarkerViewResolver测试");
        //添加逻辑视图名
        modelAndView.setViewName("fmvr");
        return modelAndView;
    }
}
```

在spring-mvc-config.xml配置文件中我们手动注册一个FreeMarkerViewResolver，并且指定配置信息：

```xml
xml复制代码<!-- 手动配置一个FreeMarkerViewResolver，专门用于解析FreeMarker视图 -->
<bean class="org.springframework.web.servlet.view.freemarker.FreeMarkerViewResolver">
    <property name="suffix" value=".ftl"/>
    <property name="contentType" value="text/html;charset=UTF-8" />
</bean>

<!--freemarkerConfig-->
<bean id="freemarkerConfig" class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">
    <!-- 配置freeMarker的模板路径 -->
    <property name="templateLoaderPath" value="/WEB-INF/ftl"/>
    <property name="defaultEncoding" value="UTF-8"/>
</bean>
```

在/WEB-INF/ftl目录下配置一个freeMarker视图模版fmvr.ftl：

```xml
xml复制代码<!DOCTYPE html>
<html lang="zn_CH">
<head>
    <title>FreeMarkerViewResolver</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body>
<h1>FreeMarkerViewResolver！</h1>
<br/>
<span style="color: #dc143c; ">${msg}</span>
<br/>
</body>
</html>
```

启动项目，访问/freeMarkerViewResolver，结果如下：

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/157bcc0bdfd047f180a1c6e996d63315~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

## ContentNegotiatingViewResolver

**ContentNegotiatingViewResolver从请求中获取媒体类型（MediaType）的方式有四种，获取顺序依次为扩展名、请求参数、Accept Header、默认值！**

**最后可以获取到多个MediaType，也就是请求支持返回多种MediaType，将返回一个MediaType类型的requestedMediaTypes集合（getMediaTypes方法）。**

### 扩展名

例如：

1. /test.xml  对应的MediaType可能解析为application/xml；
2. /test.json 对应的MediaType可能解析为application/json；
3. /test.html 对应的MediaType可能解析为text/html；

**从扩展名获取MediaType可以通过favorPathExtension属性（该属性在后续版本中可能移除）配置是否开启，默认是开启的。**

### 请求参数

例如：

1. /test?format=xml  对应的MediaType可能解析为application/xml；
2. /test?format=json  对应的MediaType可能解析为application/json；
3. /test?format=html  对应的MediaType可能解析为text/html；

**从请求参数获取MediaType可以通过favorParameter属性配置是否开启，默认是关闭的。默认的参数名为format，可通过parameterName属性配置！** **在的配置类中通过mediaTypes属性可以配置参数值和具体的MediaType的映射关系，扩展名和请求参数的方式解析时可以使用。如果开启了favorParameter，那么必须配置mediaTypes（除非配置useRegisteredExtensionsOnly=false），这么做的原因或许是为了提高隐私性，比如你可以配置参数值为“xx”，对应“text/html”。对于扩展名的方式，则无需配置mediaTypes，会自动从ServletContext.getMimeType和MediaTypeFactory中查找映射，且一般都能找到。**

### Accept Header

**通过在Accept Header可以指定MediaType，比如可以htext/jsp、text/pdf、  text/xml、text/json、或者无Accept 请求头，Accept中可以指定通过“,”指定多个MediaType。还可以指定通配符（例如text/\*），在这种情况下，其Content-Type为text/xml的View是兼容的匹配项。**

Accept Header中的MediaType字符串将会被自动解析，无需配置映射关系！

可以通过ignoreAcceptHeader属性来设置是否忽略使用Accept Header来确定请求的媒体类型MediaType，默认false，即不忽略

### 默认值

**通过defaultContentType属性配置，如果前面的方式无法找到该请求的MediaType，那么使用默认MediaType。默认为null，表示没有默认MediaType。**

### 获取匹配的View

**在获取到requestedMediaTypes和candidateViews集合之后，将会选出最匹配指定的MediaType的View并返回（通过getBestView方法）。**

1. 首先遍历candidateViews，如果某个candidateView属于SmartView类型，并且会执行重定向（即实际类型为RedirectView），那么直接返回该candidateView，方法结束。
2. 随后遍历requestedMediaTypes集合，在内部为每一个mediaType遍历candidateViews集合，如果mediaType兼容某个candidateView的getContentType方法返回的MediaType，那么直接返回该candidateView，方法结束。

### 总体流程

**ContentNegotiatingViewResolver的resolveViewName的总体流程如下，还是比较清晰的！**

![在这里插入图片描述](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f99a67df82994f338068672582eb1086~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

如果找不到合适的 View 对象，即View为null，那么：

1. 如果将 useNotAcceptableStatusCode  设为 true，那就会响应406 Not Acceptable 错误。
2. 如果设为 false（默认就是false），那就回传 null，表示交给解析器链中的下一个 View Resolver 处理。

## 默认ViewResolver

Spring 5.2.8.RELEASE版本中，默认情况下，将会注册`org.springframework.web.servlet.view.InternalResourceViewResolver`这一个视图解析器！如果指定了其他视图解析器，那么不会加载默认配置！

## 视图解析器链

**我们可以通过声明视图解析器 bean 来形成一个视图解析器链，如有必要，还可以通过设置 order 属性来指定顺序。order值越大，视图解析器在链中的位置越靠后。**

**ViewResolver的协定指定它的resolveViewName方法可以返回 null 表示当前视图解析器找不到视图，随后将会对解析器链中的下一个解析亲戚进行调用。然而，对于InternalResourceViewResolver，它将始终返回一个不为null的InternalResourceView视图对象，主要用于访问JSP视图，而确定是否真正存在JSP视图资源的唯一方法是通过RequestDispatcher进行调度。因此，我们必须始终将InternalResourceViewResolver配置为在视图解析器的总体顺序中排在最后，以尽量保证减少错误视图！**

## 视图名特殊前缀

### redirect:

**可以通过指定逻辑视图名称中的“redirect:”前缀来便捷表示需要执行重定向操作，视图名称的后半部分是重定向的 URL路径。UrlBasedViewResolver（及其子类）将其为识别为需要重定向的指令，并将逻辑视图名解析为RedirectView，最终的结果与控制器直接返回RedirectView相同。**

**一个Controller：**

```java
java复制代码/**
 * @author lx
 */
@Controller
public class RedirectController {

    @RequestMapping("/redirect1")
    public ModelAndView handle1() {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("redirect:redirect/rd.jsp");
        return modelAndView;

    }

    @RequestMapping("/redirect2")
    public View handle2() {
        return new RedirectView("redirect/rd.jsp");
    }
}
```

在/webapp/redirect目录下新建一个rd.jsp：

```xml
xml复制代码<%@ page contentType="text/html;charset=UTF-8" %>
<html>
<head>
    <title>Redirect</title>
</head>
<body>
<h1>Redirect！</h1>
</body>
</html>
```

启动后访问/redirect1和/redirect2，发现请求URL都变了：

### forward:

**除了“redirect:”前缀，还可以使用“forward:”前缀，这表示UrlBasedViewResolver及其子类会将该逻辑视图名解析为InternalResourceView，最终会执行RequestDispatcher.forward()方法实现请求转发。**

**此前缀在InternalResourceViewResolver和InternalResourceView（对于JSP）中没有用，因为默认就是请求转发，但是如果您使用另一种视图技术但仍然希望强制转发由Servlet / JSP引擎处理的资源，则该前缀很有用。**

## 配置

**除了定义bean的方式配置ViewResolver之外，我们可以通过mvc标签或者JavaConfig快速的配置需要的ViewResolver，对于常见的配置来说，mvc的配置简化了视图解析器的注册！**

**FreeMarker、Tiles、Groovy Markup、Script templates等模版同样也需要引入对应的基础视图技术，即相关基础依赖！**

基于XML标签配置式样如下，在`<mvc:view-resolvers>`中可以配置各种视图解析器！

```xml
<!--配置视图解析器-->
<mvc:view-resolvers>
    <!--配置一个InternalResourceViewResolver,主要用于解析JSP视图-->
    <!--默认前缀为"/WEB-INF/"，默认后缀为".jsp"-->
    <mvc:jsp prefix="/WEB-INF/jsp" suffix=".jsp"/>

    <!--配置一个TilesViewResolver-->
    <mvc:tiles />

    <!--配置一个FreeMarkerViewResolver-->
    <mvc:freemarker />

    <!--配置一个BeanNameViewResolver-->
    <mvc:bean-name/>

    <!--配置一个ContentNegotiatingViewResolver-->
    <mvc:content-negotiation/>

    <!--配置一个GroovyMarkupViewResolver-->
    <mvc:groovy/>

    <!--配置一个GroovyMarkupViewResolver-->
    <mvc:script-template/>
</mvc:view-resolvers>
<!--freemarker模版配置-->
<mvc:freemarker-configurer/>
```





# Web 配置

应用程序可以声明在 [特殊的 Bean 类型](https://springdoc.cn/spring/web.html#mvc-servlet-special-bean-types) 中列出的处理请求所需的基础设施 Bean。 `DispatcherServlet` 检查 `WebApplicationContext` 中的每个特殊Bean。如果没有匹配的Bean类型，它将回到 [`DispatcherServlet.properties`](https://github.com/spring-projects/spring-framework/tree/main/spring-webmvc/src/main/resources/org/springframework/web/servlet/DispatcherServlet.properties) 中所列的默认类型。

在大多数情况下，[MVC 配置](https://springdoc.cn/spring/web.html#mvc-config) 是最好的起点。它用Java或XML声明所需的Bean，并提供一个更高级别的配置回调（callback）API来定制它。

> Spring Boot依靠MVC Java配置来配置Spring MVC，并提供许多额外的便利选项。

在Servlet环境中，你可以选择以编程方式配置Servlet容器，作为一种选择，或者与 `web.xml` 文件相结合。下面的例子注册了一个 `DispatcherServlet`：

```java
public class MyWebApplicationInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext container) {
        XmlWebApplicationContext appContext = new XmlWebApplicationContext();
        appContext.setConfigLocation("/WEB-INF/spring/dispatcher-config.xml");

        ServletRegistration.Dynamic registration = container.addServlet("dispatcher", new DispatcherServlet(appContext));
        registration.setLoadOnStartup(1);
        registration.addMapping("/");
    }
}
```

`WebApplicationInitializer` 是Spring MVC提供的一个接口，它可以确保你的实现被检测到并自动用于初始化任何Servlet 3容器。`WebApplicationInitializer` 的一个抽象基类实现名为 `AbstractDispatcherServletInitializer`，通过覆盖指定 `Servlet` 映射（mapping）和 `DispatcherServlet` 配置位置的方法，使得注册 `DispatcherServlet` 更加容易。

对于使用基于Java的Spring配置的应用程序，建议这样做，如下例所示：

```java
public class MyWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return null;
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class<?>[] { MyWebConfig.class };
    }

    @Override
    protected String[] getServletMappings() {
        return new String[] { "/" };
    }
}
```

如果你使用基于XML的Spring配置，你应该直接从 `AbstractDispatcherServletInitializer` 扩展，如下例所示：

```java
public class MyWebAppInitializer extends AbstractDispatcherServletInitializer {

    // ...

    @Override
    protected Filter[] getServletFilters() {
        return new Filter[] {
            new HiddenHttpMethodFilter(), new CharacterEncodingFilter() };
    }
}
```

每个 filter 都根据其具体类型添加了一个默认名称（name），并自动映射到 `DispatcherServlet`。

`AbstractDispatcherServletInitializer` 的 `isAsyncSupported` protected 方法提供了一个单一的地方来启用对 `DispatcherServlet` 和所有映射到它的 filter 的异步支持。默认情况下，这个标志被设置为 `true`。

最后，如果你需要进一步定制 `DispatcherServlet` 本身，你可以复写 `createDispatcherServlet` 方法。

# 流程

`DispatcherServlet` 处理请求的方式如下：

+ `WebApplicationContext` 被搜索并作为一个属性（attribute）绑定在请求（request）中，controller和进程中的其他元素可以使用。它默认被绑定在 `DispatcherServlet.WEB_APPLICATION_CONTEXT_ATTRIBUTE` key 下。
+ locale 解析器被绑定到请求上，以便让流程中的元素在处理请求（渲染视图、准备数据等）时解析要使用的 locale。如果你不需要locale解析，你就不需要 locale 解析器（resolver）。
+ theme 解析器被绑定在请求上，以让诸如视图等元素决定使用哪个主题。如果你不使用主题，你可以忽略它。
+ 如果你指定了一个 multipart file 解析器，请求将被检查为 multipart file。如果发现了multipart，该请求将被包裹在一个 `MultipartHttpServletRequest` 中，以便由流程中的其他元素进一步处理。关于 multipart 处理的进一步信息，请参见 [Multipart 解析器](https://springdoc.cn/spring/web.html#mvc-multipart) 。
+ 一个适当的处理程序（handler）被搜索到。如果找到一个处理程序，与该处理程序相关的执行链（预处理程序、后处理程序和 controller）被运行，以准备渲染的模型（model）。另外，==对于有注解的 controller，响应可以被渲染（在 `HandlerAdapter` 中）而不是返回一个视图==。
+ 如果有 model 返回，视图就会被渲染。如果没有返回 model（也许是由于预处理器或后处理器拦截了请求，也许是出于安全原因），就不会渲染视图，因为该请求可能已经被满足了。

在 `WebApplicationContext` 中声明的 `HandlerExceptionResolver` Bean被用来解决请求处理过程中抛出的异常。这些异常解析器允许自定义处理异常的逻辑。更多的细节请看 [Exceptions](https://springdoc.cn/spring/web.html#mvc-exceptionhandlers)。

对于HTTP缓存支持，处理程序可以使用 `WebRequest` 的 `checkNotModified` 方法，以及《[Controller 的HTTP缓存](https://springdoc.cn/spring/web.html#mvc-caching-etag-lastmodified)》中描述的注解 controller 的进一步选项。

你可以通过在 `web.xml` 文件的 Servlet 声明中添加 Servlet 初始化参数（`init-param` 元素）来定制单个 `DispatcherServlet` 实例。下表列出了支持的参数：

| 参数                             | 说明                                                         |
| :------------------------------- | :----------------------------------------------------------- |
| `contextClass`                   | 实现 `ConfigurableWebApplicationContext` 的类，将由该Servlet实例化和本地配置。默认情况下，使用 `XmlWebApplicationContext`。 |
| `contextConfigLocation`          | 传递给上下文实例（由 `contextClass` 指定）的字符串，表示可以在哪里找到上下文。该字符串可能由多个字符串组成（使用逗号作为分隔符）以支持多个上下文。如果多个上下文位置的bean被定义了两次，那么最新的位置优先。 |
| `namespace`                      | `WebApplicationContext` 的命名空间。默认为 `[servlet-name]-servlet`。 |
| `throwExceptionIfNoHandlerFound` | 当一个请求没有找到处理程序（handler）时，是否会抛出 `NoHandlerFoundException`。然后可以用 `HandlerExceptionResolver`（例如，通过使用 `@ExceptionHandler` controller 方法）来捕获该异常，并像其他一样处理。默认情况下，这被设置为 `false`，在这种情况下，`DispatcherServlet` 将响应状态设置为404（`NOT_FOUND`）而不引发异常。请注意，如果 [default servlet handling](https://springdoc.cn/spring/web.html#mvc-default-servlet-handler) 也被配置了，未解析的请求总是被转发到 default servlet，并且永远不会出现404。 |

# 路径匹配

Servlet API将完整的请求路径作为 `requestURI` 公开，并进一步将其细分为 `contextPath`、`servletPath` 和 `pathInfo`，其值因Servlet的映射方式而异。从这些输入中，Spring MVC 需要确定用于映射处理程序（handler）的查找路径，如果适用的话，应该排除 `contextPath` 和任何 `servletMapping` 前缀。

`servletPath` 和 `pathInfo` 是经过解码的，这使得它们不可能直接与完整的 `requestURI` 进行比较以得出 `lookupPath`，这使得有必要对 `requestURI` 进行解码。然而，这也引入了自己的问题，因为路径可能包含编码的保留字符，如 `"/"` 或 `";"`，在它们被解码后又会改变路径的结构，这也会导致安全问题。此外，Servlet容器可能会在不同程度上对 `servletPath` 进行规范化处理，这使得它进一步无法对 `requestURI` 进行 `startsWith` 比较。

这就是为什么最好避免依赖基于前缀的 `servletPath` 映射类型所带来的 `servletPath`。如果 `DispatcherServlet` 被映射为带有 `"/"` 的默认 Servlet，或者没有 `"/*"` 的前缀，并且Servlet容器是4.0以上的，那么Spring MVC就能够检测到Servlet映射类型，并完全避免使用 `servletPath` 和 `pathInfo`。在3.1的Servlet容器上，假设有相同的Servlet映射类型，可以通过MVC配置中的 [路径（Path）匹配](https://springdoc.cn/spring/web.html#mvc-config-path-matching)，提供一个 `alwaysUseFullPath=true` 的 `UrlPathHelper` 来实现。

幸运的是，默认的Servlet映射 `"/"` 是一个不错的选择。然而，仍然有一个问题，即 `requestURI` 需要被解码，以便能够与 controller 映射（mapping）进行比较。这也是不可取的，因为有可能对改变路径结构的保留字符进行解码。如果这些字符不被期望，那么你可以拒绝它们（就像Spring Security HTTP 防火墙），或者你可以将 `UrlPathHelper` 配置为 `urlDecode=false`，但 controller 映射需要与编码后的路径相匹配，这可能并不总是很好。此外，有时 `DispatcherServlet` 需要与另一个Servlet共享URL空间，可能需要按前缀进行映射。

在使用 `PathPatternParser` 和解析的pattern时，上述问题得到了解决，因为它可以替代 `AntPathMatcher` 的字符串路径匹配。`PathPatternParser` 从5.3版本开始就可以在Spring MVC中使用，并且从6.0版本开始默认启用。`AntPathMatcher` 需要对查找路径进行解码或对 controller 映射进行编码，与此不同的是，经过解析的 `PathPattern` 与称为 `RequestPath` 的路径的解析表示相匹配，一次一个路径段。这允许对路径段的值进行单独解码和消毒，而不存在改变路径结构的风险。Parsed `PathPattern` 还支持使用 `servletPath` 前缀映射，只要使用Servlet路径映射，并且前缀保持简单，即没有编码的字符。关于pattern语法的细节和比较，见 [Pattern 比较](https://springdoc.cn/spring/web.html#mvc-ann-requestmapping-pattern-comparison)。

# 拦截

所有 `HandlerMapping` 的实现都支持 handler 拦截器，当你想对某些请求应用特定的功能时，这些拦截器是非常有用的—例如，检查一个 principal。拦截器必须实现 `org.springframework.web.servlet` 包中的 `HandlerInterceptor`，它有三个方法，应该可以提供足够的灵活性来进行各种预处理和后处理：

+ `preHandle(..)`: 在实际 handler 运行之前
+ `postHandle(..)`: handler 运行后
+ `afterCompletion(..)`: 在整个请求完成后

`preHandle(..)` 方法返回一个boolean值。你可以使用这个方法来中断或继续执行链的处理。当这个方法返回 `true` 时，handler 执行链继续进行。当它返回 `false` 时， `DispatcherServlet` 认为拦截器本身已经处理了请求（例如，渲染了一个适当的视图），并且不继续执行其他拦截器和执行链中的实际 handler。

关于如何配置拦截器的例子，请参见MVC配置部分的 [拦截器](https://springdoc.cn/spring/web.html#mvc-config-interceptors)。你也可以通过使用个别 `HandlerMapping` 实现的setters来直接注册它们。

`postHandle` 方法在 `@ResponseBody` 和 `ResponseEntity` 方法中用处不大，因为这些方法的响应是在 `HandlerAdapter` 中和 `postHandle` 之前写入和提交的。这意味着对响应进行任何修改都太晚了，比如添加一个额外的 header。对于这种情况，你可以实现 `ResponseBodyAdvice`，并把它声明为一个 [Controller Advice](https://springdoc.cn/spring/web.html#mvc-ann-controller-advice) Bean，或者直接在 `RequestMappingHandlerAdapter` 上配置它。

# 异常

如果在请求映射过程中发生异常或从请求处理程序（如 `@Controller`）抛出异常， `DispatcherServlet` 会委托给处理程序异常解析器（`HandlerExceptionResolver`）Bean链来解析异常并提供替代处理，这通常是一个错误响应。

下表列出了可用的 `HandlerExceptionResolver` 实现：

| `HandlerExceptionResolver`                                   | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `SimpleMappingExceptionResolver`                             | 异常类名称和错误视图名称之间的映射。对于在浏览器应用程序中渲染错误页面非常有用。 |
| [`DefaultHandlerExceptionResolver`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/web/servlet/mvc/support/DefaultHandlerExceptionResolver.html) | 解析由Spring MVC引发的异常，并将其映射到HTTP状态码。也请参见替代的 `ResponseEntityExceptionHandler` 和 [Error 响应（Response）](https://springdoc.cn/spring/web.html#mvc-ann-rest-exceptions)。 |
| `ResponseStatusExceptionResolver`                            | 解析带有 `@ResponseStatus` 注解的异常，并根据注解中的值将其映射到HTTP状态码。 |
| `ExceptionHandlerExceptionResolver`                          | 通过调用 `@Controller` 或 `@ControllerAdvice` 类中的 `@ExceptionHandler` 方法来解析异常。参见 [@ExceptionHandler methods](https://springdoc.cn/spring/web.html#mvc-ann-exceptionhandler)。 |

## 解析器链

你可以通过在Spring配置中声明多个 `HandlerExceptionResolver` Bean并根据需要设置它们的 `order` 属性来形成一个异常解析器链。`order` 属性越高，异常解析器的定位就越靠后。

`HandlerExceptionResolver` 的约定，它可以返回：

+ 一个指向错误视图的 `ModelAndView`。
+ 如果异常在解析器中被处理，则是一个空（empty）的 `ModelAndView`。
+ 如果异常仍未被解决，则为 `null`，供后续的解析器尝试，如果异常仍在最后，则允许冒泡到Servlet容器中。

[MVC 配置](https://springdoc.cn/spring/web.html#mvc-config) 自动为默认的Spring MVC异常、`@ResponseStatus` 注解的异常以及 `@ExceptionHandler` 方法的支持声明了内置解析器。你可以自定义该列表或替换它。

## 错误页面

如果一个异常仍然没有被任何 `HandlerExceptionResolver` 解析，因此，任其传播，或者如果响应状态被设置为错误状态（即4xx，5xx），Servlet容器可以在HTML中渲染一个默认的错误页面。为了定制容器的默认错误页面，你可以在 `web.xml` 中声明一个错误页面映射。下面的例子显示了如何做到这一点：

```xml
<error-page>
    <location>/error</location>
</error-page>
```

鉴于前面的例子，当一个异常冒出来或者响应有错误状态时，Servlet容器在容器内向配置的URL（例如，`/error`）进行 ERROR 调度。然后由 `DispatcherServlet` 进行处理，可能会将其映射到一个 `@Controller`，它可以被实现为返回一个带有model的错误视图名称或渲染一个JSON响应，如下例所示：

```java
@RestController
public class ErrorController {

    @RequestMapping(path = "/error")
    public Map<String, Object> handle(HttpServletRequest request) {
        Map<String, Object> map = new HashMap<>();
        map.put("status", request.getAttribute("jakarta.servlet.error.status_code"));
        map.put("reason", request.getAttribute("jakarta.servlet.error.message"));
        return map;
    }
}
```

> Servlet API并没有提供在Java中创建错误页面映射的方法。然而，你可以同时使用 `WebApplicationInitializer` 和一个最小的 `web.xml`。

# 视图

Spring MVC定义了 `ViewResolver` 和 `View` 接口，让你在浏览器中渲染模型，而不需要绑定到特定的视图技术。`ViewResolver` 提供了视图名称和实际视图之间的映射。`View` 解决了在移交给特定视图技术之前的数据准备问题。

下表提供了关于 `ViewResolver` 层次结构的更多细节：

| ViewResolver                     | 说明                                                         |
| :------------------------------- | :----------------------------------------------------------- |
| `AbstractCachingViewResolver`    | `AbstractCachingViewResolver` 的子类会缓存它们所解析的视图实例。缓存可以提高某些视图技术的性能。你可以通过将 `cache` 属性设置为 `false` 来关闭缓存。此外，如果你必须在运行时刷新某个视图（例如，当 `FreeMarker` 模板被修改时），你可以使用 `removeFromCache(String viewName, Locale loc)` 方法。 |
| `UrlBasedViewResolver`           | `ViewResolver` 接口的简单实现，无需明确的映射定义就能实现逻辑视图名称与URL的直接解析。如果你的逻辑名称与你的视图资源的名称直接匹配，而不需要任意的映射，这就很合适。 |
| `InternalResourceViewResolver`   | `UrlBasedViewResolver` 的方便子类，支持 `InternalResourceView`（实际上是Servlets和JSP）和子类，如 `JstlView`。你可以通过使用 `setViewClass(..)` 为这个解析器生成的所有视图指定视图类。请参阅 [`UrlBasedViewResolver`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/web/reactive/result/view/UrlBasedViewResolver.html) javadoc 了解详情。 |
| `FreeMarkerViewResolver`         | `UrlBasedViewResolver` 的方便子类，支持 `FreeMarkerView` 和它们的自定义子类。 |
| `ContentNegotiatingViewResolver` | `ViewResolver` 接口的实现，根据请求文件名或 `Accept` 头来解析视图。参见 [内容协商](https://springdoc.cn/spring/web.html#mvc-multiple-representations)。 |
| `BeanNameViewResolver`           | `ViewResolver` 接口的实现，它将视图名称解释为当前应用程序上下文中的bean名称。这是一个非常灵活的变体，可以根据不同的视图名称混合和匹配不同的视图类型。每个这样的 `View` 都可以被定义为Bean，例如在XML或配置类中。 |

视图解析器只是为了得到视图对象，视图对象才是真正的渲染视图，执行转发或重定向到页面。(会将模型数据全部放在请求域中)

#### 1.SpringMVC解析视图的步骤

+ ①无论方法返回什么类型，SpringMVC都会在内部将其装配为`ModelAndView`。
+ ②SpringMVC借助视图解析器`(ViewResolver)`得到视图对象(`View`)。
+ ③视图对象(`View`)真正的渲染页面。

不同的视图解析器具有不同的视图对象，不同的视图对象具有不同的功能(`InternalResourceView具有转发的功能，RedirectView具有重定向功能。`)

**对于最终究竟采取何种视图对象模型数据进行渲染，处理器并不关心，处理器工作的重点聚焦在生产模型数据的工作上，从而实现MVC的充分解耦**。

## 处理

可以通过声明一个以上的解析器Bean来实现视图解析器链，如果有必要，还可以通过设置 `order` 属性来指定排序。记住，顺序属性越高，视图解析器在链中的位置就越靠后。

`ViewResolver` 的约定，它可以返回 `null` 以表示找不到视图。然而，在JSP和 `InternalResourceViewResolver` 的情况下，弄清JSP是否存在的唯一方法是通过 `RequestDispatcher` 进行调度。因此，你必须始终将 `InternalResourceViewResolver` 配置为视图解析器整体顺序中的最后一个。

配置视图解析就像在Spring配置中添加 `ViewResolver` Bean一样简单。[MVC 配置](https://springdoc.cn/spring/web.html#mvc-config) 为 [视图（View）解析器](https://springdoc.cn/spring/web.html#mvc-config-view-resolvers) 和添加无逻辑的 [视图控制器（View Controller）](https://springdoc.cn/spring/web.html#mvc-config-view-controller) 提供了专门的配置API，这对于没有控制器逻辑的HTML模板渲染非常有用。

## 重定向

视图名称中特殊的 `redirect:` 前缀可以让你执行重定向。 `UrlBasedViewResolver`（和它的子类）将其识别为一个需要重定向的指令。视图名称的其余部分是重定向的URL。

净效果与controller返回 `RedirectView` 的效果相同，但现在controller本身可以在逻辑视图名称方面操作。逻辑视图名称（如 `redirect:/myapp/some/resource`）是相对于当前 Servlet context 重定向的，而 `redirect:https://myhost.com/some/arbitrary/path` 则是重定向到一个绝对URL。

请注意，如果一个 controller 方法被 `@ResponseStatus` 注解，该注解值优先于 `RedirectView` 设置的响应状态。

## 转发

你也可以对最终由 `UrlBasedViewResolver` 和子类解析的视图名称使用一个特殊的 `forward:` 前缀。这将创建一个 `InternalResourceView`，它做一个 `RequestDispatcher.forward()`。因此，这个前缀对 `InternalResourceViewResolver` 和 `InternalResourceView`（用于JSP）没有用处，但如果你使用另一种视图技术，但仍然想强制转发一个资源，由Servlet/JSP引擎处理，那么它就会有帮助。请注意，你也可以用链式的多个视图解析器来代替。

## 内容协商

[`ContentNegotiatingViewResolver`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/web/servlet/view/ContentNegotiatingViewResolver.html) 本身并不解析视图，而是委托给其他视图解析器，并选择与客户端请求的表示相近的视图。表示法可以从 `Accept` 头或查询参数（例如，`"/path?format=pdf"`）确定。

`ContentNegotiatingViewResolver` 通过比较请求的 `Content-Type` 和与其每个 `ViewResolvers` 相关的 `View` 所支持的媒体类型（也称为 `Content-Type`）来选择一个合适的 `View` 来处理请求。列表中第一个具有兼容的内容类型的视图将表示返回给客户端。如果 `ViewResolver` 链不能提供兼容的视图，就会查询通过 `DefaultViews` 属性指定的视图列表。后面这个选项适用于 singleton `Views`，它可以渲染当前资源的适当表示，而不考虑逻辑视图的名称。`Accept` 头可以包括通配符（例如 `text/*`），在这种情况下，`Content-Type` 为 `text/xml` 的 `View` 是一个兼容的匹配。

有关配置细节，请参见 [MVC 配置](https://springdoc.cn/spring/web.html#mvc-config) 下的 [视图（View）解析器](https://springdoc.cn/spring/web.html#mvc-config-view-resolvers) 。

# Locale

Spring架构的大多数部分都支持国际化，正如Spring Web MVC框架一样。 `DispatcherServlet` 让你通过使用客户端的 locale 自动解析消息（message）。这是由 `LocaleResolver` 对象完成的。

当一个请求进来时，`DispatcherServlet` 会寻找一个 locale 解析器，如果它找到一个，它就会尝试用它来设置locale。通过使用 `RequestContext.getLocale()` 方法，你总是可以检索到由locale解析器解析的locale。

除了自动解析 locale，你还可以在处理程序（handler）映射中附加一个拦截器（关于处理程序映射拦截器的更多信息，见 [拦截](https://springdoc.cn/spring/web.html#mvc-handlermapping-interceptor)），以便在特定情况下（例如，基于请求中的参数）改变语言。

Locale 解析器和拦截器定义在 `org.springframework.web.servlet.i18n` 包中，并以正常方式在 application context 中进行配置。以下是Spring 中 locale 解析器。

+ [时区（Time Zone）](https://springdoc.cn/spring/web.html#mvc-timezone)
+ [Header 解析器](https://springdoc.cn/spring/web.html#mvc-localeresolver-acceptheader)
+ [Cookie 解析器](https://springdoc.cn/spring/web.html#mvc-localeresolver-cookie)
+ [Session 解析器](https://springdoc.cn/spring/web.html#mvc-localeresolver-session)
+ [Locale 拦截器](https://springdoc.cn/spring/web.html#mvc-localeresolver-interceptor)

## 时区

除了获得客户端的locale之外，知道它的时区也常常是有用的。 `LocaleContextResolver` 接口提供了对 `LocaleResolver` 的扩展，让解析器提供一个更丰富的 `LocaleContext`，其中可能包括时区信息。

当可用时，用户的 `TimeZone` 可以通过使用 `RequestContext.getTimeZone()` 方法获得。时区信息会被任何与Spring的 `ConversionService` 注册的 Date/Time `Converter` 和格 `Formatter` 对象自动使用。

## Header 解析器

这个 locale resolver 检查由客户端（例如，一个Web浏览器）发送的请求中的 `accept-language` 头。通常，这个头字段包含了客户的操作系统的 locale。请注意，这个解析器不支持时区信息。

## Cookie 解析器

这个locale解析器检查客户端上可能存在的 `Cookie`，看是否指定了 `Locale` 或 `TimeZone`。如果有，它就会使用指定的细节。通过使用这个locale 解析器的属性，你可以指定 cookie name 以及 maximum age。下面的例子定义了一个 `CookieLocaleResolver`：

```xml
<bean id="localeResolver" class="org.springframework.web.servlet.i18n.CookieLocaleResolver">

    <property name="cookieName" value="clientlanguage"/>

    <!-- in seconds. If set to -1, the cookie is not persisted (deleted when browser shuts down) -->
    <property name="cookieMaxAge" value="100000"/>

</bean>
```

下表描述了 `CookieLocaleResolver` 的属性：

| 属性           | 默认                 | 说明                                                         |
| :------------- | :------------------- | :----------------------------------------------------------- |
| `cookieName`   | 类名 + LOCALE        | cookie 名                                                    |
| `cookieMaxAge` | Servlet 容器的默认值 | 一个cookie在客户端上持续存在的最长时间。如果指定为 `-1`，cookie将不会被持久化。它只在客户端关闭浏览器之前可用。 |
| `cookiePath`   | /                    | 将cookie的可见性限制在你网站的某个部分。当 `cookiePath` 被指定时，cookie只对该路径和它下面的路径可见。 |

## Session 解析器

`SessionLocaleResolver` 让你从可能与用户请求有关的会话中检索 `Locale` 和 `TimeZone`。与 `CookieLocaleResolver` 不同的是，这个策略将本地选择的 locale 设置存储在 Servlet 容器的 `HttpSession` 中。因此，这些设置在每个会话中都是临时的，因此在每个会话结束时都会丢失。

注意，与外部会话管理机制（如Spring Session 项目）没有直接关系。这个 `SessionLocaleResolver` 针对当前的 `HttpServletRequest` 评估并修改相应的 `HttpSession` 属性。

## Loacle 拦截器

你可以通过将 `LocaleChangeInterceptor` 添加到 `HandlerMapping` 定义中的一个来启用更改 locale。它检测请求中的一个参数，并相应地改变locale，在调度器的应用上下文中调用 `LocaleResolver` 的 `setLocale` 方法。下一个例子显示，对所有包含名为 `siteLanguage` 的参数的 `*.view` 资源的调用现在会改变 locale。因此，例如，对URL的请求， `https://www.sf.net/home.view?siteLanguage=nl`，将网站语言改为荷兰语。下面的例子显示了如何拦截 locale：

```xml
<bean id="localeChangeInterceptor"
        class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor">
    <property name="paramName" value="siteLanguage"/>
</bean>

<bean id="localeResolver"
        class="org.springframework.web.servlet.i18n.CookieLocaleResolver"/>

<bean id="urlMapping"
        class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
    <property name="interceptors">
        <list>
            <ref bean="localeChangeInterceptor"/>
        </list>
    </property>
    <property name="mappings">
        <value>/**/*.view=someController</value>
    </property>
</bean>
```

# 主题

你可以应用Spring Web MVC框架的主题来设置你的应用程序的整体外观和感觉，从而增强用户体验。主题是一个静态资源的集合，通常是样式表和图片，它影响应用程序的视觉风格。

> 从6.0版本开始，对主题的支持已经被废弃，转而使用CSS，并且在服务器端没有任何特殊支持。

# Multipart 解析器

来自 `org.springframework.web.multipart` 包的 `MultipartResolver` 是一个解析包括文件上传在内的 multipart 请求的策略。有一个基于容器的 `StandardServletMultipartResolver` 实现，用于Servlet multipart 请求解析。请注意，基于Apache Commons FileUpload的过时的 `CommonsMultipartResolver` 已经不可用了，因为Spring Framework 6.0有新的Servlet 5.0+基线。

为了启用 multipart 处理，你需要在 `DispatcherServlet` 的Spring配置中声明一个 `MultipartResolver` Bean，名称为 `multipartResolver`。 `DispatcherServlet` 会检测到它并将其应用于传入的请求。当收到一个内容类型为 `multipart/form-data` 的POST时，解析器会将内容包裹在当前的 `HttpServletRequest` 中，作为一个 `MultipartHttpServletRequest` 来提供对解析文件的访问，此外还将 part 作为 request parameter 公开。

##### Servlet Multipart 解析

Servlet multipart 解析需要通过Servlet容器配置来启用。要做到这一点：

+ 在Java中，在Servlet注册上设置一个 `MultipartConfigElement`。
+ 在 `web.xml` 中，在servlet声明中添加一个 `"<multipart-config>"` 部分。

下面的例子显示了如何在Servlet注册上设置一个 `MultipartConfigElement`：

```java
public class AppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

    // ...

    @Override
    protected void customizeRegistration(ServletRegistration.Dynamic registration) {

        // Optionally also set maxFileSize, maxRequestSize, fileSizeThreshold
        registration.setMultipartConfig(new MultipartConfigElement("/tmp"));
    }

}
```

一旦Servlet的 multipart configuration 到位，你可以添加一个名为 `multipartResolver` 的 `StandardServletMultipartResolver` 类型的bean。

> 这个解析器变体按原样使用你的Servlet容器的 multipart 解析器，可能会使应用程序暴露在容器实现的差异中。默认情况下，它将尝试用任何 HTTP 方法解析任何 `multipart/` content type，但这可能不受所有 Servlet 容器的支持。请参阅 [`StandardServletMultipartResolver`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/web/multipart/support/StandardServletMultipartResolver.html) javadoc 了解详情和配置选项。

# 日志

Spring MVC的DEBUG级日志被设计成紧凑、简约和人性化的。它专注于反复有用的高价值信息，而不是只有在调试特定问题时才有用的其他信息。

TRACE级别的日志通常遵循与DEBUG相同的原则（例如，也不应该是火烧眉毛），但可以用于调试任何问题。此外，一些日志信息在TRACE与DEBUG下可能会显示不同的细节水平。

好的日志来自于使用日志的经验。如果你发现任何不符合既定目标的地方，请让我们知道。

## 敏感数据

DEBUG 和 TRACE 日志可能会记录敏感信息。这就是为什么请求参数和header信息在默认情况下是被屏蔽的，它们的完整记录必须通过 `DispatcherServlet` 上的 `enableLoggingRequestDetails` 属性明确启用。

下面的例子显示了如何通过使用Java配置来做到这一点：

```java
public class MyInitializer
        extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return ... ;
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return ... ;
    }

    @Override
    protected String[] getServletMappings() {
        return ... ;
    }

    @Override
    protected void customizeRegistration(ServletRegistration.Dynamic registration) {
        registration.setInitParameter("enableLoggingRequestDetails", "true");
    }

}
```

# 过滤器

## Form Data

浏览器只能通过 HTTP GET 或 HTTP POST 提交表单数据（form data），但非浏览器客户端也可以使用HTTP PUT、PATCH和DELETE。Servlet API要求 `ServletRequest.getParameter*()` 方法只支持HTTP POST的表单字段访问。

`spring-web` 模块提供了 `FormContentFilter` 来拦截内容类型为 `application/x-www-form-urlencoded` 的 HTTP PUT、PATCH 和 DELETE 请求，从 request body 中读取 form data，并包裹 `ServletRequest`，使 form data 通过 `ServletRequest.getParameter*()` 系列方法可用。

## Forwarded Header

求通过代理（如负载均衡器）时，host、port和scheme可能会发生变化，这使得从客户角度创建指向正确host、port和scheme的链接成为一种挑战。

[RFC 7239](https://tools.ietf.org/html/rfc7239) 定义了 `Forwarded` HTTP头，代理可以用它来提供关于原始请求的信息。还有其他非标准的头，包括 `X-Forwarded-Host`、`X-Forwarded-Port`、`X-Forwarded-Proto`、`X-Forwarded-SSL` 和 `X-Forwarded-Prefix`。

`ForwardedHeaderFilter` 是一个Servlet过滤器，它修改了请求，以便a）根据 `Forwarded` 头信息改变host、port和scheme，以及b）删除这些头信息以消除进一步的影响。这个过滤器依赖于对请求的包装，因此它必须排在其他过滤器的前面，比如 `RequestContextFilter`，它应该与修改后的请求而不是原始请求一起工作。

对 forwarded 头有安全方面的考虑，因为应用程序无法知道这些头是由代理添加的，还是由恶意的客户端添加的。这就是为什么在信任边界的代理应该被配置为删除来自外部的不被信任的 `Forwarded` 头信息。你也可以将 `ForwardedHeaderFilter` 配置为 `removeOnly=true`，在这种情况下，它将删除但不使用这些头信息。

为了支持 [异步请求](https://springdoc.cn/spring/web.html#mvc-ann-async) 和 error dispatch（调度），这个过滤器应该与 `DispatcherType.ASYNC` 和 `DispatcherType.ERROR` 进行映射。如果使用Spring框架的 `AbstractAnnotationConfigDispatcherServletInitializer`（见 [Servlet 配置](https://springdoc.cn/spring/web.html#mvc-container-config)），所有过滤器都会自动注册为所有的 dispatch 类型。然而，如果通过 `web.xml` 或在Spring Boot中通过 `FilterRegistrationBean` 注册过滤器，请确保除了 `DispatcherType.ASYNC` 和 `DispatcherType.ERROR` 之外，还要包括 `DispatcherType.REQUEST`。

## Shallow ETag

`ShallowEtagHeaderFilter` 通过缓存写入响应的内容并计算出MD5哈希值来创建一个 “shallow” ETag。客户端下次发送时，它做同样的事情，但它也将计算的值与 `If-None-Match` 请求头进行比较，如果两者相等，则返回304（NOT_MODIFIED）。

这种策略可以节省网络带宽，但不能节省CPU，因为必须为每个请求计算出完整的响应。前面描述的控制器层面的其他策略，可以避免计算的发生。参见 [HTTP 缓存](https://springdoc.cn/spring/web.html#mvc-caching)。

这个过滤器有一个 `writeWeakETag` 参数，可以配置该过滤器写入类似以下的 weak ETag： `W/"02a2d595e6ed9a0b24f027f2b63b134d6"`（如 [RFC 7232 Section 2.3](https://tools.ietf.org/html/rfc7232#section-2.3) 中所定义）。

为了支持 [异步请求](https://springdoc.cn/spring/web.html#mvc-ann-async)，这个过滤器必须用 `DispatcherType.ASYNC` 来映射，这样过滤器就可以延迟并成功地生成ETag到最后一个异步dispatch的结束。如果使用Spring框架的 `AbstractAnnotationConfigDispatcherServletInitializer`（见 [Servlet 配置](https://springdoc.cn/spring/web.html#mvc-container-config)），所有过滤器都会自动注册为所有的dispatch类型。然而，如果通过 `web.xml` 或Spring Boot中的 `FilterRegistrationBean` 注册过滤器，请确保包含 `DispatcherType.ASYNC`。

## CORS

Spring MVC通过控制器（controller）上的注解为CORS配置提供了细粒度的支持。然而，当与Spring Security一起使用时，我们建议依靠内置的 `CorsFilter`，它的 order 必须在 Spring Security 的过滤器链之前。

# 注解 Contoller

Spring MVC提供了一个基于注解的编程模型，其中 `@Controller` 和 `@RestController` 组件使用注解来表达请求映射、请求输入、异常处理等内容。注解的控制器具有灵活的方法签名，不需要继承基类，也不需要实现特定的接口。下面的例子显示了一个由注解定义的控制器（Controller）：

```java
@Controller
public class HelloController {

    @GetMapping("/hello")
    public String handle(Model model) {
        model.addAttribute("message", "Hello World!");
        return "index";
    }
}
```

在前面的例子中，该方法接受一个 `Model`，并返回一个 `String` 的视图名称，但也存在许多其他选项，在本章后面会有解释。

## 声明

你可以通过在Servlet的 `WebApplicationContext` 中使用标准的Spring Bean定义来定义 controller Bean。`@Controller` stereotype 允许自动检测，与Spring对检测 classpath 中的 `@Component` 类并为其自动注册bean定义的一般支持一致。它也是注解类的一个 stereotype，表明它是一个 web 组件。

为了实现对这种 `@Controller` Bean的自动检测，你可以在你的Java配置中添加组件扫描，如下例所示：

```xml
@Configuration
@ComponentScan("org.example.web")
public class WebConfig {

    // ...
}
```

下面的例子显示了前述例子的XML配置等效：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="org.example.web"/>

    <!-- ... -->

</beans>
```

`@RestController` 是一个 [组成的注解](https://springdoc.cn/spring/core.html#beans-meta-annotations)，它本身是由 `@Controller` 和 `@ResponseBody` 组成的元注解，表示一个controller的每个方法都继承了类型级的 `@ResponseBody` 注解，因此，直接写入响应体，而不是用HTML模板进行视图解析和渲染。

### AOP 代理

在某些情况下，你可能需要在运行时用AOP代理来装饰一个controller。一个例子是，如果你选择在 controller 上直接使用 `@Transactional` 注解。在这种情况下，特别是对于controller，我们建议使用基于类的代理，这种注解会自动出现在直接出现在 controller 上。

如果 controller 实现了一个接口，并且需要AOP代理，你可能需要明确配置基于类的代理。例如，对于 `@EnableTransactionManagement`，你可以改为 `@EnableTransactionManagement(proxyTargetClass = true)`，而对于 `<tx:annotation-driven/>`，你可以改为 `<tx:annotation-driven proxy-target-class="true"/>`。

> 请记住，从6.0版本开始，通过接口代理，Spring MVC不再仅仅基于接口上的类型级 `@RequestMapping` 注解来检测 controller。请启用基于类的代理，否则接口也必须有一个 `@Controller` 注解。

## 请求映射

你可以使用 `@RequestMapping` 注解来映射请求到controller方法。它有各种属性，可以通过URL、HTTP方法、请求参数、header和媒体类型（meida type）进行匹配。你可以在类的层面上使用它来表达共享映射，也可以在方法的层面上使用它来缩小到一个特定的端点映射。

还有HTTP方法特定的 `@RequestMapping` 的快捷方式变体：

+ `@GetMapping`
+ `@PostMapping`
+ `@PutMapping`
+ `@DeleteMapping`
+ `@PatchMapping`

捷径是 [自定义注解](https://springdoc.cn/spring/web.html#mvc-ann-requestmapping-composed)，因为可以说，大多数 controller 方法应该被映射到一个特定的HTTP方法，而不是使用 `@RequestMapping`，默认情况下，它匹配所有的HTTP方法。在类的层面上仍然需要一个 `@RequestMapping` 来表达共享映射。

下面的例子有类和方法层面的映射：

```xml
@RestController
@RequestMapping("/persons")
class PersonController {

    @GetMapping("/{id}")
    public Person getPerson(@PathVariable Long id) {
        // ...
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public void add(@RequestBody Person person) {
        // ...
    }
}
```

### URI pattern

`@RequestMapping` 方法可以使用 URL pattern 进行映射。有两种选择：

+ `PathPattern` — 一个与URL路径相匹配的预解析pattern，也被预解析为 `PathContainer`。这个解析方案是为web使用而设计的，它有效地处理了编码和路径参数，并有效地进行了匹配。
+ `AntPathMatcher` — 匹配字符串 pattern 和字符串路径。这也是Spring配置中用来选择classpath、文件系统和其他位置资源的原始解析方案。它的效率较低，而且字符串路径输入对于有效处理URL的编码和其他问题是一个挑战。

`PathPattern` 是Web应用程序的推荐解析方案，也是Spring WebFlux的唯一选择。它从5.3版本开始在Spring MVC中启用，从6.0版本开始默认启用。有关路径匹配选项的定制，请参见 [MVC config](https://springdoc.cn/spring/web.html#mvc-config-path-matching)。

`PathPattern` 支持与 `AntPathMatcher` 相同的 pattern 语法。此外，它还支持捕获 pattern，例如 `{*spring}`，用于匹配路径末端的0个或多个路径段。`PathPattern` 还限制了 `**` 在匹配多个路径段时的使用，即只允许在 pattern 的末端使用。这就消除了在为一个给定的请求选择最佳匹配 pattern 时的许多模糊情况。完整的 pattern 语法请参考 [`PathPattern`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/web/util/pattern/PathPattern.html) 和 [`AntPathMatcher`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/util/AntPathMatcher.html)。

一些示例 pattern：

+ `"/resources/ima?e.png"` - 匹配路径段中的一个字符
+ `"/resources/*.png"` - 匹配一个路径段中的零个或多个字符
+ `"/resources/**"` - 匹配多个路径段
+ `"/projects/{project}/versions"` - 匹配一个路径段并将其作为一个变量捕获
+ `"/projects/{project:[a-z]+}/versions"` - 匹配并捕获一个带有正则的变量

捕获的URI变量可以用 `@PathVariable` 访问。比如说：

```java
@GetMapping("/owners/{ownerId}/pets/{petId}")
public Pet findPet(@PathVariable Long ownerId, @PathVariable Long petId) {
    // ...
}
```

你可以在类和方法层面上声明URI变量，如下例所示：

```java
@Controller
@RequestMapping("/owners/{ownerId}")
public class OwnerController {

    @GetMapping("/pets/{petId}")
    public Pet findPet(@PathVariable Long ownerId, @PathVariable Long petId) {
        // ...
    }
}
```

URI变量会自动转换为适当的类型，否则会引发 `TypeMismatchException`。简单的类型（`int`、`long`、`Date` 等）是默认支持的，你可以注册对任何其他数据类型的支持。参见 [类型转换](https://springdoc.cn/spring/web.html#mvc-ann-typeconversion) 和 [`DataBinder`](https://springdoc.cn/spring/web.html#mvc-ann-initbinder)。

你可以明确地命名URI变量（例如，`@PathVariable("customId")`），但是如果名称相同，并且你的代码是用 `-parameters` 编译器标志编译的，你可以不考虑这个细节。

语法 `{varName:regex}` 用正则表达式声明一个URI变量，其语法为 `{varName:regex}`。例如，给定 URL `"/spring-web-3.0.5.jar"`，以下方法可以提取名称、版本和文件扩展名：

```java
@GetMapping("/{name:[a-z-]+}-{version:\\d\\.\\d\\.\\d}{ext:\\.[a-z]+}")
public void handle(@PathVariable String name, @PathVariable String version, @PathVariable String ext) {
    // ...
}
```

URI路径模式也可以有嵌入的 `${…}` 占位符，在启动时通过使用 `PropertySourcesPlaceholderConfigurer` 针对本地、系统、环境和其他属性源进行解析。例如，你可以使用它来根据一些外部配置对基本URL进行参数化。

### Pattern

当多个 pattern 匹配一个URL时，必须选择最佳匹配。这是根据是否启用使用解析的 `PathPattern` 来完成的，取决于是否启用使用：

+ [`PathPattern.SPECIFICITY_COMPARATOR`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/web/util/pattern/PathPattern.html#SPECIFICITY_COMPARATOR)
+ [`AntPathMatcher.getPatternComparator(String path)`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/util/AntPathMatcher.html#getPatternComparator-java.lang.String-)

两者都有助于对 pattern 进行分类，更具体的 pattern 在上面。如果一个 pattern 的URI变量（计为1）、单通配符（计为1）和双通配符（计为2）的数量较少，那么它的具体内容就较少。在得分相同的情况下，选择较长的 pattern 。在分数和长度相同的情况下，选择URI变量多于通配符的 pattern。

默认的映射模式（`/**`）被排除在评分之外，并且总是排在最后。另外，前缀模式（如 `/public/**`）被认为比其他没有双通配符的模式更不具体。

如需了解全部细节，请按照上述链接查看 pattern 比较器（Comparators）。

### 后缀匹配

从5.3版本开始，默认情况下Spring MVC不再执行 `.*` 后缀模式匹配，即映射到 `/person` 的 controller 也隐含地映射到 `/person.*`。因此，路径扩展不再用于解释响应所要求的内容类型—例如，`/person.pdf`，`/person.xml` 等等。

当浏览器曾经发送难以一致解释的 `Accept` 头时，以这种方式使用文件扩展名是必要的。目前，这已不再是必要的，使用 `Accept` 头应该是首选。

随着时间的推移，文件名扩展名的使用已被证明在很多方面存在问题。当与URI变量、路径参数和URI编码的使用相叠加时，它可能会引起歧义。推理基于URL的授权和安全（详见下一节）也变得更加困难。

要在5.3之前的版本中完全禁止使用路径扩展，请设置如下：

+ `useSuffixPatternMatching(false)`, 见 [PathMatchConfigurer](https://springdoc.cn/spring/web.html#mvc-config-path-matching)
+ `favorPathExtension(false)`, 见 [ContentNegotiationConfigurer](https://springdoc.cn/spring/web.html#mvc-config-content-negotiation)

除了通过 `"Accept"` 头之外，有一种方法可以请求内容类型，仍然是有用的，例如在浏览器中输入URL的时候。路径扩展的一个安全选择是使用查询参数策略。如果你必须使用文件扩展，考虑通过 [ContentNegotiationConfigurer](https://springdoc.cn/spring/web.html#mvc-config-content-negotiation) 的 `mediaTypes` 属性将它们限制在明确注册的扩展列表中。

#### RFD

反射式文件下载（RFD）攻击与XSS类似，它依赖于请求输入（例如，查询参数和URI变量）被反射到响应中。然而，RFD攻击不是在HTML中插入JavaScript，而是依靠浏览器切换到执行下载，并在以后双击时将响应视为可执行脚本。

在Spring MVC中，`@ResponseBody` 和 `ResponseEntity` 方法存在风险，因为它们可以呈现不同的内容类型，客户可以通过URL路径扩展请求这些内容。禁用后缀模式匹配和使用路径扩展进行内容协商降低了风险，但并不足以防止RFD攻击。

为了防止RFD攻击，在渲染响应体之前，Spring MVC添加了 `Content-Disposition:inline;filename=f.txt` 头，以建议一个固定的安全下载文件。只有当URL路径包含一个既不允许安全也没有明确注册的内容协商的文件扩展时，才会这样做。然而，当URL被直接输入到浏览器时，它有可能产生副作用。

许多常见的路径扩展被默认允许为安全的。具有自定义 `HttpMessageConverter` 实现的应用程序可以为内容协商明确地注册文件扩展名，以避免为这些扩展名添加 `Content-Disposition` 头。参见 [Content Type](https://springdoc.cn/spring/web.html#mvc-config-content-negotiation)。

参见 [CVE-2015-5211](https://pivotal.io/security/cve-2015-5211) ，了解与RFD相关的其他建议。

### 媒体类型

**消费类型**

你可以根据请求的 `Content-Type` 来缩小请求映射的范围，如下例所示：

```java
@PostMapping(path = "/pets", consumes = "application/json") 
public void addPet(@RequestBody Pet pet) {
    // ...
}
```

> 使用 `consumes` 属性来缩小 content type 的映射。

`consumes` 属性也支持否定表达式—例如，`!text/plain` 意味着除 `text/plain` 以外的任何 content type。

你可以在类的层次上声明一个共享的 `consumes` 属性。然而，与其他大多数请求映射属性不同的是，当在类级别使用时，方法级别的 `consumes` 属性会覆盖而不是扩展类级别的声明。

> `MediaType` 为常用的媒体类型提供常量，例如 `APPLICATION_JSON_VALUE` 和 `APPLICATION_XML_VALUE`。

**生产类型**

你可以根据 `Accept` 请求头和 controller 方法产生的 content type 列表来缩小请求映射的范围，如下例所示：

```java
@GetMapping(path = "/pets/{petId}", produces = "application/json") 
@ResponseBody
public Pet getPet(@PathVariable String petId) {
    // ...
}
```

> 使用 `produces` 属性，通过 content type 缩小映射范围。

media type 可以指定一个字符集。支持否定的表达式—例如，`!text/plain` 意味着除 "text/plain" 之外的任何 content type。

你可以在类的层次上声明一个共享的 `produces` 属性。然而，与其他大多数请求映射属性不同的是，当在类级使用时，方法级 `produces` 属性覆盖而不是扩展类级声明。

### 参数

你可以根据请求参数（request parameter）的条件来缩小请求映射的范围。你可以测试是否存在一个请求参数（`myParam`），是否没有一个（`!myParam`），或者一个特定的值（`myParam=myValue`）。下面的例子显示了如何测试一个特定的值：

```java
@GetMapping(path = "/pets/{petId}", params = "myParam=myValue") 
public void findPet(@PathVariable String petId) {
    // ...
}
```

你也可以对请求头条件使用同样的方法，如下例所示：

```java
@GetMapping(path = "/pets/{petId}", headers = "myHeader=myValue") 
public void findPet(@PathVariable String petId) {
    // ...
}
```

> 你可以用 `headers` 条件匹配 `Content-Type` 和 `Accept`，但最好使用 [consumes](https://springdoc.cn/spring/web.html#mvc-ann-requestmapping-consumes) 和 [produces](https://springdoc.cn/spring/web.html#mvc-ann-requestmapping-produces)。

### Header

`@GetMapping`（和 `@RequestMapping(method=HttpMethod.GET)`）支持HTTP HEAD 透明地进行请求映射。controller 方法不需要改变。在 `jakarta.servlet.http.HttpServlet` 中应用了一个响应包装器（response wrapper），确保 `Content-Length` 头被设置为写入的字节数（不需要实际写入到响应中）。

`@GetMapping`（和 `@RequestMapping(method=HttpMethod.GET)`）被隐含地映射到并支持HTTP HEAD。一个HTTP HEAD请求的处理就像HTTP GET一样，除了不写正文，而是计算字节数并设置 `Content-Length` 头。

默认情况下，HTTP OPTIONS 是通过将 `Allow` 响应头设置为所有 `@RequestMapping` 方法中列出的具有匹配 URL pattern 的HTTP方法列表来处理。

对于没有HTTP方法声明的 `@RequestMapping`，`Allow` 头被设置为 `GET,HEAD,POST,PUT,PATCH,DELETE,OPTIONS`。controller 方法应该总是声明支持的HTTP方法（例如，通过使用HTTP方法的特定变体： `@GetMapping`, `@PostMapping`, 和其他）。

你可以显式地将 `@RequestMapping` 方法映射到 HTTP HEAD 和 HTTP OPTIONS，但在普通情况下没有必要这样做。

### 自定义注解

Spring MVC支持在请求映射中使用 [组合注解](https://springdoc.cn/spring/core.html#beans-meta-annotations)。这些注解本身是用 `@RequestMapping` 进行元注解的，并组成了一个子集（或全部）`@RequestMapping` 属性的重新声明，其目的更加狭窄，更加具体。

`@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, 和 `@PatchMapping` 是组成注解的例子。之所以提供这些注解是因为，大多数 controller 方法应该被映射到特定的HTTP方法，而不是使用 `@RequestMapping`，因为默认情况下，它与所有的HTTP方法相匹配。如果你需要一个组成注解的例子，看看这些注解是如何声明的。

Spring MVC 还支持带有自定义请求匹配逻辑的自定义请求映射（request mapping）属性。这是一个更高级的选项，需要子类化 `RequestMappingHandlerMapping` 并重写 `getCustomMethodCondition` 方法，你可以检查自定义属性并返回你自己的 `RequestCondition`。

### 明确注册

你可以以编程方式注册 handler method，你可以将其用于动态注册或用于高级情况，例如在不同的URL下同一 handler 的不同实例。下面的例子注册了一个 handler method：

```java
@Configuration
public class MyConfig {

    @Autowired
  // 注入目标 handler 和 controller 的 handler 映射（mapping）。
    public void setHandlerMapping(RequestMappingHandlerMapping mapping, UserHandler handler) 
            throws NoSuchMethodException {

      //准备请求映射的元数据。
        RequestMappingInfo info = RequestMappingInfo
                .paths("/user/{id}").methods(RequestMethod.GET).build(); 

      //获取 handler method。
        Method method = UserHandler.class.getMethod("getUser", Long.class); 

      //添加注册。
        mapping.registerMapping(info, handler, method); 
    }
}
```

## 处理器方法

`@RequestMapping` 处理器方法有一个灵活的签名，可以从一系列支持的 controller 方法参数和返回值中选择。

### 方法参数

下表描述了支持的 controller 方法参数。任何参数都不支持 Reactive 类型。

JDK 8的 `java.util.Optional` 支持作为方法参数与具有 `required` 属性的注解（例如，`@RequestParam`、`@RequestHeader` 和其他）结合使用，并等同于 `required=false`。

| Controller 方法参数                                          | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `WebRequest`, `NativeWebRequest`                             | 对 request parameter 以及 request 和 session 属性的通用访问，无需直接使用Servlet API。 |
| `jakarta.servlet.ServletRequest`, `jakarta.servlet.ServletResponse` | 选择任何特定的 request 或 response 类型—例如 `ServletRequest`、`HttpServletRequest`，或 Spring 的 `MultipartRequest`、`MultipartHttpServletRequest`。 |
| `jakarta.servlet.http.HttpSession`                           | 执行一个 session 的存在。因此，这样的参数永远不会是 `null` 的。请注意， session 访问不是线程安全的。如果允许多个请求同时访问一个 session，请考虑将 `RequestMappingHandlerAdapter` 实例的 `synchronizeOnSession` 标志设置为 `true`。 |
| `jakarta.servlet.http.PushBuilder`                           | Servlet 4.0 push builder API，用于编程式 HTTP/2 资源推送。请注意，根据Servlet规范，如果客户端不支持该HTTP/2功能，则注入的 `PushBuilder` 实例可以为空。 |
| `java.security.Principal`                                    | 目前认证的用户—如果知道的话，可能是一个特定的 `Principal` 实现类。请注意，这个参数不会被急切地解析，如果它被注解了，以便允许自定义解析器在通过 `HttpServletRequest#getUserPrincipal` 的默认解析之前将其解析。例如，Spring Security `Authentication` 实现了 `Principal`，并将通过 `HttpServletRequest#getUserPrincipal` 被注入，除非它也被 `@AuthenticationPrincipal` 注解，在这种情况下，它被自定义的Spring Security resolver 通过 `Authentication#getPrincipal` 解析。 |
| `HttpMethod`                                                 | 请求的HTTP方法。                                             |
| `java.util.Locale`                                           | 当前的请求 locale，由最具体的 `LocaleResolver` 决定（实际上是配置的 `LocaleResolver` 或 `LocaleContextResolver`）。 |
| `java.util.TimeZone` + `java.time.ZoneId`                    | 与当前请求相关的时区，由 `LocaleContextResolver` 决定。      |
| `java.io.InputStream`, `java.io.Reader`                      | 用于访问Servlet API所暴露的原始请求体。                      |
| `java.io.OutputStream`, `java.io.Writer`                     | 用于访问Servlet API所暴露的原始响应体。                      |
| `@PathVariable`                                              | 用于访问URI模板变量。见 [URI pattern](https://springdoc.cn/spring/web.html#mvc-ann-requestmapping-uri-templates)。 |
| `@MatrixVariable`                                            | 用于访问URI路径段中的name-value对。参见 [Matrix Variables](https://springdoc.cn/spring/web.html#mvc-ann-matrix-variables)。 |
| `@RequestParam`                                              | 用于访问Servlet请求参数，包括 multipart 文件。参数值被转换为声明的方法参数类型。参见 [`@RequestParam`](https://springdoc.cn/spring/web.html#mvc-ann-requestparam) 以及 [Multipart](https://springdoc.cn/spring/web.html#mvc-multipart-forms)。注意，对于简单的参数值，使用 `@RequestParam` 是可选的。参见本表末尾的 "任何其他参数"。 |
| `@RequestHeader`                                             | 用于访问请求 header。header 值被转换为声明的方法参数类型。参见 [`@RequestHeader`](https://springdoc.cn/spring/web.html#mvc-ann-requestheader)。 |
| `@CookieValue`                                               | 用于访问cookie。Cookie值被转换为声明的方法参数类型。参见 [`@CookieValue`](https://springdoc.cn/spring/web.html#mvc-ann-cookievalue)。 |
| `@RequestBody`                                               | 用于访问 HTTP request body。通过使用 `HttpMessageConverter` 实现， Body 内容被转换为声明的方法参数类型。参见 [`@RequestBody`](https://springdoc.cn/spring/web.html#mvc-ann-requestbody)。 |
| `HttpEntity<B>`                                              | 用于访问请求header和body。body 用一个 `HttpMessageConverter` 来转换。参见 [HttpEntity](https://springdoc.cn/spring/web.html#mvc-ann-httpentity)。 |
| `@RequestPart`                                               | 对于访问 `multipart/form-data` 请求中的一个part，用 `HttpMessageConverter` 来转换该 part 的 body。参见 [Multipart](https://springdoc.cn/spring/web.html#mvc-multipart-forms)。 |
| `java.util.Map`, `org.springframework.ui.Model`, `org.springframework.ui.ModelMap` | 用于访问HTML controller 中使用的model，并作为视图渲染的一部分暴露给模板。 |
| `RedirectAttributes`                                         | 指定在重定向情况下使用的属性（即追加到查询字符串中），以及临时存储到重定向后的请求中的 Flash 属性。参见 [Redirect Attributes](https://springdoc.cn/spring/web.html#mvc-redirecting-passing-data) 和 [Flash Attributes](https://springdoc.cn/spring/web.html#mvc-flash-attributes)。 |
| `@ModelAttribute`                                            | 用于访问模型中的一个现有属性（如果不存在则实例化），并应用数据绑定和验证。参见 [`@ModelAttribute`](https://springdoc.cn/spring/web.html#mvc-ann-modelattrib-method-args) 以及 [Model](https://springdoc.cn/spring/web.html#mvc-ann-modelattrib-methods) 和[`DataBinder`](https://springdoc.cn/spring/web.html#mvc-ann-initbinder)。注意，使用 `@ModelAttribute` 是可选的（例如，设置其属性）。参见本表末尾的 "任何其他参数"。 |
| `Errors`, `BindingResult`                                    | 用于访问来自命令对象（即 `@ModelAttribute` 参数）的验证和数据绑定的错误或来自 `@RequestBody` 或 `@RequestPart` 参数的验证的错误。你必须在验证过的方法参数之后立即声明一个 `Errors`，或 `BindingResult` 参数。 |
| `SessionStatus` + class-level `@SessionAttributes`           | 用于标记表单处理完成，这将触发对通过类级 `@SessionAttributes` 注解声明的 session attributes 的清理。更多细节见 [`@SessionAttributes`](https://springdoc.cn/spring/web.html#mvc-ann-sessionattributes) 。 |
| `UriComponentsBuilder`                                       | 用于准备一个相对于当前请求的host、port、scheme、context path 和Servlet 映射的字面部分的URL。参见 [URI 链接](https://springdoc.cn/spring/web.html#mvc-uri-building)。 |
| `@SessionAttribute`                                          | 用于访问任何 session attribute，与因类级 `@SessionAttributes` 声明而存储在 session 中的模型属性不同。更多细节见 [`@SessionAttribute`](https://springdoc.cn/spring/web.html#mvc-ann-sessionattribute)。 |
| `@RequestAttribute`                                          | 用于访问 request attribute。更多细节见 [`@RequestAttribute`](https://springdoc.cn/spring/web.html#mvc-ann-requestattrib)。 |
| 任何其他参数                                                 | 如果一个方法参数没有与本表中的任何先前值相匹配，并且它是一个简单的类型（由 [BeanUtils#isSimpleProperty](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/beans/BeanUtils.html#isSimpleProperty-java.lang.Class-) 决定），它被解析为 `@RequestParam` 。否则，它将被解析为 `@ModelAttribute`。 |

### 返回值

下表描述了支持的 controller 方法返回值。所有的返回值都支持响应式类型。

| Controller 方法返回值                                        | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `@ResponseBody`                                              | 返回值通过 `HttpMessageConverter` 实现进行转换并写入响应。参见 [`@ResponseBody`](https://springdoc.cn/spring/web.html#mvc-ann-responsebody)。 |
| `HttpEntity<B>`, `ResponseEntity<B>`                         | 指定完整的响应（包括HTTP header 和 body）的返回值要通过 `HttpMessageConverter` 实现转换并写入响应。参见 [ResponseEntity](https://springdoc.cn/spring/web.html#mvc-ann-responseentity)。 |
| `HttpHeaders`                                                | 用于返回一个有 header 件而无 body 的响应。                   |
| `ErrorResponse`                                              | 要呈现一个 RFC 7807 错误响应，并在 body 中提供详细信息，请参见 [Error 响应（Response）](https://springdoc.cn/spring/web.html#mvc-ann-rest-exceptions)。 |
| `ProblemDetail`                                              | 要呈现一个RFC 7807错误响应，并在 body 中提供详细信息，请参见 [Error 响应（Response）](https://springdoc.cn/spring/web.html#mvc-ann-rest-exceptions)。 |
| `String`                                                     | 用 `ViewResolver` 实现解析的视图名称，并与隐式 model 一起使用—通过命令对象和 `@ModelAttribute` 方法确定。handler method 也可以通过声明一个 `Model` 参数来以编程方式丰富模型（见 [明确的注册](https://springdoc.cn/spring/web.html#mvc-ann-requestmapping-registration)）。 |
| `View`                                                       | 一个与隐式 model 一起用于渲染的 `View` 实例 - 通过命令对象和 `@ModelAttribute` 方法确定。handler method 也可以通过声明一个 `Model` 参数来以编程方式丰富模型（见 [明确的注册](https://springdoc.cn/spring/web.html#mvc-ann-requestmapping-registration)）。 |
| `java.util.Map`, `org.springframework.ui.Model`              | 要添加到隐式 model 的属性，视图名称通过 `RequestToViewNameTranslator` 隐式确定。 |
| `@ModelAttribute`                                            | 一个要添加到 model 中的属性，视图名称通过 `RequestToViewNameTranslator` 隐式确定。注意，`@ModelAttribute` 是可选的。参见本表末尾的 "任何其他返回值"。 |
| `ModelAndView` 对象                                          | 要使用的 view 和 model attribute ，以及可选的响应状态（response status）。 |
| `void`                                                       | 如果一个方法有一个 `ServletResponse`，一个 `OutputStream` 参数，或者一个 `@ResponseStatus` 注解，那么这个方法的返回类型为 `void` （或者返回值为 `null` ），被认为是完全处理了响应。如果 controller 进行了积极的 `ETag` 或 `lastModified` 时间戳检查，也是如此（详见 [Controller](https://springdoc.cn/spring/web.html#mvc-caching-etag-lastmodified)）。如果以上都不是 true， `void` 返回类型也可以表示 REST controller 的 “no response body” 或 HTML controller 的默认视图名称选择。 |
| `DeferredResult<V>`                                          | 从任何线程异步产生前述的任何返回值—例如，作为一些事件或回调的结果。参见 [异步请求](https://springdoc.cn/spring/web.html#mvc-ann-async) 和 [`DeferredResult`](https://springdoc.cn/spring/web.html#mvc-ann-async-deferredresult)。 |
| `Callable<V>`                                                | 在Spring MVC管理的线程中异步产生上述任何返回值。参见 [异步请求](https://springdoc.cn/spring/web.html#mvc-ann-async) 和 [`Callable`](https://springdoc.cn/spring/web.html#mvc-ann-async-callable)。 |
| `ListenableFuture<V>`, `java.util.concurrent.CompletionStage<V>`, `java.util.concurrent.CompletableFuture<V>` | 替代 `DeferredResult`，作为一种便利（例如，当一个底层服务返回其中之一）。 |
| `ResponseBodyEmitter`, `SseEmitter`                          | 用 `HttpMessageConverter` 实现异步发射一个对象流（stream），以写入响应中。也支持作为 `ResponseEntity` 的 body。参见 [异步请求](https://springdoc.cn/spring/web.html#mvc-ann-async) 和 [HTTP Streaming](https://springdoc.cn/spring/web.html#mvc-ann-async-http-streaming)。 |
| `StreamingResponseBody`                                      | 异步写到响应的 `OutputStream` 中。也支持作为 `ResponseEntity` 的 body。参见 [异步请求](https://springdoc.cn/spring/web.html#mvc-ann-async) 和 [HTTP Streaming](https://springdoc.cn/spring/web.html#mvc-ann-async-http-streaming)。 |
| 通过 `ReactiveAdapterRegistry` 注册的 Reactor 和其他 reactive 类型 | 一个单值类型，例如 `Mono`，相当于返回 `DeferredResult`。一个多值类型，例如 `Flux`，可以根据请求的 media type 被视为一个流，例如 "text/event-stream"，"application/json+stream"，或者以其他方式被收集到一个 List 并作为一个单一值呈现。参见 [异步请求](https://springdoc.cn/spring/web.html#mvc-ann-async) 和 [响应式（Reactive）类型](https://springdoc.cn/spring/web.html#mvc-ann-async-reactive-types).。 |
| 任何其他返回值                                               | 如果一个返回值以任何其他方式保持未被解析，它将被视为一个 model attribute ，除非它是 [BeanUtils#isSimpleProperty](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/beans/BeanUtils.html#isSimpleProperty-java.lang.Class-) 确定的简单类型，在这种情况下，它保持未被解析。 |

### 类型转换

一些注解的 controller 方法参数表示基于 `String` 的请求输入（如 `@RequestParam`、`@RequestHeader`、`@PathVariable`、`@MatrixVariable` 和 `@CookieValue`），如果参数被声明为 `String` 以外的东西，可能需要类型转换。

对于这种情况，类型转换会根据配置的 converter 自动应用。默认情况下，支持简单类型（`int`、`long`、`Date` 和其他）。你可以通过 `WebDataBinder`（见 [`DataBinder`](https://springdoc.cn/spring/web.html#mvc-ann-initbinder)）或通过向 `FormattingConversionService` 注册 `Formatters` 来定制类型转换。参见 [Spring字段格式化](https://springdoc.cn/spring/core.html#format)。

类型转换中的一个实际问题是如何处理一个空（empty）的String源值。如果类型转换的结果是 `null` 的，这样的值会被视为缺失。对于 `Long`、`UUID` 和其他目标类型，也可能是这种情况。如果你想允许 `null` 被注入，要么在参数注解上使用 `required` 标志，要么将参数声明为 `@Nullable`。

> 从5.3开始，即使在类型转换之后，非null参数也会被强制执行。如果你的handler method也打算接受一个null值，要么将你的参数声明为 `@Nullable`，要么在相应的 `@RequestParam` 等注解中将其标记为 `required=false`。这是一个最佳实践，也是在5.3升级中遇到的回归问题的推荐解决方案。
>
> 另外，你可以特别处理例如在需要 `@PathVariable` 的情况下产生的 `MissingPathVariableException`。转换后的null值将被视为空（empty）的原始值，所以相应的 `Missing…Exception` 变体将被抛出。

## Model

你可以使用 `@ModelAttribute` 注解：

+ 在 `@RequestMapping` 方法中的一个 [方法参数](https://springdoc.cn/spring/web.html#mvc-ann-modelattrib-method-args) 上，从 model 中创建或访问一个对象，并通过 `WebDataBinder` 将其绑定到请求中。
+ 作为 `@Controller` 或 `@ControllerAdvice` 类中的方法级注解，有助于在任何 `@RequestMapping` 方法调用之前初始化 model。
+ 在一个 `@RequestMapping` 方法上，标记其返回值是一个 model attribute。

本节讨论 `@ModelAttribute` 方法 - 前面列表中的第二项。一个 controller 可以有任意数量的 `@ModelAttribute` 方法。所有这些方法都在同一个 controller 的 `@RequestMapping` 方法之前被调用。一个 `@ModelAttribute` 方法也可以通过 `@ControllerAdvice` 在各个 controller 之间共享。更多细节请看 [Controller Advice](https://springdoc.cn/spring/web.html#mvc-ann-controller-advice) 部分。

`@ModelAttribute` 方法有灵活的方法签名。它们支持许多与 `@RequestMapping` 方法相同的参数，除了 `@ModelAttribute` 本身或与请求体（request body）有关的任何东西。

下面的例子显示了一个 `@ModelAttribute` 方法：

```java
@ModelAttribute
public void populateModel(@RequestParam String number, Model model) {
    model.addAttribute(accountRepository.findAccount(number));
    // add more ...
}
```

下面的例子只增加了一个属性：

```java
@ModelAttribute
public Account addAccount(@RequestParam String number) {
    return accountRepository.findAccount(number);
}
```

> 当没有明确指定名称时，会根据 `Object` 类型选择一个默认的名称，正如 [`Conventions`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/core/Conventions.html) 的javadoc所解释的那样。你总是可以通过使用重载的 `addAttribute` 方法或通过 `@ModelAttribute` 上的 `name` 属性（用于返回值）来指定一个明确的名字。

你也可以使用 `@ModelAttribute` 作为 `@RequestMapping` 方法的方法级注解，在这种情况下，`@RequestMapping` 方法的返回值被解释为 model attribute。这通常是不需要的，因为它是HTML controller 中的默认行为，除非返回值是一个 `String`，否则会被解释为视图名称。`@ModelAttribute` 也可以自定义 model attribute 名称，正如下面的例子所示：

```java
@GetMapping("/accounts/{id}")
@ModelAttribute("myAccount")
public Account handle() {
    // ...
    return account;
}
```

## BataBinder

`@Controller` 或 `@ControllerAdvice` 类可以有 `@InitBinder` 方法来初始化 `WebDataBinder` 的实例，而这些实例又可以：

+ 将请求参数（即表单或查询数据）绑定到一个 model 对象。
+ 将基于字符串的请求值（如请求参数、路径变量、header、cookies等）转换为 controller 方法参数的目标类型。
+ 在渲染HTML表单时，将 model 对象值格式化为 `String` 值。

`@InitBinder` 方法可以注册 controller 特定的 `java.beans.PropertyEditor` 或Spring `Converter` 和 `Formatter` 组件。此外，你可以使用 [MVC配置](https://springdoc.cn/spring/web.html#mvc-config-conversion) 在全局共享的 `FormattingConversionService` 中注册 `Converter` 和 `Formatter` 类型。

`@InitBinder` 方法支持许多与 `@RequestMapping` 方法相同的参数，除了 `@ModelAttribute`（命令对象）参数。通常，它们被声明时有一个 `WebDataBinder` 参数（用于注册）和一个 `void` 返回值。下面的列表显示了一个例子：

```java
@Controller
public class FormController {

    @InitBinder 
    public void initBinder(WebDataBinder binder) {
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
        dateFormat.setLenient(false);
        binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat, false));
    }

    // ...
}
```

> 定义一个 `@InitBinder` 方法。

另外，当你通过共享的 `FormattingConversionService` 使用基于 `Formatter` 的设置时，你可以重新使用同样的方法并注册 controller 特定的 `Formatter` 实现，正如下面的例子所示：

```java
@Controller
public class FormController {

    @InitBinder 
    protected void initBinder(WebDataBinder binder) {
        binder.addCustomFormatter(new DateFormatter("yyyy-MM-dd"));
    }

    // ...
}
```

> 在自定义 formatter 上定义一个 `@InitBinder` 方法。

### Model 设计

[见 Reactive 技术栈中的等效内容](https://springdoc.cn/spring/web-reactive.html#webflux-ann-initbinder-model-design)

在Web应用程序的上下文（context）中，数据绑定涉及HTTP请求参数（即表单数据或查询参数）与模型对象及其嵌套对象中的属性的绑定。

只有遵循 [JavaBeans命名约定](https://www.oracle.com/java/technologies/javase/javabeans-spec.html) 的 `public` 属性才会被暴露出来用于数据绑定—例如， `firstName` 属性的 `public String getFirstName()` 和 `public void setFirstName(String)` 方法。

> 模型（model）对象及其嵌套对象图，有时也被称为命令对象、表单支持对象或POJO（Plain Old Java Object）。

默认情况下，Spring允许绑定到模型对象图中的所有 public 属性。这意味着你需要仔细考虑模型有哪些 public 属性，因为客户端可以针对任何 public 属性路径，甚至是一些在特定用例中不被期望的目标。

例如，给定一个HTTP表单数据端点，一个恶意的客户端可以为模型对象图中存在的属性提供数值，但不是浏览器中呈现的HTML表单的一部分。这可能导致模型对象及其任何嵌套对象上的数据被设置，而这些数据并不期望被更新。

推荐的方法是使用一个专门的模型对象，只公开与表单提交相关的属性。例如，在一个改变用户电子邮件地址的表单中，模型对象应该声明一组最小的属性，如下面的 `ChangeEmailForm`。

```java
public class ChangeEmailForm {

    private String oldEmailAddress;
    private String newEmailAddress;

    public void setOldEmailAddress(String oldEmailAddress) {
        this.oldEmailAddress = oldEmailAddress;
    }

    public String getOldEmailAddress() {
        return this.oldEmailAddress;
    }

    public void setNewEmailAddress(String newEmailAddress) {
        this.newEmailAddress = newEmailAddress;
    }

    public String getNewEmailAddress() {
        return this.newEmailAddress;
    }

}
```

如果你不能或不想为每个数据绑定用例使用一个专门的模型对象，你必须限制允许数据绑定的属性。理想情况下，你可以通过 `WebDataBinder` 的 `setAllowedFields()` 方法注册允许的字段 pattern 来实现这一点。

例如，为了在你的应用程序中注册允许的字段 pattern ，你可以在 `@Controller` 或 `@ControllerAdvice` 组件中实现一个 `@InitBinder` 方法，如下所示：

```java
@Controller
public class ChangeEmailController {

    @InitBinder
    void initBinder(WebDataBinder binder) {
        binder.setAllowedFields("oldEmailAddress", "newEmailAddress");
    }

    // @RequestMapping methods, etc.

}
```

除了注册允许的 pattern 外，还可以通过 `DataBinder` 及其子类中的 `setDisallowedFields()` 方法注册不允许的字段 pattern 。然而，请注意，"allow list" 比 "deny list" 更安全。因此，`setAllowedFields()` 应该比 `setDisallowedFields()` 更受欢迎。

请注意，与允许的字段 pattern 匹配是区分大小写的；而与不允许的字段 pattern 匹配是不区分大小写的。此外，与不允许的 pattern 相匹配的字段将不被接受，即使它也恰好与允许列表中的 pattern 相匹配。

> 在为数据绑定目的直接暴露你的 domain 模型时，正确配置允许和不允许的字段模式是极其重要的。否则，这将是一个很大的安全风险。此外，强烈建议你不要使用你的 domain 模型的类型，如JPA或Hibernate实体作为数据绑定场景的 model 对象。

## 异常

`@Controller` 和 [@ControllerAdvice](https://springdoc.cn/spring/web.html#mvc-ann-controller-advice) 类可以有 `@ExceptionHandler` 方法来处理来自 controller 方法的异常，如下例所示：

```java
@Controller
public class SimpleController {

    // ...

    @ExceptionHandler
    public ResponseEntity<String> handle(IOException ex) {
        // ...
    }
}
```

异常可以与正在传播的顶级异常相匹配（例如，直接抛出的 `IOException`），也可以与包装异常中的嵌套原因相匹配（例如，在 `IllegalStateException` 中包装的 `IOException`）。从5.3开始，这可以在任意的原因层次上进行匹配，而以前只考虑直接 cause。

对于匹配的异常类型，最好将目标异常作为方法参数来声明，如前面的例子所示。当多个异常方法匹配时，一般来说，根异常匹配比原因异常匹配更有优势。更具体地说，`ExceptionDepthComparator` 是用来根据异常与被抛出的异常类型的深度来排序的。

另外，注解声明可以缩小要匹配的异常类型，就像下面的例子所示：

```java
@ExceptionHandler({FileSystemException.class, RemoteException.class})
public ResponseEntity<String> handle(IOException ex) {
    // ...
}
```

你甚至可以使用一个特定的异常类型的列表，并使用一个非常通用的参数签名，正如下面的例子所示：

```java
@ExceptionHandler({FileSystemException.class, RemoteException.class})
public ResponseEntity<String> handle(Exception ex) {
    // ...
}
```

>   root 和 cause 异常匹配之间的区别可能令人惊讶。在前面显示的 `IOException` 变体中，该方法通常是以实际的 `FileSystemException` 或 `RemoteException` 实例作为参数来调用的，因为它们都是从 `IOException` 扩展而来。然而，如果任何这样的匹配异常是在一个本身是 `IOException` 的包装异常中传播的，那么传入的异常实例就是那个包装异常。在 `handle(Exception)` 变体中，行为更加简单。在封装的情况下，这总是和封装的异常一起被调用，在这种情况下，实际匹配的异常要通过 `ex.getCause()` 找到。只有当 `FileSystemException` 或 `RemoteException` 作为顶层异常被抛出时，传入的异常才是实际的 `FileSystemException` 或 `RemoteException` 实例。

我们通常建议你在参数签名中尽可能地具体化，以减少root和cause异常类型之间不匹配的可能性。可以考虑将一个多匹配方法分解成单独的 `@ExceptionHandler` 方法，每个方法通过其签名匹配一个特定的异常类型。

在一个多 `@ControllerAdvice` 的安排中，我们建议在一个 `@ControllerAdvice` 上声明你的主要 root 异常映射，并以相应的顺序进行优先排序。虽然 root 异常匹配比 cause 更重要，但这是在特定的 controller 或 `@ControllerAdvice` 类的方法中定义的。这意味着优先级较高的 `@ControllerAdvice` Bean上的原因匹配比优先级较低的 `@ControllerAdvice` Bean上的任何匹配（例如 root）都要优先。

最后但并非最不重要的是，`@ExceptionHandler` 方法的实现可以选择退出处理一个给定的异常实例，以其原始形式重新抛出它。这在你只对 root 级别的匹配感兴趣或者对不能静态确定的特定上下文的匹配感兴趣的情况下是很有用的。重新抛出的异常会在剩余的解析链中传播，就像给定的 `@ExceptionHandler` 方法在一开始就没有匹配一样。

在 Spring MVC 中对 `@ExceptionHandler` 方法的支持是建立在 `DispatcherServlet` 级别的 [HandlerExceptionResolver](https://springdoc.cn/spring/web.html#mvc-exceptionhandlers) 机制上。

### 方法参数

`@ExceptionHandler` 方法支持以下参数：

| Method 参数                                                  | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| 异常类型                                                     | 用于访问抛出的异常。                                         |
| `HandlerMethod`                                              | 用于访问引发异常的 controller 方法。                         |
| `WebRequest`, `NativeWebRequest`                             | 对 request parameter 以及 request 和 session 属性的通用访问，无需直接使用Servlet API。 |
| `jakarta.servlet.ServletRequest`, `jakarta.servlet.ServletResponse` | 选择任何特定的请求或响应类型（例如，`ServletRequest` 或 `HttpServletRequest` 或 Spring 的 `MultipartRequest` 或 `MultipartHttpServletRequest`）。 |
| `jakarta.servlet.http.HttpSession`                           | 执行一个 session 的存在。因此，这样的参数永远不会是 `null` 的。 请注意，session 访问不是线程安全的。如果允许多个请求同时访问一个session，请考虑将 `RequestMappingHandlerAdapter` 实例的 `synchronizeOnSession` 标志设置为 `true`。 |
| `java.security.Principal`                                    | 目前认证的用户—如果知道的话，可能是一个特定的 `Principal` 实现类。 |
| `HttpMethod`                                                 | 请求的HTTP方法。                                             |
| `java.util.Locale`                                           | 当前的请求 locale，由最具体的 `LocaleResolver` 决定—实际上就是配置的 `LocaleResolver` 或 `LocaleContextResolver`。 |
| `java.util.TimeZone`, `java.time.ZoneId`                     | 与当前请求相关的时区，由 `LocaleContextResolver` 决定。      |
| `java.io.OutputStream`, `java.io.Writer`                     | 用于访问原始 response body，如Servlet API所暴露的。          |
| `java.util.Map`, `org.springframework.ui.Model`, `org.springframework.ui.ModelMap` | 用于访问 model 的错误响应。总是空的。                        |
| `RedirectAttributes`                                         | 指定在重定向情况下使用的属性--（即附加到查询字符串中），以及临时存储到重定向后的请求中的Flash attributes。参见 [Redirect Attributes](https://springdoc.cn/spring/web.html#mvc-redirecting-passing-data) 和 [Flash Attributes](https://springdoc.cn/spring/web.html#mvc-flash-attributes)。 |
| `@SessionAttribute`                                          | 用于访问任何 session attribute，与由于类级 `@SessionAttributes` 声明而存储在 session 中的 model attributes 不同。更多细节见 [`@SessionAttribute`](https://springdoc.cn/spring/web.html#mvc-ann-sessionattribute)。 |
| `@RequestAttribute`                                          | 用于访问 request attributes。更多细节见 [`@RequestAttribute`](https://springdoc.cn/spring/web.html#mvc-ann-requestattrib)。 |

### 返回值

`@ExceptionHandler` 方法支持以下返回值：

| 返回值                                          | 说明                                                         |
| :---------------------------------------------- | :----------------------------------------------------------- |
| `@ResponseBody`                                 | 返回值通过 `HttpMessageConverter` 实例进行转换并写入响应中。参见 [`@ResponseBody`](https://springdoc.cn/spring/web.html#mvc-ann-responsebody)。 |
| `HttpEntity<B>`, `ResponseEntity<B>`            | 返回值指定通过 `HttpMessageConverter` 实例转换完整的响应（包括HTTP header 和 body）并写入响应中。参见 [ResponseEntity](https://springdoc.cn/spring/web.html#mvc-ann-responseentity)。 |
| `ErrorResponse`                                 | 要渲染一个RFC 7807错误响应，并在 body 中提供详细信息，请参见 [Error 响应（Response）](https://springdoc.cn/spring/web.html#mvc-ann-rest-exceptions)。 |
| `ProblemDetail`                                 | 要渲染一个RFC 7807错误响应，并在 body 中提供详细信息，请参见 [Error 响应（Response）](https://springdoc.cn/spring/web.html#mvc-ann-rest-exceptions)。 |
| `String`                                        | 用 `ViewResolver` 实现解析的视图名称，并与隐式 model 一起使用—通过命令对象和 `@ModelAttribute` 方法确定。handler method 也可以通过声明一个 `Model` 参数（如前所述）以编程方式丰富 model。 |
| `View`                                          | 一个与隐含 model 一起用于渲染的 `View` 实例—通过命令对象和 `@ModelAttribute` 方法确定。handler method 也可以通过声明一个 `Model` 参数（前面已经描述过），以编程方式丰富 model。 |
| `java.util.Map`, `org.springframework.ui.Model` | 要添加到隐式 model 中的属性，视图名称通过 `RequestToViewNameTranslator` 隐式确定。 |
| `@ModelAttribute`                               | 一个将被添加到 model 中的属性，其视图名称通过 `RequestToViewNameTranslator` 隐式确定。注意，`@ModelAttribute` 是可选的。见本表末尾的 "任何其他返回值"。 |
| `ModelAndView` object                           | 要使用的 view 和 model attributes，以及可选的响应状态。      |
| `void`                                          | 如果一个方法有一个 `ServletResponse` 或者 `OutputStream` 参数，或者 `@ResponseStatus` 注解，那么这个方法的返回类型为 `void` （或者返回值为 `null`），被认为是完全处理了响应。如果 controller 进行了积极的 `ETag` 或 `lastModified` 时间戳检查，也是如此（详见 [Controller](https://springdoc.cn/spring/web.html#mvc-caching-etag-lastmodified)）。如果以上都不是 true， `void` 的返回类型也可以表示REST控制器的 “no response body” 或HTML controller 的默认视图名称选择。 |
| 任何其他返回值                                  | 如果一个返回值没有与上述任何一个匹配，并且不是一个简单的类型（由 [BeanUtils#isSimpleProperty](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/beans/BeanUtils.html#isSimpleProperty-java.lang.Class-) 确定），默认情况下，它被当作一个要添加到模型中的 model attribute。如果它是一个简单的类型，它仍然是未解析的。 |

## Controller Advice

学名 **Controller增强器**。

`@ExceptionHandler`, `@InitBinder`, 和 `@ModelAttribute` 方法只适用于它们被声明的 `@Controller` 类，或类的层次结构。如果它们被声明在 `@ControllerAdvice` 或 `@RestControllerAdvice` 类中，那么它们就适用于任何controller。此外，从5.3版本开始，`@ControllerAdvice` 中的 `@ExceptionHandler` 方法可以用来处理来自任何 `@Controller` 或任何其他 controller 的异常。

`@ControllerAdvice` 与 `@Component` 进行了元注解，因此可以通过 [组件扫描](https://springdoc.cn/spring/core.html#beans-java-instantiating-container-scan) 注册为Spring Bean。`@RestControllerAdvice` 与 `@ControllerAdvice` 和 `@ResponseBody` 进行了元标注，这意味着 `@ExceptionHandler` 方法的返回值将通过响应体 message 转换呈现，而不是通过HTML视图。

在启动时，`RequestMappingHandlerMapping` 和 `ExceptionHandlerExceptionResolver` 检测 controller advice bean，并在运行时应用它们。来自 `@ControllerAdvice` 的全局 `@ExceptionHandler` 方法，会在来自 `@Controller` 的局部方法之后被应用。相比之下，全局的 `@ModelAttribute` 和 `@InitBinder` 方法会在本地方法之前应用。

`@ControllerAdvice` 注解有一些属性，可以让你缩小它们适用的 controller 和 handler 的范围。比如说：

```java
// Target all Controllers annotated with @RestController
@ControllerAdvice(annotations = RestController.class)
public class ExampleAdvice1 {}

// Target all Controllers within specific packages
@ControllerAdvice("org.example.controllers")
public class ExampleAdvice2 {}

// Target all Controllers assignable to specific classes
@ControllerAdvice(assignableTypes = {ControllerInterface.class, AbstractController.class})
public class ExampleAdvice3 {}
```

前面例子中的选择器（selector）是在运行时评估的，如果大量使用可能会对性能产生负面影响。更多细节请参见 [`@ControllerAdvice`](https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/javadoc-api/org/springframework/web/bind/annotation/ControllerAdvice.html) javadoc。

另外需要注意的是，@ControllerAdvice注解是一定需要搭配其它的注解或者接口使用的，否则就算这个注解类被识别到，没有具体的处理方法，[SpringMVC](https://so.csdn.net/so/search?q=SpringMVC&spm=1001.2101.3001.7020)是不会进行任何操作的。

# 函数式端点

Spring Web MVC包括 WebMvc.fn，这是一个轻量级的函数式编程模型，其中函数被用来路由和处理请求，约定被设计为不可更改的。它是基于注解的编程模型的一个替代方案，但以其他方式运行在相同的 [DispatcherServlet](https://springdoc.cn/spring/web.html#mvc-servlet) 上。

在 WebMvc.fn 中，HTTP请求是通过 `HandlerFunction` 处理的：一个接收 `ServerRequest` 并返回 `ServerResponse` 的函数。请求和响应对象都有不可变的契约，提供JDK 8对HTTP请求和响应的友好访问。`HandlerFunction` 相当于基于注解的编程模型中的 `@RequestMapping` 方法的主体。

传入的请求被路由到一个带有 `RouterFunction` 的处理函数（handler function）：一个接受 `ServerRequest` 并返回一个可选的 `HandlerFunction`（即 `Optional<HandlerFunction>`）的函数。当 router 函数匹配时，将返回一个处理函数；否则将返回一个空的 `Optional`。`RouterFunction` 相当于 `@RequestMapping` 注解，但主要区别在于，router 函数不仅提供数据，还提供行为。

`RouterFunctions.route()` 提供了一个 router builder，便于创建 router，如下例所示：

```java
PersonRepository repository = ...
PersonHandler handler = new PersonHandler(repository);

RouterFunction<ServerResponse> route = route() 
    .GET("/person/{id}", accept(APPLICATION_JSON), handler::getPerson)
    .GET("/person", accept(APPLICATION_JSON), handler::listPeople)
    .POST("/person", handler::createPerson)
    .build();


public class PersonHandler {

    // ...

    public ServerResponse listPeople(ServerRequest request) {
        // ...
    }

    public ServerResponse createPerson(ServerRequest request) {
        // ...
    }

    public ServerResponse getPerson(ServerRequest request) {
        // ...
    }
}
```

> 使用 `route()` 创建 router 。

如果你把 `RouterFunction` 注册为Bean，例如通过在 `@Configuration` 类中公开它，它将被servlet自动检测到，正如在 [运行一个服务器](https://springdoc.cn/spring/web.html#webmvc-fn-running) 中解释的那样。

# 异步请求



















# 配置

## 启用

## Api 配置

## 类型转换

## 校验

## 拦截器

## Content Type

## Converter

## View Controller

## 试图解析器

## 静态资源

## Default Servlet

## 路径匹配

## 高级配置



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

