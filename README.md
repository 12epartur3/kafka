1、kafka是一个数据队列，解决系统中的数据迁移问题，producer生产数据发送到kafka，consumer从kafka中读取数据进行消费。
2、kafka将一类数据称为topic，一个topic的数据可以划分为到多个partition中，每一个partition都可以在不同的机器上，所以consumer可以并行消费某个topic的数据。
3、每一个partition中有一个offset，consumer可以从任意offset开始消费数据，kafka中唯一一条数据可以用topic、partition和offset定位。
4、kafka可以设置数据过期时间，比如一天，意味着consumer必须在24h内消费数据，否则数据将会丢失。
5、kafka是集群化的（cluster），集群中每个节点称为broker，每个broker中有多个partition（每个partiton应该是一个实例），其中某一个partition是leader，其余为replica，consumer和producer的读写通过leader来完成，leader再更新各个replica，如果leader挂掉，会选举出一个replica作为leader。
6、生产操作：producer生成数据，发送到作为leader的某一个partition，这个partition再将数据分发到其他broker的同一个partition中。
7、消费操作：消费时需要指定一个consumer group id 进行消费，当consumer数量 = partition时，一个consumer消费一个partition，当consumer < partition时，一个consumer会接收到超过一个partition的数据，，当consumer > partition时，某几个consumer会idle。
