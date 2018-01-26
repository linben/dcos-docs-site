---
layout: layout.pug
navigationTitle: dcos job history
title: dcos job history
menuWeight: 1
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

显示作业运行历史记录。

# 统计

```bash
dcos job history <job-id> [OPTION]
```

# 选项

| 姓名、速记             | 默认 | 描述               |
| ----------------- | -- | ---------------- |
| `--json`          |    | 打印JSON格式的列表。     |
| `--show-failures` |    | 显示历史记录的失败表和统计信息。 |

# 位置实参

| 名字，简写            | 默认 | 描述       |
| ---------------- | -- | -------- |
| `<job-id>` |    | 指定作业 ID。 |

# 父命令

| 命令                                                | 描述                |
| ------------------------------------------------- | ----------------- |
| [dcos job](/1.10/cli/command-reference/dcos-job/) | 在 DC/OS 中部署和管理作业。 |

# 例子

## 查看作业的历史记录

在此示例中, 将显示作业历史记录。

1. 列出工作并找到ID：
    
    ```bash
dcos job list
```

以下是输出:

```bash
ID                DESCRIPTION                      STATUS       LAST SUCCESFUL RUN  
my-job            A job that sleeps                Unscheduled         N/A          
my-scheduled-job  A job that sleeps on a schedule  Unscheduled         N/A 
```

2. 查看 ` my-scheduled-job` 的作业历史记录:
    
    ```bash
dcos job history my-scheduled-job
```

Here is the output:

```bash
'my-scheduled-job'  Successful runs: 1 Last Success: 2017-02-17T23:18:33.842+0000
ID                             STARTED                       FINISHED            
20170217231831HkXNK  2017-02-17T23:18:31.651+0000  2017-02-17T23:18:33.843+0000 
```

**提示：**指定` --json `选项来查看JSON应用程序定义(例如` dcos job history my-scheduled-job `)。