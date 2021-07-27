## ZK简介

ZooKeeper 是一个典型的分布式数据一致性解决方案，分布式应用程序可以基于 ZooKeeper 实现诸如数据发布/订阅、负载均衡、命名服务、分布式协调/通知、集群管理、Master 选举、分布式锁和分布式队列等功能。

### 基本概念

#### 集群角色

- Leader：集群运行期间有且仅有一台，为客户端提供读写服务。
- Follower：集群运行期间有多台
  - 为客户端提供读服务
  - 将写事务转发给Leader
  - 参与写事务请求Proposal的投票
  - 参与Leader的选举投票
- Observer：是弱化版的Follower，一般是为了增强zk集群的读请求并发能力
  - 为客户端提供读服务
  - 将写事务转发给Leader
  - 不参与任何形式的投票

#### 节点状态

- LOOKING：寻找Leader的状态，当服务器处于此状态时，表示当前没有Leader，需要进入选举流程；
- FOLLOWING：跟随者状态，表明当前服务器角色是Follower
- OBSERVING：观察者状态，表明当前服务器是Observer
- LEADING：领导者状态，表明当前服务器角色是Leader

#### Zxid

为了保证事务顺序性，zookeeper 采用了单调递增的事务id号（zxid）来标识一次更新操作的Proposal ID。

<img src="http://static.etouch.cn/imgs/upload/1593330654.9723.png" style="zoom:80%;" />

Zxid是一个64位的数字：

- 高32位则代表了Leader周期epoch的编号，每当选举产生一个新的leader，就会从该leader的本地日志中取出最大事务Proposal 的Zxid，并从该Zxid中解析出对应的epoch值，然后进行加1操作，之后就以此编号作为新的epoch，并将低32位置0
- 低32位可以看作一个简单的单调递增的计数器，Leader在处理新的事务Proposal 的时候，都会对该计数器进行加1操作

Znode的Stat数据结构维护三个zxid：

- czxid：创建Znode的zxid
- mzxid：最近一次修改Znode的zxid
- pzxid：最近一次修改子Znode的zxid

### ServerId（myid）

每个ZooKeeper服务器，都需要在数据文件夹下创建一个名为myid的文件，该文件包含整个ZooKeeper集群唯一的ID（整数）。例如，某ZooKeeper集群包含三台服务器，hostname分别为zoo1、zoo2和zoo3，其myid分别为1、2和3，则在配置文件中其ID与hostname必须一一对应，如下所示。在该配置文件中，server.后面的数据即为myid
server.**1**=zoo1:2888:3888
server.**2**=zoo2:2888:3888
server.**3**=zoo3:2888:3888



## Zab（ Zookeeper Atomic Broadcast）协议

### Zab协议核心

Zab协议的核心：**定义了事务请求的处理方式**

- 所有的事务请求必须由一个全局唯一的服务器来协调处理，这样的服务器被叫做 **Leader服务器**。其他剩余的服务器则是 **Follower服务器**

- Leader服务器 负责将一个客户端事务请求，转换成一个 **事务Proposal**，并将该 Proposal 分发给集群中所有的 Follower 服务器，也就是向所有 Follower 节点发送数据广播请求（或数据复制）

- 分发之后Leader服务器需要等待所有Follower服务器的反馈（Ack请求），**在Zab协议中，只要超过半数的Follower服务器进行了正确的反馈**后（也就是收到半数以上的Follower的Ack请求），那么 Leader 就会再次向所有的 Follower服务器发送 Commit 消息，要求其将上一个 事务proposal 进行提交

  <img src="http://static.etouch.cn/imgs/upload/1593330735.2397.png" style="zoom:67%;" />

### Zab协议内容

Zab 协议包含两种基本模式，分别是崩溃恢复和消息广播。

- 崩溃恢复：当整个集群在启动或者当 leader 节点出现网络中断、崩溃等情况时，ZAB 协议就会进入**进入崩溃恢复模式**，选举产生新的Leader。
- 消息广播：当选举产生了新的 Leader，同时集群中已经有过半的 follower 节点完成了和 leader 状态同步以后，那么整个集群就**进入消息广播模式**。



## 崩溃恢复

崩溃恢复主要包括两部分：**Leader选举** 和 **数据恢复**

### Leader选举

在3.4.0后的Zookeeper的版本只保留了TCP版本的FastLeaderElection选举算法。org.apache.zookeeper.server.quorum.FastLeaderElection

**投票数据结构**：

- id：被推举的Leader的ServerId。
- zxid：被推举的Leader事务ID
- logicalclock（electionEpoch）：逻辑时钟，用来判断多个投票是否在同一轮选举周期中，每次进入新一轮的投票后，都会对该值进行加1操作。
- peerEpoch：被推举的Leader的epoch
- state：当前服务器的状态

**投票比较规则**：

-  peerEpoch大的胜出
- 若 peerEpoch相等，zxid大的胜出
- 若peerEpoch和 zxid 相等，ServerId大的胜出（zoo.cfg中的myid）

**QuorumCnxManager：网络I/O**

​     每台服务器在启动的过程中，会启动一个QuorumPeerManager，负责各台服务器之间的底层Leader选举过程中的网络通信。

<img src="http://static.etouch.cn/imgs/upload/1593330754.3307.webp" style="zoom:80%;" />

- **消息队列**：QuorumCnxManager内部维护了一系列的队列，用来保存接收到的、待发送的消息以及消息的发送器，除接收队列以外，其他队列都按照SID分组形成队列集合，互不干扰。
  -  recvQueue：消息接收队列，用于存放那些从其他服务器接收到的消息
  -  queueSendMap：消息发送队列，用于保存那些待发送的消息，按照SID进行分组
- **建立连接**：为了能够相互投票，Zookeeper集群中的所有机器都需要两两建立起网络连接。QuorumCnxManager在启动时会创建一个ServerSocket来监听Leader选举的通信端口(默认为3888)。开启监听后，Zookeeper能够不断地接收到来自其他服务器的创建连接请求，在接收到其他服务器的TCP连接请求时，会进行处理。为了避免两台机器之间重复地创建TCP连接，Zookeeper只允许SID大的服务器主动和其他机器建立连接，否则断开连接。在接收到创建连接请求后，服务器通过对比自己和远程服务器的SID值来判断是否接收连接请求，如果当前服务器发现自己的SID更大，那么会断开当前连接，然后自己主动和远程服务器建立连接。
- **消息接收**：由消息接收器RecvWorker负责，由于Zookeeper为每个远程服务器都分配一个单独的RecvWorker，因此，每个RecvWorker只需要不断地从这个TCP连接中读取消息，并将其保存到recvQueue队列中。
- **消息发送**：由于Zookeeper为每个远程服务器都分配一个单独的SendWorker，因此，每个SendWorker只需要不断地从对应的消息发送队列中获取出一个消息发送即可，同时将这个消息放入lastMessageSent中。在SendWorker中，一旦Zookeeper发现针对当前服务器的消息发送队列为空，那么此时需要从lastMessageSent中取出一个最近发送过的消息来进行再次发送，这是为了解决接收方在消息接收前或者接收到消息后服务器挂了，导致消息尚未被正确处理。同时，Zookeeper能够保证接收方在处理消息时，会对重复消息进行正确的处理。

**选举流程**：

<img src="http://static.etouch.cn/imgs/upload/1593330768.0642.webp"  />

### 数据恢复

这一阶段 Follower 发送他们的 lastZxid 给 Leader，Leader 根据 lastZxid 决定如何同步数据。

- **DIFF**：如果Leader的lastCommitedZxid比Follower的lastZxid大，那就将多的日志发送给Follower，让Follower补齐；
- **TRUNC（lastCommitedZxid）**：如果Leader的lastCommitedZxid比Follower的lastZxid小，那就发送命令给Follower ，将Follower多余的日志截断；
- **SNAP** ：如果Follower远远落后于Leader，那么直接进行数据传输，这样比发送丢失的日志更有效率;



## 消息广播

- 客户端发起一个写操作请求

- Leader将客户端的写请求转换成一个事务Proposal （提议），同时为每个 Proposal 分配一个全局的ID，即Zxid。

- Leader为每个 Follower分配一个单独的FIFO队列，然后将需要广播的 Proposal 依次放到队列中取，并且根据 FIFO 策略进行消息发送

- Leader等待所有Follower的反馈

- Follower接收到Proposal，会首先将其以事务日志的方式写入本地磁盘中，写成功后向Leader回复一个Ack响应

- Leader 接收到超过半数以上 Follower 的 Ack 响应消息后，即认为消息发送成功，可以发送 commit 消息

- Leader 向所有 Follower 广播 commit 消息，同时自身也会完成事务提交

- Follower接收到commit消息后，提交该事务 

  <img src="http://static.etouch.cn/imgs/upload/1593330786.5296.webp" style="zoom:80%;" />
