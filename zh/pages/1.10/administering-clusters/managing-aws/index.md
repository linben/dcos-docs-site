---
layout: layout.pug
navigationTitle: 管理 AWS
title: 管理 AWS
menuWeight: 9
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

## 缩放 AWS 群集

DC/OS AWS CloudFormation 模板经过优化以运行 dc/os, 但您可能希望根据需要更改代理节点的数量。

** 重要: **缩小 AWS 群集可能导致数据丢失。 建议您一次向下扩展1节点, 让 DC/OS 服务恢复。 例如, 如果您运行的是 DC/OS 服务, 并且从10到5节点向下扩展, 这可能会导致丢失服务的所有实例。

要使用 AWS 更改agent节点的数量:

1. 从 [AWS CloudFormation Management](https://console.aws.amazon.com/cloudformation/home) 页中, 选择您的 DC/OS 群集, 然后单击 **Update Stack**。
2. 单击 ** 指定参数 ** 页, 您可以为 ** PublicSlaveInstanceCount ** 和 ** SlaveInstanceCount ** 指定新值。
3. 在**Options**页面上，接受默认设置，然后点击**Next**。 **Tip:**您可以选择是否在失败时回滚。 默认情况下, 此选项设置为 **Yes**。
4. 在 ** Review ** 页上, 选中 "确认" 框, 然后单击 ** Create**。

您的新机器将需要几分钟时间来初始化; 您可以在EC2控制台中观看它们。 新节点注册后，DC / OS Web界面将立即更新。

<!-- ## Upgrading

See the upgrade [documentation](/1.10/installing/oss/cloud/aws/upgrading/). -->