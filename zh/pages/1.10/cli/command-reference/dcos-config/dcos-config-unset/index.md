---
layout: layout.pug
navigationTitle: dcos config unset
title: dcos config unset
menuWeight: 3
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

从配置文件中移除属性。

# 统计

```bash
dcos config unset <name> [OPTION]
```

# 选项

None.

# 位置实参

| 名字，简写          | 默认 | 描述     |
| -------------- | -- | ------ |
| `<name>` |    | 属性的名称。 |

# 父命令

| 命令                                                      | 描述           |
| ------------------------------------------------------- | ------------ |
| [dcos config](/1.10/cli/command-reference/dcos-config/) | 管理 DC/OS 配置。 |

# 例子

## 删除配置值

在此示例中, 将删除 ` 核心. ssl_verify ` 属性。

```bash
dcos config unset core.ssl_verify
```

以下是输出:

```bash
Removed [core.ssl_verify]
```