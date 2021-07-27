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

#### 节点的持久状态信息

- history：当前节点接收到事务 Proposal 的Log
- acceptedEpoch：Follower 已经接受的 Leader 更改 epoch 的 newEpoch 提议
- currentEpoch：当前所处的 Leader 年代
- lastZxid：history 中最近接收到的Proposal 的 zxid（最大zxid）

#### 会话（Session）

- session是客户端与zk服务端之间建立的长连接。
- zk在一个会话中进行心跳检测来感知客户端链接的存活
- zk客户端在一个会话中接收来自服务端的watch事件通知
- zk可以给会话设置超时时间

#### 数据节点（Znode）类型

<img src="http://static.etouch.cn/imgs/upload/1593330610.0123.png" style="zoom: 80%;" />

- 持久节点（PERSISTENT）：创建后一直存在，直到主动删除此节点

- 持久顺序节点（PERSISTENT_SEQUENTIAL）：属于持久节点，每个父节点会为他的第一级子节点维护一份时序，会记录每个子节点创建的先后顺序

- 临时节点（EPHEMERAL）：临时节点的生命周期和客户端会话绑定，一旦客户端失效，这个客户端创建的所有临时节点都会被删除。

  - 临时节点下面不能创建子节点
  - 会话失效是指会话超时，会话断开时临时节点并不会立即删除

- 临时顺序节点（EPHEMERAL_SEQUENTIAL）：属于临时节点，每个父节点会为他的第一级子节点维护一份时序，会记录每个子节点创建的先后顺序

  ![](http://static.etouch.cn/imgs/upload/1593330634.5233.png)



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

#### 版本

Znode的Stat数据结构维护当前ZNode的三个数据版本：

- version：代表当前Znode的数据版本
- cversion：代表当前Znode的子节点的版本，子节点发生变化时会增加该版本号的值
- aversion：代表当前Znode的ACL（访问控制）的版本，修改节点的访问控制权限会增加该版本号的值

#### 访问控制列表（ACL）

Znode存储两部分内容：数据和状态，状态中包含ACL信息。

ACL由三部分组成，即Scheme：id:permission：

![](http://static.etouch.cn/imgs/upload/1593330680.6685.png)

- scheme是验证过程中使用的检验策略
  - world检验策略：ACL格式为world:anyone:permission，默认方式，anyone表示任何人都可以操作权限
  - auth检验策略：ACl格式为auth：id:permission，比如auth:username:password:crdwa
  - digest检验策略：ACL格式：digest：id:permission
  - IP检验策略：ACL格式：ip：id:permission，id是ip地址
  - super检验策略：主要提供给运维人员维护节点使用
- id是权限被赋予的对象，比如ip或某个用户
- permission为可以操作的权限
  - CREATE：创建子节点的权限
  - DELETE：删除子节点的权限
  - READ：获取节点数据和子节点列表的权限 
  - WRITE：更新节点数据的权限
  - ADMIN：设置节点ACL的权限



## Watcher机制

### Watcher解决的问题

zk集群可能存在两个问题：

- 因为集群中有很多机器，当某个通用的配置发生变化后，怎么自动让所有服务器的配置同一生效
- 当集群中某个节点宕机，如何让集群中的其他节点知道

为了解决这两个问题，zk引入了watcher机制来实现发布/订阅功能，能够让多个订阅者同时监听某一个主题对象，当这个主题对象自身状态发生变化时，会通知所有订阅者。

### Watcher基本原理

zk实现watcher需要三个部分：

- zk客户端

- zk服务端

- 客户端的watchManager

  <img src="http://static.etouch.cn/imgs/upload/1593330698.9863.png" style="zoom:67%;" />

如图所示，客户端向zk服务端注册watcher的同时，会将客户端的watcher对象存储在客户端的watchManager中。zk服务器触发watch事件后，会向客户端发送通知，客户端线程从watchManager中取出对应watcher执行。

### Watcher特性

| 特性           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| 一次性         | Watcher是一次性的，一旦被触发就会移除，再次使用时需要重新注册 |
| 客户端顺序回调 | Watcher回调是顺序串行化执行的，只有回调后客户端才能看到最新的数据状态。<br/>一个Watcher回调逻辑不应该太多，以免影响别的watcher执行 |
| 轻量级         | WatchEvent是最小的通信单元，结构上只包含通知状态、事件类型和节点路径，并不会告诉数据节点变化前后的具体内容。<br/>因此如果需要知道变更前的数据或变更后的新数据， 需要业务保存更新前的数据和调用接口获取新的数据 |
| 时效性         | Watcher只有在当前session彻底失效时才会无效，若在session有效期内快速重连成功，则watcher依然存在，仍可接收到通知 |

### 客户端实现事件通知

客户端只需定义一个类实现org.apache.zookeeper.Watcher接口并实现接口的如下方法：

```java
abstract public void process(WatchedEvent event);
```

WatchedEvent有三个成员：

```java
// 通知状态
private final KeeperState keeperState; 
// 事件类型
private final EventType eventType; 
// 节点路径
private String path ; 
```

keeperState和eventType对应关系如下所示：

<img src="http://static.etouch.cn/imgs/upload/1593330717.9452.png" style="zoom: 80%;" />

对于NodeDataChanged事件：无论节点数据发生变化还是数据版本发生变化都会触发（即使被更新数据与新数据一样，数据版本都会发生变化）。

 对于NodeChildrenChanged事件：新增和删除子节点会触发该事件类型。 



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



## 数据一致性

在分布式环境中，一致性是指数据在多个副本之间是否能够保持数据一致的特性。

一致性按照从强到弱的划分：

- 强一致性：也称为原子一致性、线性一致性，两个要求：

  - 任何一次读都能读到某个数据的最近一次写的数据
  - 系统中的所有进程，看到的操作顺序，都和全局时钟下的顺序一致

- 顺序一致性：

  - 任何一次读都能读到某个数据的最近一次写的数据
  - 系统的所有进程的顺序一致，而且是合理的。即不需要和全局时钟下的顺序一致，错的话一起错，对的话一起对

- 弱一致性：系统中的某个数据被更新后，后续对该数据的读取操作可能得到更新后的值，也可能是更改前的值，不保证数据一致

- 最终一致性：是弱一致性的特殊形式，不保证在任意时刻任意节点上的同一份数据都是相同的，但是随着时间的迁移，不同节点上的同一份数据总是在向趋同的方向变化。


**ZK在只有写的情况下是线性一致性，读写的情况下是顺序一致性。**



## 分布式锁

公平锁

<img src="http://static.etouch.cn/imgs/upload/1593330801.793.png" style="zoom: 67%;" />