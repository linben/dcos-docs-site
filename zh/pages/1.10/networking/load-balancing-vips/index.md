---
layout: layout.pug
navigationTitle: 负载平衡和虚拟IP (VIP)
title: 负载平衡和虚拟IP (VIP)
menuWeight: 0
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

DC / OS提供东西向的负载均衡器 (分钟)，支持多层微服务架构。 它充当TCP层4负载平衡器，并利用Linux内核中的负载平衡功能来实现接近线速吞吐量和延迟。 其功能包括：

- 分布式应用程序负载平衡。
- 促进集群内的东西方交流。
- 用户指定DC / OS服务的FQDN地址。
- 尊重健康检查。
- 自动分配虚拟IP来服务FQDN。

您可以通过在您的应用定义中分配[ VIP ](/1.10/networking/load-balancing-vips/virtual-ip-addresses/)来使用第4层负载平衡器。 在使用VIP创建一个任务或一组任务后，它们将自动变为可用于群集中的所有节点（包括主设备）。

当您启动一组任务时，DC / OS将其分发到群集中的一组节点。 运行在每个集群代理上的Minuteman实例协调负载平衡决策。 每个agent上的Minuteman对Linux内核中的IPVS模块进行编程，其中包含与给定服务相关的所有任务的条目。 这允许Linux内核以接近线速的速度作出负载平衡的决定。 Minuteman跟踪这些任务的可用性和可达性，并使IPVS数据库保持与所有健康后端的最新状态，这意味着Linux内核可以为每个负载均衡的请求选择一个实时后端。

### 要求

- 不要防止节点之间的流量 (允许所有的TCP / UDP)。
- 不要更改` ip_local_port_range `。
- 您必须使用受支持的[操作系统](/1.10/installing/oss/custom/system-requirements/)。

#### 持续连接

保持长时间运行的持续连接, 否则, 可以快速填充 TCP 套接字表。 Linux 上的默认本地端口范围允许源连接从32768到61000。 这允许在给定的源 IP 和目标地址和端口对之间建立28232连接。 TCP连接必须经过等待状态才能被回收。 Linux内核的默认TCP时间等待时间是120秒。 如果没有持久连接，只需每秒创建235个新连接就可以耗尽连接表。

#### 持续连接

使用Mesos健康检查。 Mesos运行状况检查显示在负载平衡层上。 马拉松仅将**命令** [健康检查](/1.10/deploying-services/creating-services/health-checks/)转换为Mesos健康检查。 您可以通过类似于以下的命令模拟HTTP健康检查：

    bash
     test "$(curl -4 -w '%{http_code}' -s http://localhost:${PORT0}/|cut -f1 -d" ")" == 200

这确保返回的HTTP状态码是200。 它还假定您的应用程序绑定到本地主机。 ` ${PORT0} `被Marathon设置为一个变量。 不应使用 TCP 健康检查, 因为它们可能会提供有关服务活动的误导性信息。

** 重要: **泊坞窗容器命令健康检查在泊坞窗容器内运行。 例如, 如果使用卷毛来检查 NGINX, 则 NGINX 容器必须安装卷毛, 否则容器必须在 RW 模式下装入 `/选择/中间层 `。

## 排除故障

### DC / OS覆盖虚拟网络

如果您指定的 VIP 地址在网络的其他地方使用, 可能会出现问题。 尽管VIP是三元组，但最好确保专用于VIP的IP仅由负载平衡软件使用，并且在您的网络中根本没有使用。 因此, 您应该从 RFC1918 范围中选择 IPs。

### 端口

必须打开端口61420才能使负载平衡器正常工作。由于负载平衡器维护部分网格, 因此需要确保节点之间的连接不受阻碍。

## 下一步

- [为您的应用程序分配 VIP](/1.10/networking/load-balancing-vips/virtual-ip-addresses/)
- [了解有关实现的详细信息](https://github.com/dcos/minuteman)