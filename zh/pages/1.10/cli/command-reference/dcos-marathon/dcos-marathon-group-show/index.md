---
layout: layout.pug
navigationTitle: dcos marathon group show
title: dcos marathon group show
menuWeight: 21
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

Print a detailed list of groups.

# 统计

```bash
dcos marathon group show <group-id> [OPTION]
```

# 选项

| 名字，简写                                   | 默认 | 描述                                                                          |
| --------------------------------------- | -- | --------------------------------------------------------------------------- |
| `--group-version=<group-version>` |    | 用于该命令的组版本。 它可以被指定为绝对值或相对值。 绝对值必须是ISO8601日期格式。 相对值必须指定为负整数，它们表示当前部署的组定义中的版本。 |

# 位置实参

| 名字，简写              | 默认 | 描述   |
| ------------------ | -- | ---- |
| `<group-id>` |    | 组ID。 |

# 父命令

| 命令                                                          | 描述                  |
| ----------------------------------------------------------- | ------------------- |
| [dcos marathon](/1.10/cli/command-reference/dcos-marathon/) | 将应用程序部署和管理到DC / OS。 |

<!-- # Examples -->