### mongodb中 OBjectId 是由什么组成的？

时间戳+客户端机器ID+客户端进程ID

### mongodb的命名空间是什么？

集合名词+数据库名词

### 什么是mongodb的分片？

在多台计算机实例上存储数据记录的过程称为分片，目的是水平切分数据，提高查询性能。

### 什么是副本集？

副本集是一组有相同数据集的mongodb实例，在副本集中，一个节点是主节点，另一个是辅助节点，从主节点到辅助节点，所有的数据都会复制。

### mongodb 与mysql 的对比优劣势？

两者都属于数据存储工具。
1、相比mysql，mongodb更适合读作业比较中的业务，mongodb能充分利用机器的内存资源。如果机器内存资源丰富的话，mongodb的查询效率会快很多。
2、mysql的数据存储有严格的schema约束。而mongodb对数据格式不明确的或者数据格式经常变换的需求模型十分友好。
3、mongodb官方自带分布式文件系统，可以很方便的部署到服务器集群上。
4、mongodb自带map-reduce运算框架支持，方便数据统计。使用的是js。
5、mongodb事务关系支持薄弱。
6、mongodb稳定性欠缺。

### mongodb的sharing 过程

为什么要用sharing，副本集实现了数据的安全备份和无缝转移，但是不能实现数据的大容量存储。mongodb 的分片就是将数据分布式存储到不同的mongodb实例。
sharing 的3要素。

1. shard服务器，实际存储数据的分片，每个shard可以是一个mongodb实例，也可以是一组mongodb副本集。 官方建议为副本集。一个primay，提供读写，一个secondary 分担读。一个recoverying 负责primary宕机故障恢复，secondary与recoverying 都同步primary的数据，数据一样。
2. 配置服务（config server）必须是高可用，存储了所有shard的节点配置信息，每个chunk的shard key 范围，chunk在各个shard的分布情况以及集群中所有的db和collection的shard配置。Chunk包括了Shard Key取值在minKey和maxKey之间一组文档集合，但Chunk并不存放实际数据。整个MongoDB的chunk元信息存放在config数据库的chunks Colecttion中。由（collection，minkey，maxkey）描述一个chunk，一个chunks 的maxSize 是64m 如果一个文件超过这个范围了，会被切分到2个新的chunks，当一个shard数据太多时，chunks将会被迁移到其它的shards上。
3. 路由进程（route process）前端路由，客户端由此接入，首先去配置服务找到去那个shard上查询或者保存记录，然后连接相应的shard执行操作，最后返回结果

### mongodb实际运用中遇到的坑点

1. mongodb 内嵌文档属性名不要重名，更新数据容易混淆。
2. 不要大量删除数据，第一是cup消耗过高，造成假死，还会引发热冷数据反复交换。
3. mongodb 分页查询外加排序。查询后过滤的文档只剩下很少一部分，但是加上sort排序字段非常慢，执行计划显示扫描全表。 解决办法加上联合索引。