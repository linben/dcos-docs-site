---
layout: layout.pug
navigationTitle: dcos auth
title: dcos auth
menuWeight: 1
excerpt: ""
enterprise: false
---
# 描述

该命令管理DC / OS身份和访问。

# 统计

```bash
dcos auth 
```

# 选项

| 名字，简写          | 默认 | 描述           |
| -------------- | -- | ------------ |
| `--help, h`    |    | 打印使用。        |
| `--info`       |    | 打印此子命令的简短说明。 |
| `--version, v` |    | 显示版本信息       |

# 子命令

| 命令                                                                                          | 描述                                    |
| ------------------------------------------------------------------------------------------- | ------------------------------------- |
| [dcos auth list-providers](/1.10/cli/command-reference/dcos-auth/dcos-auth-list-providers/) | 列出DC / OS集群的配置身份验证提供程序（仅限Enterprise）。 |
| [dcos auth login](/1.10/cli/command-reference/dcos-auth/dcos-auth-login/)                   | 登录到DC / OS身份验证。                       |
| [dcos auth logout](/1.10/cli/command-reference/dcos-auth/dcos-auth-logout/)                 | 注销DC / OS身份验证。                        |