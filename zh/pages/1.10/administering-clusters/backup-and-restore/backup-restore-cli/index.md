---
layout: layout.pug
navigationTitle: 备份和还原
title: 备份和还原
menuWeight: 0
excerpt: ""
enterprise: true
---
# 基础要求

- DC / OS企业集群。
- 已安装[DC/OS CLI](/1.10/cli/install/)。
- 安装了[ DC / OS企业版CLI ](/1.10/cli/enterprise-cli/)。

**重要：**请参阅备份和还原的[限制](/1.10/administering-clusters/backup-and-restore/#limitations)。

# 备份一个群集

备份存储在主节点的本地文件系统上。 备份状态由群集中运行的服务维护，备份/恢复操作通过直接点击此服务来启动。

1. 创建一个备份并为其分配一个有意义的标签。 标签有以下限制：
    
    - 长度必须在3到25个字符之间。
    - 它不能以`.. `开头。
    - 它必须由以下字符组成：[A-Za-z0-9 _.-]。
    ```bash
dcos backup create --label=<backup-label>
```

2. 验证您的备份是否已经创建。
    
    ```bash
dcos backup list
```

或者使用以下命令将您的搜索结果精简到创建备份时使用的标签。

```bash
dcos backup list [label]
```

备份最初将转换到` STATUS_BACKING_UP `状态，并最终到达` STATUS_READY `。 如果出现问题，它将显示` STATUS_ERROR `的状态。 使用` dcos backup show< backup-id> `来检查Marathon在备份过程中出错的原因。

3. 使用由` dcos backup list `生成的ID在后续命令中引用您的备份。 备份ID将类似于`< backup-label> -ea6b49f5-79a8-4767-ae78-3f874c90e3da `。

# 删除备份

1. 删除不需要的备份。
    
    ```bash
dcos backup delete <backup-id>
```

# 还原一个群集

1. 列出可用的备份，选择要还原到的备份，并记下备份ID。
    
    ```bash
dcos backup list
```

2. 从选定的备份中恢复。
    
    ```bash
dcos backup restore <backup-id>
```

3. 监视还原操作的状态。
    
    ```bash
dcos backup show <backup-id>
```

JSON输出的` restores.component_status.marathon `参数将显示` STATUS_RESTORING `，然后显示` STATUS_READY `。