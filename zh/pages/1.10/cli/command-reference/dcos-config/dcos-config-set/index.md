---
layout: layout.pug
navigationTitle: dcos config set
title: dcos config set
menuWeight: 1
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

添加或设置 DC/OS 配置属性。下面是可用的属性。

| **属性**                  | **描述**                                                                                                          |
| ----------------------- | --------------------------------------------------------------------------------------------------------------- |
| `core.dcos_acs_token`   | DC/OS 身份验证令牌。 使用 ` dcos 授权登录 ` 登录到 DC/OS CLI 时, 它在本地存储身份验证标记值。 有关更多信息, 请参见 [ 文档 ](/1.10/security/ent/iam-api/)。 |
| `core.dcos_url`         | DC/OS 群集的公共主 URL。                                                                                               |
| `core.mesos_master_url` | The Mesos master URL. Defaults to `core.dcos_url`.                                                              |
| `core.pagination`       | 指示是否页输出。默认为 true。                                                                                               |
| `core.ssl_verify`       | 指示是验证 ssl 证书还是设置 ssl 证书的路径。                                                                                     |
| `core.timeout`          | 以秒为单位的请求超时, 最小值为1秒。默认为3分钟。                                                                                      |

# 统计

```bash
dcos config set <name> <value> [OPTION]
```

# 选项

None.

# 位置实参

| 名字，简写           | 默认 | 描述     |
| --------------- | -- | ------ |
| `<name>`  |    | 属性的名称。 |
| `<value>` |    | 财产的价值。 |

# 父命令

| 命令                                                      | 描述           |
| ------------------------------------------------------- | ------------ |
| [dcos config](/1.10/cli/command-reference/dcos-config/) | 管理 DC/OS 配置。 |

# 例子

## 设置请求超时

在此示例中, 请求超时设置为5分钟。

```bash
dcos config set core.timeout 300
```

以下是输出:

```bash
[core.timeout]: set to '300'
```

## 设置 SSL 设置

在此示例中, 验证 HTTPS 的 SSL 证书设置为 ` true `。

```bash
dcos config set core.ssl_verify true
```

以下是输出:

```bash
[core.ssl_verify]: set to 'true'
```