---
layout: layout.pug
navigationTitle: 监视、日志记录和调试
title: 监视、日志记录和调试
menuWeight: 110
excerpt: ""
---
监视构成 DC/OS 的所有部件的运行状况对于数据中心操作员和诊断诊断错误非常重要。 您可以从 "DC/OS UI 组件健康" 页监视群集组件的运行状况。 "组件健康" 页显示系统运行状况 API 的信息, 它监视核心 DC/OS 组件。

DC/OS 组件是构成 DC/OS 核心的 [ systemd 单元 ](https://www.freedesktop.org/wiki/Software/systemd/)。 这些组件由我们的内部诊断实用程序 (` dcos-diagnostics.service`) 进行监视。 此实用程序扫描所有 DC/OS 单元, 然后在每个主机上公开一个 HTTP API。 有关 DC/OS 组件的完整说明, 请参见 [ 文档 ](/1.10/overview/architecture/components/)。

"组件健康" 页提供在 systemd 中运行的所有 DC/OS 系统组件的健康状态。您可以按健康状态、主机 IP 地址或特定的 systemd 单元进行向下钻取。

## 入门

启动 [ DC/OS UI ](/1.10/gui/) 并导航到 ** 系统->> 组件 ** 页面。可以按运行状况对组件进行排序。

![system health](https://docs.mesosphere.com/1.10/img/component-system-view.png)

您可以单击 DC/OS 组件来查看详细信息, 包括角色、节点和健康。

![node detail](https://docs.mesosphere.com/1.10/img/component-node-detail.png)

通过单击节点查看组件 journald (日志) 输出, 可以进一步调试。

![log](https://docs.mesosphere.com/1.10/img/component-node-output.png)

## 健康状态

可能的健康状况是不健康和健康的。 我们从代码0和1推断这一点。

* **Healthy** 所有群集节点都健康。 这些单元被加载，不处于“活动”或“不活动”状态。
* **Unhealthy**一个或多个节点有问题。 这些单元未加载或处于“活动”或“非活动”状态。

系统健康API有四种可能的状态：0 - 3，OK; CRITICAL; WARNING; UNKNOWN。 未来的DC / OS迭代将利用这些代码在UI中提供更健壮，更详细的集群运行状况信息。

## 系统健康 HTTP API 端点

系统健康端点通过主节点上的 DC/OS 诊断实用程序公开:

```bash
curl --unix-socket /run/dcos/dcos-diagnostics.sock http://localhost/system/health/v1
```

## 聚合模式

群集健康端点的聚合由主节点上的同一诊断应用程序完成。 通过对群集中的任何母版进行一些查询, 可以进一步探索此 API:

1. SSH 到您的master node:
    
    ```bash
dcos node ssh --master-proxy --leader
```

2. 运行此命令以打开根会话:
    
    ```bash
sudo su -
```

3. 运行这些命令以获得群集健康:
    
    * 系统健康按单位:
        
        ```bash
curl --unix-socket /run/dcos/dcos-diagnostics.sock http://localhost/system/health/v1/units
```

* 节点的系统健康状况：
    
    ```bash
curl --unix-socket /run/dcos/dcos-diagnostics.sock http://localhost/system/health/v1/nodes
```

* 系统运行状况报告:
    
    ```bash
curl --unix-socket /run/dcos/dcos-diagnostics.sock http://localhost/system/health/v1/report
```

DC/OS 用户界面使用这些聚合端点来生成您在系统运行状况控制台中所浏览的数据。

## 已知问题

### 按单位曲解系统健康

您可以按 systemd 单位对系统运行状况进行排序。 但是, 此搜索可能会带来误导信息, 因为服务本身可以是健康的, 但它运行的节点不是。 这体现为一个服务, 显示 "健康", 但与该服务相关的节点 "不健康"。 有些人发现这种行为令人迷惑。

### 缺少群集主机

系统健康 API 依赖于 Mesos-DNS 来了解所有的群集主机。 它通过将 ` mesos.master ` 中的查询以及 `leader.mesos:5050/slaves` 来获取群集中的主机的完整列表, 从而找到这些主机。

此系统有一个已知的 bug, 其中的agent不会显示在从 ` leader.mesos:5050/slaves ` 如果 mesos 代理服务不正常。 这意味着系统运行状况 API 不会显示此主机。

如果您遇到此行为, 则最有可能是您的 Mesos 代理服务在缺少的主机上是不健康的。

## 排除故障

如果您有任何问题, 可以检查诊断服务是否通过 SSH "ing" Mesos 的主主机运行, 并检查诊断组件的 systemd 状态 (` dcos-d3t.service `)。
