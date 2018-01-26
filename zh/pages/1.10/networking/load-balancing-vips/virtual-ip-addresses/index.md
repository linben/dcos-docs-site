---
layout: layout.pug
navigationTitle: 使用虚拟IP地址
title: 使用虚拟IP地址
menuWeight: 10
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

DC / OS可以将流量从单个虚拟IP(VIP) 映射到多个IP地址和端口。 DC/OS vip 是 ** 基于名称的 **, 这意味着客户端使用服务地址而不是 IP 地址进行连接。

DC/OS 自动生成不与 IP vip 冲突的基于名称的 vip, 因此您不必担心冲突。 此功能允许在安装服务时自动创建基于名称的 vip。

指定的 VIP 包含以下组件:

* 私有虚拟IP地址
* 端口(服务可用的端口)
* 服务名

您可以从DC / OS GUI为应用程序分配一个VIP。 您在部署新服务时输入的值将被转换为这些Marathon应用程序定义条目：

* ` portDefinitions `如果不使用Docker容器
* ` portMappings `如果使用Docker容器

VIP遵循这个命名约定：

    <service-name>.marathon.l4lb.thisdcos.directory:<port>
    

### 先决条件

* VIP地址池是您的应用程序唯一的。

## 创建一个VIP

1. 在DC / OS [ GUI ](/1.10/gui/)上，点击**Services**，然后点击**RUN A SERVICE**。
    
    1. 从**Networking**标签中，选择**NETWORK TYPE**> **虚拟网络：dcos **。
    2. 展开 **ADD SERVICE ENDPOINT** 并提供以下响应:
        
        * **CONTAINER PORT**
        * **SERVICE ENDPOINT NAME**
        * **PORT MAPPING**
        * **LOAD BALANCED SERVICE ADDRESS**
        
        当您填写这些字段时, 马拉松设置的服务地址将出现在屏幕的底部。 您可以通过单击 ** ADD SERVICE ENDPOINT** 来为您的应用程序分配多个 vip。
        
        ![VIP service definition](/1.10/img/vip-service-definition.png)
        
        在上面的示例中, 客户端可以访问 ` service.marathon.l4lb.thisdcos:5555 `。
    
    3. 单击 ** REVIEW & RUN** 和 **RUN SERVICE **。

您可以单击 ** 网络 ** 选项卡以查看服务的网络详细信息。

![VIP output](/1.10/img/vip-service-definition-output.png)

有关端口配置的详细信息, 请参阅 [ 马拉松端口文档 ](/1.10/deploying-services/service-ports/)。

## 使用VIP与DC / OS服务

某些DC / OS服务（例如[ Kafka ](/services/kafka/)）在安装时会自动创建VIP。 命名约定是：`broker.<service.name>.l4lb.thisdcos.directory:9092`。

按照这些步骤查看卡夫卡的VIP。

### 通过GUI

1. 点击**Networking**> **Networks**，然后选择** dcos **。
2. 选择您的任务来查看详细信息。
    
    ![](/1.10/img/vip-service-details.png)

### 通过CLI

**先决条件：** Kafka服务和CLI必须[安装](/services/kafka/)。

1. 运行这个命令：
    
    ```bash
dcos kafka endpoints broker
```

输出会是这样：

```json
{
  "address": [
    "10.0.2.199:9918"
  ],
  "zookeeper": "master.mesos:2181/dcos-service-kafka",
  "dns": [
    "broker-0.kafka.mesos:9918"
  ],
  "vip": "broker.kafka.l4lb.thisdcos.directory:9092"
}
```

您可以使用此VIP来处理集群中的任何一个卡夫卡经纪人。

## 常见问题解答

### 连接似乎在随机时间关闭

这种行为常常会遇到长时间连接的应用程序，比如数据库(例如PostgreSQL)。 要修复，请尝试启用Keepalive。 Keepalive可以是特定于应用程序的机制，比如心跳，或者TCP协议中的某些协议。 保持活动是必需的，因为负载平衡器无法区分空闲或死连接，因为在任何情况下都不发送数据包。 默认超时取决于内核配置，但通常是5分钟。