# java-interview
java面试总结
1.kafka高性能架构原理分析  
https://juejin.im/post/5cc00ac95188250a59405322  
2.谈一谈Jetty 对Jetty的整体认识  
https://www.jianshu.com/p/c8282a4c4b04  
3.详解 Eureka 缓存机制  
https://www.infoq.cn/article/y_1BCrbLONU61s1gbGsU  
4.Eureka+Ribbon架构下如何实时下线服务通知调用方  
https://www.jianshu.com/p/07c2e0d59dc9 补充一点:eureka server跳过readWriteMap缓存 直接使用readOnlyMap，在服务下线时eureka server先更新注册表，然后下线节点通过spring cloud bus通知其余节点重新获取注册信息，同步刷新ribbon缓存和eureka client缓存  

