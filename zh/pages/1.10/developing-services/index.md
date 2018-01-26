---
layout: layout.pug
title: 开发DC / OS服务
menuWeight: 160
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

DC / OS包含一个服务打包规范和一个[存储库](/1.10/administering-clusters/repo/)，用于对这些包进行编目。 本节介绍在DC / OS上打包和提供自己的服务所需的内容。

# <a name="universe"></a>宇宙包装资料库

DC/OS 宇宙包含在 dc/os 上可安装的所有服务。 有关 DC/OS 宇宙的更多信息, 请参见 [ github 宇宙存储库 ](https://github.com/mesosphere/universe)。

# DC/OS 服务结构

宇宙中的每个 DC/OS 服务都由 JSON 配置文件组成。这些文件用于创建安装在 DC/OS 上的软件包。

| 文件名                      | 描述                                    | 必填  |
| ------------------------ | ------------------------------------- | --- |
| `config.json`            | 指定支持的配置属性, 表示为 JSON 架构。               | No  |
| `marathon.json.mustache` | 指定用于创建能够运行服务的马拉松应用程序定义的小胡子模板。         | No  |
| `package.json`           | 指定包的高级别元数据。                           | Yes |
| `resource.json`          | 指定所有必需的外部托管资源 (例如, 泊坞窗图像、HTTP 对象和图像)。 | No  |

有关详细信息, 请参阅 [ 创建 DC/OS 程序包 ](https://github.com/mesosphere/universe/blob/version-3.x/docs/tutorial/GetStarted.md#step-3--creating-a-dcos-package)。

# 发布包

All packaged services are required to meet a certain standard as defined by Mesosphere. For details on publishing a DC/OS service, see [Publish the package](https://github.com/mesosphere/universe/blob/version-3.x/docs/tutorial/GetStarted.md#step-5--publish-the-package).