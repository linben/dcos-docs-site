---
layout: layout.pug
navigationTitle: DNS快速参考
title: DNS快速参考
menuWeight: 20
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

本快速参考提供了可用选项的摘要。

为了帮助解释，我们将使用这个虚构的应用程序：

* 该服务处于以下层次结构中： 
 * Group: `outergroup` > Group: `subgroup` > Service Name: `myapp`
* Port: `555` 
 * Port Name: `myport`
 * Load Balanced
* 在[虚拟网络](/1.10/networking/load-balancing-vips/virtual-networks/)上运行
* 在Marathon框架上运行 (如果不确定，可能是这个) 
 * 如果您正在运行另一个框架，则将`marathon`的任何实例替换为您的框架的名称。

# 服务发现选项

使用这些选项之一来查找您的任务的DNS名称。 您应该选择满足您的要求的第一个选项：

1. `outergroupsubgroupmyapp.marathon.l4lb.thisdcos.directory:555` 
 * 这仅在服务负载平衡时才可用。 `:555 `不是DNS地址的一部分，而是表明这个地址和端口是作为一对而不是单独地进行负载平衡的。
2. `myapp-subgroup-outergroup.marathon.containerip.dcos.thisdcos.directory` 
 * 这只有在服务在[虚拟网络](/1.10/networking/load-balancing-vips/virtual-networks/)上运行时才可用。
3. `myapp-subgroup-outergroup.marathon.agentip.dcos.thisdcos.directory` 
 * 这总是可用的，并且应该在服务未在[虚拟网络](/1.10/networking/load-balancing-vips/virtual-networks/)上运行时使用。
4. `myapp-subgroup-outergroup.marathon.autoip.dcos.thisdcos.directory` 
 * 这总是可用的，应该用于解决在[虚拟网络](/1.10/networking/load-balancing-vips/virtual-networks/)上进行转换的应用程序。
5. `myapp-subgroup-outergroup.marathon.mesos` 
 * 这总是可用的，并且大部分等同于`agentip`。 然而，它比`agentip`不那么具体和性能低，因此不鼓励使用。

其他发现选项：

* `_myport._myapp.subgroup.outergroup._tcp.marathon.mesos` 
 * 这不是DNS A记录，而是DNS SRV记录。 这只有在端口有名字时才可用。 SRV记录是从一个映射