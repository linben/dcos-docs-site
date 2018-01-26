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

Mesos DNS是一个集中的，复制的DNS服务器，可以在每个主服务器上运行。 Every task started by DC/OS gets a well-known DNS name. This provides a replicated highly available DNS service on each of the masters. Every instance of Mesos DNS polls the leading Mesos master and generates a fully qualified domain name (FQDN) for every service running in DC/OS with the domain `*.mesos`. For more information, see the [Mesos DNS documentation](/1.10/networking/mesos-dns/).

## DNS Forwarder (Spartan)

Spartan acts as a DNS masquerade for Mesos DNS on each agent. The Spartan instance on each agent is configured to listen to three different local interfaces on the agent and the nameservers on the agent are set to these three interfaces. This allows containers to perform up to three retries on a DNS request. To provide a highly available DNS service, Spartan forwards each request it receives to the different Mesos DNS instances which are running on each master.

- Scale-out DNS Server on DC/OS masters with replication.
- DNS server Proxy with links to all Active/Active DNS server daemons.
- DNS server cache service for local services.

每个agent上的斯巴达实例也充当了使用称为 [ Minuteman ](/1.10/networking/load-balancing-vips/) 的 DC/OS 内部负载平衡器负载均衡的任何服务的 DNS 服务器。 Any service that is load balanced by Minuteman gets a [virtual-ip-address (VIP)](/1.10/networking/mesos-dns/) and an FQDN in the `"*.l4lb.thisdcos.directory"` domain. The FQDN allocated to a load-balanced service is then stored in Spartan. All Spartans instances exchange the records they have discovered locally from Minuteman by using GOSSIP. This provides a highly available distributed DNS service for any task that is load balanced by Minuteman. For more information, see the [Spartan repository](https://github.com/dcos/spartan).

# 负载平衡

DC/OS 提供了一个负载平衡选项-盒式: [Minuteman](/1.10/networking/load-balancing-vips/)。

Two other load balancers, [Edge-LB](/service-docs/edge-lb/) and [Marathon-LB](/service-docs/marathon-lb/) can be installed as services from the DC/OS Universe package repository.

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

[ Edge-LB](/service-docs/edge-lb/0.1.9/) 在 HAProxy 上生成。 HAProxy 提供基本功能, 如基于 TCP 和 HTTP 的应用程序的负载平衡、SSL 支持和运行状况检查。 In addition, Edge-LB provides first class support for zero downtime service deployment strategies, such as blue/green deployment. Edge-LB 订阅 Mesos, 并实时更新 HAProxy 配置。

Edge-LB proxies and load balances traffic to all services that run on DC/OS. In contrast, Marathon-LB can only work with Marathon tasks. For example, if you are using Cassandra, Edge-LB can load balance the tasks launched by Cassandra.

## Marathon-LB

[Marathon-LB](/service-docs/marathon-lb/) is based on HAProxy, a rapid proxy and north-south load balancer. HAProxy provides proxying and load balancing for TCP and HTTP based applications, with features such as SSL support, HTTP compression, health checking, Lua scripting and more. Marathon-LB subscribes to Marathon’s event bus and updates the HAProxy configuration in real time.