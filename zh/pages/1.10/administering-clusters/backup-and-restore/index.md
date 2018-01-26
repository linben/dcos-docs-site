---
layout: layout.pug
navigationTitle: 备份和还原
title: 备份和还原
menuWeight: 7
excerpt: ""
enterprise: true
---
从DC / OS 1.10开始，您可以备份群集的本地Marathon实例的状态，然后从该备份中恢复。

执行升级或降级之前，您可能希望备份群集。 如果升级过程中出现问题，或者安装的Universe程序包不能按预期的方式运行，则可能需要将群集恢复到已知的正常状态。

# 筛选

- 从DC / OS 1.10开始，备份只包括在主节点上运行的Marathon状态。
- 您只能从DC / OS Enterprise的[备份和恢复CLI ](/1.10/administering-clusters/backup-and-restore/backup-restore-cli)和[备份和恢复API](/1.10/administering-clusters/backup-and-restore/backup-restore-api)。

**重要**执行备份或还原时，Marathon将重新启动，以便能够以一致的状态执行操作。 这不会影响正在运行的任务，但是如果某个任务正在启动某个任务，则可能会遇到短暂的不可用性。