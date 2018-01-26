---
layout: layout.pug
navigationTitle: dcos auth list-providers
title: dcos auth list-providers
menuWeight: 1
excerpt: ""
enterprise: true
---
# 描述

列出您的DC / OS群集的已配置身份验证提供程序。 有关更多信息，请参阅服务帐户[文档](/1.10/security/ent/service-auth/)。

# 统计

```bash
dcos auth list-providers [OPTION]
```

# 选项

| 姓名、速记    | 默认 | 描述                    |
| -------- | -- | --------------------- |
| `--json` |    | 指定身份验证提供程序的JSON格式的列表。 |

# 父命令

| 命令                                                  | 描述            |
| --------------------------------------------------- | ------------- |
| [dcos auth](/1.10/cli/command-reference/dcos-auth/) | 管理DC/OS身份和访问。 |

# 例子

## 列出可用的提供者

在此示例中, 列出了可用的 DC/OS 身份验证提供程序。

```bash
dcos auth list-providers
```

输出会是这样：

```bash
PROVIDER ID    AUTHENTICATION TYPE                                                               
dcos-services  Authenticate using a DC/OS service user account (using username and private key)  
dcos-users     Authenticate using a standard DC/OS user account (using username and password)   
```