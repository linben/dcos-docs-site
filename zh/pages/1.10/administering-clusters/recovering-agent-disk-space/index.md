---
layout: layout.pug
navigationTitle: 恢复代理磁盘空间
title: 恢复代理磁盘空间
menuWeight: 900
excerpt: ""
enterprise: false
---
<!-- This source repo for this topic is https://github.com/dcos/dcos-docs -->

如果任务填满代理节点的保留卷，则有几个恢复空间的选项：

- 检查每个组件的健康状况并重新启动每个组件。

- 如果工作目录位于单独的卷上(如[Agent节点](/1.10/installing/oss/custom/system-requirements/#agent-nodes)中的建议)，则可以清空 卷并重新启动代理程序。

如果这两种方法都不起作用，则可能需要重新映像节点。