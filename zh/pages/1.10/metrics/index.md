---
layout: layout.pug
navigationTitle: 计量单位
title: 计量单位
menuWeight: 100
excerpt: ""
preview: true
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

[指标组件](/1.10/overview/architecture/components/#dcos-metrics)提供来自DC / OS群集主机，在这些主机上运行的容器以及在DC / OS上运行的将StatsD指标发送到Mesos指标模块的应用程序的指标。 度量值组件与 DC/OS 本地集成, 并可从 `/system/v1/metrics/v0 ` HTTP API 端点使用每主机。

## 概述

DC/OS 提供了以下类型的度量:

* ** 主机: ** 有关作为 DC/OS 群集一部分的特定节点的度量。 
* ** 容器: ** 有关在 DC/OS [ 通用容器运行时 ](/1.10/deploying-services/containerizers/ucr/) 中运行的任务的 cgroup 分配的度量或[ 泊坞窗引擎 ](/1.10/deploying-services/containerizers/docker-containerizer/) 运行时。 
* ** 应用程序: ** 有关在通用容器运行时中运行的特定应用程序的度量。

[ 度量 api ](/1.10/metrics/metrics-api/) 公开这些区域。

所有三度量层都由作为 DC/OS 分布一部分的收集器进行聚合。 这使度量可以在群集中的每个主机上运行。 它是度量生态系统的主要入口点, 它通过 Mesos 度量模块发送到它的聚合度量, 或者在运行它的机器上收集主机和容器级别的度量。

Mesos 度量模块与群集中的每个代理捆绑在一起。 通过在每个容器中公开 StatsD 主机和端口环境变量, 此模块使运行在 DC/OS 顶部的应用程序能够向收集器发布指标。 这些度量值附加有结构化数据, 如 ` agent-id `、` framework-id ` 和 ` task-id `。 DC/OS 应用程序通过环境变量 (` STATSD_UDP_HOST ` 或 ` STATSD_UDP_PORT `) 发现端点。 应用程序利用StatsD界面向系统发送自定义分析指标。

有关收集哪些度量标准的详细信息, 请参阅 [ 度量标准参考 ](/1.10/metrics/reference/)。