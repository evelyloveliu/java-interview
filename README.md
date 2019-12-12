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
5）最左匹配原则：联合索引建立的时候根据列顺序，从做到右建立索引  

![联合索引存储数据结构](https://i.loli.net/2019/04/26/5cc26d85b1a28.png)






