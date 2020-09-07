---
title: 从算法到源码：PriorityQueue源码学习
author: Kol Huang
date: 2020-08-28 21:03:00 +0800
categories: [Blogging, 容器]
tags: [Java源码学习]
comments: true
math: true
---



## 剑指 Offer 41. 数据流中的中位数



如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

* void addNum(int num) - 从数据流中添加一个整数到数据结构中。
* double findMedian() - 返回目前所有元素的中位数。



示例 1：

```java
输入：
["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
[[],[1],[2],[],[3],[]]
输出：[null,null,null,1.50000,null,2.00000]
```




示例 2：

```java
输入：
["MedianFinder","addNum","findMedian","addNum","findMedian"]
[[],[2],[],[3],[]]
输出：[null,null,2.00000,null,2.50000]
```


限制：

`最多会对 addNum、findMedia进行 50000 次调用。`



### 二分查找

用一个列表存储值，每次二分查找合适的位置插入，使得list始终有序。二分查找时，需要特殊处理以下两种情况：

```markdown
1. 列表为空，则插入在0位置。
2. 插入元素为列表中的最大值，则插入在末尾。
```

需要计算中位数时，只需判断列表长度的奇偶性即可。

```java
class MedianFinder {
    List<Integer> list;
    /** initialize your data structure here. */
    public MedianFinder() {
      	//用LinkedList会超时
        list = new ArrayList<>();
    }

    public void addNum(int num) {
        int pos = binarySearch(num);
        list.add(pos,num);
    }

    public double findMedian() {
        if(list.size() == 0)
            return 0.0;
        int mid = (list.size()-1)/2;
        //长度为偶数
        if(list.size() % 2 == 0)
            return (list.get(mid) + list.get(mid+1))*1.0 / 2;
        else//长度为奇数
            return list.get(mid)*1.0;
    }

    public int binarySearch(int num){
      	//列表为空时，插入在0位置
        if(list.size() == 0)    return 0;
      	//插入元素大于等于任何列表元素，插入在最后
        if(num >= list.get(list.size()-1))
            return list.size();
      	//二分查找插入位置
        int low = 0;
        int high = list.size()-1;
        while(low < high){
            int mid = (low + high) / 2;
            if(num == list.get(mid)){
                return mid;
            }
            if(num < list.get(mid))
                high = mid;
            if(num > list.get(mid))
                low = mid + 1;
        }
        return low;
    }

}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

值得注意的是，如果用了LinkedList来存数据会超时。

LinkedList虽然底层实现是链表，但是在执行list.add(index,element)操作时，需要将指针移到index的位置，这个过程会消耗很多时间；ArrayList能够直接根据index跳到需要插入的位置，但是需要将每个元素都后移一位再插入，也需要很多时间。所以关键在于二分查找的效率。

在执行二分查找时，用LinkedList会耗费很多时间，因为可能需要调用list.get() `logN`次，每次执行get操作都需要从链表端点开始检索，所以使用linkedList执行二分查找总的时间复杂度为`O(NlogN)`。

ArrayList在执行get操作时，时间复杂度为`O(1)`，所以使用ArrayList执行二分查找的时间复杂度为`O(logN)`。



## 优先队列/堆

建立一个小顶堆A和大顶堆B，各保存列表的一半元素，且规定：

```java
A保存较大的一半，长度为N/2（N为偶数）或（N+1)/2（N为奇数）；
B保存较小的一半，长度为N/2（N为偶数）或（N-1)/2（N为奇数）；
```

随后，中位数可仅根据 A, B 的堆顶元素计算得到。

```java
class MedianFinder {
    Queue<Integer> A, B;
    public MedianFinder() {
        A = new PriorityQueue<>(); // 小顶堆，保存较大的一半
        B = new PriorityQueue<>((x, y) -> (y - x)); // 大顶堆，保存较小的一半
    }
    public void addNum(int num) {
      	//A和B长度相等时，优先插入到A，长度不等时，说明A比B多一个元素，则把数分一个给B
        if(A.size() != B.size()) {
          	//由于这个数可能比A中最小的元素大，所以需要先把这个数放入A中，然后把A中最小的数拿出来给B
            A.add(num);
            B.add(A.poll());
        } else {
          	//由于这个数可能比B中最大的元素小，所以需要先把这个数放入B中，然后把B中最大的数拿出来给A
            B.add(num);
            A.add(B.poll());
        }
    }
    public double findMedian() {
      	//判断是奇数还是偶数
        return A.size() != B.size() ? A.peek() : (A.peek() + B.peek()) / 2.0;
    }
}
```

大佬的答案就是妙啊～我这菜🐔只能写写二分，游走在超时的边缘。

所以按这个思路，以后我如果需要找到一个无序数组的中位数，就不用对它排序了，排序的时间复杂度也要O(NlogN)，这个优先队列的方法只需要O(N)，但是空间复杂度需要O(N)。

Java的小顶堆就是优先队列`PriorityQueue`，大顶堆需要在初始化优先队列时，传一个自定义比较器。

来康康源码：

```java
	/**自然序的时候，比较器为null
		* The comparator, or null if priority queue uses elements'
    * natural ordering.
    */
  private final Comparator<? super E> comparator;


	/**
    * Creates a {@code PriorityQueue} with the default initial capacity and
    * whose elements are ordered according to the specified comparator.
    *	按照比较器来排序
    * @param  comparator the comparator that will be used to order this
    *         priority queue.  If {@code null}, the {@linkplain Comparable
    *         natural ordering} of the elements will be used.
    * @since 1.8
    */
  public PriorityQueue(Comparator<? super E> comparator) {
      this(DEFAULT_INITIAL_CAPACITY, comparator);
  }
```

顺便康康优先队列怎么add元素的：

```java
add调用了offer：
  /**
     * Inserts the specified element into this priority queue.
     *
     * @return {@code true} (as specified by {@link Queue#offer})
     * @throws ClassCastException if the specified element cannot be
     *         compared with elements currently in this priority queue
     *         according to the priority queue's ordering
     * @throws NullPointerException if the specified element is null
     */
    public boolean offer(E e) {
        if (e == null)
            throw new NullPointerException();
        modCount++;
        int i = size;	//当前优先队列中的元素个数
  			//即将超出Capacity
        if (i >= queue.length)
          	//扩容
            grow(i + 1);
        size = i + 1;
        if (i == 0)
            queue[0] = e;
        else
            siftUp(i, e);
        return true;
    }

grow怎么扩容的呢？
  /**
     * Increases the capacity of the array.
     *
     * @param minCapacity the desired minimum capacity传入一个期望的最小容量
     */
    private void grow(int minCapacity) {
        int oldCapacity = queue.length;
        // Double size if small; else grow by 50%
  			//如果旧容量小于64，则指数增长；否则线性增长
        int newCapacity = oldCapacity + ((oldCapacity < 64) ?
                                         (oldCapacity + 2) :
                                         (oldCapacity >> 1));
        // overflow-conscious code
  			//如果新的容量已经超出了能够分配的最大容量
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        queue = Arrays.copyOf(queue, newCapacity);
    }

最大数组长度为多少？
  /**一些虚拟机会保留一些头部字空间，如果分配的容量超过这个数，有可能导致OutOfMemoryError
     * The maximum size of array to allocate.
     * Some VMs reserve some header words in an array.
     * Attempts to allocate larger arrays may result in
     * OutOfMemoryError: Requested array size exceeds VM limit
     */
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

超出后会调用hugeCapacity(minCapacity)：
  private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
  			//当前期望的最小容量已经大于能够分配的最大容量了，就赋值为Integer.MAX_VALUE，为啥？
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
```

**发现了一个问题**，当前期望的最小容量已经大于能够分配的最大容量了，为什么就把minCapacity赋值为Integer.MAX_VALUE呢？

让我康康有没有老外问过这个问题，别说还真有，[Java 8 Arraylist hugeCapacity(int) implementation](https://stackoverflow.com/questions/35582809/java-8-arraylist-hugecapacityint-implementation)

有一个回答是：

The maximal array size is limited to some number which varies across different JVMs and usually is slightly less than `Integer.MAX_VALUE`. So allocating the array of `Integer.MAX_VALUE` elements you will have `OutOfMemoryError` on most of JVMs even if you have enough memory to do it. `MAX_ARRAY_SIZE` assumes to be valid array size on the most of existing JVMs. So when `ArrayList` size approaches to `Integer.MAX_VALUE` (for example, you have more than 1_500_000_000 elements and need to enlarge an array), it's enlarged to this `MAX_ARRAY_SIZE`, so it can be successfully performed (assuming you have enough memory). Only if number of elements exceeds `MAX_ARRAY_SIZE`, the `ArrayList` tries to allocate an array of `Integer.MAX_VALUE` elements (which will likely to fail on most of JVMs, but *may* succeed on some of them). This way you can safely add elements up to `MAX_ARRAY_SIZE` on almost any JVM and only after that will have problems. 

就是说大部分的JVM的最大数组size都略微的小于Integer.MAX_VALUE（这个在上面的源码里说了），当期望的容量（即minCapacity）超过可分配的最大容量时，会尝试着帮你把容量加到`Integer.MAX_VALUE`，这样就不会在扩容的过程中就产生`OutOfMemoryError`，JVM会在扩容以后再来处理这个问题，这样的方式可以帮助你安全地把容量逼近 `MAX_ARRAY_SIZE` 。



还有一个回答：

If we can avoid `OutOfMemory` on some VMs, we will, otherwise, allocate `Integer.MAX_VALUE`, and succeed if you're lucky 。

通俗易懂！为了适配大多数的JVM，所以不在扩容的时候产生`OutOfMemoryError`，因为有少部分JVM的最大容量等于 `Integer.MAX_VALUE`。