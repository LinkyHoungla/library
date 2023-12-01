# 生命周期

JSP生命周期就是从创建到销毁的整个过程，类似于servlet生命周期，区别在于JSP生命周期还包括将JSP文件编译成servlet。

以下是JSP生命周期中所走过的几个阶段：

+ 编译阶段：

  servlet容器编译servlet源文件，生成servlet类

+ 初始化阶段：

  加载与JSP对应的servlet类，创建其实例，并调用它的初始化方法

+ 执行阶段：

  调用与JSP对应的servlet实例的服务方法

+ 销毁阶段：

  调用与JSP对应的servlet实例的销毁方法，然后销毁servlet实例

很明显，JSP生命周期的四个主要阶段和servlet生命周期非常相似，下面给出图示：

![img](https://www.runoob.com/wp-content/uploads/2014/01/jsp_life_cycle.jpg)

------

## JSP编译

当浏览器请求JSP页面时，JSP引擎会首先去检查是否需要编译这个文件。如果这个文件没有被编译过，或者在上次编译后被更改过，则编译这个JSP文件。

编译的过程包括三个步骤：

+ 解析JSP文件。
+ 将JSP文件转为servlet。
+ 编译servlet。

------

## JSP初始化

容器载入JSP文件后，它会在为请求提供任何服务前调用jspInit()方法。如果您需要执行自定义的JSP初始化任务，复写jspInit()方法就行了，就像下面这样：

```
public void jspInit(){
  // 初始化代码
}
```

一般来讲程序只初始化一次，servlet也是如此。通常情况下您可以在jspInit()方法中初始化数据库连接、打开文件和创建查询表。

------

## JSP执行

这一阶段描述了JSP生命周期中一切与请求相关的交互行为，直到被销毁。

当JSP网页完成初始化后，JSP引擎将会调用_jspService()方法。

_jspService()方法需要一个HttpServletRequest对象和一个HttpServletResponse对象作为它的参数，就像下面这样：

```
void _jspService(HttpServletRequest request,
                 HttpServletResponse response)
{
   // 服务端处理代码
}
```

_jspService()方法在每个request中被调用一次并且负责产生与之相对应的response，并且它还负责产生所有7个HTTP方法的回应，比如GET、POST、DELETE等等。

------

## JSP清理

JSP生命周期的销毁阶段描述了当一个JSP网页从容器中被移除时所发生的一切。

jspDestroy()方法在JSP中等价于servlet中的销毁方法。当您需要执行任何清理工作时复写jspDestroy()方法，比如释放数据库连接或者关闭文件夹等等。

jspDestroy()方法的格式如下：

```
public void jspDestroy()
{
   // 清理代码
}
```

# 语法

## 脚本程序

脚本程序可以包含任意量的Java语句、变量、方法或表达式，只要它们在脚本语言中是有效的。

脚本程序的语法格式：

```
<% 代码片段 %>
```

或者，您也可以编写与其等价的XML语句，就像下面这样：

```
<jsp:scriptlet>
   代码片段
</jsp:scriptlet>
```

任何文本、HTML标签、JSP元素必须写在脚本程序的外面。

下面给出一个示例，同时也是本教程的第一个JSP示例：

```
<html>
<head><title>Hello World</title></head>
<body>
Hello World!<br/>
<%
out.println("Your IP address is " + request.getRemoteAddr());
%>
</body>
</html>
```

### 中文编码问题

如果我们要在页面正常显示中文，我们需要在 JSP 文件头部添加以下代码：

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
```

## JSP声明

一个声明语句可以声明一个或多个变量、方法，供后面的Java代码使用。在JSP文件中，您必须先声明这些变量和方法然后才能使用它们。

JSP声明的语法格式：

```
<%! declaration; [ declaration; ]+ ... %>
```

或者，您也可以编写与其等价的XML语句，就像下面这样：

```
<jsp:declaration>
   代码片段
</jsp:declaration>
```

程序示例：

```
<%! int i = 0; %> 
<%! int a, b, c; %> 
<%! Circle a = new Circle(2.0); %> 
```

------

## JSP表达式

一个JSP表达式中包含的脚本语言表达式，先被转化成String，然后插入到表达式出现的地方。

由于表达式的值会被转化成String，所以您可以在一个文本行中使用表达式而不用去管它是否是HTML标签。

表达式元素中可以包含任何符合Java语言规范的表达式，但是不能使用分号来结束表达式。

JSP表达式的语法格式：

```
<%= 表达式 %>
```

同样，您也可以编写与之等价的XML语句：

```
<jsp:expression>
   表达式
</jsp:expression>
```

程序示例：

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
<p>
   今天的日期是: <%= (new java.util.Date()).toLocaleString()%>
</p>
</body> 
</html> 
```

运行后得到以下结果：

```
今天的日期是: 2016-6-25 13:40:07
```

------

## JSP注释

JSP注释主要有两个作用：为代码作注释以及将某段代码注释掉。

JSP注释的语法格式：

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
<%-- 该部分注释在网页中不会被显示--%> 
<p>
   今天的日期是: <%= (new java.util.Date()).toLocaleString()%>
</p>
</body> 
</html> 
```

运行后得到以下结果：

```
今天的日期是: 2016-6-25 13:41:26
```

不同情况下使用注释的语法规则：

| **语法**       | 描述                                                 |
| :------------- | :--------------------------------------------------- |
| <%-- 注释 --%> | JSP注释，注释内容不会被发送至浏览器甚至不会被编译    |
| <!-- 注释 -->  | HTML注释，通过浏览器查看网页源代码时可以看见注释内容 |
| <\%            | 代表静态 <%常量                                      |
| %\>            | 代表静态 %> 常量                                     |
| \'             | 在属性中使用的单引号                                 |
| \"             | 在属性中使用的双引号                                 |

------

## JSP指令

JSP指令用来设置与整个JSP页面相关的属性。

JSP指令语法格式：

```
<%@ directive attribute="value" %>
```

这里有三种指令标签：

| **指令**           | **描述**                                                  |
| :----------------- | :-------------------------------------------------------- |
| <%@ page ... %>    | 定义页面的依赖属性，比如脚本语言、error页面、缓存需求等等 |
| <%@ include ... %> | 包含其他文件                                              |
| <%@ taglib ... %>  | 引入标签库的定义，可以是自定义标签                        |

------

## JSP行为

JSP行为标签使用XML语法结构来控制servlet引擎。它能够动态插入一个文件，重用JavaBean组件，引导用户去另一个页面，为Java插件产生相关的HTML等等。

行为标签只有一种语法格式，它严格遵守XML标准：

```
<jsp:action_name attribute="value" />
```

行为标签基本上是一些预先就定义好的函数，下表罗列出了一些可用的JSP行为标签：：

| **语法**        | **描述**                                                   |
| :-------------- | :--------------------------------------------------------- |
| jsp:include     | 用于在当前页面中包含静态或动态资源                         |
| jsp:useBean     | 寻找和初始化一个JavaBean组件                               |
| jsp:setProperty | 设置 JavaBean组件的值                                      |
| jsp:getProperty | 将 JavaBean组件的值插入到 output中                         |
| jsp:forward     | 从一个JSP文件向另一个文件传递一个包含用户请求的request对象 |
| jsp:plugin      | 用于在生成的HTML页面中包含Applet和JavaBean对象             |
| jsp:element     | 动态创建一个XML元素                                        |
| jsp:attribute   | 定义动态创建的XML元素的属性                                |
| jsp:body        | 定义动态创建的XML元素的主体                                |
| jsp:text        | 用于封装模板数据                                           |

------

## JSP 隐含对象

JSP 支持九个自动定义的变量，江湖人称隐含对象，它们是在 JSP 页面中自动可用的对象，无需额外的声明或初始化。

这九个隐含对象的简介见下表：

| **对象**    | **描述**                                                     |
| :---------- | :----------------------------------------------------------- |
| request     | **HttpServletRequest**类的实例，代表 HTTP 请求的对象，包含客户端发送到服务器的信息，如表单数据、URL参数等。 |
| response    | **HttpServletResponse**类的实例，代表 HTTP 响应的对象，用于向客户端发送数据和响应。 |
| out         | **JspWriter**类的实例，用于向客户端输出文本内容的对象，通常用于生成HTML。 |
| session     | **HttpSession**类的实例，代表用户会话的对象，可用于存储和检索用户特定的数据，跨多个页面。 |
| application | **ServletContext**类的实例，代表 Web 应用程序的上下文，可以用于存储和检索全局应用程序数据。 |
| config      | **ServletConfig**类的实例，包含有关当前 JSP 页面的配置信息，例如初始化参数。 |
| pageContext | **PageContext**类的实例，提供对JSP页面所有对象以及命名空间的访问 |
| page        | 类似于 Java 类中的 this 关键字，代表当前 JSP 页面的实例，可以用于调用页面的方法。 |
| exception   | **exception** 类的对象，代表发生错误的 JSP 页面中对应的异常对象，用于处理 JSP 页面中的异常情况，可用于捕获和处理页面中发生的异常。 |

------

## 控制流语句

JSP提供对Java语言的全面支持。您可以在JSP程序中使用Java API甚至建立Java代码块，包括判断语句和循环语句等等。

## 判断语句

**if…else** 块，请看下面这个例子：

## 循环语句

在JSP程序中可以使用Java的三个基本循环类型：for，while，和 do…while。

JSP运算符

JSP支持所有Java逻辑和算术运算符。

下表罗列出了JSP常见运算符，优先级从高到底：

| **类别**  | **操作符**                         | **结合性** |
| :-------- | :--------------------------------- | :--------- |
| 后缀      | () [] . (点运算符)                 | 左到右     |
| 一元      | ++ - - ! ~                         | 右到左     |
| 可乘性    | * / %                              | 左到右     |
| 可加性    | + -                                | 左到右     |
| 移位      | >> >>> <<                          | 左到右     |
| 关系      | > >= < <=                          | 左到右     |
| 相等/不等 | == !=                              | 左到右     |
| 位与      | &                                  | 左到右     |
| 位异或    | ^                                  | 左到右     |
| 位或      | \|                                 | 左到右     |
| 逻辑与    | &&                                 | 左到右     |
| 逻辑或    | \|\|                               | 左到右     |
| 条件判断  | ?:                                 | 右到左     |
| 赋值      | = += -= *= /= %= >>= <<= &= ^= \|= | 右到左     |
| 逗号      | ,                                  | 左到右     |

------

## JSP 字面量

JSP语言定义了以下几个字面量：

+ 布尔值(boolean)：true 和 false;
+ 整型(int)：与 Java 中的一样;
+ 浮点型(float)：与 Java 中的一样;
+ 字符串(string)：以单引号或双引号开始和结束;
+ Null：null。

# 指令

JSP指令用来设置整个JSP页面相关的属性，如网页的编码方式和脚本语言。

语法格式如下：

```
<%@ directive attribute="value" %>
```

指令可以有很多个属性，它们以键值对的形式存在，并用逗号隔开。

JSP中的三种指令标签：

| **指令**           | **描述**                                                |
| :----------------- | :------------------------------------------------------ |
| <%@ page ... %>    | 定义网页依赖属性，比如脚本语言、error页面、缓存需求等等 |
| <%@ include ... %> | 包含其他文件                                            |
| <%@ taglib ... %>  | 引入标签库的定义                                        |

------

## Page指令

Page指令为容器提供当前页面的使用说明。一个JSP页面可以包含多个page指令。

Page指令的语法格式：

```
<%@ page attribute="value" %>
```

等价的XML格式：

```
<jsp:directive.page attribute="value" />
```

------

## 属性

下表列出与Page指令相关的属性：

| **属性**           | **描述**                                            |
| :----------------- | :-------------------------------------------------- |
| buffer             | 指定out对象使用缓冲区的大小                         |
| autoFlush          | 控制out对象的 缓存区                                |
| contentType        | 指定当前JSP页面的MIME类型和字符编码                 |
| errorPage          | 指定当JSP页面发生异常时需要转向的错误处理页面       |
| isErrorPage        | 指定当前页面是否可以作为另一个JSP页面的错误处理页面 |
| extends            | 指定servlet从哪一个类继承                           |
| import             | 导入要使用的Java类                                  |
| info               | 定义JSP页面的描述信息                               |
| isThreadSafe       | 指定对JSP页面的访问是否为线程安全                   |
| language           | 定义JSP页面所用的脚本语言，默认是Java               |
| session            | 指定JSP页面是否使用session                          |
| isELIgnored        | 指定是否执行EL表达式                                |
| isScriptingEnabled | 确定脚本元素能否被使用                              |

------

## Include指令

JSP可以通过include指令来包含其他文件。被包含的文件可以是JSP文件、HTML文件或文本文件。包含的文件就好像是该JSP文件的一部分，会被同时编译执行。

Include指令的语法格式如下：

```
<%@ include file="文件相对 url 地址" %>
```

**include** 指令中的文件名实际上是一个相对的 URL 地址。

如果您没有给文件关联一个路径，JSP编译器默认在当前路径下寻找。

等价的XML语法：

```
<jsp:directive.include file="文件相对 url 地址" />
```

------

## Taglib指令

JSP API允许用户自定义标签，一个自定义标签库就是自定义标签的集合。

Taglib指令引入一个自定义标签集合的定义，包括库路径、自定义标签。

Taglib指令的语法：

```
<%@ taglib uri="uri" prefix="prefixOfTag" %>
```

uri属性确定标签库的位置，prefix属性指定标签库的前缀。

等价的XML语法：

```
<jsp:directive.taglib uri="uri" prefix="prefixOfTag" />
```

# 动作元素

与JSP指令元素不同的是，JSP动作元素在请求处理阶段起作用。JSP动作元素是用XML语法写成的。

利用JSP动作可以动态地插入文件、重用JavaBean组件、把用户重定向到另外的页面、为Java插件生成HTML代码。

动作元素只有一种语法，它符合XML标准：

```
<jsp:action_name attribute="value" />
```

动作元素基本上都是预定义的函数，JSP规范定义了一系列的标准动作，它用JSP作为前缀，可用的标准动作元素如下：

| 语法            | 描述                                            |
| :-------------- | :---------------------------------------------- |
| jsp:include     | 在页面被请求的时候引入一个文件。                |
| jsp:useBean     | 寻找或者实例化一个JavaBean。                    |
| jsp:setProperty | 设置JavaBean的属性。                            |
| jsp:getProperty | 输出某个JavaBean的属性。                        |
| jsp:forward     | 把请求转到一个新的页面。                        |
| jsp:plugin      | 根据浏览器类型为Java插件生成OBJECT或EMBED标记。 |
| jsp:element     | 定义动态XML元素                                 |
| jsp:attribute   | 设置动态定义的XML元素属性。                     |
| jsp:body        | 设置动态定义的XML元素内容。                     |
| jsp:text        | 在JSP页面和文档中使用写入文本的模板             |

------

## 常见的属性

所有的动作要素都有两个属性：id属性和scope属性。

+ id属性：

  id属性是动作元素的唯一标识，可以在JSP页面中引用。动作元素创建的id值可以通过PageContext来调用。

  

+ scope属性：

  该属性用于识别动作元素的生命周期。 id属性和scope属性有直接关系，scope属性定义了相关联id对象的寿命。 scope属性有四个可能的值： (a) page, (b)request, (c)session, 和 (d) application。

------

## <jsp:include>动作元素

<jsp:include>动作元素用来包含静态和动态的文件。该动作把指定文件插入正在生成的页面。语法格式如下：

```
<jsp:include page="相对 URL 地址" flush="true" />
```

　前面已经介绍过include指令，它是在JSP文件被转换成Servlet的时候引入文件，而这里的jsp:include动作不同，插入文件的时间是在页面被请求的时候。

以下是include动作相关的属性列表。

| 属性  | 描述                                       |
| :---- | :----------------------------------------- |
| page  | 包含在页面中的相对URL地址。                |
| flush | 布尔属性，定义在包含资源前是否刷新缓存区。 |

## <jsp:useBean>动作元素

**jsp:useBean** 动作用来加载一个将在JSP页面中使用的JavaBean。

这个功能非常有用，因为它使得我们可以发挥 Java 组件复用的优势。

jsp:useBean动作最简单的语法为：

```
<jsp:useBean id="name" class="package.class" />
```

在类载入后，我们既可以通过 jsp:setProperty 和 jsp:getProperty 动作来修改和检索bean的属性。

以下是useBean动作相关的属性列表。

| 属性     | 描述                                                        |
| :------- | :---------------------------------------------------------- |
| class    | 指定Bean的完整包名。                                        |
| type     | 指定将引用该对象变量的类型。                                |
| beanName | 通过 java.beans.Beans 的 instantiate() 方法指定Bean的名字。 |

在给出具体实例前，让我们先来看下 jsp:setProperty 和 jsp:getProperty 动作元素：

------

## <jsp:setProperty>动作元素

jsp:setProperty用来设置已经实例化的Bean对象的属性，有两种用法。首先，你可以在jsp:useBean元素的外面（后面）使用jsp:setProperty，如下所示：

```
<jsp:useBean id="myName" ... />
...
<jsp:setProperty name="myName" property="someProperty" .../>
```

此时，不管jsp:useBean是找到了一个现有的Bean，还是新创建了一个Bean实例，jsp:setProperty都会执行。第二种用法是把jsp:setProperty放入jsp:useBean元素的内部，如下所示：

```
<jsp:useBean id="myName" ... >
...
   <jsp:setProperty name="myName" property="someProperty" .../>
</jsp:useBean>
```

此时，jsp:setProperty只有在新建Bean实例时才会执行，如果是使用现有实例则不执行jsp:setProperty。

jsp:setProperty动作有下面四个属性,如下表：

| 属性     | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| name     | name属性是必需的。它表示要设置属性的是哪个Bean。             |
| property | property属性是必需的。它表示要设置哪个属性。有一个特殊用法：如果property的值是"*"，表示所有名字和Bean属性名字匹配的请求参数都将被传递给相应的属性set方法。 |
| value    | value 属性是可选的。该属性用来指定Bean属性的值。字符串数据会在目标类中通过标准的valueOf方法自动转换成数字、boolean、Boolean、 byte、Byte、char、Character。例如，boolean和Boolean类型的属性值（比如"true"）通过 Boolean.valueOf转换，int和Integer类型的属性值（比如"42"）通过Integer.valueOf转换。 　　value和param不能同时使用，但可以使用其中任意一个。 |
| param    | param 是可选的。它指定用哪个请求参数作为Bean属性的值。如果当前请求没有参数，则什么事情也不做，系统不会把null传递给Bean属性的set方法。因此，你可以让Bean自己提供默认属性值，只有当请求参数明确指定了新值时才修改默认属性值。 |

------

## <jsp:getProperty>动作元素

jsp:getProperty动作提取指定Bean属性的值，转换成字符串，然后输出。语法格式如下：

```
<jsp:useBean id="myName" ... />
...
<jsp:getProperty name="myName" property="someProperty" .../>
```

下表是与getProperty相关联的属性：

| 属性     | 描述                                   |
| :------- | :------------------------------------- |
| name     | 要检索的Bean属性名称。Bean必须已定义。 |
| property | 表示要提取Bean属性的值                 |

## <jsp:forward> 动作元素

　jsp:forward动作把请求转到另外的页面。jsp:forward标记只有一个属性page。语法格式如下所示：

```
<jsp:forward page="相对 URL 地址" />
```

以下是forward相关联的属性：

| 属性 | 描述                                                         |
| :--- | :----------------------------------------------------------- |
| page | page属性包含的是一个相对URL。page的值既可以直接给出，也可以在请求的时候动态计算，可以是一个JSP页面或者一个 Java Servlet. |

## jsp:plugin>动作元素

jsp:plugin动作用来根据浏览器的类型，插入通过Java插件 运行Java Applet所必需的OBJECT或EMBED元素。

如果需要的插件不存在，它会下载插件，然后执行Java组件。 Java组件可以是一个applet或一个JavaBean。

plugin动作有多个对应HTML元素的属性用于格式化Java 组件。param元素可用于向Applet 或 Bean 传递参数。

以下是使用plugin 动作元素的典型实例:

```
<jsp:plugin type="applet" codebase="dirname" code="MyApplet.class"
                           width="60" height="80">
   <jsp:param name="fontcolor" value="red" />
   <jsp:param name="background" value="black" />
 
   <jsp:fallback>
      Unable to initialize Java Plugin
   </jsp:fallback>
 
</jsp:plugin>
```

如果你有兴趣可以尝试使用applet来测试jsp:plugin动作元素，<fallback>元素是一个新元素，在组件出现故障的错误时发送给用户错误信息。

## <jsp:element> 、 <jsp:attribute>、 <jsp:body>动作元素

<jsp:element> 、 <jsp:attribute>、 <jsp:body>动作元素动态定义XML元素。动态是非常重要的，这就意味着XML元素在编译时是动态生成的而非静态。

以下实例动态定义了XML元素：

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
<jsp:element name="xmlElement">
<jsp:attribute name="xmlElementAttr">
   属性值
</jsp:attribute>
<jsp:body>
   XML 元素的主体
</jsp:body>
</jsp:element>
</body>
</html>
```

## <jsp:text>动作元素

<jsp:text>动作元素允许在JSP页面和文档中使用写入文本的模板，语法格式如下：

```
<jsp:text>模板数据</jsp:text>
```

以上文本模板不能包含重复元素，只能包含文本和EL表达式（注：EL表达式将在后续章节中介绍）。请注意，在XML文件中，您不能使用表达式如 whatever>0，因为>符号是非法的。你可以使用�ℎ������>0，因为>符号是非法的。你可以使用{whatever gt 0}表达式或者嵌入在一个CDATA部分的值。

```
<jsp:text><![CDATA[<br>]]></jsp:text>
```

如果你需要在 XHTML 中声明 DOCTYPE,必须使用到<jsp:text>动作元素，实例如下：

```
<jsp:text><![CDATA[<!DOCTYPE html
PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
"DTD/xhtml1-strict.dtd">]]>
</jsp:text>
<head><title>jsp:text action</title></head>
<body>

<books><book><jsp:text>  
    Welcome to JSP Programming
</jsp:text></book></books>

</body>
</html>
```

你可以对以上实例尝试使用<jsp:text>及不使用该动作元素执行结果的区别。

# 隐式对象

JSP隐式对象是JSP容器为每个页面提供的Java对象，开发者可以直接使用它们而不用显式声明。JSP隐式对象也被称为预定义变量。

JSP所支持的九大隐式对象：

| **对象**    | **描述**                                                     |
| :---------- | :----------------------------------------------------------- |
| request     | **HttpServletRequest** 接口的实例                            |
| response    | **HttpServletResponse** 接口的实例                           |
| out         | **JspWriter**类的实例，用于把结果输出至网页上                |
| session     | **HttpSession**类的实例                                      |
| application | **ServletContext**类的实例，与应用上下文有关                 |
| config      | **ServletConfig**类的实例                                    |
| pageContext | **PageContext**类的实例，提供对JSP页面所有对象以及命名空间的访问 |
| page        | 类似于Java类中的this关键字                                   |
| Exception   | **Exception**类的对象，代表发生错误的JSP页面中对应的异常对象 |

------

## request对象

request对象是javax.servlet.http.HttpServletRequest 类的实例。每当客户端请求一个JSP页面时，JSP引擎就会制造一个新的request对象来代表这个请求。

request对象提供了一系列方法来获取HTTP头信息，cookies，HTTP方法等等。

------

## response对象

response对象是javax.servlet.http.HttpServletResponse类的实例。当服务器创建request对象时会同时创建用于响应这个客户端的response对象。

response对象也定义了处理HTTP头模块的接口。通过这个对象，开发者们可以添加新的cookies，时间戳，HTTP状态码等等。

------

## out对象

out对象是 javax.servlet.jsp.JspWriter 类的实例，用来在response对象中写入内容。

最初的JspWriter类对象根据页面是否有缓存来进行不同的实例化操作。可以在page指令中使用buffered='false'属性来轻松关闭缓存。

JspWriter类包含了大部分java.io.PrintWriter类中的方法。不过，JspWriter新增了一些专为处理缓存而设计的方法。还有就是，JspWriter类会抛出IOExceptions异常，而PrintWriter不会。

下表列出了我们将会用来输出boolean，char，int，double，String，object等类型数据的重要方法：

| **方法**                     | **描述**                 |
| :--------------------------- | :----------------------- |
| **out.print(dataType dt)**   | 输出Type类型的值         |
| **out.println(dataType dt)** | 输出Type类型的值然后换行 |
| **out.flush()**              | 刷新输出流               |

------

## session对象

session对象是 javax.servlet.http.HttpSession 类的实例。和Java Servlets中的session对象有一样的行为。

session对象用来跟踪在各个客户端请求间的会话。

------

## application对象

application对象直接包装了servlet的ServletContext类的对象，是javax.servlet.ServletContext 类的实例。

这个对象在JSP页面的整个生命周期中都代表着这个JSP页面。这个对象在JSP页面初始化时被创建，随着jspDestroy()方法的调用而被移除。

通过向application中添加属性，则所有组成您web应用的JSP文件都能访问到这些属性。

------

## config对象

config对象是 javax.servlet.ServletConfig 类的实例，直接包装了servlet的ServletConfig类的对象。

这个对象允许开发者访问Servlet或者JSP引擎的初始化参数，比如文件路径等。

以下是config对象的使用方法，不是很重要，所以不常用：

```
config.getServletName();
```

它返回包含在<servlet-name>元素中的servlet名字，注意，<servlet-name>元素在 WEB-INF\web.xml 文件中定义。

------

## pageContext 对象

pageContext对象是javax.servlet.jsp.PageContext 类的实例，用来代表整个JSP页面。

这个对象主要用来访问页面信息，同时过滤掉大部分实现细节。

这个对象存储了request对象和response对象的引用。application对象，config对象，session对象，out对象可以通过访问这个对象的属性来导出。

pageContext对象也包含了传给JSP页面的指令信息，包括缓存信息，ErrorPage URL,页面scope等。

PageContext类定义了一些字段，包括PAGE_SCOPE，REQUEST_SCOPE，SESSION_SCOPE， APPLICATION_SCOPE。它也提供了40余种方法，有一半继承自javax.servlet.jsp.JspContext 类。

其中一个重要的方法就是 removeAttribute()，它可接受一个或两个参数。比如，pageContext.removeAttribute("attrName") 移除四个scope中相关属性，但是下面这种方法只移除特定 scope 中的相关属性：

```
pageContext.removeAttribute("attrName", PAGE_SCOPE);
```

------

## page 对象

这个对象就是页面实例的引用。它可以被看做是整个JSP页面的代表。

page 对象就是this对象的同义词。

------

## exception 对象

exception 对象包装了从先前页面中抛出的异常信息。它通常被用来产生对出错条件的适当响应。

# 客户端请求

当浏览器请求一个网页时，它会向网络服务器发送一系列不能被直接读取的信息，因为这些信息是作为HTTP信息头的一部分来传送的。您可以查阅HTTP协议来获得更多的信息。

下表列出了浏览器端信息头的一些重要内容，在以后的网络编程中将会经常见到这些信息：

| **信息**            | **描述**                                                     |
| :------------------ | :----------------------------------------------------------- |
| Accept              | 指定浏览器或其他客户端可以处理的MIME类型。它的值通常为 **image/png** 或 **image/jpeg** |
| Accept-Charset      | 指定浏览器要使用的字符集。比如 ISO-8859-1                    |
| Accept-Encoding     | 指定编码类型。它的值通常为 **gzip** 或**compress**           |
| Accept-Language     | 指定客户端首选语言，servlet会优先返回以当前语言构成的结果集，如果servlet支持这种语言的话。比如 en，en-us，ru等等 |
| Authorization       | 在访问受密码保护的网页时识别不同的用户                       |
| Connection          | 表明客户端是否可以处理HTTP持久连接。持久连接允许客户端或浏览器在一个请求中获取多个文件。**Keep-Alive** 表示启用持久连接 |
| Content-Length      | 仅适用于POST请求，表示 POST 数据的字节数                     |
| Cookie              | 返回先前发送给浏览器的cookies至服务器                        |
| Host                | 指出原始URL中的主机名和端口号                                |
| If-Modified-Since   | 表明只有当网页在指定的日期被修改后客户端才需要这个网页。 服务器发送304码给客户端，表示没有更新的资源 |
| If-Unmodified-Since | 与If-Modified-Since相反， 只有文档在指定日期后仍未被修改过，操作才会成功 |
| Referer             | 标志着所引用页面的URL。比如，如果你在页面1，然后点了个链接至页面2，那么页面1的URL就会包含在浏览器请求页面2的信息头中 |
| User-Agent          | 用来区分不同浏览器或客户端发送的请求，并对不同类型的浏览器返回不同的内容 |

------

## HttpServletRequest类

request对象是javax.servlet.http.HttpServletRequest类的实例。每当客户端请求一个页面时，JSP引擎就会产生一个新的对象来代表这个请求。

request对象提供了一系列方法来获取HTTP信息头，包括表单数据，cookies，HTTP方法等等。

接下来将会介绍一些在JSP编程中常用的获取HTTP信息头的方法。详细内容请见下表：

| **序号** | **方法****&** **描述**                                       |
| :------- | :----------------------------------------------------------- |
| 1        | **Cookie[] getCookies()**返回客户端所有的Cookie的数组        |
| 2        | **Enumeration getAttributeNames()**返回request对象的所有属性名称的集合 |
| 3        | **Enumeration getHeaderNames()**返回所有HTTP头的名称集合     |
| 4        | **Enumeration getParameterNames()**返回请求中所有参数的集合  |
| 5        | **HttpSession getSession()**返回request对应的session对象，如果没有，则创建一个 |
| 6        | **HttpSession getSession(boolean create)**返回request对应的session对象，如果没有并且参数create为true，则返回一个新的session对象 |
| 7        | **Locale getLocale()**返回当前页的Locale对象，可以在response中设置 |
| 8        | **Object getAttribute(String name)**返回名称为name的属性值，如果不存在则返回null。 |
| 9        | **ServletInputStream getInputStream()**返回请求的输入流      |
| 10       | **String getAuthType()**返回认证方案的名称，用来保护servlet，比如 "BASIC" 或者 "SSL" 或 null 如果 JSP没设置保护措施 |
| 11       | **String getCharacterEncoding()**返回request的字符编码集名称 |
| 12       | **String getContentType()**返回request主体的MIME类型，若未知则返回null |
| 13       | **String getContextPath()**返回request URI中指明的上下文路径 |
| 14       | **String getHeader(String name)**返回name指定的信息头        |
| 15       | **String getMethod()**返回此request中的HTTP方法，比如 GET,，POST，或PUT |
| 16       | **String getParameter(String name)**返回此request中name指定的参数，若不存在则返回null |
| 17       | **String getPathInfo()**返回任何额外的与此request URL相关的路径 |
| 18       | **String getProtocol()**返回此request所使用的协议名和版本    |
| 19       | **String getQueryString()**返回此 request URL包含的查询字符串 |
| 20       | **String getRemoteAddr()**返回客户端的IP地址                 |
| 21       | **String getRemoteHost()**返回客户端的完整名称               |
| 22       | **String getRemoteUser()**返回客户端通过登录认证的用户，若用户未认证则返回null |
| 23       | **String getRequestURI()**返回request的URI                   |
| 24       | **String getRequestedSessionId()**返回request指定的session ID |
| 25       | **String getServletPath()**返回所请求的servlet路径           |
| 26       | **String[] getParameterValues(String name)**返回指定名称的参数的所有值，若不存在则返回null |
| 27       | **boolean isSecure()**返回request是否使用了加密通道，比如HTTPS |
| 28       | **int getContentLength()**返回request主体所包含的字节数，若未知的返回-1 |
| 29       | **int getIntHeader(String name)**返回指定名称的request信息头的值 |
| 30       | **int getServerPort()**返回服务器端口号                      |

# 服务器响应

Response响应对象主要将JSP容器处理后的结果传回到客户端。可以通过response变量设置HTTP的状态和向客户端发送数据，如Cookie、HTTP文件头信息等。

一个典型的响应看起来就像下面这样：

```
HTTP/1.1 200 OK
Content-Type: text/html
Header2: ...
...
HeaderN: ...
  (空行)
<!doctype ...>
<html>
<head>...</head>
<body>
...
</body>
</html>
```

状态行包含HTTP版本信息，比如HTTP/1.1，一个状态码，比如200，还有一个非常短的信息对应着状态码，比如OK。

下表摘要出了HTTP1.1响应头中最有用的部分，在网络编程中您将会经常见到它们：

| **响应头**          | **描述**                                                     |
| :------------------ | :----------------------------------------------------------- |
| Allow               | 指定服务器支持的request方法（GET，POST等等）                 |
| Cache-Control       | 指定响应文档能够被安全缓存的情况。通常取值为 **public****，****private** 或**no-cache** 等等。 Public意味着文档可缓存，Private意味着文档只为单用户服务并且只能使用私有缓存。No-cache 意味着文档不被缓存。 |
| Connection          | 命令浏览器是否要使用持久的HTTP连接。**close****值** 命令浏览器不使用持久HTTP连接，而keep-alive 意味着使用持久化连接。 |
| Content-Disposition | 让浏览器要求用户将响应以给定的名称存储在磁盘中               |
| Content-Encoding    | 指定传输时页面的编码规则                                     |
| Content-Language    | 表述文档所使用的语言，比如en， en-us,，ru等等                |
| Content-Length      | 表明响应的字节数。只有在浏览器使用持久化 (keep-alive) HTTP 连接时才有用 |
| Content-Type        | 表明文档使用的MIME类型                                       |
| Expires             | 指明啥时候过期并从缓存中移除                                 |
| Last-Modified       | 指明文档最后修改时间。客户端可以 缓存文档并且在后续的请求中提供一个 **If-Modified-Since**请求头 |
| Location            | 在300秒内，包含所有的有一个状态码的响应地址，浏览器会自动重连然后检索新文档 |
| Refresh             | 指明浏览器每隔多久请求更新一次页面。                         |
| Retry-After         | 与503 (Service Unavailable)一起使用来告诉用户多久后请求将会得到响应 |
| Set-Cookie          | 指明当前页面对应的cookie                                     |

------

## HttpServletResponse类

response 对象是 javax.servlet.http.HttpServletResponse 类的一个实例。就像服务器会创建request对象一样，它也会创建一个客户端响应。

response对象定义了处理创建HTTP信息头的接口。通过使用这个对象，开发者们可以添加新的cookie或时间戳，还有HTTP状态码等等。

下表列出了用来设置HTTP响应头的方法，这些方法由HttpServletResponse 类提供：

| **S.N.** | **方法** **&** **描述**                                      |
| :------- | :----------------------------------------------------------- |
| 1        | **String encodeRedirectURL(String url)**对sendRedirect()方法使用的URL进行编码 |
| 2        | **String encodeURL(String url)**将URL编码，回传包含Session ID的URL |
| 3        | **boolean containsHeader(String name)**返回指定的响应头是否存在 |
| 4        | **boolean isCommitted()**返回响应是否已经提交到客户端        |
| 5        | **void addCookie(Cookie cookie)**添加指定的cookie至响应中    |
| 6        | **void addDateHeader(String name, long date)**添加指定名称的响应头和日期值 |
| 7        | **void addHeader(String name, String value)**添加指定名称的响应头和值 |
| 8        | **void addIntHeader(String name, int value)**添加指定名称的响应头和int值 |
| 9        | **void flushBuffer()**将任何缓存中的内容写入客户端           |
| 10       | **void reset()**清除任何缓存中的任何数据，包括状态码和各种响应头 |
| 11       | **void resetBuffer()**清除基本的缓存数据，不包括响应头和状态码 |
| 12       | **void sendError(int sc)**使用指定的状态码向客户端发送一个出错响应，然后清除缓存 |
| 13       | **void sendError(int sc, String msg)**使用指定的状态码和消息向客户端发送一个出错响应 |
| 14       | **void sendRedirect(String location)**使用指定的URL向客户端发送一个临时的间接响应 |
| 15       | **void setBufferSize(int size)**设置响应体的缓存区大小       |
| 16       | **void setCharacterEncoding(String charset)**指定响应的编码集（MIME字符集），例如UTF-8 |
| 17       | **void setContentLength(int len)**指定HTTP servlets中响应的内容的长度，此方法用来设置 HTTP Content-Length 信息头 |
| 18       | **void setContentType(String type)**设置响应的内容的类型，如果响应还未被提交的话 |
| 19       | **void setDateHeader(String name, long date)**使用指定名称和日期设置响应头的名称和日期 |
| 20       | **void setHeader(String name, String value)**使用指定名称和值设置响应头的名称和内容 |
| 21       | **void setIntHeader(String name, int value)**指定 int 类型的值到 name 标头 |
| 22       | **void setLocale(Locale loc)**设置响应的语言环境，如果响应尚未被提交的话 |
| 23       | **void setStatus(int sc)**设置响应的状态码                   |

# 标准标签库

JSP标准标签库（JSTL）是一个JSP标签集合，它封装了JSP应用的通用核心功能。

JSTL支持通用的、结构化的任务，比如迭代，条件判断，XML文档操作，国际化标签，SQL标签。 除了这些，它还提供了一个框架来使用集成JSTL的自定义标签。

根据JSTL标签所提供的功能，可以将其分为5个类别。

+ **核心标签**
+ **格式化标签**
+ **SQL 标签**
+ **XML 标签**
+ **JSTL 函数**

## JSTL 库安装

Apache Tomcat安装JSTL 库步骤如下：

从Apache的标准标签库中下载的二进包(jakarta-taglibs-standard-current.zip)。

+ 官方下载地址：http://archive.apache.org/dist/jakarta/taglibs/standard/binaries/
+ 本站下载地址：[jakarta-taglibs-standard-1.1.2.zip](http://static.runoob.com/download/jakarta-taglibs-standard-1.1.2.tar.gz)

下载 **jakarta-taglibs-standard-1.1.2.zip** 包并解压，将 **jakarta-taglibs-standard-1.1.2/lib/** 下的两个 jar 文件：**standard.jar** 和 **jstl.jar** 文件拷贝到 **/WEB-INF/lib/** 下。

将 tld 下的需要引入的 tld 文件复制到 WEB-INF 目录下。

接下来我们在 web.xml 文件中添加以下配置：

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.4" 
    xmlns="http://java.sun.com/xml/ns/j2ee" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
        http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
    <jsp-config>
    <taglib>
    <taglib-uri>http://java.sun.com/jsp/jstl/fmt</taglib-uri>
    <taglib-location>/WEB-INF/fmt.tld</taglib-location>
    </taglib>
    <taglib>
    <taglib-uri>http://java.sun.com/jsp/jstl/fmt-rt</taglib-uri>
    <taglib-location>/WEB-INF/fmt-rt.tld</taglib-location>
    </taglib>
    <taglib>
    <taglib-uri>http://java.sun.com/jsp/jstl/core</taglib-uri>
    <taglib-location>/WEB-INF/c.tld</taglib-location>
    </taglib>
    <taglib>
    <taglib-uri>http://java.sun.com/jsp/jstl/core-rt</taglib-uri>
    <taglib-location>/WEB-INF/c-rt.tld</taglib-location>
    </taglib>
    <taglib>
    <taglib-uri>http://java.sun.com/jsp/jstl/sql</taglib-uri>
    <taglib-location>/WEB-INF/sql.tld</taglib-location>
    </taglib>
    <taglib>
    <taglib-uri>http://java.sun.com/jsp/jstl/sql-rt</taglib-uri>
    <taglib-location>/WEB-INF/sql-rt.tld</taglib-location>
    </taglib>
    <taglib>
    <taglib-uri>http://java.sun.com/jsp/jstl/x</taglib-uri>
    <taglib-location>/WEB-INF/x.tld</taglib-location>
    </taglib>
    <taglib>
    <taglib-uri>http://java.sun.com/jsp/jstl/x-rt</taglib-uri>
    <taglib-location>/WEB-INF/x-rt.tld</taglib-location>
    </taglib>
    </jsp-config>
</web-app>
```

使用任何库，你必须在每个 JSP 文件中的头部包含 **<taglib>** 标签。

## 核心标签

核心标签是最常用的 JSTL标签。引用核心标签库的语法如下：

```
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

| 标签                                                       | 描述                                                         |
| :--------------------------------------------------------- | :----------------------------------------------------------- |
| [](https://www.runoob.com/jsp/jstl-core-out-tag.html)      | 用于在JSP中显示数据，就像<%= ... >                           |
| [](https://www.runoob.com/jsp/jstl-core-set-tag.html)      | 用于保存数据                                                 |
| [](https://www.runoob.com/jsp/jstl-core-remove-tag.html)   | 用于删除数据                                                 |
| [](https://www.runoob.com/jsp/jstl-core-catch-tag.html)    | 用来处理产生错误的异常状况，并且将错误信息储存起来           |
| [](https://www.runoob.com/jsp/jstl-core-if-tag.html)       | 与我们在一般程序中用的if一样                                 |
| [](https://www.runoob.com/jsp/jstl-core-choose-tag.html)   | 本身只当做<c:when>和<c:otherwise>的父标签                    |
| [](https://www.runoob.com/jsp/jstl-core-choose-tag.html)   | <c:choose>的子标签，用来判断条件是否成立                     |
| [](https://www.runoob.com/jsp/jstl-core-choose-tag.html)   | <c:choose>的子标签，接在<c:when>标签后，当<c:when>标签判断为false时被执行 |
| [](https://www.runoob.com/jsp/jstl-core-import-tag.html)   | 检索一个绝对或相对 URL，然后将其内容暴露给页面               |
| [](https://www.runoob.com/jsp/jstl-core-foreach-tag.html)  | 基础迭代标签，接受多种集合类型                               |
| [](https://www.runoob.com/jsp/jstl-core-foreach-tag.html)  | 根据指定的分隔符来分隔内容并迭代输出                         |
| [](https://www.runoob.com/jsp/jstl-core-param-tag.html)    | 用来给包含或重定向的页面传递参数                             |
| [](https://www.runoob.com/jsp/jstl-core-redirect-tag.html) | 重定向至一个新的URL.                                         |
| [](https://www.runoob.com/jsp/jstl-core-url-tag.html)      | 使用可选的查询参数来创造一个URL                              |

------

## 格式化标签

JSTL格式化标签用来格式化并输出文本、日期、时间、数字。引用格式化标签库的语法如下：

```
<%@ taglib prefix="fmt" 
           uri="http://java.sun.com/jsp/jstl/fmt" %>
```

| 标签                                                         | 描述                                     |
| :----------------------------------------------------------- | :--------------------------------------- |
| [](https://www.runoob.com/jsp/jstl-format-formatnumber-tag.html) | 使用指定的格式或精度格式化数字           |
| [](https://www.runoob.com/jsp/jstl-format-parsenumber-tag.html) | 解析一个代表着数字，货币或百分比的字符串 |
| [](https://www.runoob.com/jsp/jstl-format-formatdate-tag.html) | 使用指定的风格或模式格式化日期和时间     |
| [](https://www.runoob.com/jsp/jstl-format-parsedate-tag.html) | 解析一个代表着日期或时间的字符串         |
| [](https://www.runoob.com/jsp/jstl-format-bundle-tag.html)   | 绑定资源                                 |
| [](https://www.runoob.com/jsp/jstl-format-setlocale-tag.html) | 指定地区                                 |
| [](https://www.runoob.com/jsp/jstl-format-setbundle-tag.html) | 绑定资源                                 |
| [](https://www.runoob.com/jsp/jstl-format-timezone-tag.html) | 指定时区                                 |
| [](https://www.runoob.com/jsp/jstl-format-settimezone-tag.html) | 指定时区                                 |
| [](https://www.runoob.com/jsp/jstl-format-message-tag.html)  | 显示资源配置文件信息                     |
| [](https://www.runoob.com/jsp/jstl-format-requestencoding-tag.html) | 设置request的字符编码                    |

------

## SQL标签

JSTL SQL标签库提供了与关系型数据库（Oracle，MySQL，SQL Server等等）进行交互的标签。引用SQL标签库的语法如下：

```
<%@ taglib prefix="sql" 
           uri="http://java.sun.com/jsp/jstl/sql" %>
```

| 标签                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [](https://www.runoob.com/jsp/jstl-sql-setdatasource-tag.html) | 指定数据源                                                   |
| [](https://www.runoob.com/jsp/jstl-sql-query-tag.html)       | 运行SQL查询语句                                              |
| [](https://www.runoob.com/jsp/jstl-sql-update-tag.html)      | 运行SQL更新语句                                              |
| [](https://www.runoob.com/jsp/jstl-sql-param-tag.html)       | 将SQL语句中的参数设为指定值                                  |
| [](https://www.runoob.com/jsp/jstl-sql-dateparam-tag.html)   | 将SQL语句中的日期参数设为指定的java.util.Date 对象值         |
| [](https://www.runoob.com/jsp/jstl-sql-transaction-tag.html) | 在共享数据库连接中提供嵌套的数据库行为元素，将所有语句以一个事务的形式来运行 |

------

## XML 标签

JSTL XML标签库提供了创建和操作XML文档的标签。引用XML标签库的语法如下：

```
<%@ taglib prefix="x" 
           uri="http://java.sun.com/jsp/jstl/xml" %>
```

在使用xml标签前，你必须将XML 和 XPath 的相关包拷贝至你的<Tomcat 安装目录>\lib下:

+ XercesImpl.jar

  下载地址： http://www.apache.org/dist/xerces/j/

+ xalan.jar

  下载地址： http://xml.apache.org/xalan-j/index.html

| 标签                                                       | 描述                                                      |
| :--------------------------------------------------------- | :-------------------------------------------------------- |
| [](https://www.runoob.com/jsp/jstl-xml-out-tag.html)       | 与<%= ... >,类似，不过只用于XPath表达式                   |
| [](https://www.runoob.com/jsp/jstl-xml-parse-tag.html)     | 解析 XML 数据                                             |
| [](https://www.runoob.com/jsp/jstl-xml-set-tag.html)       | 设置XPath表达式                                           |
| [](https://www.runoob.com/jsp/jstl-xml-if-tag.html)        | 判断XPath表达式，若为真，则执行本体中的内容，否则跳过本体 |
| [](https://www.runoob.com/jsp/jstl-xml-foreach-tag.html)   | 迭代XML文档中的节点                                       |
| [](https://www.runoob.com/jsp/jstl-xml-choose-tag.html)    | <x:when>和<x:otherwise>的父标签                           |
| [](https://www.runoob.com/jsp/jstl-xml-choose-tag.html)    | <x:choose>的子标签，用来进行条件判断                      |
| [](https://www.runoob.com/jsp/jstl-xml-choose-tag.html)    | <x:choose>的子标签，当<x:when>判断为false时被执行         |
| [](https://www.runoob.com/jsp/jstl-xml-transform-tag.html) | 将XSL转换应用在XML文档中                                  |
| [](https://www.runoob.com/jsp/jstl-xml-param-tag.html)     | 与<x:transform>共同使用，用于设置XSL样式表                |

------

## JSTL函数

JSTL包含一系列标准函数，大部分是通用的字符串处理函数。引用JSTL函数库的语法如下：

```
<%@ taglib prefix="fn" 
           uri="http://java.sun.com/jsp/jstl/functions" %>
```

| 函数                                                         | 描述                                                     |
| :----------------------------------------------------------- | :------------------------------------------------------- |
| [fn:contains()](https://www.runoob.com/jsp/jstl-function-contains.html) | 测试输入的字符串是否包含指定的子串                       |
| [fn:containsIgnoreCase()](https://www.runoob.com/jsp/jstl-function-containsignoreCase.html) | 测试输入的字符串是否包含指定的子串，大小写不敏感         |
| [fn:endsWith()](https://www.runoob.com/jsp/jstl-function-endswith.html) | 测试输入的字符串是否以指定的后缀结尾                     |
| [fn:escapeXml()](https://www.runoob.com/jsp/jstl-function-escapexml.html) | 跳过可以作为XML标记的字符                                |
| [fn:indexOf()](https://www.runoob.com/jsp/jstl-function-indexof.html) | 返回指定字符串在输入字符串中出现的位置                   |
| [fn:join()](https://www.runoob.com/jsp/jstl-function-join.html) | 将数组中的元素合成一个字符串然后输出                     |
| [fn:length()](https://www.runoob.com/jsp/jstl-function-length.html) | 返回字符串长度                                           |
| [fn:replace()](https://www.runoob.com/jsp/jstl-function-replace.html) | 将输入字符串中指定的位置替换为指定的字符串然后返回       |
| [fn:split()](https://www.runoob.com/jsp/jstl-function-split.html) | 将字符串用指定的分隔符分隔然后组成一个子字符串数组并返回 |
| [fn:startsWith()](https://www.runoob.com/jsp/jstl-function-startswith.html) | 测试输入字符串是否以指定的前缀开始                       |
| [fn:substring()](https://www.runoob.com/jsp/jstl-function-substring.html) | 返回字符串的子集                                         |
| [fn:substringAfter()](https://www.runoob.com/jsp/jstl-function-substringafter.html) | 返回字符串在指定子串之后的子集                           |
| [fn:substringBefore()](https://www.runoob.com/jsp/jstl-function-substringbefore.html) | 返回字符串在指定子串之前的子集                           |
| [fn:toLowerCase()](https://www.runoob.com/jsp/jstl-function-tolowercase.html) | 将字符串中的字符转为小写                                 |
| [fn:toUpperCase()](https://www.runoob.com/jsp/jstl-function-touppercase.html) | 将字符串中的字符转为大写                                 |
| [fn:trim()](https://www.runoob.com/jsp/jstl-function-trim.html) | 移除首尾的空白符                                         |

> JSTL 1.1 与 JSTL 1.2 之间的区别？如何下载 JSTL 1.2?
>
> JSTL 1.2 中不要求 standard.jar 包。
>
> 您可以在 Maven 中央仓库中找到它们。
>
> http://repo2.maven.org/maven2/javax/servlet/jstl/
>
> http://repo2.maven.org/maven2/taglibs/standard/
>
> 由于`JSTL 1.1`已经过时，Apache已将其置于[存档中](http://archive.apache.org/dist/jakarta/taglibs/standard/)。选择`jakarta-taglibs-standard-current.zip`文件。但是，如果您正在运行`Servlet 2.5`兼容容器并且`web.xml`声明为至少`Servlet 2.5`，那么您应该能够使用新的[JSTL 1.2](http://central.maven.org/maven2/javax/servlet/jstl/1.2/jstl-1.2.jar)。需要注意的是`JSTL 1.2`并没有要求`standard.jar`。