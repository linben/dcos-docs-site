---
layout: layout.pug
navigationTitle:  Distributed Process Management
title: Distributed Process Management
menuWeight: 5
excerpt:

enterprise: false
---

<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->


本节介绍从资源分配到执行进程的DC/OS集群中进程管理。

在高层次上，当用户启动进程时，这种交互发生在DC/OS组件之间。通信发生在不同的层之间，例如用户与模拟器进行交互，以及同一层内，例如主节点与代理节点间通讯。

![Concept of distributed process management in DC/OS](/1.10/img/dcos-architecture-distributed-process-management-concept.png)

以下例子，用户使用Marathon服务启动一个基于Docker引擎的容器：

![Example of distributed process management in DC/OS](/1.10/img/dcos-architecture-distributed-process-management-example.png)

上述组件之间的按时间顺序的交互看起来像这样。 请注意执行者和任务被合并为一个块，因为在实践中往往是这样的：

![Sequence diagram for distributed process management in DC/OS](/1.10/img/dcos-architecture-distributed-process-management-seq-diagram.png)

详细来说，包括以下步骤：

<table class="table">
<thead>
<tr>
<th>步骤</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>客户端/调度器开始：客户端需要知道如何与调度器通讯，启动一个进程，例如通过Mesos-DNS或是DC/OS命令行。</td>
</tr>
<tr>
<td>2</td>
<td>Mesos主节点发送资源邀约给调度器：资源邀约是基于所有节点上的集群资源，并通过在Mesos主节点上使用<a href="https://www.cs.berkeley.edu/~alig/papers/drf.pdf">DRF</a>算法计算得到.</td>
</tr>
<tr>
<td>3</td>
<td>由于没有来自客户端的进程请求在等待，调度器会拒绝资源邀约。如果客户端没有初始化进程，调度器会拒绝来自主节点的资源邀约。</td>
</tr>
<tr>
<td>4</td>
<td>客户端初始化进程。例如，用户通过DC/OS<a href="/1.10/gui/">Services</a> 标签或是HTTP路径<code>/v2/app</code>创建了一个Marathon应用.</td>
</tr>
<tr>
<td>5</td>
<td>Mesos主节点提供资源邀约。例如, <code>cpus(*):1; mem(*):128; ports(*):[21452-21452]</code></td>
</tr>
<tr>
<td>6</td>
<td>如果资源邀约符合进程调度器的要求，它就会接受资源邀约，并发送<code>launchTask</code>请求给Mesos主节点.</td>
</tr>
<tr>
<td>7</td>
<td>Mesos主节点指导Mesos代理节点启动任务.</td>
</tr>
<tr>
<td>8</td>
<td>Mesos代理节点透过执行器启动任务.</td>
</tr>
<tr>
<td>9</td>
<td>执行器将任务状态返回给Mesos代理节点.</td>
</tr>
<tr>
<td>10</td>
<td>Mesos代理节点将任务状态返回给Mesos主节点.</td>
</tr>
<tr>
<td>11</td>
<td>Mesos主节点将任务状态返回给调度器.</td>
</tr>
<tr>
<td>12</td>
<td>调度器将进程状态返回给客户端.</td>
</tr>
</tbody>
</table>

[auth]: /1.10/security/ent/
[components]: /1.10/overview/architecture/components/
