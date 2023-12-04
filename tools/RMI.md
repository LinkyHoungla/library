# 概述

+ **定义**：`Java远程方法调用(RMI)`系统允许在一个Java虚拟机中运行的对象调用在另一个`Java`虚拟机中运行的对象的方法。RMI提供了用`Java`编程语言编写的程序之间的远程通信。

***注**：如果要连接到现有的`IDL`程序，应该使用`Java IDL`而不是RMI。*

本文简要概述了`RMI`系统，然后介绍了一个完整的客户端/服务器示例，该示例使用`RMI`的独特功能在运行时加载和执行用户定义的任务。示例中的服务器实现了一个通用的计算引擎，客户端使用该引擎计算 *[Math Processing Error]�*符号的值。

**定义**：**RMI应用程序**通常包含两个独立的程序，一个服务端和一个客户端。

+ + 典型的**服务端程序**会创建一些远程对象，使这些对象的引用可被访问，并等待客户端调用这些对象上的方法。
  + 典型的**客户端程序**会获得对服务器上一个或多个远程对象的远程引用，然后调用这些对象上的方法。RMI提供了服务器和客户机之间通信和来回传递信息的机制。这样的应用程序有时被称为分布式对象应用程序。

+ **分布式对象应用程序**的功能:

+ + **定位远程对象**。应用程序可以使用各种机制来获取对远程对象的引用。例如，一个应用程序可以用RMI的简单命名工具——RMI注册表来注册它的远程对象。或者，应用程序可以作为其他远程调用的一部分传递和返回远程对象引用。
  + **与远程对象通信**。远程对象之间通信的细节由RMI处理。对于程序员来说，远程通信类似于普通的Java方法调用。
  + **加载传递的对象的类定义**。因为RMI允许对象来回传递，所以它提供了加载对象的类定义以及传输对象数据的机制。



下面的插图描述了一个`RMI分布式应用程序`，它使用`RMI注册表`来获取对远程对象的引用。

+ 服务器调用注册表将名称与远程对象关联（或绑定）。
+ 客户端通过服务器注册表中的名称查找远程对象，然后在其上调用一个方法。
+ 图中还显示了`RMI`系统使用一个现有的`web`服务器来加载类定义，从服务器到客户端，从客户端到服务器，在需要的时候加载对象。

![img](https://pic2.zhimg.com/80/v2-30371bf951791a3371587970ce8facad_720w.webp)

`RMI`的核心和独特的特性之一是，如果对象的类没有在接收方的`Java`虚拟机中定义，它就能够下载对象的类的定义。对象的所有类型和行为（以前只能在单个`Java`虚拟机中使用）都可以传输到另一个（可能是远程的）`Java`虚拟机。RMI通过对象的实际类传递对象，因此当对象被发送到另一个`Java`虚拟机时，对象的行为不会改变。该功能允许将新的类型和行为引入远程`Java`虚拟机，从而动态扩展应用程序的行为。本文中的计算引擎示例使用此功能为分布式程序引入新的行为。

与任何其他`Java`应用程序一样，使用`Java RMI`构建的分布式应用程序由接口和类组成。接口声明方法。这些类实现接口中声明的方法，可能还声明其他方法。在分布式应用程序中，一些实现可能驻留在一些`Java`虚拟机中，而不是其他虚拟机。具有可以跨`Java`虚拟机调用的方法的对象称为远程对象。

对象通过实现远程接口而成为远程对象，远程接口具有以下特征:

+ 远程接口扩展了`java.rmi.Remote`接口。
+ 接口的每个方法在它的`throws`子句中都声明了`java.rmi.RemoteException`，以及任何特定于应用程序的异常。

当对象从一个`Java`虚拟机传递到另一个`Java`虚拟机时，`RMI`将远程对象与非远程对象区别对待。`RMI`不是在接收方的`Java`虚拟机中复制实现对象，而是为远程对象传递`远程存根（Stub）`。存根充当远程对象的本地代表或代理，对于客户端来说，它基本上是远程引用。客户端调用本地存根上的方法，该方法负责对远程对象执行方法调用。

远程对象的**存根**（Stub）实现远程对象实现的同一组远程接口。此属性允许将存根转换为远程对象实现的任何接口。但是，只有在远程接口中定义的那些方法可以从接收的Java虚拟机中调用。

`图2-3`能够更加清晰地描述RMI的交互过程。

![img](https://pic4.zhimg.com/80/v2-1ac4b21dfa026fc9272586a8c4e69df7_720w.webp)

![img](https://pic2.zhimg.com/80/v2-46b558c84de770b013fc015f362c9429_720w.webp)
