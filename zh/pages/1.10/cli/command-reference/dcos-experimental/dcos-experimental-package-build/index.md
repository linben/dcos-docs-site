---
layout: layout.pug
navigationTitle: dcos experimental package build
title: dcos experimental package build
menuWeight: 1
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

在本地构建一个包, 以将其添加到 DC/OS 或与宇宙共享。

# 统计

```bash
dcos experimental package build <build-definition> [OPTION]
```

# 选项

| 姓名、速记                                         | 默认     | 描述           |
| --------------------------------------------- | ------ | ------------ |
| `--json`                                      |        | JSON 格式的数据。  |
| `--output-directory=<output-directory>` | 当前工作目录 | 要存储数据的目录的路径。 |

# 位置实参

| 名字，简写                      | 默认 | 描述              |
| -------------------------- | -- | --------------- |
| `<build-definition>` |    | DC/OS 包生成定义的路径。 |

# 父命令

| 命令                                                                  | 描述            |
| ------------------------------------------------------------------- | ------------- |
| [dcos experimental](/1.10/cli/command-reference/dcos-experimental/) | 管理正在开发和更改的命令。 |