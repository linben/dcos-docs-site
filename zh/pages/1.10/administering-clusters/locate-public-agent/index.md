---
layout: layout.pug
navigationTitle: 查找公共代理 IP
title: 查找公共代理 IP
menuWeight: 3
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

在使用声明的公共代理节点安装了 DC/OS 后, 您可以导航到公共代理节点的公用 IP 地址。

**基础要求**

- DC / OS至少安装一个主节点和[公共agent](/1.10/overview/concepts/#public-agent-node)节点
- DC / OS [ CLI ](/1.10/cli/) 0.4.6或更高版本
- [jq](https://github.com/stedolan/jq/wiki/Installation)
- [ SSH ](/1.10/administering-clusters/sshcluster/) 配置

您可以通过从终端运行此命令来找到您的公共代理 IP。 此命令 SSHs 到您的群集以获取群集信息, 然后查询 [ ifconfig ](https://ifconfig.co/) 以确定您的公共 IP 地址。

    for id in $(dcos node --json | jq --raw-output '.[] | select(.attributes.public_ip == "true") | .id'); do dcos node ssh --option StrictHostKeyChecking=no --option LogLevel=quiet --master-proxy --mesos-id=$id "curl -s ifconfig.co" ; done 2>/dev/null
    

下面是一个公共 IP 地址为 ` 52.39.29.79 ` 的示例:

    for id in $(dcos node --json | jq --raw-output '.[] | select(.attributes.public_ip == "true") | .id'); do dcos node ssh --option StrictHostKeyChecking=no --option LogLevel=quiet --master-proxy --mesos-id=$id "curl -s ifconfig.co" ; done 2>/dev/null
    52.39.29.79