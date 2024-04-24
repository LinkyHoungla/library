# 数组

## 数组拷贝

以下几种方法都是浅拷贝（浅复制）。

浅拷贝只是复制了对象的引用地址，两个对象指向同一个内存地址，所以修改其中任意的值，另一个值都会随之变化。深拷贝是将对象及值复制过来，两个对象修改其中任意的值另一个值不会改变。

**注意**

+ 如果新数组的长度小于源数组的长度，那么新数组将截取源数组的前几个元素。
+ 如果源数组中的元素是对象引用，**那么新数组中的元素将仍然引用相同的对象，这意味着对新数组的修改可能会影响到源数组。**
+ 如果源数组包含基本数据类型（如int、char等），**新数组将包含这些基本数据类型的默认值，如0或'\0'**。

### Arrays.copyOf()

**不转换类型**

```java
copyOf(T[] original, int newLength)
```

这个方法用于复制原始数组 `original` 的前 `newLength` 个元素到一个新数组中。如果 `newLength` 大于原始数组的长度，新数组会填充默认值；如果 `newLength` 小于原始数组的长度，只复制原始数组的前 `newLength` 个元素。

**转换类型**

```java
copyOf(U[] original, int newLength, Class<? extends T[]> newType)
```

+ `original`：要复制的原始数组。
+ `newLength`：新数组的长度，可以比原始数组的长度长或短。
+ `newType`：新数组的类型，通常是一个数组类的 `Class` 对象，用于确定新数组的类型。

这个方法也是复制原始数组 `original` 的前 `newLength` 个元素到一个新数组中，但可以指定新数组的类型为 `newType`。这允许在复制过程中对数组元素进行类型转换。

#### 源码解析：

```java
public static <T,U> T[] copyOf(U[] original, int newLength, Class<? extends T[]> newType) {
    @SuppressWarnings("unchecked")
    T[] copy = ((Object)newType == (Object)Object[].class)
        ? (T[]) new Object[newLength]
        : (T[]) Array.newInstance(newType.getComponentType(), newLength);
    System.arraycopy(original, 0, copy, 0, Math.min(original.length, newLength));
    return copy;
}
```

+ `@SuppressWarnings("unchecked")`：这个注解用于告诉编译器忽略未经检查的转换警告，因为在这个方法中进行了类型转换。
+ `copy` 的类型根据 `newType` 的值确定，如果 `newType` 是 `Object[].class`，则创建一个 `Object` 类型的新数组；否则，使用 `Array.newInstance()` 方法根据 `newType` 的组件类型创建一个新数组。
+ `System.arraycopy(original, 0, copy, 0, Math.min(original.length, newLength))`：这个方法用于将原始数组的元素复制到新数组中，复制的元素数量由原始数组长度和 `newLength` 中较小的那个确定。

>  目标数组如果已经存在，将会被重构。

### Arrays.CopyOfRange()

**不转换类型**

```java
copyOfRange(U[] original, int from, int to)
```

+ `original`：要复制元素的原始数组。
+ `from`：要复制的范围的起始索引（包括）。
+ `to`：要复制的范围的结束索引（不包括）。

这个方法用于复制原始数组 `original` 中从 `from` 索引（包括）到 `to` 索引（不包括）的元素到一个新数组中。

**转换类型**

```java
copyOfRange(U[] original, int from, int to, Class<? extends T[]> newType)
```

+ `original`：要复制元素的原始数组。
+ `from`：要复制的范围的起始索引（包括）。
+ `to`：要复制的范围的结束索引（不包括）。
+ `newType`：新数组的类型，通常是一个数组类的 `Class` 对象，用于确定新数组的类型。

这个方法也是复制原始数组 `original` 中从 `from` 索引（包括）到 `to` 索引（不包括）的元素到一个新数组中，并可以指定新数组的类型为 `newType`。

#### 源码解析：

```java
public static <T,U> T[] copyOfRange(U[] original, int from, int to, Class<? extends T[]> newType) {
    int newLength = to - from;
    if (newLength < 0)
        throw new IllegalArgumentException(from + " > " + to);
    @SuppressWarnings("unchecked")
    T[] copy = ((Object)newType == (Object)Object[].class)
        ? (T[]) new Object[newLength]
        : (T[]) Array.newInstance(newType.getComponentType(), newLength);
    System.arraycopy(original, from, copy, 0, Math.min(original.length - from, newLength));
    return copy;
}
```

+ `int newLength = to - from;`：计算新数组的长度，基于指定的 `from` 和 `to` 索引。
+ `if (newLength < 0) throw new IllegalArgumentException(from + " > " + to);`：检查 `newLength` 是否为负数（即 `from` 是否大于 `to`）。如果满足条件，抛出 `IllegalArgumentException`，指示 `from` 索引大于 `to` 索引。
+ `@SuppressWarnings("unchecked")`：抑制未经检查的类型转换警告。
+ `T[] copy = ((Object)newType == (Object)Object[].class) ? (T[]) new Object[newLength] : (T[]) Array.newInstance(newType.getComponentType(), newLength);`：根据指定的 `newType` 创建一个新数组 `copy`。如果 `newType` 等于 `Object[].class`，则创建一个 `Object` 类型的新数组，长度为 `newLength`；否则，使用 `Array.newInstance` 方法根据 `newType` 的组件类型创建一个新数组，长度为 `newLength`。
+ `System.arraycopy(original, from, copy, 0, Math.min(original.length - from, newLength));`：使用 `System.arraycopy()` 方法执行实际的数组复制操作。它将元素从 `original` 数组的 `from` 索引开始复制到 `copy` 数组的 `0` 索引开始的位置。要复制的元素数量由 `Math.min(original.length - from, newLength)` 确定，以确保仅复制指定的范围内的元素。

> 目标数组如果已经存在，将会被重构。

### System.arraycopy()

Java 中的 `System.arraycopy()` 方法是一个本地方法，其实际实现由 Java 虚拟机的底层提供。

```java
public static native void arraycopy(Object src, int srcPos,
                                    Object dest, int destPos,
                                    int length);
```

+ `src`：源数组
+ `srcPos`：源数组中的起始位置
+ `dest`：目标数组
+ `destPos`：目标数组中的起始位置
+ `length`：要复制的元素个数

`System.arraycopy()` 方法由底层代码实现，能够利用硬件的特性进行快速的数据复制，因此性能非常高效。它通常比使用循环逐个复制数组元素要快得多。

**注意**

+ `System.arraycopy()` 可以用于向上或向下转型，但在使用时要谨慎，确保数据类型兼容性和运行时类型检查。
+ 如果数据类型不匹配，尽管可以通过编译，但在运行时会抛出 `java.lang.ArrayStoreException` 异常。
+ 最好避免不必要的类型转换，以保持代码的清晰性和可维护性。

+ `srcIndex + length` 必须小于等于 `srcArray.length`
+ `destIndex + length` 必须小于等于 `destArray.length`

> 目标数组必须已经存在，且不会被重构，相当于替换目标数组中的部分元素。

### clone()

Java 中的 `clone()` 方法可以用于复制数组，它是 `Object` 类中定义的方法，可以创建一个具有单独内存空间的对象。由于数组也是一个 `Object` 类，因此可以使用数组对象的 `clone()` 方法来复制数组。

+ `clone()` 方法返回的是 `Object` 类型，需要使用强制类型转换为适当的数组类型。
+ 使用 `clone()` 方法可以创建一个与原始数组具有相同内容的新数组，但在内存中是独立的。

**注意**

+ `clone()` 方法返回的是一个新的数组对象，原始数组和复制后的数组是独立的，修改一个数组不会影响另一个数组。
+ 目标数组如果已经存在，将会被重构为复制后的数组对象。

使用 `clone()` 方法可以快速方便地复制数组，但需要注意对返回值进行类型转换，并确保新数组与原始数组内容相同但独立存在。