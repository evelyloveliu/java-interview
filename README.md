# java-interview  
**1.kafka高性能架构原理分析**  
https://juejin.im/post/5cc00ac95188250a59405322  
**2.谈一谈Jetty 对Jetty的整体认识**  
https://www.jianshu.com/p/c8282a4c4b04  
**3.详解 Eureka 缓存机制**  
https://www.infoq.cn/article/y_1BCrbLONU61s1gbGsU  
**4.Eureka+Ribbon架构下如何实时下线服务通知调用方**  
https://www.jianshu.com/p/07c2e0d59dc9   
补充一点:eureka server跳过readWriteMap缓存 直接使用readOnlyMap，在服务下线时eureka server先更新注册表，然后下线节点通过spring cloud bus通知其余节点重新获取注册信息，同步刷新ribbon缓存和eureka client缓存  
**5.MongoDB VS MySQL**  
MongoDB特点：  
1）简化了开发，因为MongoDB文档自然映射到现代的面向对象编程语言。使用MongoDB可以避免将代码中的对象转换为关系表的复杂对象关系映射（ORM）层。  
2）MongoDB的灵活数据模型也意味着您的数据库模式可以随业务需求而发展。增删字段更加灵活    
3）MongoDB不支持复杂事务  
MySQL特点：  
1）在海量数据处理的时候效率会显著变慢。  
2）支持事务  
总结一下：  
MongoDB 的适用场景为：数据不是特别重要（例如通知，推送这些），数据表结构变化较为频繁，数据量特别大，数据的并发性特别高，数据结构比较特别（例如地图的位置坐标），这些情况下用 MongoDB ， 其他情况就还是用 MySQL ，这样组合使用就可以达到最大的效率  
**6.Redis多路IO复用**  
https://blog.csdn.net/u014590757/article/details/79860766  
**7.JAVA代理模式**  
1）静态代理：静态代理需要代理类与实现类实现相同的接口，需要手动编码  
2）JDK动态代理：  
DK动态代理基于拦截器和反射来实现。JDK代理是不需要第三方库支持的，只需要JDK环境就可以进行代理，使用条件：  
1）必须实现InvocationHandler接口；  
2）使用Proxy.newProxyInstance产生代理对象；  
3）被代理的对象必须要实现接口；  
JDK动态代理通过生成代理对象的接口，将InvocationHandler的实现替换被代理类的实现，实现补充或增强  
CGLIB动态代理：  
CGLIB(Code Generation Library)是一个开源项目！是一个强大的，高性能，高质量的Code生成类库，
它可以在运行期扩展Java类与实现Java接口。Hibernate用它来实现PO(Persistent Object 持久化对象)字节码的动态生成。
CCGLIB是一个强大的高性能的代码生成包。它广泛的被许多AOP的框架使用，例如Spring AOP为他们提供
方法的interception（拦截）。CGLIB包的底层是通过使用一个小而快的字节码处理框架ASM，来转换字节码并生成新的类。
CCGLIB是通过继承被代理类，将MethodInterceptor代理织入父类调用前后实现增强或者补充。所以CCGLIB不需要被代理类实现接口。  
**8.MySQL索引**  
1）MySQL索引的数据结构为B+树结构。  
2）InnoDB聚集索引的叶子节点存储行记录，因此， InnoDB必须要有，且只有一个聚集索引：如果表定义了PK，则PK就是聚集索引；如果表没有定义PK，则第一个not NULL unique列是聚集索引；否则，InnoDB会创建一个隐藏的row-id作为聚集索引；  
InnoDB普通索引的叶子节点存储主键值（即非聚集索引叶子节点只保持了索引值）。  
3）回表：当通过普通索引无法获取查询值时，需要根据普通索引返回的聚集索引，到聚集索引的B+树中去搜索，这个过程就是回表。  
4）索引覆盖：将被查询的字段，建立到联合索引里去。这样就保证了通过普通查询能够获取到查询所需的所有字段，避免回表。  
5）最左匹配原则：联合索引建立的时候根据列顺序，从做到右建立索引：比如建立（a,b,c）联合索引，在B+树中先根据a的键值进行排序，在a键值相同时，根据b键值进行排序，b键值相同时根据c键值进行排序。因此在整个联合索引的B+树中，通过a,ab,abc去遍历都是有序的，其余的组合遍历都是无序的。在查询时符合这个条件的才会通过索引进行查询，这就是最左匹配原则。  
**9.Synchronized实现原理以及JDK1.6的优化**  
1）Synchronized是通过JVM的monitor实现的。Synchronized在修饰方法时class文件通过ACC_SYNCHRONIZED 标识来判断该方法是不是同步方法，是否需要获取monitor锁，同步其他资源时，class文件通过monitorenter和monitorexit来标注同步代码块，告知JVM需要获取对应的monitor锁。  
2）monitor锁是存储在对象头中MarkWord的两位状态码，用来表示当前对象的锁状态，JDK1.6的优化其实就是针对这个状态变更的优化。JDK1.6之前只有重量级锁每次获取锁失败时，都要挂起线程，切换内核状态，所以性能较低。  
3）JDK1.6优化内容：自旋锁，适应性自旋锁，锁消除，锁粗化，偏向锁，轻量级锁，重量级锁。  
自旋锁：线程在未获取到锁的情况下，不是直接将线程挂起，而是在一定时间内重复尝试获取锁，如果重复获取锁花费的时间比线程挂起切换上下文的时间要短，则获取锁的时间会缩短，但是如果没有获取到也会导致线程分配的CPU时间被浪费。  
适应性自旋锁：线程如果自旋成功了，那么下次自旋的次数会更加多，因为虚拟机认为既然上次成功了，那么此次自旋也很有可能会再次成功，那么它就会允许自旋等待持续的次数更多。反之，如果对于某个锁，很少有自旋能够成功，那么在以后要或者这个锁的时候自旋的次数会减少甚至省略掉自旋过程，以免浪费处理器资源。  
锁消除：JVM根据逃逸分析判断资源是不是安全的，从而在运行相关代码的时候判断是否需要锁定   
锁粗化：将多个连续的加锁、解锁操作连接在一起，扩展成一个范围更大的锁，锁的粒度太小也会产生没有必要的性能消耗  
偏向锁：偏向锁是在单线程执行代码块时使用的机制，如果在多线程并发的环境下（即线程A尚未执行完同步代码块，线程B发起了申请锁的申请），则一定会转化为轻量级锁或者重量级锁。引入偏向锁主要目的是：为了在没有多线程竞争的情况下尽量减少不必要的轻量级锁执行路径。因为轻量级锁的加锁解锁操作是需要依赖多次CAS原子指令的，而偏向锁只需要在置换ThreadID的时候依赖一次CAS原子指令（由于一旦出现多线程竞争的情况就必须撤销偏向锁，所以偏向锁的撤销操作的性能损耗也必须小于节省下来的CAS原子指令的性能消耗）。  
当一个线程访问同步块并获取锁时，会在对象头和栈帧中的锁记录里存储锁偏向的线程ID，以后该线程进入和退出同步块时不需要花费CAS操作来争夺锁资源，只需要检查是否为偏向锁、锁标识为以及ThreadID即可，偏向锁的本质是降低CAS操作频次，因为频繁的进行CAS操作可能导致CPU总线风暴。  
轻量级锁：轻量级锁将对象头的内容复制到当前线程栈帧中，然后将对象头的内容改为指向栈帧的指针，释放锁时将内容在替换回来，所以轻量级锁要进行两次CAS操作，而偏向锁只需要一次（在竞争锁的时候CAS变更锁状态，同时将拥有锁的线程号设置为自己）  
重量级锁：监视器锁本质又是依赖于底层的操作系统的Mutex Lock来实现的。而操作系统实现线程之间的切换这就需要从用户态转换到核心态，这个成本非常高，状态之间的转换需要相对比较长的时间，这就是为什么Synchronized效率低的原因。JDK1.6的优化就是尽量避免Synchronized运行时锁升级到重量级锁  
一篇关于Synchronized原理的文章：https://juejin.im/post/5b4eec7df265da0fa00a118f#heading-14  

**10.对AQS以及线程池的理解**  
1）AQS的主要结构  
AQS包含了一个FIFO的双向队列Node，状态等待队列ConditionObject（每个ConditionObject代表一个状态条件，相同等待条件的线程将进入同一个ConditionObejct队列等待被唤醒。ConditionObjet中的await,notify，notifyAll与Synchronized中的await,notify,notifyAll相同点是都需要先获取到同步资源后才能调用使用相关API，区别在于Synchronized一个资源相当于一个条件，但是AQS的每个锁都可以拥有多个ConditionObject即一个锁可以设置多个条件），状态同步变量state（在ReentrantLock中state表示同步状态和重入次数，在ReenntrantWriteLock中高16位标识读状态,低16位标识写状态，在CoundownLatch和CyclicBarrier中state表示的是计数器当前的值，在Semaphore中表示当前信号量的个数）;  
2）AQS设计思想  
AQS使用模板模式，定义了一套同步状态资源的操作模板，对继承者来说值需要实现tryAcquire，tryRelease，tryAcquireShared，tryReleaseShared，isHeldExclusively以及定义state的含义即可实现自己的锁。  

AQS主要实现逻辑在acquire，release，acquireShared，releaseShared中  
```java
//独占锁获取锁实现
 public final void acquire(int arg) {  
  //尝试获取锁（不同锁的实现tryAcquire不一样，这取决于继承者想实现什么样的锁）  
        if (!tryAcquire(arg) &&  
            //addWatiter将当前线程加入Node队列的队尾，然后accquireQueued中如果前置节点为头节点会先再次尝试通过tryAcquire获取锁，获取失败后，挂起线程，返回中断标识
            acquireQueued(addWaiter(Node.EXCLUSIVE), arg))  
            selfInterrupt();  
    }
```
```java
//独占锁释放锁实现
public final boolean release(int arg) {
//尝试释放锁（不同锁的实现tryRelease不一样，这取决于继承者想实现什么样的锁）  
        if (tryRelease(arg)) {
            Node h = head;
            if (h != null && h.waitStatus != 0)
                //取出头结点然后唤醒头结点的下个节点（头结点为哨兵节点）
                unparkSuccessor(h);
            return true;
        }
        return false;
    }
```

