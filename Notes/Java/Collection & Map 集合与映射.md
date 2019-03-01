# Collection & Map 集合与映射

###简述

集合类主要负责保存、盛装其他数据，因此集合类也被称为容器类。所以的集合类都位于java.util包下，后来为了处理多线程环境下的并发安全问题，java5还在java.util.concurrent包下提供了一些多线程支持的集合类。

Collection和Map的区别在于容器中每个位置保存的元素个数，Collection 每个位置只能保存一个元素(对象)，Map保存的是"键值对"，就像一个小型数据库。我们可以通过"键"找到该键对应的"值"。

![集合与映射框架图示](http://www.runoob.com/wp-content/uploads/2014/01/java-coll.png)



## Collection 集合

