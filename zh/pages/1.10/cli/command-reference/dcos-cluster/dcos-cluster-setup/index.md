---
layout: layout.pug
navigationTitle: dcos cluster setup
title: dcos cluster setup
menuWeight: 6
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

配置到DC / OS群集的连接，连接到群集，并向DC / OS进行身份验证。

# 统计

```bash
dcos cluster setup <dcos_url> [OPTIONS]
```

# 选项

| 名字，简写                                   | 默认 | 描述                                                                                                                       |
| --------------------------------------- | -- | ------------------------------------------------------------------------------------------------------------------------ |
| `--ca-certs=<ca-certs>`           |    | [enterprise type="inline" size="small" /] The path to a list of trusted CAs to verify requests against.                  |
| `--insecure`                            |    | 允许请求绕过SSL证书验证。 类似于` dcos config set core.ssl_verify = False`                                                             |
| `--no-check`                            |    | [enterprise type="inline" size="small" /] Do not check the CA certificate downloaded from the cluster. This is insecure. |
| `--password-env=<password_env>`   |    | 包含登录密码的环境变量的名称。                                                                                                          |
| `--password-file=<password_file>` |    | 包含登录密码的文件的路径。                                                                                                            |
| `--password=<password>`           |    | 登录密码。 这是不安全的。                                                                                                            |
| `--private-key=<key_path>`        |    | 包含私钥的文件的路径。                                                                                                              |
| `--provider=<provider_id>`        |    | [enterprise type =“inline”size =“small”/] 用于登录的身份验证提供程序。                                                                 |
| `--username=<username>`           |    | 用于登录的用户名。                                                                                                                |

## SSL选项

如果您未指定SSL选项`--insecure`，`--no-check `或`--ca-certs `中的一个，则CA 从集群下载证书，并向您显示证书的sha256指纹以供验证。

# 位置实参

| 名字，简写              | 默认 | 描述                |
| ------------------ | -- | ----------------- |
| `<dcos_url>` |    | 主节点的可公开访问的代理IP地址。 |

# 父命令

| 命令                                                        | 描述           |
| --------------------------------------------------------- | ------------ |
| [dcos cluster](/1.10/cli/command-reference/dcos-cluster/) | 管理DC / OS群集。 |

# 例子

有关示例，请参阅[连接到多个群集](/1.10/cli/multi-cluster-cli/)文档。