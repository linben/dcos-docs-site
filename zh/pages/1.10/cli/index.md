---
layout: layout.pug
navigationTitle: CLI
title: CLI
menuWeight: 50
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

Dc/os 命令行界面 (dc/os CLI) 是管理群集节点、安装和管理包、检查群集状态以及管理服务和任务的实用程序。

DC / OS 1.10.0需要DC / OS CLI 0.5.x.

要列出可用命令，请运行不带参数的` dcos `：

```bash
dcos

Command line utility for the Mesosphere Datacenter Operating
System (DC/OS). The Mesosphere DC/OS is a distributed operating
system built around Apache Mesos. This utility provides tools
for easy management of a DC/OS installation.

Available DC/OS commands:

    auth            Authenticate to DC/OS cluster
    cluster         Manage connections to DC/OS clusters
    config          Manage the DC/OS configuration file
    experimental    Experimental commands. These commands are under development and are subject to change
    help            Display help information about DC/OS
    job             Deploy and manage jobs in DC/OS
    marathon        Deploy and manage applications to DC/OS
    node            Administer and manage DC/OS cluster nodes
    package         Install and manage DC/OS software packages
    service         Manage DC/OS services
    task            Manage DC/OS tasks

Get detailed command description with `dcos <command> --help`.
```

# 显示DC / OS CLI版本

要显示 DC/OS CLI 版本, 请运行:

    dcos --version
    

<a name="configuration-files"></a>

# DC / OS CLI版本和配置文件

DC / OS CLI 0.4.x和0.5.x使用不同的结构来配置文件的位置。

DC/OS CLI 0.4.x 有一个配置文件, 默认情况下存储在 ` dcos/dcos. toml ` 中。 在DC / OS CLI 0.4.x中，您可以使用环境变量[ ` DCOS_CONFIG ` ](#dcos-config)来选择更改配置文件的位置。

DC/OS CLI 0.5.x has a configuration file for each connected cluster, which by default are stored in `~/.dcos/clusters/<cluster_id>/dcos.toml`. In DC/OS CLI 0.5.x you can optionally change the base portion (`~/.dcos`) of the configuration directory using the [`DCOS_DIR`](#dcos-dir) environment variable.

** 注意: **-更新到 DC/OS cli 0.5.x 并运行任何 cli 命令触发器从旧的转换到新的配置结构。 -在调用 ` dcos cluster setup ` 后 (或发生转换后), 如果尝试使用 ` dcos config set ` 命令更新群集配置, 该命令将打印一条警告消息, 指出该命令已弃用, 并且群集配置状态现在可能已损坏。

# 环境变量

DC / OS CLI支持以下环境变量，可以动态设置。

<a name="dcos-cluster"></a>

#### ` DCOS_CLUSTER `(DC/OS CLI O. 5. x 和更高)

[ 附加 ](/1.10/cli/command-reference/dcos-cluster/dcos-cluster-attach/) 群集。要设置附加的群集, 请使用以下命令设置变量:

```bash
export DCOS_CLUSTER=<cluster_name>
```

<a name="dcos-config"></a>

#### `DCOS_CONFIG` (DC/OS CLI O.4.x only)

DC/OS 配置文件的路径。如果将 DC/OS 配置文件放在 `/home/jdoe/config/dcos. toml ` 中, 请使用以下命令设置该变量:

```bash
export DCOS_CONFIG=/home/jdoe/config/dcos.toml
```

如果配置了 ` DCOS_CONFIG ` 环境变量:

* 在转换到[新的配置结构](#configuration-files)之后，` DCOS_CONFIG `不再符合要求。
* 在调用 ` dcos cluster setup` 之前, 您可以使用 ` dcos config set ` 更改 ` DCOS_CONFIG ` 所指向的配置。 此命令将打印一条警告消息, 指出该命令已弃用, 并建议使用 ` dcos cluster setup `。

<a name="dcos-dir"></a>

#### `DCOS_DIR` (DC/OS CLI O.5.x and higher only)

DC/OS 配置文件的路径。如果将 DC/OS 配置文件放在 `/home/jdoe/config/dcos. toml ` 中, 请使用以下命令设置该变量:

```bash
export DCOS_DIR=/home/jdoe/config
```

1. Optionally set `DCOS_DIR` and run `dcos cluster setup` command.
    
        export DCOS_DIR=<path/to/config_dir> (optional, default when not set is ~/.dcos)
        dcos cluster setup <url>
        
    
    This setting generates and updates per cluster configuration under `$DCOS_DIR/clusters/<cluster_id>`. Sets newly set up cluster as the attached one.

<a name="dcos-ssl-verify"></a>

#### `DCOS_SSL_VERIFY`

Indicates whether to verify SSL certificates or set the path to the SSL certificates. You must set this variable manually. Setting this environment variable is equivalent to setting the `dcos config set core.ssl_verify` option in the DC/OS configuration [file](#configuration-files). For example, to indicate that you want to set the path to SSL certificates:

```bash
export DCOS_SSL_VERIFY=false
```

<a name="dcos-log-level"></a>

#### `DCOS_LOG_LEVEL`

Prints log messages to stderr at or above the level indicated. This is equivalent to the `--log-level` command-line option. The severity levels are:

* **debug** Prints all messages to stderr, including informational, warning, error, and critical.
* **info** Prints informational, warning, error, and critical messages to stderr.
* **warning** Prints warning, error, and critical messages to stderr.
* **error** Prints error and critical messages to stderr.
* **critical** Prints only critical messages to stderr.

For example, to set the log level to warning:

```bash
export DCOS_LOG_LEVEL=warning
```

<a name="dcos-debug"></a>

#### `DCOS_DEBUG`

Indicates whether to print additional debug messages to `stdout`. By default this is set to `false`. For example:

```bash
export DCOS_DEBUG=true
```