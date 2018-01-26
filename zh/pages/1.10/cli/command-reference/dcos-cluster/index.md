---
layout: layout.pug
navigationTitle: dcos cluster
title: dcos cluster
menuWeight: 1
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

此命令管理与 DC/OS 群集的连接。

# 统计

```bash
dcos cluster
```

# 选项

| 姓名、速记          | 默认 | 描述           |
| -------------- | -- | ------------ |
| `--help, h`    |    | 打印使用。        |
| `--info`       |    | 打印此子命令的简短说明。 |
| `--version, v` |    | 显示版本信息       |

# 子命令

| 命令                                                                                   | 描述                                                                                                               |
| ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| [dcos cluster attach](/1.10/cli/command-reference/dcos-cluster/dcos-cluster-attach/) | 将CLI连接到连接的DC / OS群集。                                                                                             |
| [dcos cluster list](/1.10/cli/command-reference/dcos-cluster/dcos-cluster-list/)     | 列出连接到DC / OS CLI的群集。                                                                                             |
| [dcos cluster remove](/1.10/cli/command-reference/dcos-cluster/dcos-cluster-remove/) | 从DC / OS CLI配置中删除群集。                                                                                             |
| [dcos cluster rename](/1.10/cli/command-reference/dcos-cluster/dcos-cluster-rename/) | 在DC / OS CLI配置中重命名群集。                                                                                            |
| [dcos cluster setup](/1.10/cli/command-reference/dcos-cluster/dcos-cluster-setup/)   | 连接，认证并将DC / OS CLI连接到DC / OS群集。 结合` dcos config set core.dcos_url `，` dcos auth login `和` docs cluster attach `。 |