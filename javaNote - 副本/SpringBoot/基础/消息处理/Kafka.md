# 为什么要用kafka

1. 缓冲和削峰：上游数据时有突发流量，下游可能扛不住，或者下游没有足够多的机器来保证冗余，kafka在中间可以起到一个缓冲的作用，把消息暂存在kafka中，下游服务就可以按照自己的节奏进行慢慢处理。
2. 解耦和扩展性：项目开始的时候，并不能确定具体需求。消息队列可以作为一个接口层，解耦重要的业务流程。只需要遵守约定，针对数据编程即可获取扩展能力。
3. 冗余：可以采用一对多的方式，一个生产者发布消息，可以被多个订阅topic的服务消费到，供多个毫无关联的业务使用。
4. 健壮性：消息队列可以堆积请求，所以消费端业务即使短时间死掉，也不会影响主要业务的正常进行。
5. 异步通信：很多时候，用户不想也不需要立即处理消息。消息队列提供了异步处理机制，允许用户把一个消息放入队列，但并不立即处理它。想向队列中放入多少消息就放多少，然后在需要的时候再去处理它们。





# 什么是Kafka

高吞吐分布式消息系统







Kafka是一个分布式的、可分区的、可复制的消息系统，下面是Kafka的几个基本术语：

1. Kafka将消息以**topic**为单位进行归纳；
2. 将向Kafka topic发布消息的程序成为**producers**；
3. 将预订topics并消费消息的程序成为**consumer**；
4. Kafka以集群的方式运行，可以由一个或多个服务组成，每个服务叫做一个**broker**







1. 

# Kafka安装使用

> http://kafka.apache.org/downloads
>
> 下载地址   选择二进制文件下载（Binary downloads），然后解压即可。
>
> Kafka的配置文件位于config目录下，因为Kafka集成了Zookeeper（Kafka存储消息的地方），所以config目录下除了有Kafka的配置文件server.properties外，还可以看到一个Zookeeper配置文件zookeeper.properties：
>
> 打开server.properties，将`broker.id`的值修改为1，每个broker的id都必须设置为Integer类型，且不能重复。Zookeeper的配置保持默认即可。
>
> 接下来开始使用Kafka。

```java

//3.0以下版本
.\bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic test


//3.0以上版本
kafka-topics.bat --create --topic zhangphil_demo --bootstrap-server localhost:9092



//查看topic
    
 kafka-topics.bat --list --zookeeper localhost:2181

.\bin\windows\kafka-topics.bat --list --topic localhost:2181

```

### 启动Zookeeper

### 启动Kafka

### 创建Topic

### 生产消息和消费消息

**启动Producers**

**启动Consumers**

# Spring Boot整合Kafaka

```


```

```java
kafka名词简介：

Producer：消息生产者
Consumer：消息消费者
Consumer Group（CG）：消费者组，一个topic可以有多个CG，每个Partition只会把消息发送给GG中的一个Consumer
Broker：一台kafka服务器就是一个broker，一个broker有多个topic
Topic：消息主题，消息分类，可看作队列
Partition：分区，为了实现扩展，一个大的topic可能分布到多个broker上，一个topic可以分为多个partition，partition中的每条消息都会被分配一个有序的id（offset），每个partiton中的消息是有序的。
Offset：kafka的存储文件都是按照offset.kafka来命名的，方便查找，第一个offset为0000000000.kafka。
Leader：分区具有被备份，主分区
Follower：从分区
```

