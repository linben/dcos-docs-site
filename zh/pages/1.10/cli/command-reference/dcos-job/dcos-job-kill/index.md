---
layout: layout.pug
navigationTitle: dcos job kill
title: dcos job kill
menuWeight: 2
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

杀了这份工作

# 统计

```bash
dcos job kill <job-id> [OPTION]
```

# 选项

| 姓名、速记    | 默认 | 描述       |
| -------- | -- | -------- |
| `run-id` |    | 作业运行 ID。 |
| `--all`  |    | 杀死所有的工作。 |

# 位置实参

| 名字，简写            | 默认 | 描述       |
| ---------------- | -- | -------- |
| `<job-id>` |    | 指定作业 ID。 |

# 父命令

| 命令                                                | 描述                |
| ------------------------------------------------- | ----------------- |
| [dcos job](/1.10/cli/command-reference/dcos-job/) | 在 DC/OS 中部署和管理作业。 |

<!-- # Examples -->