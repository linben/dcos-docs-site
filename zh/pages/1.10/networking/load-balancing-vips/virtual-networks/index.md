---
layout: layout.pug
navigationTitle: 虚拟网络
title: 虚拟网络
menuWeight: 20
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

DC / OS通过使用虚拟网络实现虚拟联网。 DC / OS虚拟网络使您能够为系统中的每个容器提供一个唯一的IP地址(“每容器IP”)，并在子网间提供隔离保证。 DC / OS虚拟网络具有以下优点：

* Mesos和Docker容器都可以在单个节点内和集群上的节点之间进行通信。
* 可以创建服务，使其流量与来自群集中任何其他虚拟网络或主机的其他流量隔离。
* 他们不需要担心应用程序中潜在的重叠端口，或者需要使用非标准端口进行服务以避免重叠。
* 您可以生成一个类任务的任意数量的实例, 并让它们都侦听同一端口, 以便客户端不必执行端口发现。
* 您可以运行需要群集内连接的应用程序，如Cassandra，HDFS和Riak。
* 您可以创建多个虚拟网络来隔离组织的不同部分，例如开发，市场营销和生产。

**注意：**子网间的隔离保证取决于您的CNI实施和/或您的防火墙策略。

# 使用虚拟网络

首先, 您或数据中心操作员需要 [ 配置虚拟网络 ](/1.10/networking/virtual-networks/)。

虚拟网络是在安装时配置的。 您或数据中心操作员将为 ` config. yaml ` 中的每个网络指定一个规范名称。 当您的服务需要启动容器时, 请通过该规范名称来引用它。

若要在马拉松应用程序定义中使用虚拟网络, 请在表单中指定 ` "network": "USER" ` 属性以及 ` ipAddress ` 字段: ` {"ipAddress": {"network": "$MYNETWORK"}} `。 `$MYNETWORK ` 的值是网络的规范名称。

# 例子

下面的马拉松应用程序定义指定一个名为 ` dcos-1 ` 的网络, 它引用同名的目标 DC/OS 虚拟网络。

```json
{
   "id":"my-networking",
   "cmd":"env; ip -o addr; sleep 30",
   "cpus":0.10,
   "mem":64,
   "instances":1,
   "backoffFactor":1.14472988585,
   "backoffSeconds":5,
   "ipAddress":{
      "networkName":"dcos-1"
   },
   "container":{
      "type":"DOCKER",
      "docker":{
         "network":"USER",
         "image":"busybox",
         "portMappings":[
            {
               "containerPort":123,
               "servicePort":80,
               "name":"foo"
            }
         ]
      }
   }
}
```

通过[马拉松](/1.10/deploying-services/service-ports/)了解更多有关端口和网络的信息。