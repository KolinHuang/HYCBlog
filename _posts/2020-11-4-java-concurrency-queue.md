---
title: Java并发 并发队列原理剖析
author: Kol Huang
date: 2020-11-4 21:16:00 +0800
categories: [Blogging, java]
tags: [concurrency]
comments: true
math: true

---





JDK 中提供了一系列场景的并发安全队列。总的来说，按照实现方式的不同可分为阻塞队列和非阻塞队列，前者使用锁实现，而后者则使用 CAS 非阻塞算法实现。

### 1. ConcurrentLinkedQueue原理探究

#### 1.1 ConcurrentLinkedQueue原理介绍

1. offer操作

   ```java
   public boolean offer(E e) {
     			//e为空就抛出空指针异常
           checkNotNull(e);
     			//构造Node节点，在构造函数内部调用unsafe.putObject
           final Node<E> newNode = new Node<E>(e);
   				//从尾节点进行插入
           for (Node<E> t = tail, p = t;;) {
               Node<E> q = p.next;
             	//如果q == null说明p是尾节点，则执行插入
               if (q == null) {
                   // p is last node
                 	//cas操作插入
                   if (p.casNext(null, newNode)) {
                     	//CAS成功，说明新增节点已经被放入链表，然后就更新尾节点为当前节点
                       // Successful CAS is the linearization point
                       // for e to become an element of this queue,
                       // and for newNode to become "live".
                       if (p != t) // hop two nodes at a time
                           casTail(t, newNode);  // Failure is OK.
                       return true;
                   }
                   // Lost CAS race to another thread; re-read next
               }
               else if (p == q)
                   // We have fallen off list.  If tail is unchanged, it
                   // will also be off-list, in which case we need to
                   // jump to head, from which all live nodes are always
                   // reachable.  Else the new tail is a better bet.
                 	//多线程操作时，由于poll操作移除元素后可能会把head变为自引用，也就是head的next变成了head
                   p = (t != (t = tail)) ? t : head;
               else
                 	//寻找尾节点
                   // Check for tail updates after two hops.
                   p = (p != t && t != (t = tail)) ? t : q;
        }
       }
   ```

   可见， offer 操作中的关键步骤是CAS操作插入尾节点，通过原子 CAS 操作来控制某时只有一个线程可以追加元素到队列末尾。 进行 CAS 竞争失败的线程会通过循环一 次次尝试进行 CAS 操作，直到 CAS 成功才会返回 ，也就是通过使用无限循环不断进行 CAS 尝试方式来 替代阻塞算法挂起调用线程。相比阻塞算法，这是使用 CPU 资源换取阻塞所带来的开销。

2. add操作

   内部调用的还是offer方法。

3. poll操作

   ```java
   		public E poll() {
         	//goto标记
           restartFromHead:
           for (;;) {
               for (Node<E> h = head, p = h, q;;) {
                 	//保存当前节点值
                   E item = p.item;
   								//CAS设置当前节点为null
                   if (item != null && p.casItem(item, null)) {
                       // Successful CAS is the linearization point
                       // for item to be removed from this queue.
                     	//CAS成功则标记当前节点并从链表中移除
                       if (p != h) // hop two nodes at a time
                           updateHead(h, ((q = p.next) != null) ? q : p);
                       return item;
                   }
                 	
                 	//当钱队列为空，则返回null
                   else if ((q = p.next) == null) {
                       updateHead(h, p);
                       return null;
                   }
                 	//如果当前节点被自引用了，则重新寻找新的队列头节点
                   else if (p == q)
                       continue restartFromHead;
                   else
                       p = q;
               }
           }
       }
   ```

   poll 方法在移除一个元素时，只是简单地使用 CAS 操作把当前节点的 item 值设置为 null ，然后通过重新设置头节点将该元素从队列里面移除，被移除的节点就成了孤立节点，这个节点会在垃圾回收时被回收掉。另外，如果在执行分支中发现头节点被修改了，要跳到外层循环重新获取新的头节点。

4. peek操作

   peek 操作的代码与 poll 操作类似，只是前者只获取队列头元素但是并不从队列里将它删除，而后者获取后需要从队列里面将它删除。另外，在第一次调用 peek 操作时，会删除哨兵节点，并让队列的 head 节点指向队列里面第一个元素或者 null 。

5. size操作

   计算当前队列元素个数，在并发环境下不是很有用，因为 CAS 没有加锁，所以从调用 size 函数到返回结果期间有可能增删元素，导致统计的元素个数不精确。

6. remove操作

   如果队列里面存在该元素则删除该元素，如果存在多个则删除第一个，并返回 true ，否则返回 false 。

7. contains操作

   判断队列里面是否含有指定对象，由于是遍历整个队列，所以像 size 操作一样结果也不是那么精确，有可能调用 该方法时元素还在队列里面，但是遍历过程中其他线程才把该元素删除了，那么就会返回 false 。



#### 1.2 小结

ConcurrentLinkedQueue 的底层使用单向链表数据结构来保存队列元素，每个元素被包装成一个 Node 节点。队列是靠头、尾节点来维护的，创建队列时头、尾节点指向一个 item 为 null 的哨兵节点。第一次执行 peek 或者 first 操作时会把 head 指向第一个真正的队列元素。由于使用非阻塞 CAS 算法，没有加锁，所以在计算 size 时有可能进行了 offer、poll或者remove操作，导致计算的元素个数不精确，所以在并发情况下size函数不是很有用。

### 2. LinkedBlockingQueue原理探究

```java
    /** Lock held by take, poll, etc */
		//执行take,poll等操作时需要获取该锁
    private final ReentrantLock takeLock = new ReentrantLock();
		//当队列为空时，执行出队操作的线程会被放入这个条件队列进行等待
    /** Wait queue for waiting takes */
    private final Condition notEmpty = takeLock.newCondition();
		//执行put、offer等操作时需要获取该锁
    /** Lock held by put, offer, etc */
    private final ReentrantLock putLock = new ReentrantLock();
		//当队列满时，执行进队操作的线程会被放入这个条件队列进行等待
    /** Wait queue for waiting puts */
    private final Condition notFull = putLock.newCondition();
    //当前队列中的元素个数
    /** Current number of elements */
    private final AtomicInteger count = new AtomicInteger();
```



#### 2.1 LinkedBlockingQueue原理介绍

1. offer操作（非阻塞）

   ```java
   		public boolean offer(E e) {
         	//e为空就抛出NPE异常
           if (e == null) throw new NullPointerException();
           final AtomicInteger count = this.count;
         	//队列满了，插入失败
           if (count.get() == capacity)
               return false;
           int c = -1;
           Node<E> node = new Node<E>(e);
           final ReentrantLock putLock = this.putLock;
         	//获取putLock锁
           putLock.lock();
           try {
             	//如果当前队列还没满
               if (count.get() < capacity) {
                 	//入队
                   enqueue(node);
                 	//count unsafe加1
                   c = count.getAndIncrement();
                 	//还没满，就唤醒notFull的条件队列里面因为调用了notFull的await操作（比如执行put方法而队列满的时候）而被阻塞的一个线程，因为队列现在有空闲，所以这里可以提前唤醒一个入队线程。
                   if (c + 1 < capacity)
                       notFull.signal();
               }
           } finally {
             	//释放锁
               putLock.unlock();
           }
         	//说明在释放锁时，队列里至少有一个元素，队列里面有元素则执行signalNotEmpty操作
           if (c == 0)
               signalNotEmpty();
           return c >= 0;
       }
   		
   		private void enqueue(Node<E> node) {
           // assert putLock.isHeldByCurrentThread();
           // assert last.next == null;
           last = last.next = node;
       }
   		//唤醒一个因为take操作时，队列为空而被阻塞的线程
   		private void signalNotEmpty() {
           final ReentrantLock takeLock = this.takeLock;
           takeLock.lock();
           try {
               notEmpty.signal();
           } finally {
               takeLock.unlock();
           }
       }
   ```

   综上，offer 方法通过使用 putLock 锁保证了在队尾新增元素操作的原子性。另外，调用条件变量的方法前一定要记得获取对应的锁，并且注意进队时只操作队列链表的尾节点。

2. put操作（阻塞）

   ```java
       public void put(E e) throws InterruptedException {
           if (e == null) throw new NullPointerException();
           // Note: convention in all put/take/etc is to preset local var
           // holding count negative to indicate failure unless set.
           int c = -1;
           Node<E> node = new Node<E>(e);
           final ReentrantLock putLock = this.putLock;
           final AtomicInteger count = this.count;
         	//获取锁的过程会响应中断
           putLock.lockInterruptibly();
           try {
               /*
                * Note that count is used in wait guard even though it is
                * not protected by lock. This works because count can
                * only decrease at this point (all other puts are shut
                * out by lock), and we (or some other waiting put) are
                * signalled if it ever changes from capacity. Similarly
                * for all other uses of count in other wait guards.
                */
             	//线程可能被虚假唤醒，也就是其他线程没有调用notFull的signal方法时，notFull.await()在某种情况下自动返回。
               while (count.get() == capacity) {
                   notFull.await();
               }
               enqueue(node);
               c = count.getAndIncrement();
               if (c + 1 < capacity)
                   notFull.signal();
           } finally {
               putLock.unlock();
           }
           if (c == 0)
               signalNotEmpty();
       }
   ```

3. poll操作（非阻塞）

   ```java
       public E poll() {
           final AtomicInteger count = this.count;
           if (count.get() == 0)
               return null;
           E x = null;
           int c = -1;
           final ReentrantLock takeLock = this.takeLock;
           takeLock.lock();
           try {
               if (count.get() > 0) {
                   x = dequeue();
                 	//c获得的是递减之前的值
                   c = count.getAndDecrement();
                 	//在删除队列的一个元素之后，队列仍然不为空，就可以提前唤醒一个由于队列空而被阻塞的线程
                   if (c > 1)
                       notEmpty.signal();
               }
           } finally {
               takeLock.unlock();
           }
         	//说明在删除队列的一个元素之前，队列是满的，在删除之后，就有空位，所以唤醒一个由于队列满而被阻塞的线程
           if (c == capacity)
               signalNotFull();
           return x;
       }
   ```

4. peek操作（非阻塞）

   ```java
       public E peek() {
           if (count.get() == 0)
               return null;
           final ReentrantLock takeLock = this.takeLock;
           takeLock.lock();
           try {
               Node<E> first = head.next;
             	//这里还需要判断一下队列是否为空，因为在获取锁之前，可能有其他线程删除了队列元素
               if (first == null)
                   return null;
               else
                   return first.item;
           } finally {
               takeLock.unlock();
           }
       }
   ```

5. take操作（阻塞）

   ```java
       public E take() throws InterruptedException {
           E x;
           int c = -1;
           final AtomicInteger count = this.count;
           final ReentrantLock takeLock = this.takeLock;
           takeLock.lockInterruptibly();
           try {
             	//阻塞当前线程，等待队列元素非空
               while (count.get() == 0) {
                   notEmpty.await();
               }
               x = dequeue();
               c = count.getAndDecrement();
               if (c > 1)
                   notEmpty.signal();
           } finally {
               takeLock.unlock();
           }
           if (c == capacity)
               signalNotFull();
           return x;
       }
   ```

6. remove操作

   ```java
       public boolean remove(Object o) {
           if (o == null) return false;
         	//双重加锁：在删除节点的时候，既不允许put也不允许take
           fullyLock();
           try {
             	//从头遍历，找到了就unlink节点
               for (Node<E> trail = head, p = trail.next;
                    p != null;
                    trail = p, p = p.next) {
                   if (o.equals(p.item)) {
                       unlink(p, trail);
                       return true;
                   }
               }
               return false;
           } finally {
               fullyUnlock();
           }
       }
   		//获取多个资源锁的顺序与释放的顺序是相反的，先获取的后释放
       void fullyLock() {
           putLock.lock();
           takeLock.lock();
       }
       void fullyUnlock() {
           takeLock.unlock();
           putLock.unlock();
       }
   		//需unsafe地更新count计数
   		void unlink(Node<E> p, Node<E> trail) {
           // assert isFullyLocked();
           // p.next is not changed, to allow iterators that are
           // traversing p to maintain their weak-consistency guarantee.
         	//节点清空，指针移动
           p.item = null;
           trail.next = p.next;
           if (last == p)
               last = trail;
           if (count.getAndDecrement() == capacity)
               notFull.signal();
       }
   ```

7. size操作

   由于进行出队、入队操作时的 count 是加了锁的， 所以结果相比 ConcurrentLinkedQueue 的 size 方法比较准确。这里考虑为何在 ConcurrentLinkedQueue 中需要遍历链表来获取 size 而不使用 一个原子变量呢？这是因为使用原子变量保存队列元素个数需要保证入队、出队操作和原子变量操作是原子性操作， 而 ConcurrentLinkedQueue 使用的是 CAS 无锁算法，所以无法做到这样。

#### 2.2 小结

LinkedBlockingQueue 的内部是通过单向链表实现的，使用头、尾节点来进行入队和出队操作，也就是入队操作都是对尾节点进行操作，出队操作都是对头节点进行操作。

对头、尾节点的操作分别使用了单独的独占锁从而保证了原子性， 所以出队和入队操作是可以同时进行的。另外对头 、尾节点的独占锁都配备了一个条件队列，用来存放被阻塞的线程，并结合入队、出队操作实现了一个生产消费模型。

### 3. ArrayBlockingQueue原理探究

构造器：

```java
		//由于这是一个有界队列，所以构造函数必须传入队列大小参数用于初始化
		public ArrayBlockingQueue(int capacity) {
        this(capacity, false);
    }
		//默认使用非公平独占锁进行出入队操作的同步
    public ArrayBlockingQueue(int capacity, boolean fair) {
        if (capacity <= 0)
            throw new IllegalArgumentException();
        this.items = new Object[capacity];
        lock = new ReentrantLock(fair);
        notEmpty = lock.newCondition();
        notFull =  lock.newCondition();
    }
```



#### 3.1 ArrayBlockingQueue原理介绍

1. offer操作（非阻塞）

   ```java
   		
   		public boolean offer(E e) {
           checkNotNull(e);
           final ReentrantLock lock = this.lock;
           lock.lock();
           try {
               if (count == items.length)
                   return false;
               else {
                   enqueue(e);
                   return true;
               }
           } finally {
               lock.unlock();
           }
       }
   
       private void enqueue(E x) {
           // assert lock.getHoldCount() == 1;
           // assert items[putIndex] == null;
           final Object[] items = this.items;
           items[putIndex] = x;
         	//取模操作
           if (++putIndex == items.length)
               putIndex = 0;
           count++;
           notEmpty.signal();
       }
   ```

2. put操作（阻塞）

   ```java
       public void put(E e) throws InterruptedException {
           checkNotNull(e);
           final ReentrantLock lock = this.lock;
           lock.lockInterruptibly();
           try {
               while (count == items.length)
                   notFull.await();
               enqueue(e);
           } finally {
               lock.unlock();
           }
       }
   ```

3. poll操作（非阻塞）

   ```java
       public E poll() {
           final ReentrantLock lock = this.lock;
           lock.lock();
           try {
               return (count == 0) ? null : dequeue();
           } finally {
               lock.unlock();
           }
       }
   
       private E dequeue() {
           // assert lock.getHoldCount() == 1;
           // assert items[takeIndex] != null;
           final Object[] items = this.items;
           @SuppressWarnings("unchecked")
           E x = (E) items[takeIndex];
           items[takeIndex] = null;
           if (++takeIndex == items.length)
               takeIndex = 0;
           count--;
           if (itrs != null)
               itrs.elementDequeued();
           notFull.signal();
           return x;
       }
   ```

4. take操作（阻塞）

   ```java
       public E take() throws InterruptedException {
           final ReentrantLock lock = this.lock;
           lock.lockInterruptibly();
           try {
               while (count == 0)
                   notEmpty.await();
               return dequeue();
           } finally {
               lock.unlock();
           }
       }
   ```

5. peek操作（非阻塞）

   ```java
       public E peek() {
           final ReentrantLock lock = this.lock;
           lock.lock();
           try {
               return itemAt(takeIndex); // null when queue is empty
           } finally {
               lock.unlock();
           }
       }
   
       final E itemAt(int i) {
           return (E) items[i];
       }
   ```

6. size操作

   ```java
   public int size() {
       final ReentrantLock lock = this.lock;
       lock.lock();
       try {
           return count;
       } finally {
           lock.unlock();
       }
   }
   ```

   size 操作比较简单，获取锁后直接返回 count，并在返回前释放锁。也许你会问，这里又没有修改 count 的值，只是简单地获取，为何要加锁呢？其实如果 count 被声明为 volatile 的这里就不需要加锁了，因为 volatile 类型的变 量保证了内存的可见性，而 ArrayBlockingQueue 中的 count 并没有被声明为 volatile 的，这是因为 count 操作都是在获取锁后进行的，而获取锁的语义之一是，获取锁后访问的变量都是从主内存获取的，这保证了变量的内存可见性。

#### 3.2 小结

ArrayBlockingQueue 通过使用全局独占锁实现了同时只能有一个线程进行入队或者出队操作，这个锁的粒度比较大，有点类似于在方法上添加 synchronized 的意思。其中offer 和 poll 操作通过简单的加锁进行入队、出队操作，而 put、 take 操作则使用条件变量实现了，如果队列满则等待，如果队列空则等待，然后分别在出队和入队操作中发送信号激活 等待线程实现同步。另外，相比 LinkedBlockingQueue, ArrayBlockingQueue 的 size 操作的结果是精确的，因为计算前加了全局锁。

### 4. PriorityBlockingQueue原理探究

#### 4.1 介绍

Priority B lockingQueue 是带优先级的无界阻塞队列，每次出队都返回优先级最高或者最低的元素。其内部是使用平衡二叉树堆实现的，所以直接遍历队列元素不保证有序。默认使用对象的 compareTo 方法提供比较规则，如果你需要自定义比较规则则可以自定义comparators。

#### 4.2 原理介绍

1. offer操作（阻塞）

   ```java
       public boolean offer(E e) {
           if (e == null)
               throw new NullPointerException();
           final ReentrantLock lock = this.lock;
           lock.lock();
           int n, cap;
           Object[] array;
         	//需要扩容
           while ((n = size) >= (cap = (array = queue).length))
               tryGrow(array, cap);
           try {
               Comparator<? super E> cmp = comparator;
             	//默认比较器
               if (cmp == null)
                   siftUpComparable(n, e, array);
               else
                 	//自定义比较器
                   siftUpUsingComparator(n, e, array, cmp);
             	//将队列元素加1，并且激活notEmpty的条件队列中的一个阻塞线程
               size = n + 1;
               notEmpty.signal();
           } finally {
               lock.unlock();
           }
           return true;
       }
   		//尝试扩容
       private void tryGrow(Object[] array, int oldCap) {
           lock.unlock(); // must release and then re-acquire main lock
           Object[] newArray = null;
         	//扩容专用的自旋锁，unsafe地更新锁状态，设置成功则进行扩容
           if (allocationSpinLock == 0 &&
               UNSAFE.compareAndSwapInt(this, allocationSpinLockOffset,
                                        0, 1)) {
               try {
                 	//当旧容量小于64时，加得快一些+(oldCap+2)，否则加oldCap的一半
                   int newCap = oldCap + ((oldCap < 64) ?
                                          (oldCap + 2) : // grow faster if small
                                          (oldCap >> 1));
                 	//上面的规则超出了容量限制，就线性+1，如果还超，就直接分配为MAX
                   if (newCap - MAX_ARRAY_SIZE > 0) {    // possible overflow
                       int minCap = oldCap + 1;
                       if (minCap < 0 || minCap > MAX_ARRAY_SIZE)
                           throw new OutOfMemoryError();
                       newCap = MAX_ARRAY_SIZE;
                   }
                   if (newCap > oldCap && queue == array)
                       newArray = new Object[newCap];
               } finally {
                   allocationSpinLock = 0;
               }
           }
         	//第一个线程CAS成功后，第二个线程会进入这段代码，然后第二个线程让出CPU，尽量让第一个线程获取锁，但是这不能保证一定会发生。
           if (newArray == null) // back off if another thread is allocating
               Thread.yield();
         	//扩容结束，加锁，但是不能保证扩容线程扩容后调用lock.lock重新获取锁
           lock.lock();
         	//当扩容线程进行扩容时，其他线程原地自旋检查当前扩容是否完毕，扩容完毕后才退出offer中的while循环。
           if (newArray != null && queue == array) {
               queue = newArray;
             	//扩容线程获取锁后，复制当前queue里的元素到新数组
               System.arraycopy(array, 0, newArray, 0, oldCap);
           		//至此，其他原地自旋的线程可以操作数组了
           }
       }
   ```

   建堆算法：

   ```java
       private static <T> void siftUpComparable(int k, T x, Object[] array) {
           Comparable<? super T> key = (Comparable<? super T>) x;
           while (k > 0) {
   						//索引父亲节点
               int parent = (k - 1) >>> 1;
               Object e = array[parent];
             	//找到了插入的位置
               if (key.compareTo((T) e) >= 0)
                   break;
             	//腾出地儿
               array[k] = e;
               k = parent;
           }
         	//插入
           array[k] = key;
       }
   ```

   默认为最小堆。

2. poll操作（非阻塞）

   ```java
       public E poll() {
           final ReentrantLock lock = this.lock;
           lock.lock();
           try {
               return dequeue();
           } finally {
               lock.unlock();
           }
       }
   
       private E dequeue() {
           int n = size - 1;
           if (n < 0)
               return null;
           else {
               Object[] array = queue;
             	//结果是第一个元素，因为是小顶堆
               E result = (E) array[0];
             	//先把最后一个位置腾出来
               E x = (E) array[n];
               array[n] = null;
               Comparator<? super E> cmp = comparator;
               if (cmp == null)
                 	//然后往前移位
                   siftDownComparable(0, x, array, n);
               else
                   siftDownUsingComparator(0, x, array, n, cmp);
               size = n;
               return result;
           }
       }
   		//从被移除的树根的左右子树中找到一个最小值来当树根，迭代寻找
       private static <T> void siftDownComparable(int k, T x, Object[] array,
                                                  int n) {
           if (n > 0) {
               Comparable<? super T> key = (Comparable<? super T>)x;
               int half = n >>> 1;           // loop while a non-leaf
             	//直到遍历到叶节点，就把先前最后的一个元素放入叶子节点
               while (k < half) {
                   int child = (k << 1) + 1; // assume left child is least
                   Object c = array[child];
                   int right = child + 1;
                   if (right < n &&
                       ((Comparable<? super T>) c).compareTo((T) array[right]) > 0)
                       c = array[child = right];
                   if (key.compareTo((T) c) <= 0)
                       break;
                 	//子树最小值替补到根节点
                   array[k] = c;
                 	//移动到子节点
                   k = child;
               }
             	//填入叶节点
               array[k] = key;
           }
       }
   ```

3. put操作（非阻塞）

   ```java
       public void put(E e) {
           offer(e); // never need to block
       }
   ```

4. take操作（阻塞）

   ```java
       public E take() throws InterruptedException {
           final ReentrantLock lock = this.lock;
           lock.lockInterruptibly();
           E result;
           try {
               while ( (result = dequeue()) == null)
                   notEmpty.await();
           } finally {
               lock.unlock();
           }
           return result;
       }
   ```

5. size操作

   ```java
       public int size() {
           final ReentrantLock lock = this.lock;
           lock.lock();
           try {
               return size;
           } finally {
               lock.unlock();
           }
       }
   ```



#### 4.3 案例介绍

使用PriorityBlockingQueue：

```java
package chap_1;

import java.util.Random;
import java.util.concurrent.PriorityBlockingQueue;

public class PriorityBlockingQueueTest {
  	//带优先级的任务
    static class Task implements Comparable<Task>{
        private int priority = 0;
        private String taskName;

        public int getPriority() {
            return priority;
        }

        public void setPriority(int priority) {
            this.priority = priority;
        }

        public String getTaskName() {
            return taskName;
        }

        public void setTaskName(String taskName) {
            this.taskName = taskName;
        }
				//根据自定义的比较器来决定任务执行顺序
        @Override
        public int compareTo(Task o) {
            if(this.priority > o.getPriority()){
                return 1;
            }else{
                return -1;
            }
        }

        public void doSomething(){
            System.out.println(taskName+":"+priority);
        }
    }

    public static void main(String[] args) {
        PriorityBlockingQueue<Task> priorityBlockingQueue = new PriorityBlockingQueue<>();
        Random random = new Random();
        for (int i = 0; i < 10; ++i) {
            Task task = new Task();
            task.setPriority(random.nextInt(10));
            task.setTaskName("taskName" + i);
            priorityBlockingQueue.offer(task);
        }

        while(!priorityBlockingQueue.isEmpty()){
            Task task = priorityBlockingQueue.poll();
            if(task != null){
                task.doSomething();
            }
        }
    }
}

```

#### 4.4 小结

PriorityBlockingQueue 队列在内部使用二叉树堆维护元素优先级，使用数组作为元素存储的数据结构，这个数组是可扩容的 。当当 前元素个数＞＝最大容量时会通过 CAS 算法扩容，出队时始终保证出队的元素是堆树的根节点，而不是在队列里面停留时间最长的元 素。使用元素的 compareTo 方法提供默认的元素优先级比较规则，用户可以自定义优先级的比较规则。





### 5. DelayQueue原理探究

DelayQueue 并发队列是一个无界阻塞延迟队列，队列中的每个元素都有个过期时间，当从队列获取元素时，只有过期元素才会出队列。队列头元素是最快要过期的元素。

#### 5.1 初步探究

 leader 变量的使用基于 Leader-Follower 模式的变体，用于尽量减少不必要的线程等待。当一个线程调用队列的 take 方法变为 leader 线程后，它会调用条件变量 available. awaitNanos(delay)等待 delay 时间，但是其他线程 (follwer线程)则会调用 available. await()进行无限等待。 leader 线程延迟时间过期后，会退出 take 方法，并通过调用`available.signal()`方法唤醒一个 follwer 线程，被唤醒的 follwer 线程被选举为新的 leader 线程。

#### 5.2 主要函数原理介绍

1. offer操作（非阻塞）

   ```java
       public boolean offer(E e) {
           final ReentrantLock lock = this.lock;
           lock.lock();
           try {
               q.offer(e);
             	//如果返回的是当前添加的元素，说明当前元素e是最先将过期的，就leader为null
               if (q.peek() == e) {
                   leader = null;
                 	//唤醒阻塞的一个follower节点
                   available.signal();
               }
               return true;
           } finally {
               lock.unlock();
           }
       }
   ```

2. take操作（阻塞）

   ```java
       public E take() throws InterruptedException {
           final ReentrantLock lock = this.lock;
           lock.lockInterruptibly();
           try {
               for (;;) {
                   E first = q.peek();
                 	//队列为空，阻塞挂起当前线程
                   if (first == null)
                       available.await();
                   else {
                     	//获取堆顶元素的延迟剩余时间
                       long delay = first.getDelay(NANOSECONDS);
                     	//延迟剩余时间小于等于0，就可以出队了
                       if (delay <= 0)
                           return q.poll();
                     	//在等待时，释放对节点的引用
                       first = null; // don't retain ref while waiting
                     	//检查leader是否为空，如果不为空，则说明其他线程也在执行take，然后把当前线程放入条件队列
                       if (leader != null)
                           available.await();
                       else {
                         	//其他线程没在执行take，则选取当前线程为leader线程
                           Thread thisThread = Thread.currentThread();
                           leader = thisThread;
                           try {
                             	//等待delay时间，这期间该线程会释放锁，所以其他线程可以offer添加元素，也可以take阻塞自己
                               available.awaitNanos(delay);
                           } finally {
                               if (leader == thisThread)
                                   leader = null;
                           }
                       }
                   }
               }
           } finally {
             	//说明当前线程从队列移除过期元素之后，又有其他线程执行了入队操作
               if (leader == null && q.peek() != null)
               		//于是激活条件队列中的等待线程
                   available.signal();
               lock.unlock();
           }
       }
   ```

3. poll操作

   ```java
       public E poll() {
           final ReentrantLock lock = this.lock;
           lock.lock();
           try {
               E first = q.peek();
               if (first == null || first.getDelay(NANOSECONDS) > 0)
                   return null;
               else
                   return q.poll();
           } finally {
               lock.unlock();
           }
       }
   ```

4. size操作

   ```java
       public int size() {
           final ReentrantLock lock = this.lock;
           lock.lock();
           try {
               return q.size();
           } finally {
               lock.unlock();
           }
       }
   ```

#### 5.3 案例介绍

略

#### 5.4 小结

其内部使用 PriorityQueue 存放数据，使用 ReentrantLock 实现线程同步。另外队列里面的元素要实现 Delayed 接口，其中一个是获取当前元素到过期时间剩余时间的接 口，在出队时判断元素是否过期了，一个是元素之间比较的接口，因为这是一个有优先级的队列。

