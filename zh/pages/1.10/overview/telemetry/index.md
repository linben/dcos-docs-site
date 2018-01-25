---
layout: layout.pug
navigationTitle: Telemetry
title: Telemetry
menuWeight: 7
excerpt: ""

enterprise: false
---

为了不断提升DC/OS的体验，遥测组件被包括在内，向Mesosphere反馈匿名使用数据。这些数据用于监视核心DC/OS组件，安装，用户界面的可靠性，并找出哪些功能最受欢迎。

- [Core telemetry](#core)
- [User interface telemetry](#user-interface)

# <a name="core"></a>核心遥测

[DC/OS Signal](/1.10/overview/architecture/components/#dcos-signal) D组件查询主节点上的诊断服务端点`/system/health/v1/report`, 并将这些数据发给[Segment](https://segment.com/docs/) ，Mesosphere使用这些数据跟踪使用状况并提供客户支持.

DC/OS信息服务的信息来自于很多组件：DC/OS诊断服务，Apache Mesos，和DC/OS包管理器(Cosmos).

对于每个类别收集这些数据：

<table class="table">
<tr>
<th>类型</th>
<th>描述</th>
</tr>
<tr>
<td>
  anonymousId
</td>
<td>
  每个群集启动时会创建此匿名ID。此ID在您的群集中持续存在。例如：
<pre>
"anonymousId": "70b28f00-e38f-41b2-a723-aab344f535b9
</pre>    </td>
</tr>
<tr>
<td>
  clusterId
</td>
<td>
  每个群集启动时会创建此<code>匿名ID</code>。此ID在您的群集中持续存在。例如：
<pre>
"clusterId": "70b28f00-e38f-41b2-a723-aab344f535b9"
</pre>
</td>
</tr>
<tr>
<td>
  customerKey (DC/OS Enterprise)
</td>
<td>
  DC/OS企业版客户码。此客户码通过邮件发送给授权用户。例如：
<pre>
"customerKey": "ab1c23de-45f6-7g8h-9012-i345j6k7lm8n",
</pre>
</td>
</tr>
<tr>
<td>
  event
</td>
<td>
  此类别的值出现在段落中。可能的值包括<code>package_list</code> (包管理器), <code>health</code> (诊断), and <code>mesos_track</code> (Mesos). 例如:
<pre>
"event": "package_list"
</pre>
</td>
</tr>
<tr>
<td>
  environmentVersion
</td>
<td>
  这是DC/OS版本。例如，如果使用的是DC/OS 1.8 8:
<pre>
"environmentVersion": "1.8",
</pre>    </td>
</tr>
<tr>
<td>
  provider
</td>
<td>
  DC/OS运行的平台。可能的值包括<code>aws</code>, <code>on-prem</code>, 和<code>azure</code>. 例如，在AWS上运行:
<pre>
"provider": "aws",
</pre>    </td>
</tr>
<tr>
<td>
  source
</td>
<td>
  这是表示一个集群的硬编码设置。例如:
<pre>
"source": "cluster",
</pre>    </td>
</tr>
<tr>
<td>
  variant
</td>
<td>
  是否是DC/OS开源版或是DC/OS企业版。例如，如果运行的是DC/OS开源版:
<pre>
"variant": "open"
</pre>
    </td>
  </tr>
</table>

## 诊断

这些信息从[DC/OS Diagnostics](/1.10/overview/architecture/components/#dcos-diagnostics) 组件收集。对于每个systemd单元，以下信息会被收集，`<UNIT_NAME>` 是组件名称:

    "health-unit-dcos-<UNIT_NAME>-total": 3, "health-unit-dcos-<UNIT_NAME>-unhealthy": 0,
    

## Mesos

这些信息从[Apache Mesos](/1.10/overview/architecture/components/#apache-mesos) 组件收集.

<table class="table">
  <tr>
<th>类型</th>
<th>描述</th>
  </tr>
<tr><td>agents_active</td><td>在线的代理节点数。例如： <pre>"agents_active": 2,</pre></td></tr>
<tr><td>agents_connected</td><td>连接的节点数。例如：<pre>"agents_connected": 2,</pre></td></tr>
<tr><td>cpu_total</td><td>可用的CPU数。例如： <pre>"cpu_total": 8,</pre></td></tr>
<tr><td>cpu_used</td><td>分配的CPU数。例如： <pre>"cpu_used": 0,</pre></td></tr>
<tr><td>disk_total</td><td>可用的磁盘空间，单位MB。例如：<pre>"disk_total": 71154,</pre></td></tr>
<tr><td>disk_used</td><td>分配的磁盘空间，单位MB。例如： <pre>"disk_used": 0,</pre></td></tr>
<tr><td>framework_count</td><td>安装的DC/OS服务数。例如： <pre>"framework_count": 2,</pre></td></tr>
<tr>
<td>
  frameworks
</td>
<td>
  那些DC/OS服务已安装。例如:
<pre>
"frameworks": [
                {
                    "name": "marathon"
                },
                {
                    "name": "metronome"
                }
            ],
</pre>    </td>
</tr>
<tr><td>mem_total</td><td>可用内存，单位MB. 例如: <pre>"mem_total": 28036,</pre></td></tr>
<tr><td>mem_used</td><td>分配的内存，单位MB. 例如: <pre>"mem_used": 0,</pre></td></tr>
<tr><td>task_count</td><td>磁盘数. 例如: <pre>"task_count": 0,</pre></td></tr>
</tr>
</table>

## 包管理器

以下信息会从[DC/OS Package Manager (Cosmos)](/1.10/overview/architecture/components/#dcos-package-manager) 组件收集.

<table class="table">
  <tr>
    
<th>类型</th>
<th>描述</th>
</tr>
<tr>
<td>package_list</td>
<td>
那些包已安装. 例如, 已安装Kafka和Spark: 

<pre>"package_list": [
{
"name": "kafka"
},
{
"name": "spark"
}
],
</pre>
</td>
</tr>
</table>

以下是一个遥测收集到的JSON反馈范例: 

```json
{
    "cosmos": {
        "properties": {
            "clusterId": "70b28f00-e38f-41b2-a723-aab344f535b9",
            "customerKey": "",
            "environmentVersion": "1.8",
            “package_list”: [
            {
            “name”: “kafka”
            },
            {
            “name”: “spark”
            }
            ],
            "provider": "aws",
            "source": "cluster",
            "variant": "open"
        },
        "anonymousId": "70b28f00-e38f-41b2-a723-aab344f535b9",
        "event": "package_list"
    },
    "diagnostics": {
        "properties": {
            "clusterId": "70b28f00-e38f-41b2-a723-aab344f535b9",
            "customerKey": "",
            "environmentVersion": "1.8",
            "health-unit-dcos-diagnostics-service-total": 3,
            "health-unit-dcos-diagnostics-service-unhealthy": 0,
            "health-unit-dcos-diagnostics-socket-total": 2,
            "health-unit-dcos-diagnostics-socket-unhealthy": 0,
            "health-unit-dcos-adminrouter-agent-service-total": 2,
            "health-unit-dcos-adminrouter-agent-service-unhealthy": 0,
            "health-unit-dcos-adminrouter-reload-service-total": 3,
            "health-unit-dcos-adminrouter-reload-service-unhealthy": 0,
            "health-unit-dcos-adminrouter-reload-timer-total": 3,
            "health-unit-dcos-adminrouter-reload-timer-unhealthy": 0,
            "health-unit-dcos-adminrouter-service-total": 1,
            "health-unit-dcos-adminrouter-service-unhealthy": 0,
            "health-unit-dcos-cosmos-service-total": 1,
            "health-unit-dcos-cosmos-service-unhealthy": 0,
            "health-unit-dcos-epmd-service-total": 3,
            "health-unit-dcos-epmd-service-unhealthy": 0,
            "health-unit-dcos-exhibitor-service-total": 1,
            "health-unit-dcos-exhibitor-service-unhealthy": 0,
            "health-unit-dcos-gen-resolvconf-service-total": 3,
            "health-unit-dcos-gen-resolvconf-service-unhealthy": 0,
            "health-unit-dcos-gen-resolvconf-timer-total": 3,
            "health-unit-dcos-gen-resolvconf-timer-unhealthy": 0,
            "health-unit-dcos-history-service-total": 1,
            "health-unit-dcos-history-service-unhealthy": 0,
            "health-unit-dcos-logrotate-agent-service-total": 2,
            "health-unit-dcos-logrotate-agent-service-unhealthy": 0,
            "health-unit-dcos-logrotate-agent-timer-total": 2,
            "health-unit-dcos-logrotate-agent-timer-unhealthy": 0,
            "health-unit-dcos-logrotate-master-service-total": 1,
            "health-unit-dcos-logrotate-master-service-unhealthy": 0,
            "health-unit-dcos-logrotate-master-timer-total": 1,
            "health-unit-dcos-logrotate-master-timer-unhealthy": 0,
            "health-unit-dcos-marathon-service-total": 1,
            "health-unit-dcos-marathon-service-unhealthy": 0,
            "health-unit-dcos-mesos-dns-service-total": 1,
            "health-unit-dcos-mesos-dns-service-unhealthy": 0,
            "health-unit-dcos-mesos-master-service-total": 1,
            "health-unit-dcos-mesos-master-service-unhealthy": 0,
            "health-unit-dcos-mesos-slave-public-service-total": 1,
            "health-unit-dcos-mesos-slave-public-service-unhealthy": 0,
            "health-unit-dcos-mesos-slave-service-total": 1,
            "health-unit-dcos-mesos-slave-service-unhealthy": 0,
            "health-unit-dcos-metronome-service-total": 1,
            "health-unit-dcos-metronome-service-unhealthy": 0,
            "health-unit-dcos-navstar-service-total": 3,
            "health-unit-dcos-navstar-service-unhealthy": 0,
            "health-unit-dcos-oauth-service-total": 1,
            "health-unit-dcos-oauth-service-unhealthy": 0,
            "health-unit-dcos-pkgpanda-api-service-total": 3,
            "health-unit-dcos-pkgpanda-api-service-unhealthy": 0,
            "health-unit-dcos-pkgpanda-api-socket-total": 3,
            "health-unit-dcos-pkgpanda-api-socket-unhealthy": 0,
            "health-unit-dcos-rexray-service-total": 2,
            "health-unit-dcos-rexray-service-unhealthy": 0,
            "health-unit-dcos-signal-service-total": 1,
            "health-unit-dcos-signal-service-unhealthy": 0,
            "health-unit-dcos-signal-timer-total": 3,
            "health-unit-dcos-signal-timer-unhealthy": 0,
            "health-unit-dcos-spartan-service-total": 3,
            "health-unit-dcos-spartan-service-unhealthy": 0,
            "health-unit-dcos-spartan-watchdog-service-total": 3,
            "health-unit-dcos-spartan-watchdog-service-unhealthy": 0,
            "health-unit-dcos-spartan-watchdog-timer-total": 3,
            "health-unit-dcos-spartan-watchdog-timer-unhealthy": 0,
            "health-unit-dcos-vol-discovery-priv-agent-service-total": 1,
            "health-unit-dcos-vol-discovery-priv-agent-service-unhealthy": 0,
            "health-unit-dcos-vol-discovery-pub-agent-service-total": 1,
            "health-unit-dcos-vol-discovery-pub-agent-service-unhealthy": 0,
            "provider": "aws",
            "source": "cluster",
            "variant": "open"
        },
        "anonymousId": "70b28f00-e38f-41b2-a723-aab344f535b9",
        "event": "health"
    },
    "mesos": {
        "properties": {
            "agents_active": 2,
            "agents_connected": 2,
            "clusterId": "70b28f00-e38f-41b2-a723-aab344f535b9",
            "cpu_total": 8,
            "cpu_used": 0,
            "customerKey": "",
            "disk_total": 71154,
            "disk_used": 0,
            "environmentVersion": "1.8",
            "framework_count": 2,
            "frameworks": [
                {
                    "name": "marathon"
                },
                {
                    "name": "metronome"
                }
            ],
            "mem_total": 28036,
            "mem_used": 0,
            "provider": "aws",
            "source": "cluster",
            "task_count": 0,
            "variant": "open"
        },
        "anonymousId": "70b28f00-e38f-41b2-a723-aab344f535b9",
        "event": "mesos_track"
    }
}
```

# <a name="user-interface"></a>用户界面遥测

DC/OS用户界面发送以下2类信息到[Segment](https://segment.com/docs/)， Mesosphere会使用这些数据统计使用状况并提供用户支持:

- 登录信息
- 通过用户界面浏览过的页面

## 选择关闭

您可以选择关闭遥测功能。更多信息，请参阅[documentation](/1.10/installing/oss/opt-out/).
