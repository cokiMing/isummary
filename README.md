## Common Interview Q&A

**消息队列**

1. MQ的主要有哪些作用(包括使用场景)

2. MQ的技术选型

3. MQ如何保证高可用性

4. MQ的重复消费该如何解决

5. MQ的消息丢失该如何解决

6. MQ的消息顺序性如何保证

7. MQ的架构设计

   

**分布式缓存**

1. redis和memcache的区别
2. redis的线程模型
3. redis支持哪些数据结构
4. redis的过期策略
5. 如何保证redis的高可用
6. redis的主从复制
7. redis的哨兵机制
8. redis的持久化方式
9. LRU算法
10. 数据库与缓存双写一致性
  * Cache Aside Pattern，读数据的时候，先从缓存中读取，缓存中不存在，则再从数据库中读取，并插入到缓存中。更新缓存时，先删除缓存，再更新数据库。如果先更新数据库的话，若之后删除缓存失败，会导致后面的请求读到的数据仍然是旧的。为什么不是更新缓存而是删除缓存，因为如果更新缓存的话，假如写操作的频率非常高，那么更新缓存的频率也非常高，而在此期间读操作可能只有一次，改成删除操作的话，那么只有在读这一次的时候才会更新缓存，大大降低了缓存的更新频率。在高并发环境下，当写操作未完成数据库更新时，由于读操作的线程未命中缓存，读取了数据库中的旧值，并更新到了缓存中，导致数据过期。这种情况下，需要在内存中维护一个更新队列，将写操作和读操作全部放入，并一一执行，即可保证一致性，优化点：对同一对象的多个读操作的更新请求可以合并。缺点：写操作数量庞大的时候可能导致读操作长时间阻塞。



**API网关的设计**

1. 网关的使用场景
2. 网关设计需要考虑的因素



**数据库相关**

1. 数据库引擎MyIsam和InnoDB的区别

2. 分库分表

3. 慢查询优化

4. mvcc

5. 数据库的三大范式

6. 数据库异构

7. 字段冗余设计方案




**JVM相关**

1. 类加载机制

2. 几种常见的垃圾回收器G1，CMS等

3. 垃圾回收算法：标记清除，复制算法，标记整理算法

4. JVM的内存模型

   线程共享的有：堆内存，方法区(元空间)

   线程私有：虚拟机栈，本地方法栈，程序计数器

5. 双亲委派模型

6. 当执行 ``` Test test = new Test() ``` 的时候，发生了什么？

   * 首先jvm会去查找Test类是否已经加载，如果没有则加载Test类
   * 在确认类已经加载完成后，jvm对该对象进行内存分配，主要有两种方式：
     * 指针碰撞：适用于内存比较规整的情况，jvm维护了一个用于指示已使用内存和空闲内存的指针，此时会将空闲一侧的内存分配出来。
     * 空闲列表：jvm维护了一个内存碎片的列表，此时会将大小合适的内存碎片分配出来；
     * 对于对象并发创建时，有两种方案，CAS和TLAB，TLAB是在Eden预留了一部分内存空间，当预留空间不足时，再进行CAS分配内存。
   * 在完成内存分配后，jvm对test对象设置零值
   * 在设置零值完成后，jvm开始设置对象头：包括锁信息，hash值，类信息
   * 在对象头设置完成后，执行init方法（由开发人员编写的逻辑）。





**数据结构与算法相关**

1. 快速排序算法的实现

   首先，选定

2. 红黑树，二叉树相关的原理

3. 跳表的相关原理和实现

4. B树和B+树

5. Mysql的存储结构为何选择B+树而不是B树或者二叉树？

   二叉树的查询效率很高，时间复杂度为O(log(n))，但是由于其节点数只有2，当数据量非常庞大的时候，树的深度会非常深，一旦查询深度过深，需要多次重新定位文件，导致查询效率低下。B树解决了树深度过深的问题，而且它的数据直接存储在每个节点中，但是需要遍历数据时必须进行中序遍历，导致效率低下。B+树的非叶子节点只存储索引，所有数据都存储在叶子节点中，且每个叶子节点都有指向下一个叶子节点的指针，当需要对数据进行遍历时，只要遍历叶子节点即可。



**java基础**

1. HashMap的原理
2. JUC相关容器
3. List，Set
4. 线程池的原理和执行流程
5. synchronize和ReentrantLock，死锁产生的场景
6. jdk1.8中的新api（Optional，Stream，lamda表达式等等）
7. AQS原理
8. IO，NIO，BIO相关
9. volatile关键字的底层实现
10. ThreadLocal类的使用与原理
11. HashMap在并发环境下可能会出现什么问题
12. aba问题



**框架相关**

1. Spring的生命周期
2. Spring的核心，IOC和AOP
3. Spring用到的设计模式
4. Springboot和Spring的对比
5. Springboot的内置web容器
6. Springboot的思想：约定大于配置
7. SpringCloud的注册中心
8. SpringCloud的配置中心
9. SpringCloud的网关
10. SpringCloud的熔断器
11. Dubbo相关知识



**分布式相关**

1. CAP理论以及相关实践
2. BASE理论
3. 分布式事务XA，2PC，3PC，TCC，本地消息表
   * XA：单机对处于同一事务的多种资源处理时使用，在执行业务逻辑前打开所有资源的事务，成功后统一commit，失败则全部rollback。
   * 2PC：分为prepare和commit两个阶段，prepare阶段协调者会询问参与者是否可以提交，参与者认为可以则将undo和redo信息写到事务日志中。commit阶段一旦任意参与者失败则协调者调用undo进行rollback，缺点：所有回滚逻辑均需要手动实现，非常麻烦，且存在参与者阻塞协调者的问题。
   * 3PC：分为canCommit，preCommit，doCommit三个阶段，参与者在preCommit阶段将undo和redo信息写入到事务日志中。在doCommit阶段出现异常则协调者调用undo进行rollback。缺点：所有回滚逻辑均需要手动实现，非常麻烦。协调者如果宕机，在一定时间内参与者会自动提交事务
   * 本地消息表：在系统A持久化业务数据的同时插入一个本地消息表并置为待消费状态，同时通过mq发送消息给系统B，消息B消费完成后回调系统A的通知接口(或可以使用zookeeper做监听)，将消息置为已消费状态。系统A需要维护一个轮询待消费状态消息的线程，将超时待消费消息重新发给B进行消费，如果多次后仍然失败，需要进行回滚。系统B必须保证消息消费的幂等性。
4. 使用redis实现的分布式锁



**运维相关**

1. docker的概念和使用
2. Linux的常用指令



**高并发相关**

1. 如何设计一个高并发的系统？
   * 拆分系统，将系统根据业务拆分成多个模块，对压力较大的模块做集群；
   * 使用分布式缓存，缓解数据库压力
   * 使用MQ进行削峰
   * 动静资源分离
   * 数据库读写分离
   * 使用分布式搜索引擎



**网络相关**

1. http的三次握手和四次挥手
2. 当我们在浏览器访问一个网址的时候，发生了什么



**其他**

1. 画一下你们的系统架构图
2. 说一说你们系统有哪些模块，模块之间是怎么通信的