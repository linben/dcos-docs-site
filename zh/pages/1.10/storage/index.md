---
layout: layout.pug
navigationTitle: 存储
title: 存储
menuWeight: 90
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

DC/OS 应用程序在终止和重新推出时会失去它们的状态。 In some contexts, for instance, if your application uses MySQL, or if you are using a stateful service like Kafka or Cassandra, you'll want your application to preserve its state.

Configure Mesos [`Mount` disk resources](/1.10/storage/mount-disk-resources/) to enable tasks that can be restarted without data loss.

Learn how to create stateful applications using [local persistent volumes](/1.10/storage/persistent-volume/) and [external persistent volumes](/1.10/storage/external-storage/).