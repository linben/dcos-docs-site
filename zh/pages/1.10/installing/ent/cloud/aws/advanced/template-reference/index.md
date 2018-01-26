---
layout: layout.pug
navigationTitle: 模板参考
title: 模板参考
menuWeight: 200
excerpt: ""
enterprise: true
---
这些高级模板参数在各个 JSON 文件中指定。在 dc/os 安装期间, 这些模板文件用于生成自定义的 dc/os 版本。

### 禅宗模板

The [Zen](#zen) templates orchestrate the individual component templates to create a DC/OS cluster.

### 禅宗模板

The [agent](#private-agent) templates create [public](/1.10/overview/concepts/#public-agent-node) or [private](/1.10/overview/concepts/#private-agent-node) agent nodes that are then attached to a DC/OS cluster as a part of an AutoScalingGroup.

### 主模板

The [master](#master) templates create master nodes, on top of the infrastructure stack already created.

### 基础结构模板

The [infrastructure](#infrastructure) template defines and creates a DC/OS specific infrastructure that works well with an existing VPC.

## <a name="zen"></a>Zen

禅宗模板 (如 ` ee.elzen-1.json `) 协调各个组件模板。

<table class="table">
  <tr>
    <th>参数名称：</th>
    <th>数据值</th>
  </tr>
  <tr>
    <td>AdminLocation</td>
    <td>可选: 指定用于访问管理区域的白名单的 IP 范围。必须是有效的 CIDR。要允许从任何 IP 地址访问, 请使用 <code> 0.0.0.0/0 </code>。</td>
  </tr>
  <tr>
    <td>CustomAMI</td>
    <td>可选: 指定 AMI ID。必须驻留在同一区域中, 并且安装了所有的 DC/OS 系统必备组件。</td>
  </tr>
  <tr>
    <td>InternetGateway</td>
    <td>Internet 网关 ID, 必须附加到 <code> Vpc </code>。由所有节点用于传出的 Internet 访问。</td>
  </tr>
  <tr>
    <td>KeyName</td>
    <td>AWS EC2 密钥对到 SSH 到实例</td>
  </tr>
  <tr>
    <td>MasterInstanceType</td>
    <td>Region-specific instance type. E.g. m3.xlarge</td>
  </tr>
  <tr>
    <td>PrivateAgentInstanceCount</td>
    <td>Specify the number of private agent nodes or accept the default.</td>
  </tr>
  <tr>
    <td>PrivateAgentInstanceType</td>
    <td>Region-specific instance type. E.g. m3.xlarge</td>
  </tr>
  <tr>
    <td>PrivateSubnet</td>
    <td>Subnet ID for use by all private agent nodes</td>
  </tr>
  <tr>
    <td>PublicAgentInstanceCount</td>
    <td>Specify the number of public agent nodes or accept the default.</td>
  </tr>
  <tr>
    <td>PublicAgentInstanceType</td>
    <td>Region-specific instance type. E.g. m3.xlarge</td>
  </tr>
  <tr>
    <td>PublicSubnet</td>
    <td>Subnet ID for use by all public agent nodes</td>
  </tr>
  <tr>
    <td>Vpc</td>
    <td>Existing VPC to use. Nodes will be launched using subnets and Internet Gateway under this VPC</td>
  </tr>
</table>

## <a name="private-agent"></a>公共代理节点

The private agent template (`advanced-priv-agent.json`) creates agents which are then attached to a DC/OS cluster as a part of an AutoScalingGroup. To configure the template, specify the VPC, subnet, and master DNS address for the DC/OS cluster to join.

<table class="table">
  <tr>
    <th>参数名称：</th>
    <th>描述</th>
  </tr>
  <tr>
    <td>InternalMasterLoadBalancerDnsName</td>
    <td>DNS Name of Internal Load Balancer. Has to be valid for agents to join a running cluster</td>
  </tr>
  <tr>
    <td>KeyName</td>
    <td>AWS EC2 密钥对到 SSH 到实例</td>
  </tr>
  <tr>
    <td>PrivateAgentInstanceCount</td>
    <td>Specify the number of private agent nodes or accept the default.</td>
  </tr>
  <tr>
    <td>PrivateAgentInstanceType</td>
    <td>Region-specific instance type. E.g. m3.xlarge</td>
  </tr>
  <tr>
    <td>PrivateAgentSecurityGroup</td>
    <td>Permissive Security group used by private Agents</td>
  </tr>
  <tr>
    <td>PrivateSubnet</td>
    <td>Subnet ID for use by all private agent nodes</td>
  </tr>
</table>

## <a name="public-agent"></a>公共代理节点

The public agent template (`advanced-pub-agent.json`) creates agents which are then attached to a DC/OS cluster as a part of an AutoScalingGroup. To configure the template, specify the VPC, subnet, and master DNS address for the DC/OS cluster to join.

<table class="table">
  <tr>
    <th>Parameter Name</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>InternalMasterLoadBalancerDnsName</td>
    <td>DNS Name of Internal Load Balancer. Has to be valid for agents to join a running cluster</td>
  </tr>
  <tr>
    <td>KeyName</td>
    <td>AWS EC2 密钥对到 SSH 到实例</td>
  </tr>
  <tr>
    <td>PublicAgentInstanceCount</td>
    <td>Specify the number of public agent nodes or accept the default.</td>
  </tr>
  <tr>
    <td>PublicAgentInstanceType</td>
    <td>Region-specific instance type. E.g. m3.xlarge</td>
  </tr>
  <tr>
    <td>PublicAgentSecurityGroup</td>
    <td>Permissive Security group used by Public Agents</td>
  </tr>
  <tr>
    <td>PublicSubnet</td>
    <td>Subnet ID for use by all public agent nodes</td>
  </tr>
</table>

## <a name="master"></a>Master

The master templates (`advanced-master-1.json`, `advanced-master-3.json`, `advanced-master-5.json`, `advanced-master-7.json`) create masters, on top of the infrastructure stack already created.

<table class="table">
  <tr>
    <th>参数名称：</th>
    <th>描述</th>
  </tr>
  <tr>
    <td>AdminSecurityGroup</td>
    <td>Admin URL Security Group. Controls Access to the Admin page</td>
  </tr>
  <tr>
    <td>ExhibitorS3Bucket</td>
    <td>S3 Bucket resource name. Used by Exhibitor for Zookeeper discovery and coordination. See Exhibitor documentation on 'shared configuration': https://github.com/Netflix/exhibitor/wiki/Shared-Configuration for more information</td>
  </tr>
  <tr>
    <td>KeyName</td>
    <td>AWS EC2 Key Pair to SSH into instances</td>
  </tr>
  <tr>
    <td>LbSecurityGroup</td>
    <td>Loadbalancer Security Group. Rules allow masters and private agent nodes to communicate.</td>
  </tr>
  <tr>
    <td>MasterInstanceType</td>
    <td>Region-specific instance type. E.g. m3.xlarge</td>
  </tr>
  <tr>
    <td>MasterSecurityGroup</td>
    <td>Security group used by master nodes</td>
  </tr>
  <tr>
    <td>PrivateAgentSecurityGroup</td>
    <td>Security group used by Private Agents, typically with limited access to the outside</td>
  </tr>
  <tr>
    <td>PrivateSubnet</td>
    <td>Subnet ID for use by all private agent nodes</td>
  </tr>
  <tr>
    <td>PublicAgentSecurityGroup</td>
    <td>Permissive Security group used by Public Agents</td>
  </tr>
  <tr>
    <td>PublicSubnet</td>
    <td>Subnet ID for use by all public agent nodes</td>
  </tr>
</table>

## <a name="infrastructure"></a>基础结构

The infrastructure template (`infra.json`) defines, and creates, a DC/OS specific infrastructure that works well with a VPC already created. This is the lowest building block of a DC/OS cluster and the components created in this stack are consumed by the dependent templates (master and agents).

<table class="table">
  <tr>
    <th>参数名称：</th>
    <th>描述</th>
  </tr>
  <tr>
    <td>AdminLocation</td>
    <td>Optional: Specify the IP range to whitelist for access to the admin zone. Must be a valid CIDR.</td>
  </tr>
  <tr>
    <td>InternetGateway</td>
    <td>Internet Gateway ID, must be attached to the 'Vpc'. Used by all nodes for outgoing Internet access.</td>
  </tr>
  <tr>
    <td>KeyName</td>
    <td>AWS EC2 密钥对到 SSH 到实例</td>
  </tr>
  <tr>
    <td>PrivateSubnet</td>
    <td>Subnet ID for use by all private agent nodes</td>
  </tr>
  <tr>
    <td>PublicSubnet</td>
    <td>Subnet ID for use by all public agent nodes</td>
  </tr>
  <tr>
    <td>Vpc</td>
    <td>Existing VPC to use. Nodes will be launched using subnets and Internet Gateway under this VPC</td>
  </tr>
</table>
