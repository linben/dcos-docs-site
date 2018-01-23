---
layout: layout.pug
navigationTitle:  Frequently Asked Questions
title: Frequently Asked Questions
menuWeight: 203
excerpt:

enterprise: false
---

<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->


## 问：我可以在已经运行的Mesos集群上安装DC/OS吗？?
我们建议从全新的集群开始，确保将所有缺省值设置为预期值。这可以防止与不匹配的版本和配置有关的意外情况。

## 问：DC/OS的操作系统要求是什么？?
请参阅[系统要求](/1.10/installing/ent/custom/system-requirements/)。

## 问：DC/OS安装ZooKeeper，还是可以使用我自己的ZooKeeper法定人数？
DC/OS运行由Exhibitor和systemd监督的自己的ZooKeeper，但用户也可以创建自己的ZooKeeper定额。默认情况下安装的ZooKeeper法定人数将在`master.mesos:[2181|2888|3888]`。

## 问：群集创建后是否需要维护引导节点?
如果您在集群[配置文件](/1.10/installing/ent/custom/configuration/configuration-parameters/)中指定参展商存储后端类型（不包括`exhibitor_storage_backend：static`)，则必须在集群的整个生命周期中维护外部存储，以方便领导选举。如果你的集群是关键任务的，那么你应该通过使用S3或运行引导ZooKeeper作为法定数据来加强你的外部存储。来自外部存储器的服务中断是可以容忍的，但永久的状态丢失可能会导致意外情况。

## 问：如何将Mesos属性添加到节点以使用Marathon约束?

在DC / OS中，将`MESOS_ATTRIBUTES=<key>:<value>`行添加到您要添加的每个属性的文件`/var/lib/dcos/mesos-slave-common`(可能需要创建）。 更多信息可以通过[Mesos文档](http://mesos.apache.org/documentation/latest/attributes-resources/)找到。

## 问：我如何优雅地关闭代理？

- _要正常地杀死代理节点的Mesos进程并允许systemd重新启动它，请使用以下命令。 注意：如果正在使用Auto Scaling组，节点将被自动替换_：

    ```bash
    sudo systemctl kill -s SIGUSR1 dcos-mesos-slave
    ```
- _对于public agent：_

    ```bash
    sudo systemctl kill -s SIGUSR1 dcos-mesos-slave-public
    ```

- T要优雅地终止进程并防止systemd重新启动，请添加一个`停止`命令

    ```bash
    sudo systemctl kill -s SIGUSR1 dcos-mesos-slave && sudo systemctl stop dcos-mesos-slave
    ```

- _对于public agent:_

    ```bash
    sudo systemctl kill -s SIGUSR1 dcos-mesos-slave-public && sudo systemctl stop dcos-mesos-slave-public
    ```
