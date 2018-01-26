---
layout: layout.pug
navigationTitle: 更新节点
title: 更新节点
menuWeight: 801
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

通过使用维护窗口或手动杀死代理, 可以更新活动的 DC/OS 群集中的代理节点。 维护窗口是首选的方法, 因为它通常更稳定, 容易出错。

如果您正在缩小群集、重新配置代理节点或将节点移动到新的 IP, 则这些步骤非常有用。 当您更改 Mesos 属性 (`/var/lib/dcos/mesos-slave-common `) 或资源 (`/var/lib/dcos/Mesos-resources `) 时, 必须删除代理节点, 然后在新 UUID 下使用主节点重新注册它。 然后, master 将识别新的属性和资源规范。

** 警告: ** 在代理上运行的所有任务都将被杀死, 因为您正在更改代理属性或资源。Mesos 将重新注册的代理视为新代理。

### 基础要求

* [ SSH 已安装并已配置 ](/1.10/administering-clusters/sshcluster/)。当通过手动杀死代理删除节点时, 这是必需的。
* Access to the [Admin Router permissions](/1.10/overview/architecture/components/#admin-router).

# 使用维护窗口更新节点

使用维护窗口, 您可以同时从群集外部排出多个节点。不需要 SSH 访问。

您可以定义一个维护计划, 以便在更改代理属性或资源之前疏散您的任务。

1. 定义维护计划。 例如, 下面是一个基本的维护计划 JSON 文件, 其中指定了示例计算机 (` machine_ids `) 和维护窗口 (` 不可用 `):
    
    ```json
{
  "windows" : [
    {
      "machine_ids" : [
        { "hostname" : "10.0.2.107", "ip" : "10.0.2.107" },
        { "hostname" : "10.0.2.5", "ip" : "10.0.2.5" }
      ],
      "unavailability" : {
        "start" : { "nanoseconds" : 1 },
        "duration" : { "nanoseconds" : 3600000000000 }
      }
    }
  ]
}
```

For a more complex example, see the [maintain-agents.sh](https://github.com/vishnu2kmohan/dcos-toolbox/blob/master/mesos/maintain-agents.sh) script.

2. Invoke the `⁠⁠⁠⁠machine/down` endpoint with the machine JSON definition specified. For example, [here](https://github.com/vishnu2kmohan/dcos-toolbox/blob/master/mesos/down-agents.sh) is a script that calls `/machine/down/`.
    
    **Important:** Invoking `machine/down` sends a `⁠⁠⁠⁠TASK_LOST`⁠⁠⁠⁠ message for any tasks that were running on the agent. Some DC/OS services, for example Marathon, will relocate tasks, but others will not, for example Kafka and Cassandra. For more information, see the DC/OS [service guides](/services/) and the Mesos maintenance primitives [documentation](https://mesos.apache.org/documentation/latest/maintenance/).

3. Perform your maintenance.

4. Add the nodes back to your cluster by invoking the `⁠⁠⁠⁠machine/up` endpoint with the add agents JSON definition specified. For example:
    
    ```json
[
  { "hostname" : "10.0.2.107", "ip" : "10.0.2.107" },
  { "hostname" : "10.0.2.5", "ip" : "10.0.2.5" }
]
```

# Updating nodes by manually killing agents

Draining nodes by using terminate signal, SIGUSR1, is easy to integrate with automation tools that can execute tasks on nodes in parallel, for example Ansible, Chef, and Puppet.

1. [ SSH ](/1.10/administering-clusters/sshcluster/) 配置.
2. Stop the agents.
    
    * **Private agent**
        
        ```bash
sudo sh -c 'systemctl kill -s SIGUSR1 dcos-mesos-slave && systemctl stop dcos-mesos-slave'
```

* **Public agent**
    
    ```bash
⁠⁠⁠⁠sudo sh -c 'systemctl kill -s SIGUSR1 dcos-mesos-slave-public && systemctl stop dcos-mesos-slave-public'
```

3. Perform your maintenance.

4. Add the nodes back to your cluster.
    
    1. 重新加载了systemd配置。
        
        ```bash
﻿⁠⁠sudo systemctl daemon-reload
```

2. Remove the `latest` metadata pointer on the agent node:
    
    ```bash
⁠⁠⁠⁠sudo rm /var/lib/mesos/slave/meta/slaves/latest
```

3. Start your agents with the newly configured attributes and resource specification⁠⁠.
    
    * **Private agent**
        
        ```bash
sudo systemctl start dcos-mesos-slave
```

* **Public agent**
    
    ```bash
sudo systemctl start dcos-mesos-slave-public
```

**Tip:** You can check the status with this command:

```bash
sudo systemctl status dcos-mesos-slave
```