# 集合

![image](image/collection.png)

## Collection
存储Value
- List  有序集合，可以包含重复元素，可以按索引访问
    - ArrayList   数组，非线程安全
    - LinkedList  双向链表，非线程安全
    - Vector  数组，线程安全，synchronize同步。性能较差。
- Set  无序集合，不能包含重复元素，值唯一，根据equals和hashcode来判断。
    - HashSet  数组+链表，非线程安全，无序
    - TreeSet  红黑树，非线程安全，有序

## [Map](Map.md)

 
## 拓展
- [ArrayList 的 subList 使用注意事项](List-ArrayList.md#1-arraylist中的sublist) 
- [有序集合对比 ArrayList LinkedList Vector](List-ArrayList.md#2-对比-arraylist和linkedlist-vector)
