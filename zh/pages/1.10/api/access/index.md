---
layout: layout.pug
navigationTitle: 族类排料
title: 族类排料
menuWeight: 1
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

可以使用以下方法获取群集 URL:

- 登录到 DC/OS GUI, 并从浏览器地址栏复制方案和域名。
- 登录到 DC/OS CLI, 并键入 ` dcos 配置显示核心. dcos_url ` 以获取群集 url。

# API 端口

在主节点上, 可以通过标准端口访问管理路由器: ` 80 ` (HTTP) 和 ` 443 ` (如果启用了 HTTPS)。

在代理节点上, 可通过端口 ` 61001 ` (HTTP) 访问管理路由器代理。

# 代理节点访问

通过使用以下方法, 可以找到特定代理节点的主机名:

- 登录到 DC/OS GUI, 导航到节点页, 并复制所需节点的主机名。
- 登录到DC / OS CLI，使用` dcos node `列出节点，并复制所需节点的主机名。

要确定哪些代理是公共代理, 请参阅 [ 查找公共代理 IP ](/1.10/administering-clusters/locate-public-agent/)。

# 入口

在大多数生产部署中，对群集的管理访问应通过外部代理路由到DC / OS主节点，以在主节点之间分配流量负载。 例如，默认的AWS模板配置AWS Elastic Load Balancer。

主节点和私有代理节点通常不能公开访问。 出于安全原因, 对这些节点的侵入应由路由器或防火墙控制。 为了管理集群，管理员和操作员应该在防火墙内部使用与DC / OS节点相同的网络中的VPN服务器。 使用VPN可确保您可以直接从工作站安全地访问节点。

公共代理节点通常是可公开访问的。 在公共代理节点上运行的[ Marathon-LB ](/service-docs/marathon-lb/)可以作为私有代理节点上运行的应用程序的反向代理和负载平衡器。 为了获得更高的安全性，可以使用外部负载平衡（中间负载平衡器，公共节点上的应用程序或直接应用到私有节点上的应用程序）。 为了获得更高的安全性，可以使用外部负载平衡（中间负载平衡器，公共节点上的应用程序或直接应用到私有节点上的应用程序）。...

如果要允许公共访问公共节点，则应配置防火墙以阻止除应用程序所需的所有端口的访问。...

有关更多信息，请参阅[保护群集](/1.10/administering-clusters/)。