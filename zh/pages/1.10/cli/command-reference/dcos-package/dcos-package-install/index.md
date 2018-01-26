---
layout: layout.pug
navigationTitle: dcos package install
title: dcos package install
menuWeight: 1
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

安装新软件包

# 统计

```bash
dcos package install <package-name> [OPTION]
```

# 选项

| 名字，简写                                       | 默认 | 描述                                                                         |
| ------------------------------------------- | -- | -------------------------------------------------------------------------- |
| `--app`                                     |    | Application only.                                                          |
| `--app-id=<app-id>`                   |    | The application ID.                                                        |
| `--cli`                                     |    | Command line only.                                                         |
| `--options=<file>`                    |    | Path to a JSON file that contains customized package installation options. |
| `--package-version=<package-version>` |    | The package version.                                                       |
| `--yes`                                     |    | Disable interactive mode and assume "yes" is the answer to all prompts.    |

# 位置实参

| 名字，简写                  | 默认 | 描述          |
| ---------------------- | -- | ----------- |
| `<package-name>` |    | DC/OS 包的名称。 |

# 父命令

| 命令                                                        | 描述                                          |
| --------------------------------------------------------- | ------------------------------------------- |
| [dcos package](/1.10/cli/command-reference/dcos-package/) | Install and manage DC/OS software packages. |

# 例子

有关示例, 请参见 [ 文档 ](/1.10/deploying-services/config-universe-service/)。