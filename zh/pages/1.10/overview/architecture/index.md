---
layout: layout.pug
navigationTitle:  Architecture
title: Architecture
menuWeight: 2
excerpt:

enterprise: false
---

<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->


DC/OS是运行分布式容器化软件的平台，如应用程序，任务和服务等。作为一个平台，DC/OS与基础架构层是不同的，独立于基础架构层。 这意味着它的基础架构可以由虚拟或物理硬件组成，只要提供计算，存储和网络即可.

![DC/OS Architecture Layers](/1.10/img/dcos-architecture-layers.png)

## 软件层

在软件层，DC/OS提供了软件包管理和软件包存储库，可以轻松安装和管理多种类型的服务：数据库，消息队列，流处理器，产生物存储库，监控解决方案，持续集成工具，源代码管理，日志聚合器等等。除了这些打包的应用程序和服务之外，用户可以安装他们自己的定制应用程序，服务和任务调度器。

有关更多信息，请参阅[任务类型]（/1.10/overview/architecture/task-types/）。

## 平台层

在平台层，有几十个组件分为以下几类：

- 集群管理
- 容器编排
- 容器运行时
- 日志和性能指标
- 网络
- 包管理
- IAM和安全性[企业版 type="inline" size="small" /]
- 存储

这些组件分为多个节点类型：

- 主节点
- 私有计算节点
- 公共计算节点

对于要安装的DC/OS，每个节点必须已经配置了任一支持的操作系统。
 
有关详细信息，请参阅[组件](/1.10/overview/architecture/components/), [节点类型](/1.10/overview/architecture/node-types/), 和[主机操作系统](/1.10/overview/concepts/#host-operating-system).

##基础架构层

在基础架构层，DC/OS可以安装在公共云，私有云或本地硬件上。 其中一些具有自动配置工具，但几乎可以使用在任何基础架构上，只要是通过IPv4网络互联的x86架构计算机即可。

有关更多信息，请参阅[安装]（/1.10/installing/）。

##外部组件

除了在数据中心运行的软件之外，DC/OS还包含并集成了几个外部组件：[图形界面](/1.10/gui/), [命令行界面](/1.10/cli/), [软件包存储库](/1.10/administering-clusters/repo/), 和[容器存储库](/1.10/overview/concepts/#container-registry).
