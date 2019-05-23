# 关键字 synchronized 同步

在Java 中，每一个对象都有且仅有一个同步锁，即同步锁依赖对象而存在。当我们调用某对象的synchronized方法时，就获取了该对象的同步锁。不同线程对同步锁的访问是互斥的。synchronized 有以下三条原则：
- 当一个线程访问“某对象”的“synchronized方法”或者“synchronized代码块”时，其他线程对“该对象”的该“synchronized方法”或者“synchronized代码块”的访问将被阻塞。
- 当一个线程访问“某对象”的“synchronized方法”或者“synchronized代码块”时，其他线程仍然可以访问“该对象”的非同步代码块。
- 当一个线程访问“某对象”的“synchronized方法”或者“synchronized代码块”时，其他线程对“该对象”的其他的“synchronized方法”或者“synchronized代码块”的访问将被阻塞。

关于实例锁和全局锁
- 实例锁：锁在某一个实例对象上，对应synchronized 关键字
- 全局锁：锁针对的是类，无论实例多少个对象，线程将共享该锁，对应synchronized static 关键字