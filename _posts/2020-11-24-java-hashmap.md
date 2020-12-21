---
title: Java源码分析之HashMap
author: Kol Huang
date: 2020-11-24 20:30:00 +0800
categories: [Blogging, 容器]
tags: [Java源码学习]
comments: true
math: true

---









## 类前注释

**关键点：**

* `HashMap`和`HashTable`大致相当，只不过`HashMap`是非线程安全的，并且允许空值。`HashMap`集合内的元素顺序是无法保证的，可能会改变。

* 如果哈希函数将元素正确分散在存储桶中，`get`和`put`操作只花费常量时间。迭代整个集合，花费的时间为桶的数量 x 键值对的数量。如果需要对此集合进行迭代操作，那么一定不要把桶的数量初始化地太大，或者装载因子设地太低。

* 如前所述，`HashMap`有两个参数会影响其实例的性能：（1）`initail capacity`和`load factor`。`capacity`表示哈希表中桶的数量；`load factor`是在`capacity`自动增长前，表可以装得多满。当哈希表中的键值对数量超过了当前`capacity`和`load factor`的乘积，哈希表将执行`rehash`，增加一倍的桶。

* 一般情况下，`load factor = 0.75`能够比较好的平衡时间和空间开销。更高的值会降低空间负载，但是会增加查询成本（会影响哈希表的大多数操作，例如`get`和`put`）。设置其初始容量时，应考虑映射中的预期条目数及其负载因子，以最大程度地减少重新哈希操作的次数。当初始容量大于 （最大键值对数量/`load factor`）时，不会发生任何的`rehash`操作。

* 许多键使用相同的`HashCode()`对降低哈希表的性能，为了改善这种影响，当键是可比较的时，`HashMap`会根据比较的顺序来安排键，来帮助打破性能瓶颈。

* 如果没有其他方法来保证线程安全，那么在创建`HashMap`的时候，可以使用以下方法来保证线程安全：

  ```jav
  Map m = Collections.synchronizedMap(new HashMap(...));
  ```

* 支持`fail-fast`机制：当迭代器创建后，有其他线程修改了此HashMap，那么会快速并且简洁地抛出`ConcurrentModificationException`异常。

  

## Implementation notes

JDK1.8在JDK1.7的基础上增加了红黑树进行优化。即当链表长度超过8时，链表就转换为红黑树，利用红黑树快速增删改查的特点提高HashMap的性能，其中会用到红黑树的插入、删除、查找等算法。





## 变量

```java
//默认的initial capacity=16
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
//允许的最大capacity
static final int MAXIMUM_CAPACITY = 1 << 30;
//默认的load factor=0.75
static final float DEFAULT_LOAD_FACTOR = 0.75f;
//当桶中的entry数量大于8时，这个bin会被转化为树。 
static final int TREEIFY_THRESHOLD = 8;
//当发生了resize操作时，节点数需低于6才能从树节点转化为普通的bin节点。
static final int UNTREEIFY_THRESHOLD = 6;
//节点可被树化的最小容量。
static final int MIN_TREEIFY_CAPACITY = 64;

//存放键值对的表格
transient Node<K,V>[] table;

transient Set<Map.Entry<K,V>> entrySet;
//map中的键值对数量
transient int size;
//用于记录线程修改操作的次数，用于fail-fast
transient int modCount;
//threshold = capacity * load factor，当键值对数量超过了这个阈值，就需要resize了
int threshold;
```





## 四种构造方法

1. ```java
   		public HashMap() {
           this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
       }
   //所有参数均按默认初始化
   ```

2. ```java
       public HashMap(int initialCapacity) {
           this(initialCapacity, DEFAULT_LOAD_FACTOR);
       }
   //自定义初始容量，其他参数默认
   ```

3. ```java
       public HashMap(int initialCapacity, float loadFactor) {
           if (initialCapacity < 0)
               throw new IllegalArgumentException("Illegal initial capacity: " +
                                                  initialCapacity);
           if (initialCapacity > MAXIMUM_CAPACITY)
               initialCapacity = MAXIMUM_CAPACITY;
           if (loadFactor <= 0 || Float.isNaN(loadFactor))
               throw new IllegalArgumentException("Illegal load factor: " +
                                                  loadFactor);
           this.loadFactor = loadFactor;
           this.threshold = tableSizeFor(initialCapacity);
       }
   //初始容量和load factor都自定义
   ```

4. ```java
       public HashMap(Map<? extends K, ? extends V> m) {
           this.loadFactor = DEFAULT_LOAD_FACTOR;
           putMapEntries(m, false);
       }
   //使用一个现成的集合来初始化
       final void putMapEntries(Map<? extends K, ? extends V> m, boolean evict) {
           int s = m.size();
           if (s > 0) {
               if (table == null) { // pre-size
                   float ft = ((float)s / loadFactor) + 1.0F;
                   int t = ((ft < (float)MAXIMUM_CAPACITY) ?
                            (int)ft : MAXIMUM_CAPACITY);
                   if (t > threshold)
                       threshold = tableSizeFor(t);
               }
               else if (s > threshold)
                   resize();
               for (Map.Entry<? extends K, ? extends V> e : m.entrySet()) {
                   K key = e.getKey();
                   V value = e.getValue();
                 	//将值逐个放入哈希表中，这个方法放到后面讲put的时候再分析
                   putVal(hash(key), key, value, false, evict);
               }
           }
       }
   		//把cap规整为2的指数
       static final int tableSizeFor(int cap) {
           int n = cap - 1;
           n |= n >>> 1;
           n |= n >>> 2;
           n |= n >>> 4;
           n |= n >>> 8;
           n |= n >>> 16;
           return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
       }
   
   ```



## put方法

```java
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }

    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
      	//用 (n-1) & hash作为这个键值对要放入数组中的位置，很巧妙
      	//n肯定是2的指数，那么n-1除最高位外，其余均为1
      	//例如n = 16, n - 1 = 15 = (0 1111)
      	//将这个值与hash相与，能够保证i不超出数组的范围，所以这是替代mod运算的高效方式
      
      	//如果这个位置不存在节点，之前没有hash到这里过，就直接创建一个新节点并赋值
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
          	//这个位置已经有节点了
            Node<K,V> e; K k;
          	//key是否存在，如果存在就直接覆盖
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
              	////链表中存在这个key，赋给e
                e = p;
          	//当前节点是否为TreeNode
            else if (p instanceof TreeNode)
              	//红黑树直接插入键值对
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
              	//不是TreeNode的话，遍历链表准备插入节点
                for (int binCount = 0; ; ++binCount) {
                  	//找到了末尾
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                  	//链表中存在这个key，赋给e
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
          	//e不为空，说明在table[i]这个位置的链表中找到了key对应的节点覆盖value即可
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
              	// Callbacks to allow LinkedHashMap post-actions
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
      	//回调函数，用于LinkedHashMap post-actions
        afterNodeInsertion(evict);
        return null;
    }

    // Create a regular (non-tree) node
    Node<K,V> newNode(int hash, K key, V value, Node<K,V> next) {
        return new Node<>(hash, key, value, next);
    }
    static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;
      ...
    }
```

借用网图，出处见水印：

![java_HashMap_put方法逻辑](/HYCBlog/assets/img/java/java_HashMap_put方法逻辑.png)

## get方法

```java
    public V get(Object key) {
        Node<K,V> e;
        return (e = getNode(hash(key), key)) == null ? null : e.value;
    }
		//返回key的哈希
    static final int hash(Object key) {
        int h;
      	//hashCode是用来在散列存储结构中确定对象的存储地址的
      	//														高16位不变，低16位和高16位异或
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
		
    final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
      	//去(n-1)&hash的位置找，取此位置的链表首节点first
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
          	//总是会先检查首节点是否为要找的节点
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            if ((e = first.next) != null) {
              	//如果这个首节点是TreeNode，就执行红黑树查找
                if (first instanceof TreeNode)
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
              	//否则就遍历链表，直到找到key对应的节点
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
      	//没找到就返回null
        return null;
    }
```



## Resize方法

初始化，或者double哈希表的大小。

```java
    final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
      	//原来的哈希表不为空
        if (oldCap > 0) {
          	//如果最大capacity已经达到了限定的最大值，不作resize
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
          	//如果当前capacity大于16并且加倍后，小于限定的最大值，就加倍
          	//threshold也加倍
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
      	//oldCap==0,就初始化capacity为threshold
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
      	//如果oldCap==0 && oldThr == 0，就初始化为默认的capacity和默认的threshold
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
      	
      	//经过以上操作后，newThr == 0说明在oldCap == 0 && oldThr > 0时将oldThr赋给了newCap
        if (newThr == 0) {
          	//更新newThr为newCap * loadFactor
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
      	//新建一个节点数组
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
      	//整体迁移
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
                  	//j位置只有一个节点
                    if (e.next == null)
                      	//直接rehash
                        newTab[e.hash & (newCap - 1)] = e;
                  	//如果不止一个节点，并且是树节点
                    else if (e instanceof TreeNode)
                      	//直接调用红黑树的分裂方法
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                  	//不止一个节点，但都是普通节点
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                      	//遍历链表节点
                        do {
                            next = e.next;
                          	//e.hash & oldCap == 0说明e.hash值小于oldCap
                          	//将低的一半放在loHead链表中
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                          	//将e.hash值高的一半放在hiHead链表中
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                      	//loHead链表存放在原位
                        if (loTail != null) {
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
                      	//hiHead链表放在j + oldCap的位置
                      	//以oldCap为offset
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }

```





## treeifyBin方法

当当前hash值对应索引的链表长度大于MIN_TREEIFY_CPACITY时，将这个链表节点转化为红黑树节点。

```java
    final void treeifyBin(Node<K,V>[] tab, int hash) {
        int n, index; Node<K,V> e;
        if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
            resize();
        else if ((e = tab[index = (n - 1) & hash]) != null) {
            TreeNode<K,V> hd = null, tl = null;
            do {
                TreeNode<K,V> p = replacementTreeNode(e, null);
                if (tl == null)
                    hd = p;
                else {
                    p.prev = tl;
                    tl.next = p;
                }
                tl = p;
            } while ((e = e.next) != null);
            if ((tab[index] = hd) != null)
                hd.treeify(tab);
        }
    }

```



## 总结

* 在`HashMap`中，table的长度必须是2的n次方（一定是合数），但是素数导致冲突的概率要小于合数，HashTable初始化桶的大小为11，就是素数的应用，但是HashTable扩容后不能保证容量还是素数。之所以要选择以2的n次方作为数组的长度，是为了在取模和扩容时做优化。同时为了减少冲突，HashMap将HashCode的高16位与第16位相异或，充分利用键的特性。
* 扩容是一个特别耗性能的操作，所以当程序员在使用HashMap的时候，估算map的大小，初始化的时候给一个大致的数值，避免map进行频繁的扩容。
* 负载因子是可以修改的，也可以大于1，但是建议不要轻易修改，除非情况非常特殊。



参考：https://blog.csdn.net/woshimaxiao1/article/details/83661464