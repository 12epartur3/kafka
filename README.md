1、kafka 是一个数据队列，解决系统中的数据迁移问题，producer 生产数据发送到 kafka，consumer 从 kafka 中读取数据进行消费。
2、kafka 将一类数据称为 topic，一个 topic 的数据可以划分为到多个 partition 中，每一个 partition 都可以在不同的机器上，所以 consumer 可以并行消费某个 topic 的数据。
3、每一个 partition 中有一个offset，consumer可以从任意 offset 开始消费数据，kafka 中唯一一条数据可以用 topic、partition 和 offset 定位。
4、kafka 可以设置数据过期时间，比如一天，意味着consumer必须在24h内消费数据，否则数据将会丢失。
5、kafka 是集群化的（cluster），集群中每个节点称为 broker（物理概念，A single Kafka server is called a Kafka Broker），每个 kafka 集群中有多个partition，其中某一个 partition 是 leader，其余为 replica，consumer 和 producer 的读写通过leader来完成，leader再更新各个replica，如果leader挂掉，会选举出一个replica作为leader，一个 topic 的 leader partition 和其 replica 应该分布在不同的 broker 上（不同物理机器 https://www.conduktor.io/kafka/kafka-topic-replication/）。
6、生产操作：producer 生成数据，发送到作为 leader 的某一个 partition，这个partition再将数据分发到其他broker的同一个partition中。
7、消费操作：消费时需要指定一个 consumer group id 进行消费，当 consumer 数量 = partition时，一个consumer消费一个 partition，当consumer < partition时，一个consumer会接收到超过一个partition的数据，，当consumer > partition时，某几个consumer会idle。
8、By default, Kafka consumers will only consume data that was produced after it first connected to Kafka. Which means that to read historical data in Kafka, one must specify it as an input to the command, as we will see in the practice section.
9、kafka 客户端连接的 brokerlist 可以只是一部分：In practice, it is common for the Kafka client to reference at least two bootstrap servers in its connection URL, in the case one of them not being available, the other one should still respond to the connection request. That means that Kafka clients (and developers / DevOps) do not need to be aware of every single hostname of every single broker in a Kafka cluster, but only to be aware and reference two or three in the connection string for clients.
reference：https://www.conduktor.io/kafka/kafka-topics/
