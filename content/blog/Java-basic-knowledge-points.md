---
title: Java基础知识点归纳
date: 2021-04-02T20:56:09+08:00
tags: ["Java"]
---

### 面向对象

- 怎么理解什么是面向对象？
- 怎么理解Java中封装、继承、多态？
- 为什么需要接口？

- 接口和抽象类的区别？

- Object有哪些方法？9个

- Object中hashcode是干什么？

- Collection集合容器迭代器Itertor是什么？

- “==”和equals区别？

- hashcode和equals有什么联系

- 浅拷贝和深拷贝是什么 区别？

- Java反射？

- Java异常有哪些？

- Java的error有哪些？

***

### Java有哪些容器？（每个容器底层需要理解掌握 + 应用场景）

- HashMap底层是怎样的？ jdk1.7和1.8有什么区别

- HashMap怎么扩容的？

- HashMap在扩容的时候插入元素会怎样？

- 为什么HashMap在多线程下会出现线程安全问题-死循环？
- ConcurrentHashMap怎么实现线程安全的？ jdk1.7和1.8

- LinkedHashMap底层是怎样的，怎么实现有序插入？

***

### 进程和线程的区别？ps：协程是什么

- 进程的通信方式有哪些？哪个比较高效，为什么？

- 线程的通信方式有哪些？（线程通信指的是线程互斥同步）
- 线程有哪些状态？每个状态具体操作是？
- 如何创建线程？
- 继承Thread和实现Runnable区别
- 实现Runnable和Callable有什么区别
- 线程池有哪些？
- 线程池有哪些参数，每个代表什么？
- 线程池的拒绝策略有哪些？
- 线程池工作机制，每一步是怎样的？
- 有哪些阻塞队列？分别作用？
- CyclicBarrier、CountDownLatch、Semaphore 的用法
- start和run的区别？
- wait和sleep的区别？
- 如何解决多线程并发问题？加锁有哪些？
- synchronized 和 ReentrantLock的区别
- 什么是CAS、AQS 应用场景有哪里？
- CountDownLatch是什么？
- ThreadLocal是什么？（线程本地变量/存储）
- Java内存模型是怎样的？
- Volatile是解决什么问题？
- Volatile和Synchronized区别
- synchronized怎么优化加锁过程？锁升级、锁消除过程
- 什么是AQS？（AbstractQueuedSynchronizer）
- 什么是乐观锁、悲观锁？

***

### Java虚拟机

- 运行时数据区域<图> 各个区域分别的指责是什么？
- 垃圾回收？在哪些区域GC？
- 如何判断一个对象是否可回收？
- GCRoot包含哪些内容？
- 方法区的回收有哪些对象
- 垃圾回收算法有哪些？分别是怎样GC过程？ 分代收集算法指的是？
- 垃圾收集器有哪些？  重点CMS和G1收集器（concurrent mark sweep）
- 回收策略：什么时候会发生YoungGC / FullGC（场景）（考虑Eden、Survivor）
- 什么时候会发生FullGC？(Concurrent Mode Failure)（具体使用场景会是怎样会）
- 内存分配策略有哪些？对象分配优先在哪？如果是大对象尼？
- 是不是一收集完就开始GC？为什么
- 类加载机制是怎样？每个步骤分别指责是？
- 类加载器有哪些？（三种）
- 什么是双亲委派模型？作用是什么？(可以加上自定义类加载器extends ClassLoader 重写findClass方法)（ClassNotFoundException）
- 什么NIO

***

### Spring

- Spring和SpringBoot的区别
- Spring的IOC 和 AOP
- 依赖注入DI有哪些
- Spring的实现事务的方式有哪些
- Spring的事务传播级别有哪些
- Spring的事务隔离级别有哪些
- @Autowird怎么注入（类型 名称）
- Spring中有哪些设计模式？分别有哪些
- Spring的单例模式是严格的单例模式吗？为什么
- 有哪些方法可以实现单例模式？
- Spring的三级缓存了解吗

***

### 数据库

- 数据库的事务特性？（四个）
- 事务并发一致性问题有哪些？分别过程是怎样？（丢失修改）
- 怎么分别解决并发一致性问题？
- 事务隔离级别有哪些？
- 什么是MVCC？怎么解决并发一致性问题
- MVCC解决了哪两种隔离级别？
- undo日志用来干嘛？biglog日志？redo日志？
- 如何解决幻读问题？（在X隔离级别使用XX和XX）
- Net-Key Lock包含什么？
- 快照读和当前读的区别？
- 数据库三范式指的哪些？
- select where group by having
- inner join left join right join
- between order by 顺序
- SQL优化有哪些？（结构优化 + 查询优化）
- MySQL主从复制怎样实现？
- 为什么读写分离？
- 为什么需要分库分表？
- 怎样分库分表？
- 行级锁和表级锁？（基于索引）

***

### 索引

- MySQL索引底层怎么实现的？
- 为什么使用B+树而不是二叉搜索树或者红黑树？
- 聚簇索引和非聚簇索引是什么？
- 自适应哈希索引？
- 联合索引？最左匹配原则？
- 索引优化有哪些？（哪些情况下索引会失效？）
- 索引的优点有哪些？
- 索引使用条件，是不是越多越好？为什么
- 查询性能优化方式有哪些？
- innoDB和MyISAM有什么区别？
- MySQL数据类型有哪些（tinyint、smallInt、mediumInt、int、bigInt）[8,16,24,32,64位]
- int（11）数字代表什么？

***

### Redis的五大基本类型

- 键都是字符串，值是不同的对象类型
- Redis的过期键删除策略有哪些（三种）分别是怎样，优缺点
- Redis的持久化机制会有哪些（两种）分别是怎样的，优缺点
- 如果一边执行持久化机制，一边执行过期删除策略，redis会是怎样的操作行为？
- Redis的AOF是怎样实现？
- AOF重写是怎样的？
- AOF重写期间服务器进行命令处理，导致数据不一致怎么办？
- 为什么Redis支持高并发量，速度快？
- Redis单线程是指的是什么单线程?
- Redis分片有了解吗?
- Redis怎么实现主从复制？
- Redis的内存淘汰策略有哪些？
- Redis中会出现哪些问题？（雪崩、击穿、穿透）
- 如何保证redis和DB数据一致性问题？https://dler.cloud/subscribe/C7CwhGaDo3h1s1hH?mu=ssr

***

### RocketMQ

- RocketMQ怎么实现这么高的吞吐量的？
- RocketMQ怎么保证消息不丢失？
- RocketMQ怎么保证消息不重复消费？
- RocketMQ怎么保证消息的有序性？

***

### 补充

- Java的序列化有哪些？
- 关于spring的问题补充
- rocketMQ的问题补充
- mySQL的问题
- Redis的问题补充
- bitmap
- topK的问题
- 场景设计题目补充

***

### SQL和算法