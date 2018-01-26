---
layout: layout.pug
navigationTitle: 部署服务和吊舱
title: 部署服务和吊舱
menuWeight: 130
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

DC/OS 使用马拉松来管理流程和服务。马拉松是 DC/OS 的 "init 系统"。马拉松开始并监视您的应用程序和服务, 自动修复故障。

本机马拉松实例作为 DC/OS 安装的一部分安装。 在 dc/os 启动后, 您可以通过 dc/os web 界面的 ** 服务 ** 选项卡或使用 ` dcos 马拉松 ` 命令的 dc/os CLI 来管理本机马拉松实例。

DC/OS 服务是在 DC/OS 上部署的马拉松应用程序。 DC / OS服务可以从包存储库获得，例如[ Mesosphere Universe ](/1.10/overview/concepts/#mesosphere-universe)，或者您可以创建自己的。

# DC/OS 服务

您可以运行从 [ "宇宙软件包存储库" ](/1.10/gui/catalog/) 创建或安装软件包的 DC/OS 服务。 在 DC/OS web 界面的 ** 服务 ** 选项卡上, 当它们运行时, 您创建的服务和从 "从宇宙中安装的" 都将出现。

您自己创建的服务由马拉松管理, 并且可以通过 ` dcos 马拉松 ` 子 (例如 ` dcos 马拉松应用程序添加 < myapp >) 配置和运行 <a href="/1.10/cli/command-reference/"> 来自 DC/OS cli </a>. json`) 或通过 DC/OS web 界面。

# 宇宙包装资料库

由中间层或社区创建的封装的 dc/os 服务, 如火花或卡夫卡, 出现在 dc/os web 界面的 ** 目录 ** 选项卡上, 或者您可以从 [ dc/os cli ](/1.10/cli/command-reference/) 中搜索包。 您可以使用` dcos package install< package-name> `命令，从DC / OS Web界面或通过DC / OS CLI配置和运行Universe服务。