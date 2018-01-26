---
layout: layout.pug
navigationTitle: dcos cluster list
title: dcos cluster list
menuWeight: 3
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

列出连接到DC / OS CLI的群集。

# 统计

```bash
dcos cluster list [--attached --json]
```

# 选项

| 名字，简写        | 默认 | 描述           |
| ------------ | -- | ------------ |
| `--attached` |    | 仅附加群集。       |
| `--json`     |    | 打印JSON格式的列表。 |

# 父命令

| 命令                                                        | 描述           |
| --------------------------------------------------------- | ------------ |
| [dcos cluster](/1.10/cli/command-reference/dcos-cluster/) | 管理DC / OS群集。 |

# 例子

有关示例，请参阅[连接到多个群集](/1.10/cli/multi-cluster-cli/)文档。