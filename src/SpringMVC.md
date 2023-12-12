# SpringMVC 初始化

![image-20231209132123740](assets/image-20231209132123740.png)

## HTTPServletBean

### init

```java
public final void init() throws ServletException {
    PropertyValues pvs = new ServletConfigPropertyValues(getServletConfig(), this.requiredProperties);
    if (!pvs.isEmpty()) {
        try {
            BeanWrapper bw = PropertyAccessorFactory.forBeanPropertyAccess(this);
            ResourceLoader resourceLoader = new ServletContextResourceLoader(getServletContext());
            bw.registerCustomEditor(Resource.class, new ResourceEditor(resourceLoader, getEnvironment()));
            initBeanWrapper(bw);
            bw.setPropertyValues(pvs, true);
        }
        catch (BeansException ex) {
            if (logger.isErrorEnabled()) {
                logger.error("Failed to set bean properties on servlet '" + getServletName() + "'", ex);
            }
            throw ex;
        }
    }

    initServletBean();
}
```

1. **设置 Bean 属性：** 方法开始时，通过 `ServletConfig` 获取初始化参数，并将这些参数设置为 `HttpServletBean` 的属性。这些参数会影响 Servlet 的行为。
   - 从 `ServletConfig` 获取初始化参数并封装为 `PropertyValues`。
     - `getServletConfig()` 是 `HttpServlet` 类提供的方法，用于获取当前 Servlet 的 `ServletConfig` 对象。
     - `this.requiredProperties` 是在 `HttpServletBean` 中定义的属性，用于指定所需的属性名，可以为空。
     - 创建一个 `ServletConfigPropertyValues` 对象，它会通过 `getServletConfig()` 获取当前 Servlet 的配置信息，将配置的初始化参数转化为 `PropertyValues` 对象 `pvs`。这个对象包含了 Servlet 初始化参数的名称和对应的值。
   - 通过 `BeanWrapper` 将初始化参数设置到 `HttpServletBean` 中。
     - 创建了一个 `BeanWrapper` 对象 `bw`，它是 Spring 提供的用于操作 JavaBean 属性的工具。`forBeanPropertyAccess(this)` 将当前 Servlet 对象传入，准备将参数映射到 Servlet 的属性上。
   - 注册自定义编辑器 `ResourceEditor`，用于处理资源（Resource）类型的属性。这个编辑器通过 `ResourceLoader` 来处理 `Resource` 属性的转换和初始化。
     - `bw.registerCustomEditor(Resource.class, new ResourceEditor(resourceLoader, getEnvironment()));`: 这行代码注册了一个自定义的属性编辑器到 `BeanWrapper` 中。它告诉 `BeanWrapper` 对象在设置属性值时如何处理类型为 `Resource` 的属性。`ResourceEditor` 是一个用于处理 `Resource` 类型属性的自定义编辑器，它需要 `resourceLoader` 和 `environment` 来初始化。
     - `initBeanWrapper(bw);`: 这可能是一个自定义的方法，或者是在上下文中其他位置的一个方法调用。它可能用于初始化 `BeanWrapper` 对象。
     - `bw.setPropertyValues(pvs, true);`: 这一行代码将 `pvs` 中的属性值设置到 Servlet 对象中。`pvs` 是一个属性值集合，它包含了从 Servlet 配置中获取的初始化参数。`setPropertyValues` 方法将这些参数值设置到对应的 Servlet 属性中。
2. **让子类初始化：** `initServletBean()` 是一个留给子类实现的方法，用于让子类做一些自定义的初始化操作。在 `HttpServletBean` 中，这个方法是一个空方法，子类可以覆盖它来添加自己的初始化逻辑。

#### ServletConfig

`ServletConfig` 是一个接口，代表了 Servlet 的配置信息，它包含了 Servlet 的初始化参数和一些其他配置信息。

`ServletConfig` 接口代表了当前 Servlet 在 `web.xml` 中的配置信息。它允许开发者获取关于 Servlet 的初始化参数和上下文信息。

在 `web.xml` 中，可以在 `<servlet>` 标签下使用 `<init-param>` 标签为一个 Servlet 配置一些初始化参数。例如：

```xml
<servlet>
    <servlet-name>MyServlet</servlet-name>
    <servlet-class>com.example.MyServlet</servlet-class>
    <init-param>
        <param-name>param1</param-name>
        <param-value>value1</param-value>
    </init-param>
    <!-- More init-param entries -->
</servlet>
```

当 Servlet 配置了初始化参数后，当 Web 容器初始化 Servlet 对象时，它会自动将这些初始化参数封装到一个 `ServletConfig` 对象中，并在调用 Servlet 的 `init()` 方法时将这个 `ServletConfig` 对象传递给 Servlet。

通过 `ServletConfig`，开发者可以：

1. **获取初始化参数：** 可以使用 `getInitParameter(String name)` 方法通过参数名获取对应的初始化参数值。
2. **获取 Servlet 名称和上下文信息：** 可以获取 Servlet 的名称、ServletContext 等信息。

##### ServletConfigPropertyValues

`ServletConfigPropertyValues` 是 Spring Framework 中的一个类，用于从 `ServletConfig` 中提取参数值并封装成 `PropertyValues` 对象。

#### BeanWrapper

`BeanWrapper` 是 Spring 提供的一个工具，用于操作 JavaBean 中的属性，它提供了一种统一的方式来访问和修改 JavaBean 对象的属性值。

它主要提供了以下功能：

1. **属性访问和设置：** 通过 `BeanWrapper`，可以轻松地获取和设置 JavaBean 中的属性值，而无需直接使用 Java 反射。
2. **类型转换和验证：** `BeanWrapper` 提供了类型转换功能，可以将输入的数据转换为适当的类型。如果类型不匹配，它还可以执行验证操作。
3. **嵌套属性访问：** 如果 JavaBean 中包含其他对象作为属性，`BeanWrapper` 允许通过“点”表示法（例如 `address.city`）来访问嵌套属性。
4. **事件处理：** 它支持事件监听机制，可以监听属性值的改变事件，使得对属性值的修改可以触发相关的事件处理。

使用 `BeanWrapper` 可以简化对 JavaBean 属性的操作，使得属性的访问和修改更加方便。通常情况下，它被广泛用于数据绑定、表单处理、属性校验等方面的操作。

##### registerCustomEditor

`registerCustomEditor` 方法是用于注册自定义属性编辑器的 Spring 框架方法。它允许你为特定类型的属性指定一个自定义的属性编辑器，用于在属性设置时进行类型转换或其他自定义的处理。

```java
void registerCustomEditor(Class<?> requiredType, PropertyEditor propertyEditor)
```

参数说明：

- `requiredType`：要注册自定义编辑器的属性类型。
- `propertyEditor`：自定义的属性编辑器实例。

通过 `registerCustomEditor` 方法，你可以告诉 Spring 当遇到特定类型的属性时使用自定义的属性编辑器。例如，如果你希望在将字符串转换为日期类型时使用特定的日期格式，你可以编写一个自定义的 `PropertyEditor` 来处理这种转换，并使用 `registerCustomEditor` 方法将其注册。

示例：

```java
BeanWrapper bw = PropertyAccessorFactory.forBeanPropertyAccess(object);
bw.registerCustomEditor(Date.class, new CustomDateEditor(new SimpleDateFormat("yyyy-MM-dd"), true));
```

在这个示例中，我们使用 `registerCustomEditor` 方法为 `Date` 类型的属性注册了一个自定义的日期编辑器 `CustomDateEditor`，指定了日期的格式为 "yyyy-MM-dd"。当 `BeanWrapper` 设置 `Date` 类型的属性时，将会使用这个自定义的日期编辑器进行转换。

#### PropertyAccessorFactory

`PropertyAccessorFactory` 是 Spring 提供的一个工厂类，用于创建 `PropertyAccessor` 实例，用于对 Java 对象的属性进行访问和操作。

它主要有以下功能：

1. **创建属性访问器：** 通过 `forBeanPropertyAccess()` 方法，可以创建一个用于访问 JavaBean 属性的 `BeanWrapper` 实例。
2. **创建其他类型的属性访问器：** 除了 `BeanWrapper`，`PropertyAccessorFactory` 还提供了其他方法，如 `forType()` 和 `forClass()`，用于创建其他类型的属性访问器，比如 `Map`、`Properties`、数组、`List` 等。
3. **自定义属性编辑器：** 可以通过 `registerCustomEditor()` 方法注册自定义的属性编辑器，用于在属性设置时进行类型转换和处理。
4. **支持线程安全：** 创建的 `PropertyAccessor` 实例通常是线程安全的，可以在多线程环境中共享使用。

这个工厂类提供了一种简单的方式来获取属性访问器，并且根据需要提供自定义的属性编辑器，使得对于属性的访问和修改更加方便和灵活。通常情况下，它用于 Spring 中进行属性编辑、数据绑定、类型转换等操作。

#### ResourceEditor

`ResourceEditor` 是 Spring 框架中用于处理 `Resource` 类型属性的属性编辑器。它用于将字符串转换为 `Resource` 对象，允许在 Spring 应用程序中方便地处理资源文件（如文件、URL、类路径资源等）。

这个编辑器通常用于处理属性文件路径或资源路径的转换。它负责将字符串路径转换为 `Resource` 对象，这样在处理资源时就可以更方便地使用 Spring 提供的资源抽象，如 `FileSystemResource`、`ClassPathResource`、`UrlResource` 等。

`ResourceEditor` 可以通过 `PropertyEditor` 接口来实现，它包含两个主要的方法：

- `setAsText(String text)`：将给定的文本转换为 `Resource` 对象。
- `getAsText()`：将 `Resource` 对象转换为字符串形式。

通过 `ResourceEditor`，你可以方便地将路径字符串转换为 Spring 资源对象，并在应用程序中使用这些资源对象。

在 Spring 配置或代码中，你可以注册 `ResourceEditor` 到 `BeanWrapper` 中，以处理包含资源路径的属性。例如：

```java
ResourceLoader resourceLoader = new ServletContextResourceLoader(servletContext);
PropertyEditor resourceEditor = new ResourceEditor(resourceLoader);

BeanWrapper beanWrapper = PropertyAccessorFactory.forBeanPropertyAccess(bean);
beanWrapper.registerCustomEditor(Resource.class, resourceEditor);
```

这样，在设置特定属性时，`ResourceEditor` 将会将给定的路径字符串转换为相应的 `Resource` 对象，使得 Spring 应用能够方便地处理各种资源。

#### ResourceLoader

`ServletContextResourceLoader` 是基于 Servlet 上下文的资源加载器，可以通过 Servlet 上下文获取相应的资源。Spring 的 `ResourceLoader` 接口是用于抽象访问不同类型资源的统一方式。它可以加载类路径、文件系统、URL、以及其他位置的资源，并且可以适用于不同的环境。

主要的实现类包括：

1. **`DefaultResourceLoader`：** 这是 `ResourceLoader` 的默认实现。它支持通过指定路径前缀（例如 `classpath:`）从类路径加载资源，也可以通过文件系统路径加载资源。
2. **`ServletContextResourceLoader`：** 这个实现类用于在 Web 应用程序中加载资源，它基于 Servlet 上下文。可以加载位于 Web 应用程序根目录下的资源文件。
3. **`UrlResourceLoader`：** 这个实现类允许通过 URL 加载资源。它可以加载远程服务器上的资源。

Spring 的 `Resource` 接口表示资源，它可以代表不同类型的资源，如文件、类路径、URL 等。这些资源可以通过 `ResourceLoader` 进行加载和访问。

通过 `ResourceLoader`，你可以实现统一的资源加载方式，而不需要关心底层资源存储的位置或类型。这使得在不同环境下切换资源加载方式变得更加简单，并且使得应用程序对于资源的处理更加灵活。

#### Resource

Spring 中的 `Resource` 是一个接口，它代表着应用程序中的资源，如文件、类路径、URL 等。`Resource` 接口的主要目的是提供对这些资源的抽象，使得它们可以被统一地加载、访问和操作。

Spring 提供了多个实现 `Resource` 接口的类，用于不同场景下的资源处理：

- `FileSystemResource`：表示文件系统中的资源。
- `ClassPathResource`：表示类路径下的资源。
- `UrlResource`：表示 URL 地址资源。
- `ServletContextResource`：表示在 Servlet 环境下的资源。
- `ByteArrayResource`：表示字节数组资源。
- `InputStreamResource`：表示输入流资源等。

这些实现允许你通过统一的方式处理各种资源，无论它们来自何处。你可以使用这些资源来读取、写入、加载或者访问应用程序中的各种文件、配置文件、图像、模板等内容。

通过 `Resource` 接口和它的实现类，Spring 允许你在应用程序中以统一的方式访问不同类型的资源，并且提供了一些便捷的方法来操作这些资源，比如读取文件内容、获取资源路径、检查资源是否存在等等。这使得在应用中对资源的处理变得更加灵活和方便。