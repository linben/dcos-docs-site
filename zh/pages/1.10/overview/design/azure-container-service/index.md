---
layout: layout.pug
excerpt:
title: >
    Reference implementation: The Azure Container Service
navigationTitle: ACS
menuWeight: 1
---

[DC/OS](https://github.com/dcos) 是一个由[Apache Mesos](https://mesos.apache.org)提供支持的分布式操作系统 - 将CPU，RAM，网络等聚合起来，看作分布式内核，然后实现核心分布式系统组件，处理系统范围的任务，例如调度，dns，服务发现和其他，而不考虑底层基础设施。[Azure容器服务](https://azure.microsoft.com/documentation/articles/container-service-deployment/) 是针对利用Microsoft Azure基础架构功能优化的DC/OS的参考实现。如果您已经拥有Azure帐户，则可以通过[创建Azure容器服务集群](https://aka.ms/acscreate) 来尝试构建于Microsoft Azure上的DC/OS的参考实现。（如果您没有帐户，请先获取[免费Azure试用账户](https://azure.microsoft.com/pricing/free-trial/) 。）

本文档说明:

- 使用DC/OS的优势
- Azure基础设施及实现架构
- 用来构建DC/OS和ACS集群的物料清单

## DC/OS的优势

DC/OS由Apache Mesos提供支持，作为一组计算机集群的分布式内核，可将其作为一个单元处理，保留了每个计算机的控制权。在DC/OS中，系统的内核实际上是任何公共和私有的Mesos主节点和代理节点的数量; 失败的Mesos主节点被一个备用主节点透明地替换，并且处理主导节点选举。当然，也会处理故障节点和进程。

DC/OS应用程序在其分布式用户空间中用作系统组件。最明显的是系统Marathon组件，它是DC/OS分布式系统的`init`; 但是这还包括Admin Router服务，Mesos-DNS服务, Exhibitor以及其他系统组件，用作用户进程并管理主节点和代理节点。

更多有关DC/OS架构的说明，请参阅[DC/OS架构](/1.10/overview/architecture/); 对于更多详细的组件讨论，请参阅[DC/OS组件介绍](/1.10/overview/architecture/components/).

### 为什么选择DC/OS而不是Mesos?

直接使用Mesos，事实上[很多公司取得了成功](https://mesos.apache.org/documentation/latest/powered-by-mesos/), 您可想要或者自己部署Mesos. 然而，DC/OS有一些重要功能，开源Mesos不具备，而您可能需要的功能:

1. 部署和管理Mesos很复杂，正是因为它可以管理非常复杂的环境。 DC/OS使复杂变得简单易用，并得到社区的支持。
2. DC/OS不仅在工业上而且在互联网上都实现了容错。
3. DC/OS universe里的安装包为开发人员，数据科学家和系统管理员提供了所有他们最喜欢的开源软件包。
4. 实时性能数据可供您最喜爱的诊断和分析软件包使用。
5. DC/OS有三种自动化管理分布式操作系统的方法：强大的CLI，实用的用户界面和丰富的API。

### 软件包Universe

你需要使用的工具，以下列出了第1天使用DC/OS涉及的内容，按照许可类型分类。

#### Apache License V2

- ArangoDB
- Apache Cassandra
- Crate
- Elastic Search
- Etcd
- Exhibitor
- Apache Hadoop
- Hue
- Jenkins
- Apache Kafka
- Linkerd
- Mr Redis
- Namerd
- Quobyte
- Riak
- Spark Notebook
- Apache Spark
- Apache Storm
- Docker Swarm
- Apache Zeppelin

#### Simplified BSD

- Datadog
- Nginx

#### MIT

- OpenVPN Admin
- OpenVPN
- Ruxit

And while it's easy to think of using the universe of packages available with the DC/OS packaging system, you can also publish there to give your skills back to the community, too.
尽管使用DC/OS Universe提供的软件包很容易，但您也可以自己发布，将您的软件包贡献给社区。

您可以想自己部署DC/OS，但是您可以从参考Azure容器服务的“云”的DC/OS实施开始。

## Azure容器服务基础设施及优化

Azure容器服务是最好的开始使用DC/OS的方法之一，DC/OS作为关键业务流程选项之一构建，DC/OS实现经过优化，可以在Microsoft Azure和本地，Azure Stack轻松创建和使用。mesos和分布式集群，这些分布式集群可以像一个大型系统一样在数据中心或Azure中使用和管理。

Azure容器服务实现为您带来很多好处:

1. 开始使用DC/OS最很简单的方法。点击几个按键，提供一些参数，您就可以开始部署您的应用程序。如果您已有Azure账号，[试一下吧](https://aka.ms/acscreate).
2. 装置DC/OS并专门针对Azure进行优化：配置高可用的DC/OS集群，创建和配置所有VM，存储，网络，负载平衡器等
3. 如果您决定在系统不断发展时对您的应用程序有所帮助，增加与Azure服务的集成。
4. 完整的支持：微软支持基础设施，Mesosphere支持DC/OS.

默认ACS架构如下图:

![Azure Container Service architecture using DC/OS.](/1.10/img/dcos-acs.png)



## DC/OS组件列表

以下列表显示了DC/OS本身使用的组件。您会注意到核心组件Mesos，Marathon，Python等等

- adminrouter
- boost-system
- boto
- bouncer
- cosmos
- curl
- dcos-cluster-id
- dcos-diagnostics
- dcos-history-service
- dcos-image
- dcos-image-deps
- dcos-installer
- dcos-installer-ui
- dcos-metrics
- dcos-oauth
- dcos-signal
- dcos-ui
- dnspython
- erlang
- exhibitor
- flask
- hadoop
- hdfs-mesos
- java
- libevent
- logrotate
- marathon
- mesos
- mesos-buildenv
- mesos-dns
- mesos-metrics-module
- mesos-modules-private
- minuteman
- ncurses
- networking_api
- openssl
- python
- python-dateutil
- python-docopt
- python-jinja2
- python-kazoo
- python-markupsafe
- python-passlib
- python-pyyaml
- python-requests
- python-retrying
- six
- spartan
- strace
- toybox
- treeinfo.json
- zk-value-consensus
