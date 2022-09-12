# Redis 集群常见面试题总结

相关面试题 ：

- 如何保证 Redis 服务高可用？
- 主从复制方案是怎么做的？
- Sentinel（哨兵）  有什么作用？
- Redis 为什么要将 Sentinel 设计成分布式架构？
- Sentinel 选举 Leader 是如何达成共识的？
- Raft 算法有什么作用？
- Redis 缓存的数据量太大怎么办?
- Redis Cluster 虚拟槽分区有什么优点？
- Gossip 协议有什么作用？
- ......

单机 Redis 存在单点风险问题，如果 Redis 宕机的话，就会导致大量的请求直接打到数据库，严重的情况下，数据库很可能会直接被打死。

如何保证 Redis 服务的高可用呢？ 我们可以基于主从复制搭建一个 Redis 集群，master（主节点）主要负责处理写请求，slave（从节点）主要负责处理读请求，master 宕机的话，从 slave 中选出一台作为 master 即可！

Redis Sentinel（哨兵）就是一个基于主从复制的 Redis 集群解决方案。不过，Redis Sentinel 这种方案主要是提高了 Redis 服务的可用性和读吞吐量，并不能缓解写压力以及解决缓存数据量过大的问题。

如果我们的缓存的写请求比较多且需要缓存的数据比较大的话，Redis Sentinel 是没办法做到的，这个时候，我们就需要用到 Redis 分片集群了。Redis Cluster 是 Redis 官方推出的分片集群解决方案 。

## 主从复制 

在主从复制这种方案下，我们通过 [Redis replication](https://redis.io/docs/manual/replication/)（Redis 默认使用异步复制）来搭建一个一主(master)多从(slave)的 Redis 服务架构来提高可用性和读吞吐量。master 主要负责处理**写**请求，slave 主要负责处理**读**请求，master 宕机的话，**从 slave 中选出一台作为 master 即可**。



![img](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fguide-blog-images.oss-cn-shenzhen.aliyuncs.com%2Fgithub%2Fjavaguide%2Fredisredis-master-slave.png&sign=3412224267d83577188d9359a96978df580c860757146bf1a49252b7ffbf4a5a)





具体要配置多少 slave 节点呢？ 这个主要取决于读吞吐量，因为 slave 节点分担的是读请求，写请求由 master 节点负责。

这样的方式有什么痛点呢？

 一旦 master 宕机，slave 晋升成 master，同时需要**修改应用方的主节点地址**，还需要**命令所有从节点去复制新的主节点**，整个过程需要人工干预。人工干预大大增加了问题的处理时间以及出错的可能性，如果能够**自动化**地完成故障切换就好了。

接下来介绍的 Redis Sentinel（哨兵）就可以帮助我们来解决这个痛点。

简单总结一下主从复制这种方案的优缺点：

- 优点 ：master 发生宕机的话可以手动将某一台 slave 升级为 master，Redis 服务可用性提高。slave 可以分担读请求，读吞吐量大幅提高。

- 缺点 ：手动将 slave 升级为 master 的过程费时且容易出错，，不支持横向扩展无法缓解写压力和存储压力。

## 哨兵模式Redis Sentinel 

Sentinel（哨兵） 是 Redis 的一种运行模式 ，它主要的作用就是对 Redis 运行节点进行监控。当 master 节点出现故障的时候， Sentinel 会帮助我们实现故障转移，自动将某一台 slave 升级为 master，确保整个 Redis 系统的可用性。整个过程完全自动，不需要人工介入。

Sentinel 模式基于主从复制，只是多了一个 Sentinel 角色来帮助我们监控 Redis 节点的运行状态并自动实现故障转移。

![img](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fguide-blog-images.oss-cn-shenzhen.aliyuncs.com%2Fgithub%2Fjavaguide%2Fredisredis-master-slave-sentinel.png&sign=f6f7293bd46791f3b7ff7cc0ea11acb02c7551de2a64860e3949f59a838170c9)





根据 [Redis Sentinel 官方文档](https://redis.io/topics/sentinel)，Sentinel 节点主要有下面这些作用：

- 监控：监控所有 Redis 节点的状态是否正常。

- 通知 ：可以通过 API 通知系统管理员或其他计算机程序，其中一个受监控的 Redis 实例出现问题。

- 故障转移：如果一个 master 出现故障，Sentinel 会帮助我们实现故障转移，自动将某一台 slave 升级为 master，确保整个 Redis 系统的可用性。

Redis Sentinel 本身设计的就是一个分布式系统，建议多个 Sentinel 节点协作运行。这样做的好处是：

- 多个 Sentinel 节点通过投票的方式来确定 Sentinel 节点是否真的不可用，避免误判（比如网络问题可能会导致误判）。

- Sentinel 自身就是高可用。

Sentinel 也是一个 Redis 进程，只是不对外提供读写服务，通常建议哨兵配置成单数（大于等于 3 台）。

在这些 Sentinel 中会有一个 Leader 的角色来负责故障转移。

- 如何选择出 Leader 角色呢？ 

​	这就需要用到分布式领域的 共识算法 了。简单来说，共识算法就是让分布式系统中的节点就一个问题达成共识。在 Sentinel 选举 Leader 这个场景下，这些 Sentinel 要达成的共识就是谁才是 Leader 。



大部分共识算法都是基于 Paxos 算法改进而来，在 Sentinel 选举 Leader 这个场景下使用的是 [Raft 算法](https://javaguide.cn/distributed-system/theorem&algorithm&protocol/raft-algorithm.html)。这是一个比 Paxos 算法更易理解和实现的共识算法—Raft 算法。更具体点来说，Raft 是 Multi-Paxos 的一个变种，其简化了 Multi-Paxos 的思想，变得更容易被理解以及工程实现。

简单总结一下主从复制这种方案的优缺点：

- 优点 ：支持故障自动切换。

- 缺点 ：部署相对麻烦且需要耗费更多的服务器资源，不支持横向扩展无法缓解写压力和存储压力。

## 切片模式Redis Cluster 

主从复制和 Redis Sentinel 方案本质都是通过增加副本的方式提高了 Redis 服务的可用性和读吞吐量。这两种方案都不支持横向扩展来缓解写压力以及解决缓存数据量过大的问题。

如果写压力太大或者缓存数据量太大的话，我们可以提高服务器硬件配置或者采用 Redis 切片集群。

- 什么是 Redis 切片集群？ 

简单来说，就是部署多台 Redis 实例，这些 Redis 实例之间平等，并没有主从之说，它们同时对外提供读/写服务，缓存的数据库相对均匀地分布在这些 Redis 实例上。Redis 切片集群对于横向扩展非常友好，只需要增加 Redis 实例到集群中即可。



Redis 切片集群应该怎么做呢？ 在 Redis 3.0 之前，我们通常使用的是 Twemproxy、Codis 等分片集群方案。到了 Redis 3.0 的时候，Redis 官方推出了分片集群解决方案 - [Redis Cluster](https://redis.io/topics/cluster-tutorial) 。



![img](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F85fbed524d8342adb054961525c6e257.png&sign=8015d7a8eaf38573446a22c336d94971969809fc53b94faeb3fc2f28f901ba4e)





Redis Cluster 通过分片（sharding）来进行数据管理，提供复制和故障转移等功能，可以非常方便地帮助我们解决 Redis 大数据量缓存的问题。并且，这种方案可以很方便地进行横向拓展（增加 Redis 机器）。



Redis Cluster 并没有使用一致性哈希，采用的是 虚拟槽分区 ，每一个键值对都属于一个 hash slot（ 虚拟槽） 。



Redis Cluster 通常有 16384 hash slots ，要计算给定 key 的 hash slot ，我们只需要通过对每个 key 计算 CRC16 值，然后对 16384(hash slot 的数量) 取模。







假设集群有 3 个 Redis 节点组成，每个节点负责整个集群的一部分数据：



●Node 1 ： 0 - 5500 的 hash slot.

●Node 2 ： 5501 - 11000 的 hash slot. .

●Node 3 ： 11001 - 16383 的 hash slot..



Redis Cluster 是去中心化的，任何一台机器宕机，另外两个节点，不影响的。因为 key 找的是 hash slot，不是机器。并且，我们想要添加新的节点比如 Node4 进入 Redis Cluster 也非常方便，只需要将一些 hash slot 从 节点 Node1，Node2，Node3 移动到 Node4 即可。



从上面的介绍中，我们可以简单总结出 Redis Cluster 虚拟槽分区机制的有点：解耦了数据和节点之间的关系，提升了集群的横向扩展性和容错性。



Redis Cluster 是一个典型的分布式系统，分布式系统中的各个节点需要互相通信。既然要相互通信就要遵循一致的通信协议，Redis Cluster 中的各个节点基于 Gossip 协议 来实现数据的最终一致性（每个 Redis 节点都维护了一份集群的状态信息）。



Gossip 协议是分布式系统中非常重要的一个协议，我的建议是学有余力的小伙伴尽量要搞懂。不过，这个在面试中问的也不多，除非是你在简历上写了自己熟悉 Gossip 协议。关于 Gossip 协议的详细解读，推荐你看看下面这两篇文章：



●[一万字详解 Redis Cluster Gossip 协议 - 程序员历小冰](https://segmentfault.com/a/1190000038373546) ：深度好文！

●[Life in a Redis Cluster: Meet and Gossip with your neighbors](https://cristian.regolo.cc/2015/09/05/life-in-a-redis-cluster.html) ：动图讲解。



Redis Cluster 各个节点通信主要交换的信息包括：



1myslots ：当前节点所负责的 slots 信息

2flags : 当前节点当前的状态

3......



Redis Cluster 交换所用的消息体结构 clusterMsg 源码如下 ，地址：https://github.com/redis/redis/blob/d96f47cf06/src/cluster.h 。







C

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

typedef struct {

​    char sig[4];        /* Signature "RCmb" (Redis Cluster message bus). */

​    uint32_t totlen;    /* Total length of this message */

​    uint16_t ver;       /* Protocol version, currently set to 1. */

​    uint16_t port;      /* TCP base port number. */

​    uint16_t type;      /* Message type */

​    uint16_t count;     /* Only used for some kind of messages. */

​    uint64_t currentEpoch;  /* The epoch accordingly to the sending node. */

​    uint64_t configEpoch;   /* The config epoch if it's a master, or the last

​                               epoch advertised by its master if it is a

​                               slave. */

​    uint64_t offset;    /* Master replication offset if node is a master or

​                           processed replication offset if node is a slave. */

​    char sender[CLUSTER_NAMELEN]; /* Name of the sender node */

​    unsigned char myslots[CLUSTER_SLOTS/8];

​    char slaveof[CLUSTER_NAMELEN];

​    char myip[NET_IP_STR_LEN];    /* Sender IP, if not all zeroed. */

​    char notused1[32];  /* 32 bytes reserved for future usage. */

​    uint16_t pport;      /* Sender TCP plaintext port, if base port is TLS */

​    uint16_t cport;      /* Sender TCP cluster bus port */

​    uint16_t flags;      /* Sender node flags */

​    unsigned char state; /* Cluster state from the POV of the sender */

​    unsigned char mflags[3]; /* Message flags: CLUSTERMSG_FLAG[012]_... */

​    union clusterMsgData data;

} clusterMsg;



简单总结一下 Redis Cluster 这种方案的优缺点：



●优点 ：可以横向扩展缓解写压力和存储压力，具备故障转移功能。

●缺点 ：需要耗费更多的服务器资源。



 参考 



●Redis Sentinel 官方文档：https://redis.io/topics/sentinel

●Replication vs Sharding. Sentinel vs Cluster：https://rtfm.co.ua/en/redis-replication-part-1-overview-replication-vs-sharding-sentinel-vs-cluster-redis-topology/

●Redis Cluster 官方教程：https://redis.io/topics/cluster-tutorial

●Redis Cluster 官方公开 PDF 讲义：https://redis.io/presentation/Redis_Cluster.pdf