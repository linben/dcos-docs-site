---
layout: layout.pug
navigationTitle: 代理路由
title: 代理路由
menuWeight: 11
excerpt: ""
enterprise: false
---
Admin Router Agent 在DC / OS代理程序节点上运行并公开以下API路由。

Admin Router Agent 侦听端口 ` 61001 ` (HTTP)。

DC/OS 企业添加组件通信的可选 SSL 加密。 因此, 在 ` 严格 ` 和 ` 许可 ` 安全模式下, 管理路由器代理还监听端口 ` 61002 ` (HTTPS)。

有关 api 路由的工作方式的详细信息, 请参阅 [ DC/OS api 参考 ](/1.10/api/)。

  


[ngindox api='/1.10/api/nginx.agent.yaml']