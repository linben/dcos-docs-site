---
layout: layout.pug
navigationTitle: dcos config show
title: dcos config show
menuWeight: 2
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

打印当前 [ 附加 ](/1.10/cli/command-reference/dcos-cluster/dcos-cluster-attach/) 群集的 DC/OS 配置文件内容。

# 统计

```bash
dcos config show <name> [OPTION]
```

# 选项

None.

# 位置实参

| 姓名、速记          | 默认 | 描述     |
| -------------- | -- | ------ |
| `<name>` |    | 属性的名称。 |

# 父命令

| 命令                                                      | 描述           |
| ------------------------------------------------------- | ------------ |
| [dcos config](/1.10/cli/command-reference/dcos-config/) | 管理 DC/OS 配置。 |

# 例子

## 查看特定的配置值

在此示例中, 显示了 DC/OS URL。

```bash
dcos config show core.dcos_url
```

以下是输出:

```bash
https://your-cluster-9vqnkrq5pt2n-2781474.cloue-1.elb.amazonaws.com
```

## 查看所有配置值

在此示例中, 将显示所有配置值。

```bash
dcos config show
```

以下是输出:

```bash
core.dcos_url https://your-cluster-9vqnkrq5pt2n-2781474.cloue-1.elb.amazonaws.com
core.ssl_verify false
```