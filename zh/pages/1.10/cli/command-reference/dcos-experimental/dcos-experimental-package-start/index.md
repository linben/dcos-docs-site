---
layout: layout.pug
navigationTitle: dcos experimental service start
title: dcos experimental service start
menuWeight: 2
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

从非本地 DC/OS 包启动服务。有关如何将软件包添加到 DC/OS 的信息, 请参见 ` dcos experimental package add`。

# 统计

```bash
dcos experimental service start <package-name> [OPTION]
```

# 选项

| 名字，简写                                       | 默认 | 描述                      |
| ------------------------------------------- | -- | ----------------------- |
| `--json`                                    |    | JSON-formatted data.    |
| `--options=<options-file>`            |    | 包含自定义包执行选项的 JSON 文件的路径。 |
| `--package-version=<package-version>` |    | 包版本。                    |

# 位置实参

| 名字，简写                  | 默认 | 描述          |
| ---------------------- | -- | ----------- |
| `<package-name>` |    | DC/OS 包的名称。 |

# 父命令

| 命令                                                                  | 描述            |
| ------------------------------------------------------------------- | ------------- |
| [dcos experimental](/1.10/cli/command-reference/dcos-experimental/) | 管理正在开发和更改的命令。 |