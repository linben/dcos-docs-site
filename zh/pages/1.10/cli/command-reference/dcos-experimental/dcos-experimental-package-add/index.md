---
layout: layout.pug
navigationTitle: dcos experimental add
title: dcos experimental add
menuWeight: 0
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

将 dc/os 包添加到 dc/os。

# 统计

```bash
dcos experimental package add [OPTION]
```

# 选项

| 姓名、速记                                       | 默认 | 描述            |
| ------------------------------------------- | -- | ------------- |
| `--dcos-package=<dcos-package>`       |    | 到 DC/OS 包的路径。 |
| `--json`                                    |    | JSON 格式的数据。   |
| `--package-name=<package-name>`       |    | DC/OS 包的名称。   |
| `--package-version=<package-version>` |    | 包版本。          |

# 父命令

| 命令                                                                  | 描述            |
| ------------------------------------------------------------------- | ------------- |
| [dcos experimental](/1.10/cli/command-reference/dcos-experimental/) | 管理正在开发和更改的命令。 |