---
layout: layout.pug
navigationTitle:  Boot Sequence
title: Boot Sequence
menuWeight: 6
excerpt:

enterprise: false
---

<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->


在安装过程中，DC/OS组件服务都是并行启动的，但是由于相互依赖关系，所以初始化并以相对一致的顺序响应。

DC/OS诊断服务监视组件服务和节点运行状况。当所有组件服务都健康时，节点被标记为健康。

## 主节点

以下是在主节点上DC/OS组件服务的启动顺序。

1. DC/OS诊断服务启动
    1. 从systemd获取组件状态
    1. 报告节点处于非健康状态，除非所有组件(systemd服务)都处于健康状态
    1. 报告集群处于非健康状态，除非所有节点都处于健康状态
1. Exhibitor启动
    1. 创建ZooKeeper配置，启动ZooKeeper
1. Mesos主节点启动
    1. 注册本地ZooKeeper
    1. 从ZooKeeper发现其他Mesos主节点
    1. 选举一个主导的主节点
1. Mesos-DNS启动
    1. 发现主导的Mesos主节点(从zookeeper或从本地mesos-master中?)
    1. 从主导的Mesos主节点中获取集群状态
1. 网络组件启动
    1. 转发DNS查询请求到Mesos-DNS
    1. 初始化VIP
    1. 初始化重叠网络
1. 容器编排组件启动
    1. 注册到本地ZooKeeper中
    1. 选举主导的主节点
    1. 从Mesos-DNS中发现主导的Mesos主节点
    1. 主导节点注册到主导的Mesos主节点
1. 日志和性能指标组件启动
1. 安全组件启动
1. Admin Router启动
    1. 从Mesos-DNS中发现主导的Mesos主节点
    1. 从Mesos-DNS中发现主导的Marathon
    1. 将未授权的流量转发到认证组件
    1. 通过代理将API和GUI的流量转发到发现的组件
    1. 将管理服务的流量转发到相应的DC/OS服务
    1. DC/OS图形界面提供服务

## 代理节点

以下是在代理节点上DC/OS组件服务的启动顺序。

1. DC/OS诊断服务启动
    1. 从systemd获取组件状态
    1. 报告节点处于非健康状态，除非所有组件(systemd服务)都处于健康状态
1. Mesos代理程序启动
    1. 从ZooKeeper中发现主导的Mesos主节点
    1. 在主导的Mesos主节点上注册
    1. 主导的Mesos主节点使用注册IP与新加入的代理节点通讯
    1. 主导的Mesos主节点开始提供新节点的资源给新任务的调度器
    1. 在DC/OS的API，图形界面，命令行中可以看到新加入的DC/OS节点
1. 网络组件启动
    1. 转发DNS查询到Mesos-DNS
    1. 初始化VIP
    1. 初始化重叠网络
1. 日志和性能指标组件启动
1. Admin Router代理程序启动
    1. 从Mesos-DNS中发现主导的Mesos主节点
    1. 通过代理将内部API流量发送到发现的组件

## 服务

在DC/OS安装和初始化完成后，可以开始安装DC/OS服务。服务可以通过DC/OS包管理从Mesosphere Universe或直接从Marathon安装。

以下是DC/OS服务的启动顺序.

1. 主导的Mesos主节点提供代理节点的资源给Marathon
1. 主导的Marathon在代理节点上为服务调度相应的资源
1. Mesos代理节点以容器化任务的试启动服务

### 调度服务

一些DC/OS服务也是调度器，与DC/OS交互管理任务

以下是DC/OS调度服务的启动顺序

1. 主导的Mesos主节点提供代理节点的资源给Marathon
1. 主导的Marathon在有足够资源的代理节点上调度服务
1. Mesos代理程序以一个或多个容器化任务的方式启动服务
1. 调度服务从Mesos-DNS中发现主导的Mesos主节点
1. 调度服务向主导的Mesos主节点注册
1. 主导的Mesos主节点向新的调度服务提供代理节点上的资源

### 更多信息

有关任务和服务的更多信息，请参阅[Distributed Process Management](/1.10/overview/architecture/distributed-process-management/).
