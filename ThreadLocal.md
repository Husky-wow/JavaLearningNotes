# 介绍

ThreadLocal类用来提供线程内部的局部变量。这种变量在多线程环境下访问时能保证各个线程变量相对独立于其他线程内的变量。

因此可知ThreadLocal的作用是：

1. 提供线程内的局部变量，不同线程之间不会互相干扰；
2. 这种变量在线程的生命周期内起作用，减少同一个线程内多个函数或组件之间一些公共变量传递的复杂度。

总结：

1. 线程并发：多线程并发；
2. 传递数据：在同一线程中不同组件中传递公共变量（我现在理解为在同一线程中开启的不同子线程）；
3. 线程隔离：每个线程的变量都是独立的，不会相互影响；

# 基本使用

public void set(T value) 设置**当前线程**绑定的**局部变量**

public T get() 获取**当前线程**绑定的**局部变量** 

# ThreadLocal与synchronized

# 	ThreadLocal的内部结构

## JDK1.8之前的方案

## JDK1.8的方法

1. 每个Thread线程内部都有一个Map(ThreadLocalMap);
2. Map里面存储ThreadLocal对象(key)和线程变量副本(value);
3. Thread内部的Map是由ThreadLocal维护的，由ThreadLcoal负责向map获取和设置线程的变量值；
4. 对于不同的线程，每次获取副本值时，别的线程并不能获取到当前线程的副本值，形成了副本的隔离，互不干扰；

这样设计的优点：

1. 每个map存储的entry数量变少-尽量避免哈希冲突；
2. 当Thread销毁的时候ThreadLocal也会销毁，减少内存的使用；

