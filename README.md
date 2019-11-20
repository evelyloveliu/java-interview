# java-interview
java面试总结
1.kafka高性能架构原理分析  
https://juejin.im/post/5cc00ac95188250a59405322  
2.谈一谈Jetty 对Jetty的整体认识  
https://www.jianshu.com/p/c8282a4c4b04  
3.详解 Eureka 缓存机制  
https://www.infoq.cn/article/y_1BCrbLONU61s1gbGsU  
4.Eureka+Ribbon架构下如何实时下线服务通知调用方  
https://www.jianshu.com/p/07c2e0d59dc9   
补充一点:eureka server跳过readWriteMap缓存 直接使用readOnlyMap，在服务下线时eureka server先更新注册表，然后下线节点通过spring cloud bus通知其余节点重新获取注册信息，同步刷新ribbon缓存和eureka client缓存  
5.MongoDB VS MySQL  
MongoDB特点：  
1）简化了开发，因为MongoDB文档自然映射到现代的面向对象编程语言。使用MongoDB可以避免将代码中的对象转换为关系表的复杂对象关系映射（ORM）层。  
2）MongoDB的灵活数据模型也意味着您的数据库模式可以随业务需求而发展。增删字段更加灵活    
3）MongoDB不支持复杂事务  
MySQL特点：  
1）在海量数据处理的时候效率会显著变慢。  
2）支持事务  
总结一下：  
MongoDB 的适用场景为：数据不是特别重要（例如通知，推送这些），数据表结构变化较为频繁，数据量特别大，数据的并发性特别高，数据结构比较特别（例如地图的位置坐标），这些情况下用 MongoDB ， 其他情况就还是用 MySQL ，这样组合使用就可以达到最大的效率  



