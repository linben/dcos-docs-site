---
layout: layout.pug
navigationTitle: 常见问题解答
title: 常见问题解答
menuWeight: 203
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

## Q. 我是否可以在已经运行的 Mesos 群集上安装 DC/OS？

我们建议从一个新的集群开始, 以确保所有的默认值都设置为期望值。这可防止与不匹配的版本和配置有关的意外情况。

## Q. DC/OS 的 OS 要求是什么？

请参见 [ 系统要求 ](/1.10/installing/ent/custom/system-requirements/)。

## Q. DC/OS 安装管理员, 或者我可以使用我自己的动物园管理员的法定人数？

DC/OS 运行自己的动物园管理员, 由参展商和 systemd, 但用户可以创建自己的动物园管理员仲裁, 以及。 默认情况下安装的管理员程序仲裁将在 ` master mesos: [2181 | 2888 | 3888] `。

## Q. 在创建群集后是否需要维护引导节点？

如果您在群集配置 [ 文件 ](/1.10/installing/ent/custom/configuration/configuration-parameters/) 中指定了非 ` exhibitor_storage_backend: 静态 ` 的展商存储后端类型, 则必须维护群集的生存期的外部存储区, 以方便领导选举。 如果你的集群是关键任务的，那么你应该通过使用S3或运行引导ZooKeeper作为法定数据来加强你的外部存储。 来自外部存储器的服务中断是可以容忍的，但永久的状态丢失可能会导致意外情况。

## Q. 如何将Mesos属性添加到节点以使用Marathon约束？

在DC / OS中，向文件`/var/lib/dcos/mesos-slave-common`中添加`MESOS_ATTRIBUTES=<key>:<value>`为每个你想添加的属性。 有关更多信息，请参阅[通过Mesos文档](http://mesos.apache.org/documentation/latest/attributes-resources/)。

## Q. 我如何优雅地关闭代理？

- *要优雅地终止代理节点的Mesos进程并允许systemd重新启动它，请使用以下命令。 _注意：如果Auto Scaling组正在使用中，节点将被自动替换*：
    
    ```bash
sudo systemctl kill -s SIGUSR1 dcos-mesos-slave
```

- *对于public agent:*
    
    ```bash
sudo systemctl kill -s SIGUSR1 dcos-mesos-slave-public
```

- 要正常地终止进程并防止systemd重新启动，请添加一个` stop `命令：
    
    ```bash
sudo systemctl kill -s SIGUSR1 dcos-mesos-slave && sudo systemctl stop dcos-mesos-slave
```

- *对于public agent:*
    
    ```bash
sudo systemctl kill -s SIGUSR1 dcos-mesos-slave-public && sudo systemctl stop dcos-mesos-slave-public
```