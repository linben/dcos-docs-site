---
layout: layout.pug
navigationTitle: 添加代理节点
title: 添加代理节点
menuWeight: 800
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

您可以将代理节点添加到现有的 DC/OS 群集。

Agent nodes are designated as [public](/1.10/overview/concepts/#public-agent-node) or [private](/1.10/overview/concepts/#private-agent-node) during installation. 默认情况下, 它们在 [ gui ](/1.10/installing/oss/custom/gui/) 或 [ cli ](/1.10/installing/oss/custom/cli/) 安装期间被指定为专用。

### 基础要求

* 使用 [ 自定义 ](/1.10/installing/oss/custom/) 安装方法安装 DC/OS。
* 归档的DC/OS安装程序文件(`dcos-install.tar`)来自您的[安装](/1.10/installing/oss/custom/gui/#backup)。
* 满足 [ 系统要求 ](/1.10/installing/oss/custom/system-requirements/) 的可用代理节点。
* CLI JSON 处理器 [ jq ](https://github.com/stedolan/jq/wiki/Installation)。
* 已安装和配置 SSH。这是访问 DC/OS 群集中的节点所必需的。

### 安装 DC/OS 代理节点

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
    
    ```bash
sudo mkdir -p /opt/dcos_install_tmp
```

4. 解 `dcos-install.tar` 文件:
    
    ```bash
sudo tar xf dcos-install.tar -C /opt/dcos_install_tmp
```

5. 运行此命令可在代理节点上安装 DC/OS。您必须将代理节点指定为公共或专用。
    
    公共代理节点:
    
    ```bash
sudo bash /opt/dcos_install_tmp/dcos_install.sh slave
```

公共代理节点:

```bash
sudo bash /opt/dcos_install_tmp/dcos_install.sh slave_public
```

** 提示: **可以通过从 DC/OS CLI 运行此命令来验证节点类型。

* 运行此命令可对私有代理进行计数。
    
    ```bash
dcos node --json | jq --raw-output '.[] | select(.reserved_resources.slave_public == null) | .id' | wc -l
```

* 运行此命令可对私有代理进行计数。
    
    ```bash
dcos node --json | jq --raw-output '.[] | select(.reserved_resources.slave_public != null) | .id' | wc -l
```