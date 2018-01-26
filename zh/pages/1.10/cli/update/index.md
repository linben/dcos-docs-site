---
layout: layout.pug
navigationTitle: 更新 CLI
title: 更新 CLI
menuWeight: 3
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

您可以将 DC/OS CLI 更新为最新版本或降级为旧版本。

# <a name="upgrade"></a>升级 CLI

** 重要: **如果从 PyPI 或从 DC/OS UI 1.7 或更早版本下载了 cli, 则必须完全 [ 卸载 ](/1.10/cli/uninstall/) cli。 您无法升级。

您可以将现有的 DC/OS CLI 安装升级到最新版本。

1. 删除当前的 CLI 二进制文件。例如, 如果您安装到 `//本地/bin/`:
    
    ```bash
rm -rf /usr/local/bin/dcos
```

2. 将 DC/OS CLI 二进制文件 (` dcos `) 下载到您的本地目录中 (例如, `/usr/local/bin/`)。 使用所需的升级版本 (` <version> `) 更新命令:
    
    ```bash
curl https://downloads.dcos.io/binaries/cli/darwin/x86-64/dcos-<dcos-version>/dcos
```

** 重要: **CLI 必须安装在您的 DC/OS 群集外部的系统上。

3. 使 CLI 二进制可执行文件。
    
    ```bash
chmod +x dcos
```

** 提示: **如果系统无法找到可执行文件, 则可能需要重新打开命令提示符, 或者手动将安装目录添加到 PATH 环境变量中。

4. 将 CLI 指向您的 DC/OS 主节点。在此示例中, ` http://example.com ` 是主节点 IP 地址。
    
    ```bash
dcos cluster setup http://example.com
```

按照 DC/OS CLI 中的说明进行操作。有关安全性的详细信息, 请参阅 [ 文档 ](/1.10/security/ent/)。

您的CLI现在应该通过群集进行身份验证！ 输入` dcos `开始。

```bash
dcos
Command line utility for the Mesosphere Datacenter Operating
System (DC/OS). The Mesosphere DC/OS is a distributed operating
system built around Apache Mesos. This utility provides tools
for easy management of a DC/OS installation.

Available DC/OS commands:

   auth             Authenticate to DC/OS cluster
   config           Manage the DC/OS configuration file
   help             Display help information about DC/OS
   marathon         Deploy and manage applications to DC/OS
   node             Administer and manage DC/OS cluster nodes
   package          Install and manage DC/OS software packages
   service          Manage DC/OS services
   task             Manage DC/OS tasks

Get detailed command description with 'dcos <command> --help'.
```

# <a name="downgrade"></a>降级CLI

您可以将现有的DC / OS CLI安装降级到较旧的版本。

1. 删除当前的 CLI 二进制文件:
    
    ```bash
rm path/to/binary/dcos
```

2. 从要安装新的 dc/os cli 二进制文件的目录中, 输入此命令以使用指定的降级版本 (` <version> `) 更新 dc/os cli:
    
    ```bash
curl https://downloads.dcos.io/binaries/cli/darwin/x86-64/<version>/dcos
```