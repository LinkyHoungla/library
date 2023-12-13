# 基础

超文本标记语言（HyperText Markup Language，HTML）是一种用于创建网页的标准标记语言。

您可以使用 HTML 来建立自己的 WEB 站点，HTML 运行在浏览器上，由浏览器来解析。

> 注意：对于中文网页需要使用 `<meta charset="utf-8">` 声明编码，否则会出现乱码。有些浏览器（如 360 浏览器）会设置 GBK 为默认编码，则你需要设置为 `<meta charset="gbk">`。

## 版本

| 版本 | 发布时间 |
| :---: | :---: |
| HTML | 1991 |
| HTML+ | 1993 |
| HTML 2.0 | 1995 |
| HTML 3.2 | 1997 |
| HTML 4.01 | 1999 |
| XHTML 1.0 | 2000 |
| HTML5 | 2012 |
| XHTML5 | 2013 |

### 声明

`<!DOCTYPE>` 声明有助于浏览器中正确显示网页。

网络上有很多不同的文件，如果能够正确声明 HTML 的版本，浏览器就能正确显示网页内容。

> doctype 声明是不区分大小写的

**HTML5**：

```HTML 
<!DOCTYPE html>
```

**HTML 4.01**：

```HTML
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

**XHTML 1.0**：

```HTML
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

> 在HTML 4.01 中，`<!DOCTYPE>` 声明需引用 DTD （文档类型声明），因为 HTML 4.01 是基于 SGML（Standard Generalized Markup Language 标准通用标记语言）。
>
> HTML 4.01 规定了三种不同的 `<!DOCTYPE>` 声明，分别是：Strict、Transitional 和 Frameset。
>
> HTML5 不是基于 SGML，因此不要求引用 DTD。

> 对于设置 `<meta charset="utf-8" />` 后出现网页乱码问题，其实归根到底就是：你通过 meta 标签设置的编码和网页文件在保存时所使用的文档编码不相同造成的！
>
> 保存 html 文件时，文档编码和 meta 设置的编码，一定要相同，只要不相同，就一定会出现乱码！
>
> 之所以一定要写上 `<!doctype html>`，就是为了防止浏览器的怪异模式，强制浏览器按照标准模式渲染网页！

## 元素

HTML 文档由 HTML 元素定义。

大多数 HTML 元素可以嵌套（HTML 元素可以包含其他 HTML 元素）。HTML 文档由相互嵌套的 HTML 元素构成。

**语法**

+ HTML 元素以开始标签起始
+ HTML 元素以结束标签终止
+ 元素的内容是开始标签与结束标签之间的内容
+ 某些 HTML 元素具有空内容（empty content）
+ 空元素在开始标签中进行关闭（以开始标签的结束而结束）
+ 大多数 HTML 元素可拥有属性

HTML 标签对大小写不敏感；许多网站都使用大写的 HTML 标签，但万维网联盟（W3C）在 HTML 4 中推荐使用小写，而在未来 (X)HTML 版本中强制使用小写。

### 空元素

没有内容的 HTML 元素被称为空元素。空元素是在开始标签中关闭的。

> `<br>` 就是没有关闭标签的空元素（`<br>` 标签定义换行）。

在 XHTML、XML 以及未来版本的 HTML 中，所有元素都必须被关闭。

在开始标签中添加斜杠，比如 `<br />`，是关闭空元素的正确方法，HTML、XHTML 和 XML 都接受这种方式。

## 属性

属性是 HTML 元素提供的附加信息。

+ HTML 元素可以设置属性
+ 属性可以在元素中添加附加信息
+ 属性一般描述于开始标签
+ 属性总是以名称/值对的形式出现，比如：name="value"。

属性值应该始终被包括在引号内。双引号是最常用的，不过使用单引号也没有问题。

> 在某些个别的情况下，比如属性值本身就含有双引号，那么必须使用单引号。

属性和属性值对大小写不敏感，万维网联盟在其 HTML 4 推荐标准中推荐小写的属性/属性值；而新版本的 (X)HTML 要求使用小写属性。

**常用属性**

| 属性 | 描述 |
| :---: | :---- |
| class | 为html元素定义一个或多个类名（classname）(类名从样式文件引入) |
|  id   | 定义元素的唯一id |
| style | 规定元素的行内样式（inline style） |
| title | 描述了元素的额外信息 (作为工具条使用) |

# 布局

