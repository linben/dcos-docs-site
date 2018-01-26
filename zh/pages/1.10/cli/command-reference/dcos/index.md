---
layout: layout.pug
navigationTitle: dcos
title: dcos
menuWeight: 0
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

# 描述

中间层数据中心操作系统 (DC/OS) 的命令行实用程序。

# 统计

```bash
dcos [options] [<command>] [<args>...]
```

在没有选项、命令或参数的情况下运行命令会打印可用的命令。

# 选项

| 姓名、速记                           | 默认 | 描述                         |
| ------------------------------- | -- | -------------------------- |
| `--debug`                       |    | 启用调试模式                     |
| `--help, h`                     |    | 打印使用。                      |
| `--log-level=<log-level>` |    | 设置日志记录级别。此设置不影响发送到标准输出的结果。 |
| `--version, v`                  |    | 显示版本信息                     |

` < 日志级别 > `严重性级别为:

* 调试打印所有邮件。
* 信息打印信息、警告、错误和重要消息。
* 警告打印警告、错误和重要消息。
* 错误打印错误和重要消息。
* 关键信息仅打印关键消息到 stderr。