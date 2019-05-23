# Map 

数据结构：键值对，Key和Value

通过查找Key来获取对应的Value，Key的值不允许重复。

- [HashMap](Map-HashMap.md)  数组+链表/红黑树，非线程安全，无序，值不唯一， 实际开发过程中key值基本都是String类型的。
- HashTable  数组+链表，继承Dictionary，线程安全，无序，值不能为空
- [TreeMap](Map-TreeMap.md) 红黑树，非线程安全，有序，不唯一
- ConcurrentHashMap， 底层数组+链表/红黑树，线程安全，无序，不唯一

## Map相关方法的底层实现原理
- Map底层实现的基础是数组和链表，通过数组的方式存储Map的每个元素。
- 新增 put(Object key, Object value) ，新创建的Map对象设置为myMap数组的最后一个，让数组的长度加一。
- 查找 get(Object key) ，通过对存储Map对象的数组的遍历，找到key值，然后返回Value。
- key可以是任意类型，实际开发过程中key值基本都是String类型的，基本数据类型，引用数据类型。


##  [HashMap](Map-HashMap.md)
- 存储<key,value>，实现Map，key/value可以为null
- 线程不安全，效率较高。
- 实现HashMap的同步：Map m = Collections.synchronizedMap(new HashMap())

## HashTable
- 存储<key,value>，继承Dictionary，key/value不能为null
- 线程安全，效率较低。
