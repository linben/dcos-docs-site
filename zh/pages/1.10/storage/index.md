---
layout: layout.pug
navigationTitle: 存储
title: 存储
menuWeight: 90
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

DC/OS 应用程序在终止和重新推出时会失去它们的状态。 在某些情况下，例如，如果您的应用程序使用MySQL，或者您正在使用像Kafka或Cassandra这样的有状态服务，那么您将希望应用程序保持其状态。

配置 Mesos [ ` 装入 ` 磁盘资源 ](/1.10/storage/mount-disk-resources/), 以启用可以在不丢失数据的情况下重新启动的任务。

了解如何使用 [ 本地持久卷 ](/1.10/storage/persistent-volume/) 和 [ 外部持久卷 ](/1.10/storage/external-storage/) 创建有状态的应用程序。