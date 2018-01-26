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

The [metrics component](/1.10/overview/architecture/components/#dcos-metrics) provides metrics from DC/OS cluster hosts, containers running on those hosts, and from applications running on DC/OS that send StatsD metrics to the Mesos Metrics Module. The metrics component is natively integrated with DC/OS and is available per-host from the `/system/v1/metrics/v0` HTTP API endpoint.

## 概述

DC/OS 提供了以下类型的度量:

* ** 主机: ** 有关作为 DC/OS 群集一部分的特定节点的度量。 
* ** 容器: ** 有关在 DC/OS [ 通用容器运行时 ](/1.10/deploying-services/containerizers/ucr/) 中运行的任务的 cgroup 分配的度量或[ 泊坞窗引擎 ](/1.10/deploying-services/containerizers/docker-containerizer/) 运行时。 
* ** 应用程序: ** 有关在通用容器运行时中运行的特定应用程序的度量。

[ 度量 api ](/1.10/metrics/metrics-api/) 公开这些区域。

所有三度量层都由作为 DC/OS 分布一部分的收集器进行聚合。 This enables metrics to run on every host in the cluster. It is the main entry point to the metrics ecosystem, aggregating metrics sent to it by the Mesos Metrics module, or gathering host and container level metrics on the machine on which it runs.

The Mesos Metrics module is bundled with every agent in the cluster. This module enables applications running on top of DC/OS to publish metrics to the collector by exposing StatsD host and port environment variables inside every container. These metrics are appended with structured data such as `agent-id`, `framework-id`, and `task-id`. DC/OS applications discover the endpoint via an environment variable (`STATSD_UDP_HOST` or `STATSD_UDP_PORT`). Applications leverage this StatsD interface to send custom profiling metrics to the system.

For more information on which metrics are collected, see the [Metrics Reference](/1.10/metrics/reference/).