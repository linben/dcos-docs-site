---
layout: layout.pug
navigationTitle: 网络
title: 网络
menuWeight: 70
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

DC / OS提供了许多开箱即用的工具，从容器之间的基本网络连接到更高级的功能（如负载平衡和服务发现）。

# IP Per Container

允许容器在任何类型的基于 IP 的虚拟网络上运行, 每个容器都有自己的网络命名空间。

DC/OS 通过使用容器网络接口 (CNI) 支持通用容器运行时 (UCR) 的每个容器的 IP。 DC/OS 通过使用容器网络模型 (全国) 为泊坞窗容器运行时支持每个容器的 IP。

DC/OS 为每个容器中的 IP 提供了一个现成的虚拟网络解决方案, 称为 dc/os 覆盖, 既适用于 UCR 和泊坞窗容器运行时。 DC/OS 覆盖使用 Mesos 中的 CNI/全国支持来提供每个容器的 IP。有关更多信息, 请参见 [ Containerizer 文档 ](/1.10/deploying-services/containerizers/)。

# 基于 DNS 的服务发现

DC/OS 包括高可用性、分布式、基于 DNS 的服务发现。DC/OS 中的服务发现机制包含以下组件:

- 一个集中式组件, 称为 Mesos DNS, 它在每个主机上运行。
- 在每个代理上运行的称为Spartan的分布式组件。

## Mesos DNS

Mesos DNS是一个集中的，复制的DNS服务器，可以在每个主服务器上运行。 每个由 DC/OS 启动的任务都得到一个众所周知的 DNS 名称。 这在每个主控形状上提供了一个复制的高可用性 DNS 服务。 Mesos DNS 的每个实例都轮询前导 Mesos 主机, 并为在 DC/OS 中运行的每个服务使用域 ` *. mesos ` 生成完全限定的域名 (FQDN)。 For more information, see the [Mesos DNS documentation](/1.10/networking/mesos-dns/).

## DNS 转发器 (Spartan)

斯巴达在每个代理上充当 Mesos dns 的 dns 伪装。 每个代理上的斯巴达实例配置为侦听代理上的三不同的本地接口, 并将代理上的服务器设置为这三接口。 这允许容器在 DNS 请求上执行多达三重试。 为了提供高可用性的 dns 服务, 斯巴达人将收到的每个请求转发给每个主机上运行的不同的 Mesos dns 实例。

- Scale-out DNS Server on DC/OS masters with replication.
- 具有指向所有活动/活动 dns 服务器守护进程的链接的 dns 服务器代理。
- 本地服务的 DNS 服务器缓存服务。

每个agent上的斯巴达实例也充当了使用称为 [ Minuteman ](/1.10/networking/load-balancing-vips/) 的 DC/OS 内部负载平衡器负载均衡的任何服务的 DNS 服务器。 Any service that is load balanced by Minuteman gets a [virtual-ip-address (VIP)](/1.10/networking/mesos-dns/) and an FQDN in the `"*.l4lb.thisdcos.directory"` domain. 分配给负载平衡服务的 FQDN 然后存储在斯巴达人中。 所有斯巴达人的实例都是用八卦来交换他们从民兵当地发现的记录。 这为由民兵负载平衡的任何任务提供了高度可用的分布式 DNS 服务。 有关详细信息, 请参阅 [ 斯巴达存储库 ](https://github.com/dcos/spartan)。

# 负载平衡

DC/OS 提供了一个负载平衡选项-盒式: [Minuteman](/1.10/networking/load-balancing-vips/)。

其他两个负载平衡器, [Edge-LB](/service-docs/edge-lb/) 和 [ Marathon-LB ](/service-docs/marathon-lb/) 可以作为来自 DC/OS 宇宙软件包存储库的服务安装。

|                                    | Minuteman | Edge-LB | Marathon-LB |
| ---------------------------------- | --------- | ------- | ----------- |
| Open Source                        | X         |         | X           |
| Enterprise                         | X         | X       | X           |
| North-South (External to Internal) |           | X       | X           |
| East-West (Internal to Internal)   | X         | X       | X           |
| Layer 4 (Transport Layer)          | X         | X       | X           |
| Layer 7 (Application Layer)        |           | X       | X           |
| Marathon Services                  | X         | X       | X           |
| Non-Marathon Service Tasks         | X         | X       |             |
| 0 hop load balancing               | X         |         |             |
| No single point of failure         | X         |         |             |

## Minuteman

Minuteman是一个分布式层4虚拟 IP 东-西负载平衡器, 默认情况下安装。 它具有高度的可伸缩性和高度可供使用, 提供0跃点负载平衡, 没有单一的瓶颈, 并容忍主机故障。

## Edge-LB

[ Edge-LB](/service-docs/edge-lb/0.1.9/) 在 HAProxy 上生成。 HAProxy 提供基本功能, 如基于 TCP 和 HTTP 的应用程序的负载平衡、SSL 支持和运行状况检查。 此外, Edge-LB 为零停机服务部署策略 (如蓝/绿部署) 提供了一流的支持。 Edge-LB 订阅 Mesos, 并实时更新 HAProxy 配置。

Edge-LB proxies and load balances traffic to all services that run on DC/OS. 相比之下, Marathon-LB 只能与马拉松的任务。 For example, if you are using Cassandra, Edge-LB can load balance the tasks launched by Cassandra.

## Marathon-LB

[ Marathon-LB ](/service-docs/marathon-lb/) 基于 HAProxy、快速代理和南北负载平衡器。 HAProxy 为基于 TCP 和 http 的应用程序提供代理和负载平衡, 其中包括 SSL 支持、http 压缩、健康检查、Lua 脚本等功能。 Marathon-LB 订阅马拉松的事件总线, 并实时更新 HAProxy 配置。