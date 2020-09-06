---
layout: layout.pug
excerpt:
title: >
    Design: Distributed DNS
navigationTitle: Distributed DNS
menuWeight: 3
---

## 背景
任务在DC/OS中频繁移动，资源必须由应用程序协议动态解析，并且按名称引用。这意味着DNS是DC/OS的一个组成部分。没有在每个项目中实施ZooKeeper或Mesos客户端，而是选择了DNS作为DC/OS中所有组件的通道。

这是通过使用运行在每个DC/OS主节点上的Mesos-DNS实现的。在客户端系统中，把每个主节点放到`/etc/resolv.conf`中。 如果一个主节点出现故障，对该主节点的DNS查询将超时。 DNS转发器（Spartan）通过将DNS查询复制重到多个节点，以返回的第一个结果为准来解决此问题。

为了进一步降低风险，DNS转发器（Spartan）将查询路由到它认为最优化的节点进行查询 。 具体而言，如果一个域以`mesos`结尾，它将向Mesos主节点发送查询。 如果不以`mesos`结尾，则将查询发送到配置的上游节点中的两个。

# 实现
DNS转发器（Spartan）本身非常简单。 它具有重制分发逻辑，并托管一个只有一个记录`ready.spartan`的域`spartan`。 这个记录的目的是验证DNS转发器（斯巴达）的可用性。 许多服务（包括ICMP）在启动之前先ping这个地址。

DNS转发器（Spartan）从Exhibitor那里学习信息。出于这个原因，主节点上的Exhibitor[配置正确][1]至关重要。如果使用静态主节点配置集群，则会从静态配置文件中加载它们。

## ZooKeeper
DNS转发器 (Spartan) 还可以实现ZooKeeper高可用。您可以使用地址`zk-1.zk`, `zk-2.zk`, `zk-3.zk`, `zk-4.zk`, `zk-5.zk`. 如果ZooKeeper少于5个，DNS转发器(Spartan) 会将多个地址指向同一个.

## Watchdog
由于DNS是一个很特殊，很敏感的子系统，我们选择使用watchdog来保护它。在每个节点都安装了一个服务，每5分钟运行一次检查`ready.spartan`是否可以被解析。为了避免谐波效应，它在第一次启动时等待1分钟，以避免spartan赛车。您可以通过在系统健康[仪表盘][2]中查看DNS Forwarder (Spartan) Watchdog的健康状态，来了解watchdog的状态. 

除了watchdog之外，我们还运行genresolv，它检查DNS转发器（Spartan）是否健康并生成resolv.conf。 如果它认为DNS转发器（Spartan）不正常，那么它会使用您配置到DC/OS集群中的上游解析器重写resolv.conf。

## DNS转发接口
DNS转发器（Spartan）创建自己的网络接口。这个接口实际上是一个名为`spartan`的虚拟设备。该设备承载3个IP，`198.51.100.1/32`, `198.51.100.2/32`, `198.51.100.3/32`。 您可以在系统健康[仪表盘][2]中查看DNS Forwarder（Spartan）组件的运行状况。

[1]: /1.10/installing/oss/custom/configuration/configuration-parameters/
[2]: /1.10/monitoring/
