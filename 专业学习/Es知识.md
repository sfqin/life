> Java语言开发 基于Lucene 的搜索服务器，提供了分布式多用户 能力的全文搜索引擎。

### es 的倒排索引是什么？

传统的文章检索是，通过关键词挨个匹配数据记录，找到符合条件的结果。倒排索引的思想是，通过分词策略，形成了词和记录的映射关系表，这种词典+映射表即为倒排索引。这样做的好处是，能实现关键词查询时间复杂度为O(1)，缺点是要事先维护足够丰富的词库，以及较好的语言分词器。 倒排索引的底层实现是基于 FST 的数据结构，一种类 Trie (字典树) 的数据结构，其优点是空间占用小，通过对词典点钟单词前缀和后缀的重复利用，压缩存储空间；另外一个就是查询速度快，查询的时间复杂度是O(len(str))

### es 索引文档的过程

1. client 往集群中写数据，发送请求
2. 结点1接收到请求后，使用文档_id 来确定文档属于分片0。请求会被转到另外的结点3
3. 节点3在主分片上执行写操作，如果成功，则将请求并行转发到节点1与节点2的副本分片上，等待返回结果。所有副本分片都返回成功，节点3将协调节点1报告成功，节点1向client端返回成功。 第二步分片的过程是借助路由算法实现的，根据路由和文档id计算目标的分片id。

### es 搜索记录的过程

分为 query 和 fetch 两个阶段，query阶段定位到位置，但是不取。 1）架设一个索引数据有5主+1副本 10个分片，一次请求会命中一个。2）每个分片在本地进行查询，结果返回到本地有序优先队列。3）2中的结果发送到路由结点，产生一个全局的排序列表。 fetch 阶段的目的是取数据，路由节点获取到所有的文档，返回给客户端。

### 调优

- 写入调优 采取批量写入，尽量使用自动生成的id
- 查询调优 禁用批量terms，充分利用倒排索引，能用keyword尽量用keyword类型， 数据量大可以先基于事件敲定索引再检索，设置合理的路由机制

### master选举过程 

 es Node类型 4种
 node.master true/false true其表示这个node是一个master的候选节点，可以参与选举。正常运行时候只有一个master，多余1个时会发生脑裂。
 node.data true/false  true表示这个node是一个数据结点，会存储分配在该node的shard的数据并负责这些shard的写入和查询。
 此外任何一个集群内的node都可以执行任何请求，其会负责将请求转发给对应的node进行处理，所以当node.master和node.data 都为false时候，这个node可以作为一个proxy node 专门接受请求转发，结果聚合等