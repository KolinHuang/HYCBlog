---
title: Java源码分析之ArrayList
author: Kol Huang
date: 2020-09-16 18:27:00 +0800
categories: [Blogging, 容器]
tags: [Java源码学习]
comments: true
math: true
---



## 类前注释

先看源码开始的一大段注释，基本上把ArrayList的功能和实现介绍了一下：

```java
/**
Resizable-array implementation of the List interface.  Implements
all optional list operations, and permits all elements, including
null.  In addition to implementing the List interface,
this class provides methods to manipulate the size of the array that is
used internally to store the list.  (This class is roughly equivalent to
Vector, except that it is unsynchronized.)
 List 接口的可调整大小的数组实现。 实现所有可选的列表操作，并允许所有元素，包括null。
除了实现List接口之外，此类还提供一些方法来操纵内部用于存储列表的数组的大小。 （此类与Vector大致等效，但它是不同步的。）
划重点！可调整大小！提供了一些方法来操纵数组的大小！
 *
The size, isEmpty, get, set,
iterator, and listIterator operations run in constant
time.  The add operation runs in amortized constant time,
that is, adding n elements requires O(n) time.  All of the other operations
run in linear time (roughly speaking).  The constant factor is low compared
to that for the LinkedList implementation.
size isEmpty，get，set，iterator和listIterator方法在恒定时间内运行。
 添加方法以固定的时间运行，也就是说，添加n个元素需要O（n）时间。 
 所有其他操作均以线性时间运行（大致而言）。 与LinkedList实现相比，常数因子较低。
 *
Each ArrayList instance has a capacity.  The capacity is
the size of the array used to store the elements in the list.  It is always
at least as large as the list size.  As elements are added to an ArrayList,
its capacity grows automatically.  The details of the growth policy are not
specified beyond the fact that adding an element has constant amortized
time cost.
每个ArrayList实例都有一个容量。 容量是用于在列表中存储元素的数组的大小。 
它总是至少与列表大小一样大。 将元素添加到ArrayList时，其容量会自动增长。
 除了添加元素具有固定的摊销时间成本外，没有指定增长策略的详细信息。

 *
An application can increase the capacity of an ArrayList instance
before adding a large number of elements using the ensureCapacity
operation.  This may reduce the amount of incremental reallocation.
应用程序可以通过使用sureCapacity方法在添加大量元素之前增加ArrayList实例的容量。 这可以减少增量重新分配的数量。
sureCapacity这个方法是做什么的，以及如何做的？
 *

<strong>Note that this implementation is not synchronized.</strong>
If multiple threads access an ArrayList instance concurrently,
and at least one of the threads modifies the list structurally, it
must be synchronized externally.  (A structural modification is
any operation that adds or deletes one or more elements, or explicitly
resizes the backing array; merely setting the value of an element is not
a structural modification.)  This is typically accomplished by
synchronizing on some object that naturally encapsulates the list.
请注意，此实现未同步。 如果多个线程同时访问ArrayList实例，并且至少有一个线程在结构上修改列表，则必须在外部进行同步。
（结构修改是添加或删除一个或多个元素或显式调整后备数组大小的任何操作；仅设置元素的值不是结构修改。）
通常，通过在自然封装列表的某个对象上进行同步来完成此操作。


 *
If no such object exists, the list should be "wrapped" using the
{@link Collections#synchronizedList Collections.synchronizedList}
method.  This is best done at creation time, to prevent accidental
unsynchronized access to the list:<pre>
  List list = Collections.synchronizedList(new ArrayList(...));</pre>
如果不存在这样的对象，则应使用{@link Collections＃synchronizedList Collections.synchronizedList}方法“包装”列表。
最好在创建时完成此操作，以防止意外的不同步访问列表：
List list = Collections.synchronizedList(new ArrayList(...));
Collections.synchronizedList()方法研究一下

 *
 <a name="fail-fast">
 The iterators returned by this class's {@link #iterator() iterator} and
 {@link #listIterator(int) listIterator} methods are <em>fail-fast</em>:</a>
 if the list is structurally modified at any time after the iterator is
 created, in any way except through the iterator's own
 {@link ListIterator#remove() remove} or
 {@link ListIterator#add(Object) add} methods, the iterator will throw a
 {@link ConcurrentModificationException}.  Thus, in the face of
 concurrent modification, the iterator fails quickly and cleanly, rather
 than risking arbitrary, non-deterministic behavior at an undetermined
 time in the future.
 在创建迭代器之后的任何时间如果列表在结构上有任何修改，
则此类的{@link #iterator（）迭代器}和{@link #listIterator（int）listIterator}方法返回的迭代器会fail-fast。
除了通过迭代器自己的{@link ListIterator＃remove（）remove}或{@link ListIterator＃add（Object）add}方法）之外，
迭代器都会抛出{@link ConcurrentModificationException}。 
因此，面对并发修改，迭代器会快速抛出异常，不会冒任何风险。

 *
Note that the fail-fast behavior of an iterator cannot be guaranteed
as it is, generally speaking, impossible to make any hard guarantees in the
presence of unsynchronized concurrent modification.  Fail-fast iterators
throw {@code ConcurrentModificationException} on a best-effort basis.
Therefore, it would be wrong to write a program that depended on this
exception for its correctness:  the fail-fast behavior of iterators
should be used only to detect bugs.

注意，不能保证迭代器的快速失败行为，
因为通常来说，在存在不同步的并发修改的情况下，不可能做出任何严格的保证。
快速失败的迭代器会尽最大努力抛出{@code ConcurrentModificationException}。 
因此，编写依赖于此异常的程序的正确性是错误的：迭代器的快速失败行为应仅用于检测错误。
就是说不能依赖这个异常来编写任何相关判断性的程序片段，这个异常的行为是不确定的，有可能发生了结构性修改，但是没有抛出异常。
```

从上面的注释可以看出几个要点：

* ArrayList通过一些方法来调整数组的大小。

* 添加n个元素需要`O(n)`的时间。`size(),isEmpty(),get(),set(),iterator()和listIterator()`方法在恒定时间内运行。与LinkedList实现相比，常数因子较低。

* 应用程序可以通过使用sureCapacity方法在添加**大量元素**之前增加ArrayList实例的容量。 这可以减少**增量重新分配的数量。**

* 这个容器实现是非同步的，如果多个线程访问ArrayList实例，就需要在外部进行同步。可以用下面这种方法：

  * ```java
    List list = Collections.synchronizedList(new ArrayList(...));
    ```

* 在**创建迭代器之后**，出了迭代器自带的`remove`和`add`方法之外，**其他对列表有任何结构性修改的行为**都会导致程序抛出`ConcurrentModificationException`异常。但是不能保证一定会抛出这个异常，所以不能根据这个异常来编写任何判断性的程序片段。



## 变量

这个类开头定义了如下变量（部分）：

```java
private static final long serialVersionUID = 8683452581122892189L;
//初始化时的默认容量。
private static final int DEFAULT_CAPACITY = 10;
//用于空实例的共享空数组实例。这个没懂是啥
private static final Object[] EMPTY_ELEMENTDATA = {};


//共享的空数组实例，用于默认大小的空实例。 我们将此与EMPTY_ELEMENTDATA区别开来，以了解添加第一个元素时需要膨胀多少。
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

/**
存储ArrayList元素的数组缓冲区。ArrayList的容量是此数组缓冲区的长度。 
添加第一个元素时，任何具有elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA的空ArrayList
都将扩展为DEFAULT_CAPACITY。
*/
transient Object[] elementData; //非私有以简化嵌套类访问
private int size;
```

几个要点：

* 当初始化时未指定容量，默认的容量为10。
* `EMPTY_ELEMENTDATA`用于有参构造器，参数为0的初始化；`DEFAULTCAPACITY_EMPTY_ELEMENTDATA`用于无参构造器的初始化。
* `elementData`定义为非私有，来简化嵌套类的访问。这边使用了关键字`transient`。简单地介绍一下：
  * 一旦变量被transient修饰，变量将不再是对象持久化的一部分，该变量内容在序列化后无法获得访问。
  * transient关键字**只能修饰变量，而**不能修饰方法和类。注意，本地变量是不能被transient关键字修饰的。变量如果是用户自定义类变量，则该类需要实现Serializable接口。
  * 被transient关键字修饰的变量不再能被序列化，一个静态变量不管是否被transient修饰，均不能被序列化。
  * 在Java中，对象的序列化可以通过实现两种接口来实现，若实现**的是Serializable接口，**则所有的序列化将**会自动进行**，若实现的是**Externalizable接口**，则没有任何东西可以自动序列化，**需要在writeExternal方法中进行手工指定所要序列化的变量**，**这与是否被transient修饰无关。**



### 关于modCount变量

几乎在所有线程不安全的容器中都有这么一个变量`modCount`，记录了修改容器结构的操作次数。例如在ArrayList中，modCount在迭代器实现中使用：

```java
/**
     * An optimized version of AbstractList.Itr
     */
private class Itr implements Iterator<E> {
    int cursor;       // index of next element to return
    int lastRet = -1; // index of last element returned; -1 if no such
    int expectedModCount = modCount;

    Itr() {}

    public boolean hasNext() {
      return cursor != size;
  }

  @SuppressWarnings("unchecked")
  public E next() {
      checkForComodification();
      int i = cursor;
      if (i >= size)
        throw new NoSuchElementException();
      Object[] elementData = ArrayList.this.elementData;
      if (i >= elementData.length)
        throw new ConcurrentModificationException();
      cursor = i + 1;
      return (E) elementData[lastRet = i];
  }

  public void remove() {
      if (lastRet < 0)
        throw new IllegalStateException();
      checkForComodification();

      try {
        ArrayList.this.remove(lastRet);
        cursor = lastRet;
        lastRet = -1;
        expectedModCount = modCount;
      } catch (IndexOutOfBoundsException ex) {
        throw new ConcurrentModificationException();
      }
  }

  @Override
  @SuppressWarnings("unchecked")
  public void forEachRemaining(Consumer<? super E> consumer) {
      Objects.requireNonNull(consumer);
      final int size = ArrayList.this.size;
      int i = cursor;
      if (i >= size) {
        return;
      }
      final Object[] elementData = ArrayList.this.elementData;
      if (i >= elementData.length) {
        throw new ConcurrentModificationException();
      }
      while (i != size && modCount == expectedModCount) {
        consumer.accept((E) elementData[i++]);
      }
      // update once at end of iteration to reduce heap write traffic
      cursor = i;
      lastRet = i - 1;
      checkForComodification();
  }
	//重点关注这个方法，这个迭代器类中的所有方法都调用此方法
  final void checkForComodification() {
      if (modCount != expectedModCount)
        throw new ConcurrentModificationException();
      }
}
```

从上面的源码可以看出，迭代器类所有的方法都调用了同一个方法`checkForComodification()`，在这个方法中，比较了修改的次数和期望修改的次数是否相等`modCount != expectedModCount`。如果在迭代过程中，有其他线程修改了此容器，那么modCount肯定会增加，那么就会造成二者次数不相等，进而会快速的抛出异常，也就是Fail-Fast机制。

此外，在本迭代器中的remove方法会修改`modCount`，但是会紧接着将二者同步`expectedModCount=modCount`，以保证下一次调用`checkForComodification`方法时，不会抛出异常。



## 构造器

三种构造器，

1. 无参构造：初始化数组为默认的空数组

   ```java
   public ArrayList() {
     	this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
   }
   ```

2. 整型的有参构造：根据传入的参数，初始化指定长度的数组

   ```java
   public ArrayList(int initialCapacity) {
       if (initialCapacity > 0) {
         this.elementData = new Object[initialCapacity];
       } else if (initialCapacity == 0) {
         this.elementData = EMPTY_ELEMENTDATA;
       } else {
         throw new IllegalArgumentException("Illegal Capacity: "+
                                            initialCapacity);
       }
   }
   ```

3. Collection类型的有参构造。这里有一个官方的bug，toArray()返回的不一定是`Object[]`类型。所以可能还需要下面的`Arrays.copyOf`操作

   ```java
   public ArrayList(Collection<? extends E> c) {
       elementData = c.toArray();
     	//collection长度不为空
       if ((size = elementData.length) != 0) {
         // c.toArray might (incorrectly) not return Object[] (see 6260652)
         if (elementData.getClass() != Object[].class)
           elementData = Arrays.copyOf(elementData, size, Object[].class);
       } else {
         // replace with empty array.
         this.elementData = EMPTY_ELEMENTDATA;
       }
   }
   ```

### 官方的Bug

> c.toArray might (incorrectly) not return Object[] (see 6260652)
>
> 为什么toArray()方法不一定返回的就是Object[]类型呢？

看如下代码：

```java
public static void main(String[] args) {
    //通过ArrayList的toArray()传过来的数组一定是Object[]类型的
    List<String> list = new ArrayList<>(Arrays.asList("list"));
    System.out.println(list.getClass());//输出class java.util.ArrayList
    Object[] listArray = list.toArray();
    System.out.println(listArray.getClass());//输出class [Ljava.lang.Object;
    listArray[0] = new Object();
    //调用Arrays.asList()返回一个列表，当调用这个列表的toArray()方法时，返回的就是String[]类型
    List<String> al = Arrays.asList("asList");
  	System.out.println(al.getClass());//输出class java.util.Arrays$ArrayList
    Object[] asListArray = al.toArray();
    System.out.println(asListArray.getClass());//输出class [Ljava.lang.String;
}
```

这是因为在创建子类实例时，即使是父亲引用，其指向的实例依旧是子类实例，其调用的实例方法当然是子类的实例方法。所以我们看到`al`对象的类型是`java.util.Arrays$ArrayList`，属于`java.util.Arrays`的一个静态内部类，从源码里找这个内部类的`toArray()`方法是如何实现的：

```java
private static class ArrayList<E> extends AbstractList<E>
  implements RandomAccess, java.io.Serializable
{
    private static final long serialVersionUID = -2764017481108945198L;
    private final E[] a;
	...
    //之前纠结了半天这里为什么传入String类型也可以，因为单个String可以作为一个元素的String[]数组。原因可以看Arrays.asList()方法
    ArrayList(E[] array) {
    	a = Objects.requireNonNull(array);
  	}
    @Override
    public Object[] toArray() {
      	return a.clone();
    }
    @Override
    @SuppressWarnings("unchecked")
    public <T> T[] toArray(T[] a) {
        int size = size();
        if (a.length < size)
          return Arrays.copyOf(this.a, size,
                               (Class<? extends T[]>) a.getClass());
        System.arraycopy(this.a, 0, a, 0, size);
        if (a.length > size)
          a[size] = null;
        return a;
    }
...
}
```

这里的`toArray()`方法返回的是对象`a`的克隆，对象a的类型是`E[]`，和实例化这个内部类时传入的参数类型一致。

```java
//传入了数量可变的参数，下面都作为数组传入ArrayList构造器中
public static <T> List<T> asList(T... arg) {//源码中，参数名是a，为了跟内部类中的数组a区分，我把它改为arg
  	//在调用asList的时候，会实例化Arrays$ArrayList内部类
  	return new ArrayList<>(arg);
}
```

当调用`asList`方法的时候，会实例化上面那个`Arrays$ArrayList`内部类得到对象`obj`，如果传入的参数`arg`是`String`类型，那么`obj`中的数组`a`类型就会是`String[]`。因为这个内部类中的`toArray()`方法返回的是对象`a`的克隆，对象a的类型是`E[]`，进而导致在调用`obj`的`toArray()`方法时，返回的数组类型是`String[]`。

**总而言之，**就是`Arrrays`类中的静态内部类`ArrayList`实现的`toArray()`方法返回的数组类型不一定是`Object[]`类型，取决于调用`Arrays.asList(T... arg)`时，传入参数的类型。



## 扩容方法

遇到的第一个扩容情形，`ensureCapacity()`方法。在应用程序需要添加大量数据的时候，可以调用此方法，来增加实例容量，这样可以减少自动扩容时，增量重新分配的数量。

```java
public void ensureCapacity(int minCapacity) {
    int minExpand = (elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
      // 数组当前不为空就赋值为0，保证后续能够扩容成功。
      ? 0
      // 当前就是个空数组，那么赋值为DEFAULT_CAPACITY，保证第一次初始化，容量不会小于10
      : DEFAULT_CAPACITY;
    //要求的最小容量大于当前容量，就显式扩容
    if (minCapacity > minExpand) {
      ensureExplicitCapacity(minCapacity);
    }
}
```

`ensureCapacity()`方法调用了`ensureExplicitCapacity()`方法来明确扩容需求：

```java
private void ensureExplicitCapacity(int minCapacity) {
    modCount++;
    // overflow-conscious code
  	//要求的最小容量必须大于当前数组长度才能扩容
    if (minCapacity - elementData.length > 0)
      grow(minCapacity);
}
```

继续调用`grow()`方法，扩容的代码就写在`grow()`方法中：

```java
/**
* Increases the capacity to ensure that it can hold at least the
* number of elements specified by the minimum capacity argument.
*
* @param minCapacity the desired minimum capacity
*/
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
  	//增量为旧容量扩大一半
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    //如果还不够，就直接赋值为指定大小
    if (newCapacity - minCapacity < 0)
      newCapacity = minCapacity;
  	//如果新容量的大小大于最大数组长度，就赋值为可用的最大容量。
    if (newCapacity - MAX_ARRAY_SIZE > 0)
      newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
  	//将数组中的数据复制到扩容后的数组中
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```

`grow()`方法会将数组的大小扩大到**原来的1.5倍。**



## 克隆方法

```java
/**
     * Returns a shallow copy of this ArrayList instance.  (The
     * elements themselves are not copied.)
     *
     仅对列表进行浅拷贝，只拷贝了一个列表实例，列表内的元素没有被拷贝，还是原来的元素
     * @return a clone of this ArrayList instance
     */
    public Object clone() {
        try {
          	//此类实现了Cloneable接口，因此可以使用super.clone()方法
            java.util.ArrayList<?> v = (java.util.ArrayList<?>) super.clone();
            v.elementData = Arrays.copyOf(elementData, size);
            v.modCount = 0;
            return v;
        } catch (CloneNotSupportedException e) {
            // this shouldn't happen, since we are Cloneable
            throw new InternalError(e);
        }
    }
```

做一组实验，在列表中存放一组对象，然后拷贝一次列表，紧接着对原列表中的对象属性进行修改，查看新列表中的对象属性是否跟着被更改了：

```java
class Node{
    String name;
    Node(String name){
        this.name = name;
    }

    @Override
    public String toString() {
        return "Node{" +
                "name='" + name + '\'' +
                '}';
    }
}
public class Main1 {
    public static void main(String[] args) {
      	//原列表
        ArrayList<Node> list = new ArrayList<>();
      	//存入两个Node对象
        list.add(new Node("111"));
        list.add(new Node("222"));
        System.out.println(list.toString());
      	//调用clone方法
        ArrayList<Node> cplist = (ArrayList<Node>) list.clone();
        System.out.println(list);
        System.out.println(cplist);
      	//修改原列表中的对象属性
        Node node = list.get(0);
        node.name = "修改了";
        System.out.println(list);
        System.out.println(cplist);
    }
}
```

上面的程序输出：

```markdown
[Node{name='111'}, Node{name='222'}]
[Node{name='111'}, Node{name='222'}]
[Node{name='111'}, Node{name='222'}]
[Node{name='修改了'}, Node{name='222'}]
[Node{name='修改了'}, Node{name='222'}]	//修改原列表中对象时，新列表中的对象也被修改了
```

由此得出，这个clone方法只是拷贝了一个列表的实例，列表内的元素没有被拷贝。





## 转数组的两种方法

```java
/**
第一种方法，返回object数组
* Returns an array containing all of the elements in this list
* in proper sequence (from first to last element).
可以安全的修改返回的数组，无需担心会改变列表的内容。
* This method acts as bridge between array-based and collection-based
* APIs.
*/
public Object[] toArray() {
  	return Arrays.copyOf(elementData, size);
}

/**
 第二种方法，返回指定数组类型
两种情况:
1. 传入的参数数组长度大于当前列表的长度，那么返回的数组的剩余部分会被填充为null
2. 传入的参数数组长度小于当前列表的长度，那么返回的数组长度会跟列表长度相等。
  */
@SuppressWarnings("unchecked")
public <T> T[] toArray(T[] a) {
    if (a.length < size)
      // Make a new array of a's runtime type, but my contents:
      //调用Arrays.copyOf()方法，返回指定类型的数组
      return (T[]) Arrays.copyOf(elementData, size, a.getClass());
    System.arraycopy(elementData, 0, a, 0, size);
    if (a.length > size)
      a[size] = null;
    return a;
}
//可以跟进去看看Arrays.copyOf如何实现的
public static <T,U> T[] copyOf(U[] original, int newLength, Class<? extends T[]> newType) {
    @SuppressWarnings("unchecked")
  	//先判断是否是object类型，如果不是，就用Array.newInstance创建一个指定类型和长度的数组
    T[] copy = ((Object)newType == (Object)Object[].class)
      ? (T[]) new Object[newLength]
      : (T[]) Array.newInstance(newType.getComponentType(), newLength);
  	//将原数组拷贝进新数组，
    System.arraycopy(original, 0, copy, 0,
                     Math.min(original.length, newLength));
    return copy;
}
```

这段代码中使用了一个方法：`System.arraycopy()`。之前没见过这个方法，查查看：

```markdown
是System类下的一个静态方法。
public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
src:源数组;
srcPos:源数组要复制的起始位置;
dest:目的数组;
destPos:目的数组放置的起始位置;
length:复制的长度.
当length大于src.length - srcPos，即可复制的有效长度时，会抛出ArrayIndexOutOfBoundsException。
当length大于des.length - destPos时，即可存放的有效长度时，也会抛出ArrayIndexOutOfBoundsException。
话说这个拷贝是深拷贝吗？也是浅拷贝，数组内的对象没有被拷贝一份，只是拷贝引用。
```





## 4种删除方法

```java
public E remove(int index) {
    rangeCheck(index);

    modCount++;
    E oldValue = elementData(index);

    int numMoved = size - index - 1;
    if (numMoved > 0)
      //从索引位置开始，整体前移
      System.arraycopy(elementData, index+1, elementData, index,
                       numMoved);
    //释放最后一个位置的引用，让GC回收它
    elementData[--size] = null; // clear to let GC do its work

    return oldValue;
}

//删除索引值最低的匹配元素，不存在则返回false
public boolean remove(Object o) {
    if (o == null) {
      for (int index = 0; index < size; index++)
        if (elementData[index] == null) {
          fastRemove(index);
          return true;
        }
    } else {
      for (int index = 0; index < size; index++)
        if (o.equals(elementData[index])) {
          fastRemove(index);
          return true;
        }
    }
    return false;
}

//私有的删除方法，不检查边界，不返回移除的元素，即快速删除
private void fastRemove(int index) {
    modCount++;
    int numMoved = size - index - 1;
    if (numMoved > 0)
      System.arraycopy(elementData, index+1, elementData, index,
                       numMoved);
    elementData[--size] = null; // clear to let GC do its work
}

//删除所有元素
public void clear() {
    modCount++;
    // clear to let GC do its work
    for (int i = 0; i < size; i++)
      elementData[i] = null;
    size = 0;
}
```

