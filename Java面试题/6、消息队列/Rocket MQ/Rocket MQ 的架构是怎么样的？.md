
https://www.yuque.com/hollis666/xx5hr2/fkx1hga7xlpbfbuv

Rocket MQ 主要由 Producer、Broker、Consumer 三部分组成，如下图所示：
![[Pasted image 20240422135434.png]]
- Producer：消息生产者，负责将消息发送到 Broker

- Broker：消息中转服务器，负责存储和转发消息。Rocket MQ 支持多个Broker 构成集群，每个 Broker 都拥有独立的存储空间和消息队列

- Consumer：消息消费者，负责从 Broker 消费消息

- NameServer：名称服务，负责维护 Broker 的元数据信息，包括 Broker 地址、Topic 和 Queue 等信息。Producer 和 Consumer 在启动时需要连接到 NameServer 获取到 Broker 的地址信息

- Topic：消息主题，是消息的逻辑分类单位。Producer 将消息发送到特定的 Topic 中，Consumer 从指定的 Topic 中消费信息

- Message Queue：消息队列，是 Topic 的物理实现。一个 Topic 可以有多个 Queue，每个 Queue 都是独立的存储单元。Producer 发送的消息会被存储到对应的 Queue 上，Consumer 从指定的 Queue 中消费消息
