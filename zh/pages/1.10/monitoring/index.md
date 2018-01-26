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

![system health](/1.10/img/component-system-view.png)

您可以单击 DC/OS 组件来查看详细信息, 包括角色、节点和健康。

![node detail](/1.10/img/component-node-detail.png)

通过单击节点查看组件 journald (日志) 输出, 可以进一步调试。

![log](/1.10/img/component-node-output.png)

## 健康状态

Possible health states are unhealthy and healthy. We infer this from codes 0 and 1.

* **Healthy** All cluster nodes are healthy. The units are loaded and not in the "active" or "inactive" state.
* **Unhealthy** One or more nodes have issues. The units are not loaded or are in the "active" or "inactive" state.

The system health API has four possible states: 0 - 3, OK; CRITICAL; WARNING; UNKNOWN. Future DC/OS iterations will leverage these codes to give more robust and detailed cluster health state information in the UI.

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
    
    * System health by unit:
        
        ```bash
curl --unix-socket /run/dcos/dcos-diagnostics.sock http://localhost/system/health/v1/units
```

* System health by node:
    
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

The system health API relies on Mesos-DNS to know about all the cluster hosts. It finds these hosts by combining a query from `mesos.master` A records as well as `leader.mesos:5050/slaves` to get the complete list of hosts in the cluster.

This system has a known bug where an agent will not show up in the list returned from `leader.mesos:5050/slaves` if the Mesos agent service is not healthy. This means the system health API will not show this host.

If you experience this behavior it's most likely your Mesos agent service on the missing host is unhealthy.

## 排除故障

If you have any problems, you can check if the diagnostics service is running by SSH’ing to the Mesos leading master and checking the systemd status of the diagnostics component (`dcos-d3t.service`).