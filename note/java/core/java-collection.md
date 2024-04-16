# 概述

早在 Java 2 中之前，Java 就提供了特设类。比如：Dictionary, Vector, Stack, 和 Properties 这些类用来存储和操作对象组。

虽然这些类都非常有用，但是它们缺少一个核心的，统一的主题。由于这个原因，使用 Vector 类的方式和使用 Properties 类的方式有着很大不同。

为此，整个集合框架就围绕一组标准接口而设计。你可以直接使用这些接口的标准实现，诸如： LinkedList, HashSet, 和 TreeSet 等,除此之外你也可以通过这些接口实现自己的集合。

Java 集合框架主要包括两种类型的容器，一种是集合（Collection），存储一个元素集合，另一种是图（Map），存储键/值对映射。

Collection 接口又有 3 种子类型，List、Set 和 Queue，再下面是一些抽象类，最后是具体实现类，常用的有 ArrayList、LinkedList、HashSet、LinkedHashSet、HashMap、LinkedHashMap 等等。

集合框架是一个用来代表和操纵集合的统一架构。所有的集合框架都包含如下内容：

+ **接口**
  + 代表集合的抽象数据类型；之所以定义多个接口，是为了以不同的方式操作集合对象
  + 例如：Collection、List、Set、Map
+ **实现（类）**
  + 是集合接口的具体实现；从本质上讲，它们是可重复使用的数据结构
  + 例如：ArrayList、LinkedList、HashSet、HashMap 
+ **算法**
  + 是实现集合接口的对象里的方法执行的一些有用的计算；这些算法实现了多态，那是因为相同的方法可以在相似的接口上有着不同的实现
  + 例如：搜索和排序

除了集合，该框架也定义了几个 Map 接口和类。Map 里存储的是键/值对。尽管 Map 不是集合，但是它们完全整合在集合中。

# 接口

![img](https://www.runoob.com/wp-content/uploads/2014/01/2243690-9cd9c896e0d512ed.gif)

## Collection

Collection 是最基本的集合接口，一个 Collection 代表一组 Object，即 Collection 的元素, Java不提供直接继承自Collection 的类，只提供继承于的子接口（如 List 和 set ）。

Collection 接口存储一组不唯一，无序的对象。

## List

List 接口是一个有序的 Collection，使用此接口能够精确的控制每个元素插入的位置，能够通过索引（元素在 List 中位置，类似于数组的下标）来访问List中的元素，第一个元素的索引为 0，而且允许有相同的元素。

List 接口存储一组不唯一，有序（插入顺序）的对象。

## Set

Set 具有与 Collection 完全一样的接口，只是行为上不同，Set 不保存重复的元素。

Set 接口存储一组唯一，无序的对象。

### SortedSet

继承于Set保存有序的集合。

## Map

Map 接口存储一组键值对象，提供 key（键）到 value（值）的映射。

### Map.Entry

描述在一个 Map 中的一个元素（键/值对）。是一个 Map 的内部接口。

### SortedMap

继承于 Map，使 Key 保持在升序排列。

## Enumeration

这是一个传统的接口和定义的方法，通过它可以枚举（一次获得一个）对象集合中的元素。这个传统接口已被迭代器取代。

**Set 和 List 的区别**
> 1. Set 接口实例存储的是无序的，不重复的数据。List 接口实例存储的是有序的，可以重复的元素。
> 2. Set 检索效率低下，删除和插入效率高，插入和删除不会引起元素位置改变；实现类有 HashSet, TreeSet 。
> 3. List 和数组类似，可以动态增长，根据实际存储的数据的长度自动增长 List 的长度。查找元素效率高，插入删除效率低，因为会引起其他元素位置改变；实现类有 ArrayList, LinkedList, Vector 。

# 实现类

Java 提供了一套实现了 Collection 接口的标准集合类。其中一些是具体类，这些类可以直接拿来使用，而另外一些是抽象类，提供了接口的部分实现。

![img](https://pic2.zhimg.com/v2-3cde2f6c4e1b5ba7f4d776da9ed943ed_r.jpg)

## AbstractCollection

实现了大部分的集合接口。

## AbstractList

继承于 AbstractCollection 并且实现了大部分 List 接口。

### Vector

该类和 ArrayList 非常相似，但是该类是同步的，可以用在多线程的情况，该类允许设置默认的增长长度，默认扩容方式为原来的 2 倍。

#### Stack

栈是 Vector 的一个子类，它实现了一个标准的后进先出的栈。

### AbstractSequentialList
继承于 AbstractList ，提供了对数据元素的链式访问而不是随机访问。

#### LinkedList

该类实现了 List 接口，允许有 null（空）元素。主要用于创建链表数据结构，该类没有同步方法，如果多个线程同时访问一个List，则必须自己实现访问同步，解决方法就是在创建 List 时候构造一个同步的List。

例如：

`List list=Collections.synchronizedList(newLinkedList(...));`

LinkedList 查找效率低。

### ArrayList

该类也是实现了 List 的接口，实现了可变大小的数组，随机访问和遍历元素时，提供更好的性能。该类也是非同步的，在多线程的情况下不要使用。ArrayList 增长当前长度的 50%，插入删除效率低。

## AbstractSet

继承于AbstractCollection 并且实现了大部分 Set 接口。

### HashSet

该类实现了 Set 接口，不允许出现重复元素，不保证集合中元素的顺序，允许包含值为 null 的元素，但最多只能一个。

#### LinkedHashSet

具有可预知迭代顺序的 Set 接口的哈希表和链接列表实现。

### TreeSet

该类实现了 Set 接口，可以实现排序等功能。

## AbstractMap

实现了大部分的 Map 接口。

### HashMap

HashMap 是一个散列表，它存储的内容是键值对（key - value）映射。
该类实现了 Map 接口，根据键的 HashCode 值存储数据，具有很快的访问速度，最多允许一条记录的键为 null，不支持线程同步。

#### LinkedHashMap

继承于 HashMap，使用元素的自然顺序对元素进行排序.

### TreeMap

继承了 AbstractMap，并且使用一颗树。

### WeakHashMap

继承 AbstractMap 类，使用弱密钥的哈希表。

### IdentityHashMap

继承 AbstractMap 类，比较文档时使用引用相等。

## Dictionary

Dictionary 类是一个抽象类，用来存储键/值对，作用和 Map 类相似。

### Hashtable

Hashtable 是 Dictionary（字典）类的子类，位于 java.util 包中。

#### Properties
Properties 继承于 Hashtable，表示一个持久的属性集，属性列表中每个键及其对应值都是一个字符串。

### BitSet
一个Bitset类创建一种特殊类型的数组来保存位值。BitSet中数组大小会随需要增加。

# ArrayList

ArrayList 是 Java 中提供的动态数组实现，它继承自 AbstractList 并实现了 List 接口。ArrayList 位于 `java.util` 包中，使用前需要引入：

```java
import java.util.ArrayList; // 引入 ArrayList 类
```

## 基本方法

### 初始化

```java
ArrayList<E> list = new ArrayList<>(); // 初始化 ArrayList
```

+ **E**: 泛型数据类型，用于设置 ArrayList 中元素的数据类型，只能为引用数据类型。
+ **list**: 对象名，可以根据需要自定义。

### 添加元素

使用 `add()` 方法向 ArrayList 中添加元素：

```java
list.add(element); // 向 ArrayList 中添加元素
```

### 访问元素

使用 `get()` 方法访问 ArrayList 中的元素：

```java
E element = list.get(index); // 获取 ArrayList 中指定索引位置的元素
```

### 修改元素

使用 `set()` 方法修改 ArrayList 中的元素：

```java
list.set(index, newElement); // 修改 ArrayList 中指定索引位置的元素
```

### 删除元素

使用 `remove()` 方法删除 ArrayList 中的元素：

```java
list.remove(index); // 删除 ArrayList 中指定索引位置的元素
```

### 计算大小

使用 `size()` 方法计算 ArrayList 中的元素数量：

```java
int size = list.size(); // 获取 ArrayList 中的元素数量
```

### 迭代

使用 `for` 循环迭代 ArrayList 中的元素：

```java
for (int i = 0; i < list.size(); i++) {
    E element = list.get(i);
    // 对元素进行操作
}
```

或者使用增强型 `for-each` 循环：

```java
for (E element : list) {
    // 对元素进行操作
}
```

### 排序

使用 `Collections.sort()` 方法对 ArrayList 中的元素进行排序：

```java
import java.util.Collections;

Collections.sort(list); // 对 ArrayList 中的元素进行排序
```

## 常用方法


|        方法        |                       描述                        |
| :----------------: |:-----------------------------------------------:|
|      `add()`       |             将元素插入到指定位置的 ArrayList 中             |
|     `addAll()`     |             添加集合中的所有元素到 ArrayList 中             |
|     `clear()`      |               删除 ArrayList 中的所有元素               |
|     `clone()`      |                 复制一份 ArrayList                  |
|    `contains()`    |               判断元素是否在 ArrayList 中               |
|      `get()`       |             通过索引值获取 ArrayList 中的元素              |
|    `indexOf()`     |              返回 ArrayList 中元素的索引值               |
|   `removeAll()`    |          删除存在于指定集合中的 ArrayList 中的所有元素           |
|     `remove()`     |               删除 ArrayList 中的单个元素               |
|      `size()`      |               返回 ArrayList 中元素数量                |
|    `isEmpty()`     |                判断 ArrayList 是否为空                |
|    `subList()`     |               截取部分 ArrayList 的元素                |
|      `set()`       |              替换 ArrayList 中指定索引的元素              |
|      `sort()`      |               对 ArrayList 元素进行排序                |
|    `toArray()`     |                将 ArrayList 转换为数组                |
|    `toString()`    |               将 ArrayList 转换为字符串                |
| `ensureCapacity()` |               设置 ArrayList 的容量大小                |
|  `lastIndexOf()`   |          返回指定元素在 ArrayList 中最后一次出现的位置           |
|   `retainAll()`    |          保留 ArrayList 中在指定集合中也存在的那些元素           |
|  `containsAll()`   |           查看 ArrayList 是否包含指定集合中的所有元素           |
|   `trimToSize()`   |           将 ArrayList 中的容量调整为数组中的元素个数           |
|  `removeRange()`   |            删除 ArrayList 中指定索引范围内的元素             |
|   `replaceAll()`   |          将给定的操作内容替换掉 ArrayList 中每一个元素           |
|    `removeIf()`    |            删除所有满足特定条件的 ArrayList 元素             |
|    `forEach()`     |           遍历 ArrayList 中每一个元素并执行特定操作            |
