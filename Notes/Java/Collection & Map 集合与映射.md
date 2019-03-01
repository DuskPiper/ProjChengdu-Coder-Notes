# Collection & Map 集合类与映射

###简述

集合有两个基本的接口：Collection集合类 和 Map映射。

Collection集合类主要负责保存、盛装其他数据，因此集合类也被称为容器类。所以的集合类都位于java.util包下，后来为了处理多线程环境下的并发安全问题，java5还在java.util.concurrent包下提供了一些多线程支持的集合类。

Collection和Map的区别在于容器中每个位置保存的元素个数，Collection 每个位置只能保存一个元素(对象)，Map保存的是"键值对"，就像一个小型数据库。我们可以通过"键"找到该键对应的"值"。



## Collection 集合

### 简述

整个集合框架就围绕一组标准接口而设计。

![集合继承关系](https://raw.githubusercontent.com/DuskPiper/ProjChengdu-Coder-Notes/master/Illustration/%E9%9B%86%E5%90%88%E7%BB%A7%E6%89%BF%E5%85%B3%E7%B3%BB.jpg)

### 接口

- `Collection` 为集合层级的根接口。一个集合代表一组对象。Java不提供此接口直接的实现。

- `Set` 是一个不能包含重复元素的集合。

- `List `  是一个有序集合，可包含重复元素。可以通过它的索引来访问任何元素。List像长度动态变换的数组。

- `Map ` 是一个将key映射到value的对象.一个Map不能包含重复的key：每个key最多只能映射一个value。
- `Map.Entry` 描述在一个Map中的一个元素（键/值对）。是一个Map的内部类。
- `SortedSet` 继承于`Set` ，保存有序的集合。
- `SortedMap` 继承于 `Map` ，保存key有序的映射。
- `Iterator` 迭代器。

### 实现类

####List类

#####`LinkedList`

- 实现`List`接口，允许`null`元素。基于链表数据结构。

- 线程不安全。没有同步方法，要实现多线程同步只能在创建时构造同步：

  ```java
  List list = Collections.synchronizedList(new LinkedList());
  ```

- 查找效率低。
- 插入删除效率高。
- 可以方便地用做Stack, Queue, Dequeue。

#####`ArrayList`

- 实现`List`接口，允许`null`元素。
- 随机访问和遍历时，效率优于`LinkedList`。
- 非同步，禁止在多线程下使用。
- 装满时增加当前长度的50%，插入和删除效率低。
- 线程不安全，实现多线程同步的方法同LinkedList。

#####`Vector`

- 基于动态数组，实现类似于`ArrayList`，但线程安全。线程安全导致其访问更慢。
- 包含许多传统方法，这些方法不属于集合框架。
- 装满时增加当前长度的100%。
- 使用很少，因为性能低下。

#### Set类

##### `HashSet`

- 基于**HashMap**实现，底层采用HashMap保存元素。
- 通过检查元素的`hashCode`来实现重复检查。
- 支持`O(1)`快速查找，但不支持有序性操作。
- 失去插入顺序信息，用Iterator遍历的结果顺序是不定的。

##### `TreeSet`

- 成员有序。
- 通过封装了一个HashSet的成员变量来实现的，底层**基于红黑树**。
- 支持有序性操作，如`ceiling()` `floor()`。
- 操作复杂度`O(logN)`，较优但低于HashSet的`O(1)`。

##### `LinkedHashSet`

- 具有HashSet的`O(1)`查找效率，且内部基于双向链表维护了元素的插入顺序。

#### Queue类

##### `PriorityQueue`

- 通过二叉小顶堆实现，可以用一棵完全二叉树表示。
- 插入和删除的时间复杂度是`O(logN)`。
- 实现优先队列。优先队列的作用是能保证每次取出的元素都是队列中权值最小的。
- 默认通过自然顺序排序，也就是数字默认是小的在队列头，字符串则按字典序排列。可以传入自定义的`Comparator`。

####Iterator

- Iterator可以删除访问的当前元素，而for循环中执行`remove()`删除操作可能带来 `ConcurrentModificationException`。

## Map 映射

### 简述

Map保存具有映射关系的数据，因此Map集合里保存着两组数，一组值用户保存Map里的key，另一组值用户保存Map里的value，key和value都可以是任何引用类型的数据。Map的key不允许重复，因此key也组成了keySet和entrySet。

### 实现类

##### `TreeMap`

- `implements SortedMap`，元素有序。支持有序性操作，如`ceiling()` `floor()`。
- 底层基于红黑树实现。
- 与TreeSet相同，可以基于自然排序，也可以传入自定义的`Comparator`。
- 如果使用自定义类作为TreeMap的key，则应重写该类的equals()方法和compareTo()方法时应保持一致的返回结果：两个key通过equals()方法比较返回true时，它们通过compareTo()方法比较应该返回0。如果不一致，TreeMap与Map接口的规则会冲突。

##### `HashMap`

- 基于`hash()`实现。有`O(1)`的操作复杂度。
- 使用LinkedList处理hash Collision。
- 如果大量碰撞导致LinkedList过长(`>= TREEIFY_THRESHOLD`)，则将其转换为红黑树。
- 如果bucket满了(`> loadFactor * capacity​`)，就要resize。此时扩容100%并重新hash计算index位置。
- 非同步因此非线程安全。可以使用ConcurrentHashMap来保证线程安全。

##### `LinkedHashMap`

- 是HashMap的子类，具有一切HashMap的特性。
- 使用双向链表来维护元素的顺序，通常为插入顺序(默认)或LRU顺序。
- 每个Entry包含before和after指针。
- 维护了插入的先后顺序，因此其可以用来做缓存。
- LinkedHashMap在向哈希表添加一个键值对的同时，也会将其链入到双向链表中，以设定迭代顺序。

##### `Hashtable`

- 已经几乎不使用了。
- 是线程安全版的HashMap，但效率很低，因此采用同样线程安全的ConcurrentHashMap代替。
- 继承自Dictionary类。
- 不允许键或者值为`null`。



## Differences 区分 

###Collections和Array的区别

- 数组长度在初始化时指定，只能保存定长的数据。而集合可以保存数量不确定的数据；且能保存具有映射关系的数据（即关联数组，键值对 key-value）。
- 数组的元素可以是基本类型或者对象；集合只能保存对象（实际上是保存对象的引用）。基本数据类型的变量要转换成对应的包装类才能放入集合。

### Set和List的区别

- Set 接口实例存储的是无序的，不重复的数据。List 接口实例存储的是有序的，可以重复的元素。
- Set 检索效率低下，删除和插入效率高，插入和删除不会引起元素位置改变 **<实现类有HashSet,TreeSet>**。
- List 和数组类似，但可以动态增长，根据实际存储的数据的长度自动增长List的长度。查找元素效率高，插入删除效率低，因为会引起其他元素位置改变 **<实现类有ArrayList,LinkedList,Vector>** 。

### ArrayList和Vector区别

- ArrayList在内存不够时默认是扩展50% + 1个，Vector是默认扩展1倍。

- Vector提供indexOf(obj, start)接口，ArrayList没有。

- Vector属于线程安全级别的，但是大多数情况下不使用Vector，因为线程安全需要更大的系统开销。

###ArrayList和LinkedList异同

- 存储结构上 ArrayList 底层使用数组进行元素的存储，LinkedList 使用双向链表作为存储结构。

- 在性能上 ArrayList 在存储大量元素时候的增删效率平均低于LinkedList。

- 在根据角标获取元素的时间效率上ArrayList优于LinkedList。

- 使用for循环去遍历LinkedList效率很低。

- 两者均与允许存储 null 也允许存储重复元素。

- 两者都是线程不安全的，都可以使用 `Collections.synchronizedList(List<E> list)` 方法生成一个线程安全的 List。

### 存放`null`的能力

- List可以存储null，添加几个就存储几个。

- Set可以存储null，但只能存储一个，即使添加多个也只能存储一个。

- HashMap可以存储null键值对，键和值都可以是null，但如果添加的键值对的键相同，则后面添加的键值对会覆盖前面的键值对，即之后存储后添加的键值对。

- **Hashtable不能碰null，不管是值还是键，一见null就报空指针。**
- **PriorityQueue不能存null**，因为堆中不能插入null作为节点

### HashMap和Hashtable异同

- HashMap可以存储null键值对，Hashtable不能存储null键值对。
- HashMap是非synchronized/线程安全，而Hashtable是synchronized/线程安全。
- 因为线程安全的缘故，Hashtable的读写效率低于HashMap。
- Java 5提供了线程安全的ConcurrentHashMap，它是HashTable的替代，比HashTable的扩展性更好。
- HashMap内部使用`hash(Object key)`扰动函数对 key 的 `hashCode` 进行扰动后作为 `hash` 值。HashTable 是直接使用 key 的 `hashCode()` 返回值作为 hash 值。
- HashMap默认容量为 $2^4$ 且容量一定是 $2^n$；HashTable 默认容量是11，不一定是 $2^n$。



##References 参考资料

- [由浅入深理解java集合(一)——集合框架 Collection、Map](https://www.jianshu.com/p/589d58033841)
- [40个Java集合面试问题和答案](http://www.importnew.com/15980.html)
- [Java 集合框架 - RUNNOB](http://www.runoob.com/java/java-collections.html)
- [Java中Vector和ArrayList的区别](https://www.cnblogs.com/wanlipeng/archive/2010/10/21/1857791.html)
- [List、Set、Map集合存放null解析及HashMap、Hashtable异同点解析](https://blog.csdn.net/koushr/article/details/47345619)
- [HashSet 的实现原理](http://wiki.jikexueyuan.com/project/java-collection/hashset.html)
- [搞懂 HashSet & LinkedHashSet 源码以及集合常见面试题目](https://juejin.im/post/5ad6313df265da2386706662)
- [深入理解Java PriorityQueue](https://www.cnblogs.com/CarpenterLee/p/5488070.html)
- [由浅入深理解java集合(五)——集合 Map](https://www.jianshu.com/p/0580eb808eea)
- [Java HashMap工作原理及实现](https://yikun.github.io/2015/04/01/Java-HashMap%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86%E5%8F%8A%E5%AE%9E%E7%8E%B0/)
- [Map 综述（二）：彻头彻尾理解 LinkedHashMap](https://blog.csdn.net/justloveyou_/article/details/71713781)