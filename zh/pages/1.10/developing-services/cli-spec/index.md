---
layout: layout.pug
navigationTitle: CLI规范
title: CLI规范
menuWeight: 3
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

[ DC / OS命令行界面(CLI)](/1.10/cli)是用于管理群集节点，安装和管理软件包，检查群集状态以及管理服务和任务的实用程序。

DC / OS CLI是开放和可扩展的：任何人都可以创建一个新的子命令，并使其可供最终用户安装。 For example, the [Spark DC/OS service](https://github.com/mesosphere/spark-build) provides CLI extensions for working with Spark. When installed, you can type the following command to submit Spark jobs and query their status:

    dcos spark [<flags>] <command>
    

This document is intended for a developer creating new DC/OS CLI subcommands. See also [Universe Getting Started](https://github.com/mesosphere/universe/blob/version-3.x/docs/tutorial/GetStarted.md).

# DC / OS CLI如何发现子命令

When you run the `dcos` command, it searches the current shell's PATH for executables with names that are prefixed with `dcos-` in the `~/.dcos/clusters/<cluster_id>/subcommands/<package_name>/env/bin` directory.

## Installing a CLI subcommand

To install a CLI subcommand, run:

    dcos package install <package>
    

或

    dcos package install <package> --cli
    

The same [packaging format and repository](https://github.com/mesosphere/universe/blob/version-3.x/README.md) is used for both DC/OS services and CLI subcommands.

**Note:** CLI modules are [cluster-specific](/1.10/cli/multi-cluster-cli) and stored in `~/.dcos/clusters/<cluster_id>/subcommands`. You must install a CLI module for each cluster. For example, if you connect to cluster 1, and install the Spark module, then connect to cluster 2 which is also running Spark, Spark CLI commands are not available until you install the module for that cluster.

## Creating a DC/OS CLI subcommand

### 要求

* Executables for Mac, Linux, and Windows

### Standard flags

You must assign a standard set of flags to each DC/OS CLI subcommand, described below:

    --info
    --help
    -h
    

#### --info

The `--info` flag shows a short, one-line description of the function of your subcommand. This content is displayed when the user runs `dcos help`.

##### Example from the Spark CLI:

    dcos spark --info
    Spark DC/OS CLI Module
    

When you run the `dcos` command without parameters, the info is returned for each subcommand:

    dcos | grep spark
          spark        Spark DC/OS CLI Module
    

#### --help and -h

The `--help` and `-h` flags both show the detailed usage for your subcommand.

Example from the Marathon CLI:

    dcos marathon --help
    Description:
        Deploy and manage applications to DC/OS.
    ...
    

### Subcommand naming conventions

The DC/OS CLI subcommand naming convention is:

    dcos <subcommand> <resource> <verb>
    

A `resource` is typically a noun and `verb` is an action supported by the resource. For example, in the following command, `resource` is `app` and the action is `add`:

    dcos marathon app add
    

### Subcommand logging

The environment variable `DCOS_LOG_LEVEL` is set to the log level the user sets at the command line.

The logging levels are described in [Python’s logging HOWTO](https://docs.python.org/2/howto/logging.html#when-to-use-logging): DEBUG, INFO, WARNING, ERROR and CRITICAL.

### Packaging a CLI subcommand

To make your subcommand available to end users:

1. Add a package entry to the Mesosphere Universe repository. See the [Universe README](https://github.com/mesosphere/universe/blob/version-3.x/README.md) for the specification.

The package entry must contain a file named [resource.json](https://github.com/mesosphere/universe/blob/version-3.x/README.md#resourcejson) that contains links to the executable subcommands.

When you run `dcos package install <package> --cli`:

1. The package entry for <package> is retrieved from the repository.
2. The `resource.json` file is parsed to find the CLI resources.
3. The executable for the user's platform is downloaded.

### DC/OS CLI 模块

The [DC/OS CLI module](https://github.com/dcos/dcos-cli) has a set of tools useful to subcommand developers.

## Example: Hello World subcommand

The [Hello World example](https://github.com/mesosphere/dcos-helloworld) implements a new subcommand called `helloworld`:

    dcos package install helloworld --cli
    dcos helloworld