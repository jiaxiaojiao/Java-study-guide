# 面试总结

## Java基础知识
#### 1. 栈和队列
```text
栈和队列的数据类型： 
相同的： 都允许在端点插入和删除元素。
不同点： 栈只在栈顶访问，遵循后进先出原则（LIFO）；队列是在队尾插入数据，在队头删除数据（FIFO）

```

#### 2. 接口和抽象类
```text
抽象类： 含有抽象方法，用abstract修饰。对类抽象，继承关系。
接口interface：供别人调用的方法和函数。对行为抽象，实现。接口中的方法都是抽象方法。实现接口使用implements。
抽象方法： 只有声明，没有具体实现。abstract void fun()

```

#### 3. int和Integer的
```text
int是基本数据类型。存数据值。 默认0
Integer是int的包装类。 实例化后才可以使用。new Integer是生成一个指针指向这个对象。默认null

```
#### 4. 自动拆箱/装箱
```text
自动装箱
用基本类型的值给它的包装类型赋值时，系统会自动将基本类型转成它的包装类型。

自动拆箱
当需要基本类型的时候，系统会自动将包装类型转成基本类型。

```

#### 5. 常量池
```text
分为两种： 静态常量池 / 运行时常量池

静态常量池： class文件中的常量池：字符串 类和方法。
运行时常量池： class文件被加载进内存之后，常量池保存在了方法区。 通常说的常量池，是运行时常量池。 

好处： 
1. 避免频繁的创建和销毁对象而影响系统性能。
2. 实现了对象的共享

```

#### 6. == 和equals
```text

== 
比较基本类型值。
比较对象内存地址。

equals() 方法
是Object对象中的方法
继承Object的类都会重写这个方法。
```

#### 7. 重载和重写
```text
重载，同一个类中，方法名相同，参数不同，方法返回值和访问修饰符可以不同，发生在编译时。
重写，父子类之间，方法名相同，参数相同。

```

#### 8. String 和 StringBuffer ，StringBuilder的区别
```text
String 字符串长度不可变，底层使用final char[] value，不可以继承。
StringBuffer StringBuilder 字符串长度可变，底层使用char[] value，

拼接字符串
String 需要创建多个，效率低
StringBuilder  线程不安全，
StringBuffer  线程安全，synchronized

效率 String < StringBuffer < StringBuilder
```

## 集合框架

#### 1. ArrayList
```text
Collection 存储Value
|-- List 有序集合，可以包含重复元素，可以按索引访问
    |-- ArrayList   数组，非线程安全

底层使用数组，内存中开辟一块连续的空间来存储，数据存储是连续的，支持下标来访问元素。
更多元素添加进来时会请求更大的空间，每次对size增长50%。
使用在查询多，插入删除比较少的场景。用的多。

```

#### 2. LinkedList
```text
Collection 存储Value
|-- List 有序集合，可以包含重复元素，可以按索引访问
    |-- LinkedList  双向链表，非线程安全

底层使用双向链表。使用在查询少，插入删除比较多的场景。

```

#### 3. HashMap
```text
Map 存储键值对，Key和Value
|-- HashMap 数组+链表/红黑树，无序，值不唯一

实际开发过程中key值基本都是String类型的。
key/value可以为null
线程不安全，效率较高。
实现HashMap的同步：Map m = Collections.synchronizedMap(new HashMap())

```

#### 4. LinkedHashMap
```text
Map 存储键值对，Key和Value
|-- LinkedHashMap 有序，非线程安全

Key和Value都允许空


```

#### 5. ConcurrentHashMap
```text
Map 存储键值对，Key和Value
|-- ConcurrentHashMap 数组+链表/红黑树，线程安全，无序，不唯一

```

#### 6. 1.7版本和1.8版本的HashMap的区别
```text
1.7版本的HashMap
    1. 数据结构哈希表（数组+链表）
    2. 默认大小 16
    3. 达到总容量的0.75，数组扩容。
    4. 存储元素，首先计算hashcode，计算数组的索引值，对应索引没有元素直接添加，有元素equals比较内容如果相等就覆盖 不相等后加的放在前面。
    5. 效率低


1.8之后的HashMap
    1. 数据结构：数组+链表+红黑树
    2. 碰撞元素>8 & 总容量大于64，引入红黑树。
    3. 末尾插入。
    4. 效率高。
```

#### 7. 1.7版本和1.8版本的ConcurrentHashMap的区别
```text
1.8之后的HashMap
    1. 取消segments字段，直接采用transient volatile HashEntry<K,V>[] table保存数据，采用table数组元素作为锁，从而实现了对每一行数据进行加锁，进一步减少并发冲突的概率。
    2. 数组结构： 数组+链表+红黑树。
    3. 效率高

```

#### 8. HashMap能不能排序
```text
HashMap无序。
如果要对HashMap进行排序，可以借助List集合。keySet()/entrySet()

```

#### 9. HashMap的长度为什么是2的幂次方。
```text
为了提升性能。
当 array.length长度是2的次幂时，key.hashcode % array.length等于key.hashcode & (array.length - 1)

好处： 
1. 能利用 与运算& 操作代替 取模运算% 操作，提升性能
2. 数组扩容时，仅仅关注 “特殊位” 就可以重新定位元素

```

## 多线程

#### 1. 创建线程的几种方式？wait sleep分别是谁的方法，区别，线程间的通信方式。
```text
1. 通过继承Thread类实现一个线程
   Java单继承，不适合资源共享。不推荐。
2. 通过实现Runnable接口实现一个线程
   很容易实现资源共享。推荐。
3. 覆写Callable接口实现多线程。
   call() 有返回值。

wait是Object类的方法。
sleep是Thread类的方法。
区别：
    1. sleep 是Thread类的静态方法，调用此方法会让当前线程暂停指定的时间，将执行机会（CPU）让给其他线程，但是不会释放锁，因此休眠时间结束后自动恢复（程序回到就绪状态）。
    2. wait  是Object类的方法，调用对象的wait方法导致线程放弃CPU的执行权，同时也放弃对象的锁（线程暂停执行），进入对象的等待池（wait pool），只有调用对象的notify或notifyAll方法才能唤醒等待池中的线程进入等锁池（lock pool），如果线程重新获得对象的锁就可以进入就绪状态。
    3. wait notify和notifyAll 只能在同步控制方法中或者同步控制块中使用，而sleep可以在任何地方使用。
    4. sleep必须捕获异常，而wait，notify和notifyAll不需要捕获异常。

线程之间的通信方式：
分布式系统中说的两种通信机制：共享内存机制和消息通信机制。
    1. 消息队列。 常用，灵活。
       通过自定义数据结构，可以传输复杂和简单的数据结构。
    2. 使用全局变量。常用。
       定义全局变量时最好使用volatile来定义，以防编译器对此变量进行优化。
    3. 使用事件CEvent类实现线程间通信。
       CEvent对象有两种状态：有信号和无信号，线程可以监视处于有信号状态的事件，以便在适当的时候执行对事件的操作。

```

#### 2. 死锁，怎么排查死锁？
```text
死锁： 
    两个或两个以上的进程(或线程)在执行过程中，由于竞争资源或者是彼此通信而造成的一种阻塞的现象。
    若无外力作用，它们都将无法推进下去。
    此时称系统处于死锁状态或系统产生了死锁，这些永远在互相等待的进程称为死锁进程。
可能原因：
    1. 锁的嵌套（也包括多线程并发锁嵌套问题）。临界变量。
    2. 中断操作 锁没有释放。
    3. 系统资源不足。
    4. 进程运行推进的顺序不合适。
    5. 资源分配不当。

排查死锁：
    1. Jconsole“检测死锁”。 JDK自带的图形化界面工具
    2. 利用 jstack 定位死锁。JDK自带的命令行工具，主要用于线程Dump分析。
    3. 查看CPU占用。
    4. 压力测试使用jstack找到系统的代码性能问题。

死锁的处理方法：
    1. 从性能出发，优化SQL
    2. 从业务逻辑出发，看能不能去掉竞争资源的关联。
    3. try catch 等一会儿再执行。

```

#### 3. 创建线程池的几种方式，线程池有什么好处？
```text
Java通过Executors提供了四个静态方法创建线程池：
    1. newCachedThreadPool 创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则创建新线程。
    2. newfixedThreadPool 创建一个定长线程池，可控制线程最大并发量，超出的线程会在队列中等待。
    3. newScheduledThreadPool 创建一个定长线程池，支持定时及周期性任务执行。
    4. newSingleThreadExecutor 创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序（FIFO,LIFO,优先级）执行。

线程池的作用？
    1. 限定线程的个数，不会由于线程过多，导致系统运行缓慢或崩溃。
    2. 线程池不需要每次都去创建或销毁，节约了资源。
    3. 响应时间更快。

```

#### 4. 线程继承和接口的区别，接口有什么好处？
```text
类似问题1 创建线程的几种方式。
1. 通过继承Thread类，重写Thread的run()方法，将线程运行的逻辑放在其中。
2. 通过实现Runnable接口，实例化Thread类。

实现Runnable接口 相对于继承Thread类来说，有如下优势：
1. 适合多个相同程序代码的线程去处理同一资源的情况。
2. 避免由于Java的单继承特性带来的局限性。扩展性好。
3. 增强了程序的健壮性，代码能够被多个线程共享，代码与数据是独立的。资源共享。

```

#### 5. Synchronized  lock reentrantLock，区别，用法，原理。
```text
共同点：
    1. 加锁方式同步，阻塞式同步。
    
区别：
    Synchronized 
        1. 是java语言的关键字，是原生语法层面的互斥，需要jvm实现。
        2. 便利性：使用比较简洁，并且由编译器去保证锁的加锁和释放。
        3. 不如ReentrantLock灵活。
        4. 性能：Synchronized引入了偏向锁，轻量级锁（自旋锁）后，性能与ReentrantLock差不多。
    ReentrantLock 可重入锁
        1. 是JDK 1.5之后提供的API层面的互斥锁，需要lock()和unlock()方法配合try/finally语句块来完成。
        2. 便利性：需要手工声明来加锁和释放锁，为了避免忘记手工释放锁造成死锁，所以最好在finally中声明释放锁。
        3. 更灵活。
        4. 性能高。
        5. ReentrantLock可以指定是公平锁还是非公平锁。而synchronized只能是非公平锁。所谓的公平锁就是先等待的线程先获得锁。
        6. ReentrantLock提供了一个Condition（条件）类，用来实现分组唤醒需要唤醒的线程们，而不是像synchronized要么随机唤醒一个线程要么唤醒全部线程。
        7. ReentrantLock提供了一种能够中断等待锁的线程的机制，通过lock.lockInterruptibly()来实现这个机制。

ReentrantLock实现的原理：
ReentrantLock的实现是一种自旋锁，通过循环调用CAS操作来实现加锁。它的性能比较好也是因为避免了使线程进入内核态的阻塞状态。


```

ReentrantLock的用法：

```java
public class SynDemo{
	public static void main(String[] arg){
		Runnable t1=new MyThread();
		new Thread(t1,"t1").start();
		new Thread(t1,"t2").start();
	}
}
class MyThread implements Runnable { 
	private Lock lock=new ReentrantLock();
	public void run() {
		lock.lock();
		try{
			for(int i=0;i<5;i++)
				System.out.println(Thread.currentThread().getName()+":"+i);
		}finally{
			lock.unlock();
		}
	}
}
```

#### 6. CountDownLatch 与 CyclicBarrier 用法
```text

```

#### 7. volatile关键字的作用和原理
```text

```

#### 8. 乐观锁和悲观锁
```text

```

#### 9. 对公平锁，非公平锁，可冲入锁，自旋锁，读写锁的理解。
```text

```

#### 10. CAS是什么，底层原理
```text

```

#### 11. ArrayBlockingQueue ， LinkedBlockingQueue， SynchronousQueue等堵塞队列的理解。
```text

```

#### 12. ThreadPoolExecutor的传入参数及内部工作原理。
```text

```

#### 13. 分布式环境下，怎么保证线程安全。
```text

```

## JVM相关问题
- JVM内存机制
- 介绍下垃圾回收机制，垃圾回收机制有哪些算法，各自的特点。
- 聊聊GC， Major GC ，FullGC区别，垃圾收集器有哪些，他们的区别？
- OutOfMemoryError  怎么处理
- JVM调优有哪些参数
- 自己写一个类叫 java.lang.String 类加载过程，双亲委派模型。
## 框架相关问题
- SSM 
- ORM
- Spring使用了哪些设计模式？
- Spring注入bean的方式
- 对Spring IOC 和 Spring AOP的理解
- Spring事务隔离级别和传播机制
- Mybatis的缓存机制（一级缓存和二级缓存）
- Mybatis的mapper文件中 # $ 的区别
- Spring MVC的流程
- Spring 和 Spring Boot的区别
- 对SpringBoot的理解
- RPC框架有哪些，他们的区别？
- Dubbo的使用和理解
- Spring Cloud的使用和组件，谈谈你的理解。
## 消息中间件
- MQ 消息队列
- Kafka
- 如何保证消息中间件的高可用？
- 如何保证消息中间件重复发送消息？
- 消息队列积压了大量的消息，你该怎么处理？
- 如何保证消费者消费消息是有顺序的？
- 让你开发一个消息中间件，你会怎么架构？
## Redis
- Redis 默认访问端口。
- Redis 数据类型
- Redis的持久化机制？
- Redis的过期策略？
- 怎么保证Redis的高可用？
- 什么是缓存穿透，如何避免？
- 什么是缓存雪崩，如何避免？
- 如何保证缓存与数据的双写一致性？
- Redis单线程模型原理，为什么能支撑高并发？
- Redis哨兵架构的理解和底层原理？
## 数据库
- MySQL Oracle 默认访问端口
- SQL优化
- 什么情况下索引会失效？
- 数据库的存储引擎，比如：MySQL的MyISAM和InnoDB的区别？
- 索引的最左原则
- 索引的底层原理
- 分库分表，分库分表方案

## 其他
- 分布式事务是怎么解决的？
- 分布式Session方案
- 设计一个秒杀场景
- 怎么防止表单多次提交
- Linux的基本操作命令
- ElasticSearch的使用和原理
- zookeeper的使用和原理
- zookeeper 默认访问端口
- JDK源码，看源码
- 个人博客
- 开源项目

## HR面试