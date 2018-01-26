---
layout: layout.pug
navigationTitle: 转换代理节点类型
title: 转换代理节点类型
menuWeight: 700
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

对于现有的 DC/OS 群集, 可以将代理节点转换为公共或专用。

Agent节点被指定为[ public ](/1.10/overview/concepts/#public-agent-node)或<a href =“/1.10/overview/concepts/#private-agent-node” >私人</a>在安装过程中。 默认情况下, 它们在 [ gui ](/1.10/installing/oss/custom/gui/) 或 [ cli ](/1.10/installing/oss/custom/cli/) 安装期间被指定为专用。

### 基础要求

这些步骤必须在配置为 DC/OS 节点的计算机上执行。在该节点上运行的任何任务都将在转换过程中终止。

- DC / OS使用[自定义](/1.10/installing/oss/custom/)安装方法进行安装，并且至少部署了一个[”主“](/1.10/overview/concepts/#master)和一个[私人](/1.10/overview/concepts/#private-agent-node)代理节点。
- 归档的DC/OS安装程序文件(`dcos-install.tar`)来自您的[安装](/1.10/installing/oss/custom/gui/#backup)。 
- CLI JSON 处理器 [ jq ](https://github.com/stedolan/jq/wiki/Installation)。
- 已安装和配置 SSH。这是访问 DC/OS 群集中的节点所必需的。

### 确定节点类型

您可以通过从DC / OS CLI运行此命令来确定节点类型。

- 运行此命令以确定群集中有多少私有代理。` 0 ` 的结果表明没有私有代理。
    
    ```bash
dcos node --json | jq --raw-output '.[] | select(.reserved_resources.slave_public == null) | .id' | wc -l
```

- 运行此命令以确定集群中有多少个公共代理。 ` 0 `的结果表示没有公共代理。
    
    ```bash
dcos node --json | jq --raw-output '.[] | select(.reserved_resources.slave_public != null) | .id' | wc -l
```

### 卸载DC / OS private agent 软件

1. 卸载agent节点上的DC / OS。
    
    ```bash
sudo /opt/mesosphere/bin/dcos-shell
sudo -i pkgpanda uninstall
sudo systemctl stop dcos-mesos-slave
sudo systemctl disable dcos-mesos-slave
```

2. 删除agent节点上的旧目录结构。
    
    ```bash
sudo rm -rf /etc/mesosphere /opt/mesosphere /var/lib/mesos /var/lib/dcos
```

3. 重新启动本游戏
    
    ```bash
sudo reboot
```

### 安装DC / OS并转换agent节点

将已存档的 DC/OS 安装程序文件 (` dcos-install.tar `) 复制到代理节点。 此存档是在GUI或CLI [安装](/1.10/installing/oss/custom/gui/#backup)方法中创建的。

1. 将文件复制到代理节点。例如, 可以使用安全副本 (scp) 将 `dcos-install.tar` 复制到您的主目录中:
    
    ```bash
scp ~/dcos-install.tar $username@$node-ip:~/dcos-install.tar
```

2. SSH 到机器:
    
    ```bash
ssh $USER@$AGENT
```

3. 为安装程序文件创建一个目录:
    
        bash
         sudo mkdir -p /opt/dcos_install_tmp

4. 解 `dcos-install.tar` 文件:
    
    ```bash
sudo tar xf dcos-install.tar -C /opt/dcos_install_tmp
```

5. 运行此命令可在代理节点上安装 DC/OS。您必须将代理节点指定为公共或专用。
    
    公共代理节点:
    
    ```bash
sudo bash /opt/dcos_install_tmp/dcos_install.sh slave
```

public agent节点:

```bash
sudo bash /opt/dcos_install_tmp/dcos_install.sh slave_public
```