# HashMap

- 实现Map接口，元素以键值对的方式存储
- Key和Value 都允许为空。Key不允许重复，只能有一个Key为null。
- 不能保证放入元素的顺序，无序。
- 非线程安全。

## 底层实现和原理
#### 原来的HashMap
1. 数据结构哈希表（数组+链表）
2. 默认大小 16
3. 达到总容量的0.75，数组扩容。
4. 存储元素，首先计算hashcode，计算数组的索引值，对应索引没有元素直接添加，有元素equals比较key如果相等就覆盖 不相等后加的放在前面。
5. 效率低


#### 1.8之后的HashMap
1. 数据结构：数组+链表+红黑树
2. 碰撞元素>8 & 总容量大于64，引入红黑树。
3. 末尾插入。
4. 效率高。

![image](image/map-hashmap-1.md)


## 常用方法
#### put(K,V)
```text
public V put(K key, V value) {  
        return putVal(hash(key), key, value, false, true);  
    }  
  
  //插入值， onlyIfAbsent，为真的话，就是不替换，无就插，有就不插    methods evict，表示需要调整二叉树结构 在LinkedHashMap使用
final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {  
        Node<K,V>[] tab; Node<K,V> p; int n, i;  
        if ((tab = table) == null || (n = tab.length) == 0)     //1 首先判断table数组的长度是否为0或table数组是否为空，即通常情况下表示刚创建一个空的HashMap时，当你调用put（K，V）方法时才会分配内存，即tab = resize()
            n = (tab = resize()).length;  
        if ((p = tab[i = (n - 1) & hash]) == null)        //2首先判断tab[(n - 1) & hash]处是否为空,如果是代表该数组下标为[(n - 1) & hash]的位置无元素，可直接put  
            tab[i] = newNode(hash, key, value, null);              // 没有数据，就是放一个链表头节点
        else {  
            Node<K,V> e; K k;  
            if (p.hash == hash &&  ((k = p.key) == key || (key != null && key.equals(k))))    //哈希值和equals值均同 则用新值换旧值
                e = p;  
            else if (p instanceof TreeNode)                         //否则  判断是否需要红黑树结构
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);  
            else {                                                  //否则 为链表结构
                for (int binCount = 0; ; ++binCount) {  
                    if ((e = p.next) == null) {  
                        p.next = newNode(hash, key, value, null);  
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st  
                            treeifyBin(tab, hash);                  //把链表转为二叉树存储
                        break;  
                    }  
                    if (e.hash == hash && ((k = e.key) == key || (key != null && key.equals(k))))//如果Hash值相同，则调用equals方法来确定是否存在该元素，则执行break语句  
                        break;//跳出for循环，执行下面的if语句，更新value的值.
  
                    p = e;  
                }  
            }  
            if (e != null) { // existing mapping for key  // 替换操作，key一样，旧值换为新值
                V oldValue = e.value;  
                if (!onlyIfAbsent || oldValue == null)  
                    e.value = value;  
                afterNodeAccess(e);//  
                return oldValue;  
            }  
        }  
        ++modCount;  
        if (++size > threshold)  
            resize();  
        afterNodeInsertion(evict);   //LinkedHashMap使用
        return null;  
    }
```

#### V get(Object key) 

```text
public V get(Object key) {  
        Node<K,V> e;  
        return (e = getNode(hash(key), key)) == null ? null : e.value;   //null值返回
    }  
final Node<K,V> getNode(int hash, Object key) {  
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;  
    if ((tab = table) != null && (n = tab.length) > 0 && (first = tab[(n - 1) & hash]) != null) {   //通过该hash值与table的长度n-1相与得到数组的索引first  
        if (first.hash == hash && ((k = first.key) == key || (key != null && key.equals(k))))  
            return first;            // always check first node
        if ((e = first.next) != null) {  
            if (first instanceof TreeNode)        //代表该HashMap为数组+红黑树结构  
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);  
            do {                                //否则代表是数组+链表结构  
                if (e.hash == hash && ((k = e.key) == key || (key != null && key.equals(k))))  
                return e;  
            } while ((e = e.next) != null);  
        }
    }  
    return null;  
}
```

## 总结：
1. HashMap内部是基于Hash表实现的，该Hash表为Node类型数组+链表/红黑树，其中链表与红黑树是用来解决冲突的，即当往HashMap中put某个元素时，相同的hash值的两个值会被放到数组中的同一个位置上形成链表或红黑树。
2. HashMap存在扩容机制，是通过resize()方法实现的，即当HashMap中的元素个数超过数组大小*loadFactor时，就会进行数组扩容，loadFactor的默认值为0.75，数组的大小*loadFactor=threshold（即扩容的临界值），默认情况下，数组大小为16，那么当HashMap中元素个数超过16*0.75=12的时候，就把数组的大小扩展为 2*16=32，即扩大一倍
3. 另外从put与get的源码可以看到HashMap的Key与Value都允许为null，同时可以看到HashMap中的put与get方法均无synchronized关键字修饰，即HashMap不是线程安全的。
4. HashMap中的元素是唯一的（即同一个key只存在唯一的V与之对应），因为在put的过程中如果可能出现相同元素（K相同V不同），则原来的V将会被替换。

## 其他
```text
https://www.jianshu.com/p/3bf097f4cf0a
https://blog.csdn.net/P_Doraemon/article/details/80353579
https://blog.csdn.net/yinbingqiu/article/details/60965080
https://www.cnblogs.com/wxw7blog/p/7722274.html
https://www.cnblogs.com/java-jun-world2099/p/9258605.html

```