# ArrayList


#### 1. ArrayList中的subList

关于集合类，《阿里巴巴Java开发手册》规定：

> 【强制】 ArrayList的subList结果不可强转成ArrayList，否则会抛出ClassCastException异常，即java.util.RandomAccessSubList cannot be cast to java.util.ArrayList.
说明： subList返回的是ArrayList的内部类SubList，并不是ArrayList，而是ArrayList的一个视图，对于SubList子列表的所有操作最终会反映在原列表上。

> 【强制】 在subList场景中，高度注意对原集合元素的增加或删除，均会导致子列表的遍历、增加、删除产生ConcurrentModificationException 异常。

subList是List接口中定义的一个方法，该方法主要用于返回一个集合中的一段、可以理解为截取一个集合中的部分元素，他的返回值也是一个List。

如果将返回值强转成ArrayList会报错。

原因： 

subList 方法返回的是一个视图。返回了一个类SubList，这个类是ArrayList的一个内部类。内部类SubList的构造函数SubList把原来的List和部分属性直接赋值给了自己。也就是说，SubList并没有重新创建一个List，而是直接引用了原有的List（返回了父类的视图），只是指定了范围（从fromIndexb包括 到 toIndex不包括）。

SubList只是ArrayList的内部类，他们之间并没有继承关系，故无法直接进行强制类型转换。

**subList对List的影响/使用时的问题：**
- 通过set修改subList元素时，原List中对应的值也修改了。同样修改原List的值，subList也修改。
- 通过add添加subList元素时，原List也添加了元素。
- 通过add添加原List元素时，异常。

总结： 
- 对父(sourceList)子(subList)List做的非结构性修改（non-structural changes），都会影响到彼此。
- 对子List做结构性修改，操作同样会反映到父List上。
- 对父List做结构性修改，会抛出异常ConcurrentModificationException。

#### 2. 对比 ArrayList和linkedList Vector
都实现了List接口。
- ArrayList
 - 底层使用数组，内存中开辟一块连续的空间来存储，数据存储是连续的，支持下标来访问元素。
 - 更多元素添加进来时会请求更大的空间，每次对size增长50%。
 - 非线程安全。
 - 使用在查询多，插入删除比较少的场景。数组查询查询特定元素比较快。
- Vector 
 - 与ArrayList几乎相同，但是Vector是线程安全的。synchronize 同步。性能较差。
 - 更多元素添加进来时会请求更大的空间，每次请求其大小的双倍空间。
- LinkedList 
- 底层使用双向链表。使用在查询少，插入删除比较多的场景。
- 非线程安全




