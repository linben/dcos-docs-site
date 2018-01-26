---
layout: layout.pug
navigationTitle: dcos config
title: dcos config
menuWeight: 2
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

此命令管理运行 [ dcos cluster setup ](/1.10/cli/command-reference/dcos-cluster/dcos-cluster-setup) 时创建的 DC/OS 配置文件。 配置文件位于 `~/.dcos/clusters/<cluster_id>/dcos.toml`. 如果未更改任何配置属性, 则在运行 ` dcos config show` 时应看到此输出:

    cluster.name <cluster_name>
    core.dcos_acs_token ********
    core.dcos_url <cluster_url>
    core.ssl_verify `true` or `false`
    

## 环境变量

配置属性具有相应的环境变量。 如果属性位于 `core` 部分 (ex。 ` core.foo `), 它对应于环境变量 ` DCOS_FOO `。 所有其他属性 (ex。 ` foo. bar `) 对应于环境变量 ` DCOS_FOO_BAR `。

环境变量优先于相应的配置属性。

# 统计

```bash
dcos config
```

# 选项

| 姓名、速记          | 默认 | 描述           |
| -------------- | -- | ------------ |
| `--help, h`    |    | 打印使用。        |
| `--info`       |    | 打印此子命令的简短说明。 |
| `--version, v` |    | 显示版本信息       |

# 子命令

| 命令                                                                                    | 描述                |
| ------------------------------------------------------------------------------------- | ----------------- |
| [dcos config set](/1.10/cli/command-reference/dcos-config/dcos-config-set/)           | 添加或设置 DC/OS 配置属性。 |
| [dcos config show](/1.10/cli/command-reference/dcos-config/dcos-config-show/)         | 打印 DC/OS 配置文件内容。  |
| [dcos config unset](/1.10/cli/command-reference/dcos-config/dcos-config-unset/)       | 从配置文件中移除属性。       |
| [dcos config validate](/1.10/cli/command-reference/dcos-config/dcos-config-validate/) | 验证对配置文件的更改。       |