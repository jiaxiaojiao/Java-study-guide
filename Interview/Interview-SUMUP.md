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
共同点： 加锁方式同步，阻塞式同步。都是可重入锁。
    
区别：
    Synchronized 同步
        1. 是java语言的关键字，是原生语法层面的互斥，需要jvm实现。
        2. 便利性：使用比较简洁，并且由编译器去保证锁的加锁和释放。
           在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生。
        3. 不如ReentrantLock灵活。
            ①. 等待的线程会一直等待下去，不能够响应中断。
            ②. 无法判断锁的状态。
        4. 性能：Synchronized引入了偏向锁，轻量级锁（自旋锁）后，性能与ReentrantLock差不多。
        5. 悲观锁。
    Lock 锁
        1. Lock是一个类，接口，通过这个类可以实现同步访问。
        2. 需要手动释放锁。
        3. 乐观锁。
    ReentrantLock 可重入锁
        1. 是唯一实现了Lock接口的类，并且ReentrantLock提供了更多的方法
        1. 是JDK 1.5之后提供的API层面的互斥锁，需要lock()和unlock()方法配合try/finally语句块来完成。
        2. 便利性：需要手工声明来加锁和释放锁。
           为了避免忘记手工释放锁造成死锁，所以最好在finally中声明释放锁。
        3. 更灵活。
            ①. ReentrantLock可以指定是公平锁还是非公平锁。而synchronized只能是非公平锁。所谓的公平锁就是先等待的线程先获得锁。
            ②. ReentrantLock提供了一个Condition（条件）类，用来实现分组唤醒需要唤醒的线程们，而不是像synchronized要么随机唤醒一个线程要么唤醒全部线程。
            ③. ReentrantLock提供了一种能够中断等待锁的线程的机制，通过lock.lockInterruptibly()来实现这个机制。
            ④. 可以判断锁的状态，知道有没有成功获取锁。
            ⑤. 锁等待时间。
            ⑥. 快速轮询。
        4. 性能高。可以提高多个线程进行读操作的效率。

```

Synchronized 底层实现：
```text
    synchronized映射成字节码指令就是增加来两个指令：monitorenter和monitorexit。
    当一条线程进行执行的遇到monitorenter指令的时候，它会去尝试获得锁，如果获得锁那么锁计数+1（为什么会加一呢，因为它是一个可重入锁，所以需要用这个锁计数判断锁的情况），如果没有获得锁，那么阻塞。
    当它遇到monitorexit的时候，锁计数器-1，当计数器为0，那么就释放锁。
    释放锁有两种机制：一种就是执行完释放；另外一种就是发送异常，虚拟机释放。
    
    底层实现的另一种解释：synchronized关键字需要一个引用类型的参数，这个参数也叫做监听器（monitor）；
    JVM通过这个监听器来管理所有需要同步的线程（synchronized这个监听器的所有线程）运行状态，
    成功占有该monitor的线程即成为该监听器的owner，其他线程则被状态切换至阻塞状态并维护在一个队列中准备下一次的竞争。
    
    尽可能用Synchronized， 原因是Synchronized优化了：
    1. 线程自旋和适应性自旋。
    2. 锁消除。把不必要的同步在编译阶段进行移除。
    3. 锁粗化。
    4. 轻量级锁。
    5. 偏向锁。
```
对象、监视器、同步队列和执行线程间的关系如下图：
![image](image/synchronized-1.png)

synchronized的使用场景： 
```text
①方法同步  public synchronized void method1
②代码块同步 synchronized(this){ //TODO }
③方法同步  public synchronized static void method3
④代码块同步 synchronized(Test.class){ //TODO}
⑤代码块同步 synchronized(o) {}


```

ReentrantLock实现的原理：
```text
    ReentrantLock的实现是一种自旋锁，通过循环调用CAS操作来实现加锁。
    它的性能比较好也是因为避免了使线程进入内核态的阻塞状态。

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
帮助Java并发编程。

1. CountDownLatch用法 
    
    CountDownLatch类位于java.util.concurrent包下，利用它可以实现类似计数器的功能。。
    
    构造器：
    ```text
       public CountDownLatch(int count) {  };  //参数count为计数值
    ```
    
    重要的方法：
    ```text
       public void await() throws InterruptedException { };   //调用await()方法的线程会被挂起，它会等待直到count值为0才继续执行
       public boolean await(long timeout, TimeUnit unit) throws InterruptedException { };  //和await()类似，只不过等待一定的时间后count值还没变为0的话就会继续执行
       public void countDown() { };  //将count值减1
    ```

2. CyclicBarrier用法
    
    字面意思回环栅栏，通过它可以实现让一组线程等待至某个状态之后再全部同时执行。
    叫做回环是因为当所有等待线程都被释放以后，CyclicBarrier可以被重用。
    我们暂且把这个状态就叫做barrier，当调用await()方法之后，线程就处于barrier了。
    
    CyclicBarrier类位于java.util.concurrent包下，CyclicBarrier提供2个构造器：
    ```text
       // 参数parties指让多少个线程或者任务等待至barrier状态；参数barrierAction为当这些线程都达到barrier状态时会执行的内容。
       public CyclicBarrier(int parties, Runnable barrierAction) {}    
       public CyclicBarrier(int parties) {}
    ```
    
    CyclicBarrier中最重要的方法就是await方法，它有2个重载版本：
    ```text
       // 常用，用来挂起当前线程，直至所有线程都到达barrier状态再同时执行后续任务；
       public int await() throws InterruptedException, BrokenBarrierException { };
       // 让这些线程等待至一定的时间，如果还有线程没有到达barrier状态就直接让到达barrier的线程执行后续任务。
       public int await(long timeout, TimeUnit unit)throws InterruptedException,BrokenBarrierException,TimeoutException { };
    ```

3. Semaphore用法
    
    Semaphore翻译成字面意思为 信号量，Semaphore可以控同时访问的线程个数，通过 acquire() 获取一个许可，如果没有就等待，而 release() 释放一个许可。
    
    Semaphore类位于java.util.concurrent包下，它提供了2个构造器：
    ```text
       public Semaphore(int permits) {          //参数permits表示许可数目，即同时可以允许多少线程进行访问
           sync = new NonfairSync(permits);
       }
       public Semaphore(int permits, boolean fair) {    //这个多了一个参数fair表示是否是公平的，即等待时间越久的越先获取许可
           sync = (fair)? new FairSync(permits) : new NonfairSync(permits);
       }
    ```
    
    Semaphore类中比较重要的几个方法，首先是acquire()、release()方法：
    ```text
       public void acquire() throws InterruptedException {  }     //获取一个许可，若无许可能够获得，则会一直等待，直到获得许可。
       public void acquire(int permits) throws InterruptedException { }    //获取permits个许可
       public void release() { }          //释放一个许可。 注意，在释放许可之前，必须先获获得许可。
       public void release(int permits) { }    //释放permits个许可
    ```
    
    这4个方法都会被阻塞，如果想立即得到执行结果，可以使用下面几个方法：
    
    ```text
       public boolean tryAcquire() { };    //尝试获取一个许可，若获取成功，则立即返回true，若获取失败，则立即返回false
       public boolean tryAcquire(long timeout, TimeUnit unit) throws InterruptedException { };  //尝试获取一个许可，若在指定的时间内获取成功，则立即返回true，否则则立即返回false
       public boolean tryAcquire(int permits) { }; //尝试获取permits个许可，若获取成功，则立即返回true，若获取失败，则立即返回false
       public boolean tryAcquire(int permits, long timeout, TimeUnit unit) throws InterruptedException { }; //尝试获取permits个许可，若在指定的时间内获取成功，则立即返回true，否则则立即返回false
    ```
    下面对上面说的三个辅助类进行一个总结：
    
    1）CountDownLatch和CyclicBarrier都能够实现线程之间的等待，只不过它们侧重点不同：
    
    CountDownLatch一般用于某个线程A等待若干个其他线程执行完任务之后，它才执行；
    
    而CyclicBarrier一般用于一组线程互相等待至某个状态，然后这一组线程再同时执行；
    
    另外，CountDownLatch是不能够重用的，而CyclicBarrier是可以重用的。
    
    2）Semaphore其实和锁有点类似，它一般用于控制对某组资源的访问权限。


#### 7. volatile关键字的作用和原理

一旦一个共享变量（类的成员变量、类的静态成员变量）被volatile修饰以后，就具备了两次语义：
- 保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。但不能保证原子性。
- 禁止进行指令重排序。

volatile的原理和实现机制。下面这段话摘自《深入理解Java虚拟机》：“观察加入volatile关键字和没有加入volatile关键字时所生成的汇编代码发现，加入volatile关键字时，会多出一个lock前缀指令”。

lock前缀指令实际上相当于一个内存屏障（也成内存栅栏），内存屏障会提供3个功能：
- 它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面；即在执行到内存屏障这句指令时，在它前面的操作已经全部完成；
- 它会强制将对缓存的修改操作立即写入主存；
- 如果是写操作，它会导致其他CPU中对应的缓存行无效。


#### 8. 乐观锁和悲观锁

悲观锁：假设一定会发生并发冲突，通过阻塞其他所有线程来保证数据的完整性。

乐观锁：假设不会发生并发冲突，直接不加锁去完成某项更新，如果冲突就返回失败。

乐观锁的一种实现方式-CAS(Compare and Swap 比较并交换)

#### 9. 对公平锁，非公平锁，可重入锁，自旋锁，读写锁的理解。
公平锁：加锁前先查看是否有排队等待的线程，有的话优先处理排在前面的线程，先来先得。

非公平所：线程加锁时直接尝试获取锁，获取不到就自动到队尾等待。

可重入锁： 当一个线程得到一个对象后，再次请求该对象锁时是可以再次得到该对象的锁的。
           具体概念就是：自己可以再次获取自己的内部锁。
           Java里面内置锁(synchronized)和Lock(ReentrantLock)都是可重入的。

自旋锁（spinlock）：是指当一个线程在获取锁的时候，如果锁已经被其它线程获取，那么该线程将循环等待，然后不断的判断锁是否能够被成功获取，直到获取到锁才会退出循环。

读写锁： 实际是一种特殊的自旋锁，它把对共享资源的访问者划分成读者和写者，读者只对共享资源进行读访问，写者则需要对共享资源进行写操作。这种锁相对于自旋锁而言，能提高并发性，因为在多处理器系统中，它允许同时有多个读者来访问共享资源，最大可能的读者数为实际的逻辑CPU数。写者是排他性的，一个读写锁同时只能有一个写者或多个读者（与CPU数相关），但不能同时既有读者又有写者。

#### 10. CAS是什么，底层原理
乐观锁的一种实现方式-CAS(Compare and Swap 比较并交换)

在计算机科学中，比较和交换（Conmpare And Swap）是用于实现多线程同步的原子指令。 它将内存位置的内容与给定值进行比较，只有在相同的情况下，将该内存位置的内容修改为新的给定值。 这是作为单个原子操作完成的。 原子性保证新值基于最新信息计算; 如果该值在同一时间被另一个线程更新，则写入将失败。 操作结果必须说明是否进行替换; 这可以通过一个简单的布尔响应（这个变体通常称为比较和设置），或通过返回从内存位置读取的值来完成（摘自维基本科）
 

#### 11. ArrayBlockingQueue ， LinkedBlockingQueue， SynchronousQueue等堵塞队列的理解。
- LinkedBlockingQueue
    
    LinkedBlockingQueue是使用比较多的队列，在SingleThreadPool(单个线程的线程池)、FixedThreadPool（固定线程数的线程池）使用的都是LinkedBlockingQueue。
    
    在LinkedBlockingQueue中有两个ReentrantLock，takeLock和putLock分别是在添加元素和取出元素的时候添加的锁。
    
    LinkedBlockingQueue是无界队列。
    
- ArrayBlockingQueue
    
    ArrayBlockingQueue是有界队列,在阿里的开源框架RocketMq中就使用到了，其添加和获取都是用的同一个ReentrantLock。
    
- SynchronousQueue

    SynchronousQueue是只有一个元素的队列
    
    - 添加和获取方法都用到了transfer方法。其实是根据是否有元素来区分是添加元素还是获取元素。
    ```text
    public boolean offer(E e) {
        if (e == null) throw new NullPointerException();
        return transferer.transfer(e, true, 0) != null;
    }
    
    public E take() throws InterruptedException {
        E e = transferer.transfer(null, false, 0);
        if (e != null)
            return e;
        Thread.interrupted();
        throw new InterruptedException();
    }
    ```
    - 有两种元素添加获取方式，非公平获取和公平获取
        
        非公平用的是TransferStack，是后进先出的方式
        
        公平方式用的是TransferQueue，是先进先出方式
    - 加锁是用的CAS的方式替换
        
        使用场景：在CachedThreadPool线程池中，使用到了SynchronousQueue。
- ConcurrentLinkedQueue
    
    ConcurrentLinkedQueue数据结构进行了非常巧妙的设计，在添加是从tail节点，获取是从head节点，而且做到了方并发，因为不用锁，所以效率更高。
    
    在netty的读取byteBuffer和获取Selector中都用了ConcurrentLinkedQueue。
- LinkedTransferQueue

    LinkedTransferQueue是jdk才出现的队列，是LinkedBlockingQueue、SynchronousQueue、ConcurrentLinkedQueue的超集，实现了他们三个的功能，其添加和获取都是用的xfer方法。


#### 12. ThreadPoolExecutor的传入参数及内部工作原理。
线程池执行器
ThreadPoolExecutor位于java.util.concurrent包，有4个带参数的构造方法。最终被调用的构造方法如下。其他构造方法只是提供了默认的ThreadFactory或者RejectedExecutionHandler作为参数。
```text
public ThreadPoolExecutor(int corePoolSize,  //  核心线程的数量 当有新任务来到，当前运行的线程数少于corePoolSize的时候，ThreadPoolExecutor二话不说就启动一个新的线程来执行这个任务。
                              int maximumPoolSize,  // 最大可运行的线程数量。当前运行的线程数量大于等于maximumPoolSize时，ThreadPoolExecutor将不会再创建新的线程。
                              long keepAliveTime,  // 核心线程池等待时长限制。
                              TimeUnit unit,   // keepAliveTime的时间单位
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler)
```

ThreadPoolExecutor的主要逻辑是，当用户调用execute(Runnable command) ，大致逻辑如下：
1.  查看当前运行状态，如果不是RUNNING状态，将直接拒绝新任务。否则进入步骤2。
2.  查看当前运行线程的数量，如果数量少于核心线程数，将直接创建新的线程执行该任务。否则进入步骤3。
3.  将该任务添加到阻塞队列，等待核心线程执行完上一个任务再来获取。如果添加到阻塞队列失败，进入步骤4。
4.  尝试创建一个非核心线程执行该任务，前提是线程的数量少于等于最大线程数。如果失败，拒绝该任务。

#### 13. 分布式环境下，怎么保证线程安全。
- 基于数据库实现的分布式锁
- 基于排他锁实现的分布式锁
- 基于缓存的分布式锁（Redis，memcached等）
- 使用Zookeeper实现分布式锁


## JVM相关问题

#### 1. JVM内存结构，运行机制

Java的内存结构： 
- 方法区：常量池、变量等存储地方；（持久区）
- 堆：实例对象存储地方；GC重点关照位置；（新生代和老年代）
- 程序计数器：记录程序下一步指令；
- Java方法栈：方法程序运行地方；Java栈总是与线程关联在一起的，每当创建一个线程，JVM就会为该线程创建对应的Java栈；
- 本地方法栈：java方法与本地相关联

JVM运行机制：JVM转入环境和配置（java.exe-->jvm.cfg）、装载JVM（通过LoadJavaVM来装载）、启动JVM获得本地调用接口、运行java程序；


#### 2. 介绍下垃圾回收机制，垃圾回收机制有哪些算法，各自的特点。

GC的对象是堆空间和永久区，防止内存泄漏。

垃圾回收算法
- **引用计数法**， 通过引用数量来回收垃圾。问题：引用和去引用 影响性能，很难处理循环引用。 没有被Java采用。
- **标记清除法**， 现在垃圾回收算法的思想基础。
- **标记压缩法**， 适用于存活对象较多的场合，如老年代。
- **复制算法**，相对高效，不适合存活对象较多的场合，如老年代。问题，空间浪费。

#### 3. 聊聊Minor GC、Major GC和Full GC之间的区别，垃圾收集器有哪些，他们的区别？
Minor GC： 从年轻代空间（包括 Eden 和 Survivor 区域）回收内存
Major GC： 清理老年代
Full GC： 清理整个堆空间—包括年轻代和老年代

垃圾收集器
- Serial收集器 ：串行收集器是最古老，最稳定以及效率高的收集器，可能会产生较长的停顿，只使用一个线程去回收。新生代、老年代使用串行回收；新生代复制算法、老年代标记-压缩；垃圾收集的过程中会Stop The World（服务暂停） 
- ParNew收集器 ： 其实就是Serial收集器的多线程版本。新生代并行，老年代串行；新生代复制算法、老年代标记-压缩。
- Parallel收集器 ： 类似ParNew收集器，Parallel收集器更关注系统的吞吐量。
- Parallel Old 收集器 ： Parallel Scavenge收集器的老年代版本，使用多线程和“标记－整理”算法。这个收集器是在JDK 1.6中才开始提供
- CMS收集器 ： 是一种以获取最短回收停顿时间为目标的收集器。目前很大一部分的Java应用都集中在互联网站或B/S系统的服务端上，这类应用尤其重视服务的响应速度，希望系统停顿时间最短，以给用户带来较好的体验。
- G1收集器 ： G1是目前技术发展的最前沿成果之一，HotSpot开发团队赋予它的使命是未来可以替换掉JDK1.5中发布的CMS收集器。与CMS收集器相比G1收集器有以下特点：1. 空间整合，2. 可预测停顿。

#### 4. OutOfMemoryError  怎么处理
导致OutOfMemoryError异常的常见原因有以下几种：
- 内存中加载的数据量过于庞大，如一次从数据库取出过多数据；
- 集合类中有对对象的引用，使用完后未清空，使得JVM不能回收；
- 代码中存在死循环或循环产生过多重复的对象实体；
- 使用的第三方软件中的BUG；
- 启动参数内存值设定的过小；

此错误常见的错误提示：
- tomcat:java.lang.OutOfMemoryError: PermGen space
- tomcat:java.lang.OutOfMemoryError: Java heap space
- weblogic:Root cause of ServletException java.lang.OutOfMemoryError
- resin:java.lang.OutOfMemoryError
- java:java.lang.OutOfMemoryError

解决java.lang.OutOfMemoryError的方法有如下几种：
- 增加jvm的内存大小。
- 优化程序，释放垃圾。

重点排查以下几点：
- 检查代码中是否有死循环或递归调用。
- 检查是否有大循环重复产生新对象实体。
- 检查对数据库查询中，是否有一次获得全部数据的查询。一般来说，如果一次取十万条记录到内存，就可能引起内存溢出。这个问题比较隐蔽，在上线前，数据库中数据较少，不容易出问题，上线后，数据库中数据多了，一次查询就有可能引起内存溢出。因此对于数据库查询尽量采用分页的方式查询。
- 检查List、MAP等集合对象是否有使用完后，未清除的问题。List、MAP等集合对象会始终存有对对象的引用，使得这些对象不能被GC回收。

#### 5. JVM调优有哪些参数

#### 6. 自己写一个类叫 java.lang.String 类加载过程，双亲委派模型。

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

#### 1. 分布式事务是怎么解决的？

#### 2. 分布式Session方案

#### 3. 设计一个秒杀场景

#### 4. 怎么防止表单多次提交

#### 5. Linux的基本操作命令

#### 6. ElasticSearch的使用和原理

#### 7. zookeeper的使用和原理
zookeeper是什么？
官方说辞：Zookeeper 分布式服务框架是Apache Hadoop 的一个子项目，它主要是用来解决分布式应用中经常遇到的一些数据管理问题，如：统一命名服务、状态同步服务、集群管理、分布式应用配置项的管理等。
 
 

#### 8. zookeeper 默认访问端口
```text
2181	/etc/zookeeper/conf/zoo.cfg中clientPort=<port>	对客户端提供服务的端口
2888	/etc/zookeeper/conf/zoo.cfg中server.x=[hostname]:nnnnn[:nnnnn]，标蓝部分	follower用来连接到leader，只在leader上监听该端口。
3888	/etc/zookeeper/conf/zoo.cfg中server.x=[hostname]:nnnnn[:nnnnn]，标蓝部分	用于leader选举的。只在electionAlg是1,2或3(默认)时需要。

```


#### 9. JDK源码，看源码
挺喜欢看源码的。看用法，了解底层。

#### 10. 个人博客
https://github.com/jiaxiaojiao

#### 11.开源项目
无

## HR面试