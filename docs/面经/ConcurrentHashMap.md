## ConcurrentHashMap

### 1.7

分段锁的思想。

![JDK1.7的ConcurrentHashMap](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-6/ConcurrentHashMap%E5%88%86%E6%AE%B5%E9%94%81.jpg)

首先将数据分为一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据时，其他段的数据也能被其他线程访问。

```java
 final Segment<K,V>[] segments;
```

主干是一个Segment数组，一个ConcurrentHashMap维护一个Segment数组

```java
transient volatile HashEntry<K,V>[] table;
static class Segment<K,V> extends ReentrantLock implements Serializable {
}
```

Segment继承了ReentrantLock，所以它就是一种可重入锁（ReentrantLock)。在ConcurrentHashMap，一个Segment就是一个子哈希表，Segment里维护了一个HashEntry数组，并发环境下，对于不同Segment的数据进行操作是不用考虑锁竞争的。

### 1.8

JDK1.8取消了分段锁，数据结构和HashMap1.8的结构类似，采用CAS和sunchronized来保证并发安全。

synchronized只锁定当前链表或者红黑树的首节点，这样只要hash不冲突，就不会产生并发。

![JDK1.8的ConcurrentHashMap](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-6/JDK1.8-ConcurrentHashMap-Structure.jpg)

### 1.7和1.8的区别

- 底层数据结构

  1.7：Segment数组+HashEntry

  1.8：Node数组+链表+红黑树

- 线程安全

  1.7：利用分段锁，锁加在segment上

  1.8：利用synchronized+CAS，只对链表或者红黑树的首节点加锁