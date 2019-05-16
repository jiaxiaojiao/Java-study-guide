# collection 集合

![image](./image/collection.png)

## Collection 集合
存储Value
- List  有序集合，可以包含重复元素，可以按索引访问
    - ArrayList   数组，非线程安全
    - LinkedList  双向链表，非线程安全
    - Vector  数组，线程安全，synchronize同步。性能较差。
- Set  无序集合，不能包含重复元素，值唯一，根据equals和hashcode来判断。
    - HashSet  数组+链表，非线程安全，无序
    - TreeSet   红黑树，非线程安全，有序

## Map
数据结构：键值对，Key和Value

通过查找Key来获取对应的Value，Key的值不允许重复。

- HashMap  数组+链表/红黑树，非线程安全，无序，值不唯一， 实际开发过程中key值基本都是String类型的。
- HashTable  数组+链表，继承Dictionary，线程安全，无序，值不能为空
- TreeMap 红黑树，非线程安全，有序，不唯一
- ConcurrentHashMap， 底层数组+链表/红黑树，线程安全，无序，不唯一

#### Map相关方法的底层实现原理
- Map底层实现的基础是数组和链表，通过数组的方式存储Map的每个元素。
- 新增 put(Object key, Object value) ，新创建的Map对象设置为myMap数组的最后一个，让数组的长度加一。
- 查找 get(Object key) ，通过对存储Map对象的数组的遍历，找到key值，然后返回Value。
- key可以是任意类型，实际开发过程中key值基本都是String类型的，基本数据类型，引用数据类型。

