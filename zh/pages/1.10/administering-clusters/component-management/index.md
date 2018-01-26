---
layout: layout.pug
navigationTitle: 组件管理
title: 组件管理
menuWeight: 5
excerpt: ""
enterprise: false
---
组件管理 API 控制着 DC/OS 组件服务的安装和管理。 它由 DC/OS 安装程序在安装、升级和卸载过程中使用。 它不是为 DC/OS 用户的交互设计的。

## 组件包管理器

DCOS 组件包管理器 (Pkgpanda) 实现组件管理 API 并在所有 DC/OS 节点上运行。

[ pkgpanda ](https://github.com/dcos/dcos/tree/master/pkgpanda) 由两部分组成: 一个包生成器和一个包管理程序。

- ** 包生成器 "** 从源代码和预编译的工件生成并捆绑组件包, 作为 DC/OS 发布构建过程的一部分。
- ** 包管理器 ** 包括在 DC/OS 的一部分中, 并在每个节点上运行, 管理该节点上已安装和已激活的组件包。

由包生成器生成的组件包作为每个版本的 DC/OS 安装程序的一部分进行分发。 安装程序将组件包运送到每个节点, 并协调组件管理 API 来安装它们。 组件包包含一个或多个 systemd 服务定义、二进制文件和配置档。

## 组件运行状况

组件运行状况由 DC/OS 诊断组件监视。有关组件监视的详细信息, 请参阅 [ 监视 ](/1.10/monitoring/)。

## 组件

组件日志被发送到 journald, 并由 DC/OS 日志组件公开。有关组件日志的详细信息, 请参阅 [ 日志记录 ](/1.10/monitoring/logging/)。

## 途径

组件管理 API 通过管理路由器和管理路由器代理在所有节点的 `/pkgpanda/` 路径下公开。

## 资源

[swagger api='/1.10/api/pkgpanda.yaml']