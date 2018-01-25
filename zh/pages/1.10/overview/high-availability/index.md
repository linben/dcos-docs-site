---
layout: layout.pug
navigationTitle: High Availability
title: High Availability
menuWeight: 6
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

本文档讨论DC/OS中的高可用性（HA）功能以及在DC/OS上构建HA应用程序的最佳实践。

# 领导者/跟随者 架构

HA系统中常见的模式是领导者/跟随者的概念。这有时也被称为：主/从，主/副或其他组合。这个体系架构就是，有一个主授权进程，并具有N个备用进程。在某些系统中，备用进程也可能能够处理请求或执行其他操作。 例如，当使用主副架构的MySQL数据库时，副本能够响应只读请求，但不能响应写入（只有主进程接受写入）。

在DC/OS中，一些组件遵循领导者/跟随者模式。 我们将在这里讨论其中的一些，以及它们如何工作。

#### Mesos

Mesos可以运行在高可用模式下，需要3个或5个主节点。当以HA模式运行，一个主节点被选举为领导者，其他主节点是跟随者。每个主节点日志都是复制的，其中包含着集群的状态。领导主节点的选举是使用ZooKeeper。更多信息，请参阅[Mesos高可用文档](https://mesos.apache.org/documentation/latest/high-availability/).

#### Marathon

Marathon可以运行在高可用模式下，允许运行多个Marathon实例（HA至少是2个），其中一个被选举为领导者。Marathon使用ZooKeeper完成领导者选举。跟随者不能接受写或是API请求，跟随者将所有API请求代理转发给领导Marathon实例.

#### ZooKeeper

ZooKeeper在无数DC/OS服务中使用以提供一致性。ZooKeeper可以被用作分布式锁服务，状态存储库，消息系统。ZooKeeper使用\[Paxos-like\](https://en.wikipedia.org/wiki/Paxos_(computer_science&#41;) 日志复制和领导者/跟随者的架构来保证多个ZooKeeper实例间的一致性。更有有关ZooKeeper如何工作的讯息，请参阅[ZooKeeper内部文档](https://zookeeper.apache.org/doc/r3.4.8/zookeeperInternals.html).

# 故障域隔离

故障域隔离是构建高可用系统的一个重要部分。为了正确处理故障情况，系统必须跨故障域进行分布以保持出现问题时继续提供服务。 有不同类型的故障域，其中的几个例子是：

- 物理域：其中包括主机，机柜，数据中心，地区和可用域.
- 网络域：连接在同一网络的主机可能属于同一个网络片区。例如，一台共享交换机故障或是加载了无效的配置.

使用DC/OS，您可以将主节点分布到不同的机架，以达到高可用。代理节点可以跨地区分布，建议使用属性标记来描述其位置。像ZooKeeper这样的同步服务也应该保持在同一个区域内，以减少网络延迟。更多信息，请查阅高可用配置[文档](/1.10/installing/high-availability/).

对于应用程序高可用，需要他们可以跨故障区分布。在Marathon上，这可以通过使用[`UNIQUE`和`GROUP_BY`限制运算符](https://mesosphere.github.io/marathon/docs/constraints.html) 来完成.

# 解耦的考虑

高可用的服务应该分开，按照服务分开。 例如，Web服务应该与数据库和共享缓存解耦。

# 消除单点故障

单点故障会以很多形式存在。例如，当系统中所有服务使用同一个ZooKeeper集群时，ZooKeeper服务会变成故障单点。可以通过为不同的服务配置不同的ZooKeeper集群来减少风险。有一个Exhibitor服务[Universe package](https://github.com/mesosphere/exhibitor-dcos) 让这个变的很容易.

其他常见的单点故障例如：

- 单个数据库实例 (如MySQL)
- 一次性服务
- 非高可用的负载均衡

# 快速故障发现

有多种方式完成快速故障发现。类似ZooKeeper的服务可用于故障识别，例如发现网络分片或是主机故障。服务健康检查也可用于检查特定类型的故障。作为最佳实践，服务*应该*暴露健康检查路径，可以被用于类似Marathon的服务。

# 快速故障切换

当遇到故障时, 故障切换[尽可能快地完成](https://en.wikipedia.org/wiki/Fail-fast). 快速故障切换可以通过以下方式完成:

- 使用具有高可用的负载均衡，例如[Marathon-LB](/service-docs/marathon-lb/), 或内部的[4层负载均衡](/1.10/networking/load-balancing-vips/).
- 按照[12-factor app](http://12factor.net/) 准则来构建应用程序.
- 构建服务时遵循REST最佳实践: 特别是避免提交请求时避免在服务器上存储客户端状态.

出现错误时，大多数的DC/OS服务将采用快速故障模式。具体来说Mesos和Marathon都将在失败的情况下关闭，比如失去领导者。
